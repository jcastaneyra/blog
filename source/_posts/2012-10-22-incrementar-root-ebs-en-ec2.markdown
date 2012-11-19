---
layout: post
title: "Como incrementar el root EBS de una instancia EC2"
date: 2012-10-22 12:06
comments: true
categories: aws
---

{% img right https://s3-us-west-1.amazonaws.com/jcastaneyra-blog/images/aws_logo.jpeg %}

Las instancias de EC2 que corren con EBS (Elastic Block Storage) cuando
se crean vienen con un tamaño relativamente pequeño (talvez 8 Gb o un
poco más), ¿que hay si este tamaño lo quiero incrementar?, ¿que tengo que
hacer?

<!--more-->

*Assumptions*: Se asume  de que ya se tienen instaladas y configuradas las herramientas
de línea de comandos de AWS, también cabe mencionar que los comandos que
aquí aparecen les precede el prompt $, en el caso de la instancia remota
también precedido por el host.

Backups: Respaldar información sensible, si se tiene información
importante en /mnt respaldarla ya que esta se perderá al hacer este
proceso.

Al conectarme por ssh a mi instancia y verificar el tamaño del
filesystem veo que tan sólo tiene 7.9G.

```
ubuntu@ip-10-170-185-61:~$ df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/xvda1      7.9G  773M  6.8G  11% /
udev            288M  8.0K  288M   1% /dev
tmpfs           119M  160K  118M   1% /run
none            5.0M     0  5.0M   0% /run/lock
none            296M     0  296M   0% /run/shm
```

Siguiendo el tutorial de Eric Hammond de alestic.com (y modificado
ligeramente ya que algunas cosas no corrieron), desde mi lap donde
tengo instalada la herramienta de aws, corro lo siguiente, primero
hay que obtener el id de instancia.

```
$ ec2-describe-instances                                                                            
RESERVATION	r-54a9ea12	672080195794	default
INSTANCE	i-2ddc0374	ami-0d153248	ec2-50-18-95-56.us-west-1.compute.amazonaws.com	ip-10-170-185-61.us-west-1.compute.internal	running	jcastaneyra_west	0		t1.micro	2012-10-12T14:47:45+0000	us-west-1c	aki-8d396bc8		monitoring-disabled	50.18.95.56	10.170.185.61			ebs					paravirtual	xen	vcdyJ1350053264046	sg-6b43132e	default
BLOCKDEVICE	/dev/sda1	vol-3885d716	2012-10-12T14:47:49.000Z	true
TAG	instance	i-2ddc0374	Name	

```

El id es i-2ddc0374, y el tamaño al que quiero llegar es de 20 Gb, por
lo que voy a correr lo siguiente desde línea de comandos.

```
$ instanceid=i-2ddc0374
$ size=20
```

Hay que obtener el volumen y la zona actual de la instancia.

```
$ oldvolumeid=$(ec2-describe-instances $instanceid | egrep "^BLOCKDEVICE./dev/sda1" | cut -f3)
$ zone=$(ec2-describe-instances $instanceid | egrep "^INSTANCE" | cut -f12)
```

Si se ejecuta el siguiente comando debería aparecer.

```
$ echo "instance $instanceid in $zone with original volume $oldvolumeid"
instance i-2ddc0374 in us-west-1c with original volume vol-3885d716
```

Ahora, hay que detener la instancia (ojo, detener no terminar).

```
$ ec2-stop-instances $instanceid
INSTANCE	i-2ddc0374	running	stopping
```

Desasociar el volumen actual de la instancia.

```
$ while ! ec2-detach-volume $oldvolumeid; do sleep 1; done
ATTACHMENT	vol-3885d716	i-2ddc0374	/dev/sda1	detaching	2012-10-12T14:47:49+0000
```

Crear un snapshot del volumen actual.

```
$ snapshotid=$(ec2-create-snapshot $oldvolumeid | cut -f2)
$ while ec2-describe-snapshots $snapshotid | grep -q pending; do sleep 1; done
$ echo "snapshot: $snapshotid"
snapshot: snap-5d91ed71
```

Crear un nuevo volumen basado en el snapshot recien creado.

```
$ newvolumeid=$(ec2-create-volume   --availability-zone $zone   --size $size   --snapshot $snapshotid | cut -f2)
$ echo "new volume: $newvolumeid"
new volume: vol-e6b0e2c8
```

Asociar el nuevo volumen a la instancia.

```
$ ec2-attach-volume   --instance $instanceid   --device /dev/sda1   $newvolumeid
ATTACHMENT	vol-e6b0e2c8	i-2ddc0374	/dev/sda1	attaching	2012-10-12T15:25:12+0000
$ while ! ec2-describe-volumes $newvolumeid | grep -q attached; do sleep 1; done
```

Ahora bien, se arranca la instancia.

```
$ ec2-start-instances $instanceid
INSTANCE	i-2ddc0374	stopped	pending
$ while ! ec2-describe-instances $instanceid | grep -q running; do sleep 1; done
```

Buscamos ahora la nueva ip pública.

*NOTA*: si la instancia tenía una elastic ip
asociada tendremos que volverla a asociar de nuevo.

```
$ ec2-describe-instances $instanceid
RESERVATION	r-54a9ea12	672080195794	default
INSTANCE	i-2ddc0374	ami-0d153248	ec2-50-18-24-164.us-west-1.compute.amazonaws.com	ip-10-170-201-117.us-west-1.compute.internal	running	jcastaneyra_west	0		t1.micro	2012-10-12T15:26:54+0000	us-west-1c	aki-8d396bc8			monitoring-disabled	50.18.24.164	10.170.201.117			ebs					paravirtual	xen	vcdyJ1350053264046	sg-6b43132e	default
BLOCKDEVICE	/dev/sda1	vol-e6b0e2c8	2012-10-12T15:25:12.000Z	false
TAG	instance	i-2ddc0374	Name	
```

Al conectarnos de nueva cuenta con ssh vemos que el filesystem ya está
con un nuevo tamaño.

```
ubuntu@ip-10-170-201-117:~$ df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/xvda1       20G  777M   18G   5% /
udev            288M  8.0K  288M   1% /dev
tmpfs           119M  160K  118M   1% /run
none            5.0M     0  5.0M   0% /run/lock
none            296M     0  296M   0% /run/shm
```

Después de esto ya podríamos borrar el volumen viejo, e incluso si
estamos seguros que todo quedó bien podríamos también borrar el
snapshot.

```
$ ec2-delete-volume $oldvolumeid
VOLUME	vol-3885d716
$ ec2-delete-snapshot $snapshotid
SNAPSHOT	snap-5d91ed71
```

NOTA: Como el nuevo volumen fue creado manualmente, al momento de
terminar la instancia este no se borra automáticamente, creo que hay un
parametro en los atributos de la instancia para volver a habilitar el
borrado automático, pero yo no lo encontré, pero digo, borrarlo
manualmente tampoco me llevó mucho tiempo.


LINKS.

[alestic blog](http://alestic.com/2010/02/ec2-resize-running-ebs-root)


