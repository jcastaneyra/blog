---
layout: post
title: "Recuperando GRUB con el disco de instalación de Debian"
date: 2006/10/06
comments: true
categories: 
---

Anoche me puse a instalar un nuevo disco duro de 160 Gb en una maquinilla que tengo en la casa, que está corriendo Debian Sarge y que está funcionando como servidor.
Este nuevo disco duro vino a sustituir un par de discos (uno de 6 Gb en el que estaban / y swap y el otro de 2.5 Gb que tenía /home), los cuáles ya tenían todo instalado, y como no quise instalar de nueva cuenta el Debian (que hueva de imaginarse todo lo que implicaría, instalar desde ceros, levantar servicios, usuarios, etc.), decidí usar el GParted Live CD, muy útil por cierto, para copiar las particiones del disco duro principal (el disco duro que tenía /boot, y swap) y expandir la partición / hasta ocupar todo el disco.

<!-- more -->

La copia de estas particiones y la expansión de / fue exitosa, pero con lo que no conté fue con que el Grub no estaría en el MBR de este nuevo disco duro, obvio verdad, bueno, pues me di a la tarea de reinstalar Grub en este disco, busqué en internet y todo parecía facil, pero a lo que pensé me llevaría no más de una hora me tomó cinco horas dado a unos pequeños errores que cometí, y he aquí mis experiencias.
Bien, lo primero que hice fue arrancar con el disco de instalación de Debian, y hasta el momento que dice que si queremos particionar el disco duro, ahí presionamos las teclas &lt;Alt&gt; &lt;F2&gt;, ahí tendremos acceso al shell.
Una vez en el shell, creamos un directorio donde montaremos la partición en donde se encuentra /boot, en este caso yo cree /disk de la siguiente manera:
<pre># mkdir /disk</pre>
Para saber que partición vamos a montar, habrá que darle el comando siguiente:
<pre># fdisk -l</pre>
Con este comando nos saldrá una lista de particiones, y tendrán un nombre raro, pero aún así será facil reconocer la partición que nos interesa montar (la que tiene /boot), lo siguiente será montar la partición (el dispositivo me imagino que variará, aún así este dispositivo lo veremos en la lista de particiones mostradas por el comando anterior):
<pre># mount -t ext3 /dev/bus0/lun0/disk0/part1 /disk</pre>
Ahora bien, haremos esta partición montada como nuestra partición root:
<pre># chroot /disk</pre>
En mi caso, como los discos los cambié de IDE, los nombres de las particiones cambiaron, por lo tanto tuve que editar /boot/grub/menu.lst, aquí fue donde tuve los problemas que me hicieron tardarme las cinco horas, todo por no fijarme bien al momento de editar, tuve que modificar la línea de root a (hd0,0) y me faltó editar en donde se encontraba el kernel, entonces como esta apuntaba a otra partición en otro dispositivo debí haberla editado por eso ahí me pasé las horas sin saber porque no podía arrancar, pero bien, ya que encontré el problema, así más o menos quedó lo que edité:
<pre lang="bash">
title           Debian GNU/Linux, kernel 2.6.16-2-686
root            (hd0,0)
kernel          /boot/vmlinuz-2.6.16-2-686 root=/dev/hda1 ro
initrd          /boot/initrd.img-2.6.16-2-686
savedefault
boot</pre>
Posteriormente, hubo que ejecutar:
<pre># grub</pre>
En el shell de grub, ejecuté lo siguiente:
<pre lang="bash">
root (hd0,0)
setup (hd0)
quit</pre>
Y listo, a reiniciar!
Con todo esto quedó, ya después sólo copié /home que estaba en el segundo disco duro,  claro de haber sido cuidadoso al leer lo que estaba haciendo, esto en teoría debió haber durado menos de una hora, entonces aquí está la experiencia para los que quieran cambiar de discos duros puedan hacerlo en el menor tiempo del que yo lo hice.
:D
