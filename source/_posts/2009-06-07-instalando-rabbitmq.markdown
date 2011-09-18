---
layout: post
title: "Instalando RabbitMQ"
date: 2009/06/07
comments: true
categories: 
---

<a name="rabbitmq_spanish"/><strong>Instalando RabbitMQ</strong></a> (<a href="#rabbitmq_english">English</a>)

RabbitMQ es un sistema de message queue (MQ), el cual provee comunicaciones asíncronas, es decir que el productor y consumidor no tienen la necesidad de interactuar con los mensajes al mismo tiempo, además de que es una implementación del protocolo AMQP (Advanced Message Queuing Protocol), un protocolo para mensajeo de alto rendimiento, y por último decir que RabbitMQ está desarrollado con Erlang, Erlang es un lenguaje de programación funcional.

En Ubuntu 9.04
--------------

La instalación de RabbitMQ en Ubuntu 9.04 es tan simple como correr el comando

```
sudo aptitude install rabbitmq-server
```

Y con el cual si no has instalado Erlang te mostrará que paquetes tendrá que instalar para que lo tengas también.

```
jcastaneyra@ubuntu:~/sources$ sudo aptitude install rabbitmq-server
[sudo] password for jcastaneyra:
Reading package lists... Done
Building dependency tree
Reading state information... Done
Reading extended state information
Initializing package states... Done
Writing extended state information... Done
The following NEW packages will be installed:
erlang-base{a} erlang-nox{a} libltdl7{a} odbcinst1debian1{a} unixodbc{a}
The following partially installed packages will be configured:
rabbitmq-server
0 packages upgraded, 5 newly installed, 0 to remove and 8 not upgraded.
Need to get 28.6MB of archives. After unpacking 46.9MB will be used.
Do you want to continue? [Y/n/?]
```

Una vez instalado ya está corriendo.

En Mac leopard
--------------

*UPDATE (27/05/2010)*: Desde hace un rato que ya estoy usando Homebrew para instalar cosas en Mac, ya existe una fórmula para instalar también RabbitMQ  y por supuesto Erlang, para más información visitar <a href="http://github.com/mxcl/homebrew">http://github.com/mxcl/homebrew</a> y seguir las instrucciones para instalar Homebrew. Con RabbitMQ, incluso si se quiere una versión específica de RabbitMQ se puede modificar la fórmula :)

La instalación es a través de los MacPorts, hay que bajar el instalador de la página www.macports.org/install.php, pero para poder compilar es necesario tener XCode instalado. Los MacPorts son instalados en /opt, por lo que hay que asegurarnos que se tengamos agregados los paths necesarios en el profile para ejecutar los comandos de MacPorts.

En mi caso en .profile (pero también puede ser el .bash_login) debe estar algo así (si no está lo agrego):

```
# MacPorts Installer addition on 2009-02-25_at_15:37:48: adding an appropriate PATH variable for use with MacPorts.
export PATH=/opt/local/bin:/opt/local/sbin:$PATH
# Finished adapting your PATH environment variable for use with MacPorts.

# MacPorts Installer addition on 2009-02-25_at_15:37:48: adding an appropriate MANPATH variable for use with MacPorts.
export MANPATH=/opt/local/share/man:$MANPATH
# Finished adapting your MANPATH environment variable for use with MacPorts.
```

Una vez que están los MacPorts hay que instalar primero erlang, así que ejecutamos

```
sudo port install erlang
```

Y esperamos un buen rato a que se instale.

Una vez instalado erlang, bajamos la última versión de RabbitMQ

```
mkdir /tmp/rabbitmq && cd /tmp/rabbitmq
curl -O http://www.rabbitmq.com/releases/rabbitmq-server/v1.5.5/rabbitmq-server-generic-unix-1.5.5.tar.gz
tar xvzf rabbitmq-server-generic-unix-1.5.5.tar.gz
sudo mv rabbitmq_server-1.5.5/ /opt/local/lib/erlang/lib
```

Ahora ya lo podemos ejecutar.

```
sudo /opt/local/lib/erlang/lib/rabbitmq_server-1.5.5/sbin/rabbitmq-server
```

Recursos
--------

1.  [http://www.rabbitmq.com][rabbitmq]
2.  [http://playtype.net/past/2008/10/9/installing_rabbitmq_on_osx/][playtype]



<a name="rabbitmq_english"/><strong>Installing RabbitMQ</strong></a> (<a href="#rabbitmq_spanish">Español</a>)

RabbitMQ is a complete and highly reliable Enterprise Messaging system, it provides asynchronous communications,  meaning that the sender and receiver of the message do not need to interact with the message queue at the same time, also it is a AMQP implementation (Advanced Message Queuing Protocol) a protocol for high performance messaging, and last RabbitMQ is developed with Erlang, a functional programming language.

In Ubuntu 9.04
--------------

RabbitMQ instalation in Ubuntu 9.04 is so simple, it is just needed to run a command

```
sudo aptitude install rabbitmq-server
```

With this also install all packages needed by rabbitmq, Erlang is installed also.

```
jcastaneyra@ubuntu:~/sources$ sudo aptitude install rabbitmq-server
[sudo] password for jcastaneyra:
Reading package lists... Done
Building dependency tree
Reading state information... Done
Reading extended state information
Initializing package states... Done
Writing extended state information... Done
The following NEW packages will be installed:
erlang-base{a} erlang-nox{a} libltdl7{a} odbcinst1debian1{a} unixodbc{a}
The following partially installed packages will be configured:
rabbitmq-server
0 packages upgraded, 5 newly installed, 0 to remove and 8 not upgraded.
Need to get 28.6MB of archives. After unpacking 46.9MB will be used.
Do you want to continue? [Y/n/?]
```

Once the installation is finished RabbitMQ will be running.

In Mac leopard
--------------

*UPDATE (27/05/2010)*: I am using Homebrew to install some stuff in Mac from some months ago, Homebrew has a formula to install RabbitMQ and also Erlang, for more info please visit <a href="http://github.com/mxcl/homebrew">http://github.com/mxcl/homebrew</a> for install instructions. Also if you want to install a different version of RabbitMQ you can do it just editing the formula :)

RabbitMQ installation is done through MacPorts, you need to download the MacPorts installer from www.macports.org/install.php, also XCode is needed. MacPorts are installed in /opt, for that reason we put these paths in the .profile file (could be .bash_login), if you don't have these paths add them to your profile file:

```
# MacPorts Installer addition on 2009-02-25_at_15:37:48: adding an appropriate PATH variable for use with MacPorts.
export PATH=/opt/local/bin:/opt/local/sbin:$PATH
# Finished adapting your PATH environment variable for use with MacPorts.

# MacPorts Installer addition on 2009-02-25_at_15:37:48: adding an appropriate MANPATH variable for use with MacPorts.
export MANPATH=/opt/local/share/man:$MANPATH
# Finished adapting your MANPATH environment variable for use with MacPorts.
```

In order to install erlang execute the following command:

```
sudo port install erlang
```

You need to wait some time, maybe several minutes :)

Once you have erlang installed, we download the last RabbitMQ version

```
mkdir /tmp/rabbitmq && cd /tmp/rabbitmq
curl -O http://www.rabbitmq.com/releases/rabbitmq-server/v1.5.5/rabbitmq-server-generic-unix-1.5.5.tar.gz
tar xvzf rabbitmq-server-generic-unix-1.5.5.tar.gz
sudo mv rabbitmq_server-1.5.5/ /opt/local/lib/erlang/lib
```

Now we execute rabbitmq-server with this

```
sudo /opt/local/lib/erlang/lib/rabbitmq_server-1.5.5/sbin/rabbitmq-server
```

Links
-----

1.  [http://www.rabbitmq.com][rabbitmq]
2.  [http://playtype.net/past/2008/10/9/installing_rabbitmq_on_osx/][playtype]

[rabbitmq]: http://www.rabbitmq.com
[playtype]: http://playtype.net/past/2008/10/9/installing_rabbitmq_on_osx/


