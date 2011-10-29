---
layout: post
title: "Installing RabbitMQ + Stomp in Ubuntu/Mac"
date: 2010/05/30
comments: true
categories: 
---

<a name="rabbitmq_stomp_english"></a> <a href="#rabbitmq_stomp_spanish">En español</a>

![Screenshot of RabbitMQ logo][rabbitmqlogo]

In the project I'm working now it was needed to have RabbitMQ running with Stomp as a message queue for Orbited, we know that Orbited comes with MorbidQ, but in our system we already had running RabbitMQ and used by other processes, so, two queue systems at the same time were not needed.

<!-- more -->

But, searching for Stomp over RabbitMQ info or some kind of tutorial was difficult and painful, a friend of mine would say "a real pain in the ass", there was info but not too clear. For that reason here is another note to myself, just in case.

*  RabbitMQ 1.6.0 installed, [here there are some steps][steps] to install it.
*  Mercurial is needed to download Stomp code
*  Download Stomp code

```
hg clone -r rabbitmq_v1_6_0 http://hg.rabbitmq.com/rabbitmq-stomp
```

*  Compile Stomp (use your RabbitMQ include directory, maybe in Linux /usr/lib/erlang/lib/rabbitmq_server-1.6.0/include, in Mac I have it here /usr/local/lib/erlang/lib/rabbitmq_server-1.6.0/include)

```
cd rabbitmq-stomp
make RABBIT_SERVER_INCLUDE_DIR=/usr/lib/erlang/lib/rabbitmq_server-1.6.0/include
```

*  Copy your compiled Stomp to the Erlang libraries

```
sudo mkdir -p /usr/lib/erlang/lib/rabbitmq-stomp
sudo cp -R * /usr/lib/erlang/lib/rabbitmq-stomp
```

*  Add the Stomp configuration for RabbitMQ (also use your correct directory here, in Linux /usr/lib/erlang/lib/rabbitmq-stomp/ebin and in my Mac /usr/local/lib/erlang/lib/rabbitmq-stomp/ebin)

```
sudo vim /etc/rabbitmq/rabbitmq.conf

#Add the lines below
NODENAME=rabbit
NODE_IP_ADDRESS=0.0.0.0
NODE_PORT=5672

LOG_BASE=/var/log/rabbitmq
MNESIA_BASE=/var/lib/rabbitmq/mnesia

SERVER_START_ARGS='
     -pa /usr/lib/erlang/lib/rabbitmq-stomp/ebin
     -rabbit
       stomp_listeners [{"0.0.0.0",61613}]
       extra_startup_steps [{"STOMP-listeners",rabbit_stomp,kickstart,[]}]'
```

And that's it, if you start RabbitMQ will see how STOMP-Listeners are starting (In Linux you maybe will need to stop RabbitMQ with the following command sudo /etc/init.d/rabbitmq-server stop before running the next step, in my Mac I use this command to start RabbitMQ sudo /usr/local/lib/erlang/lib/rabbitmq_server-1.6.0/sbin/rabbitmq-server)

```
deploy@localhost:~$ sudo rabbitmq-server 
RabbitMQ 1.6.0 (AMQP 8-0)
Copyright (C) 2007-2009 LShift Ltd., Cohesive Financial Technologies LLC., and Rabbit Technologies Ltd.
Licensed under the MPL.  See http://www.rabbitmq.com/

node        : rabbit@localhost
log         : /var/log/rabbitmq/rabbit.log
sasl log    : /var/log/rabbitmq/rabbit-sasl.log
database dir: /var/lib/rabbitmq/mnesia/rabbit

starting database             ...done
starting core processes       ...done
starting recovery             ...done
starting persister            ...done
starting guid generator       ...done
starting builtin applications ...done
starting TCP listeners        ...done
starting STOMP-listeners      ...done

broker running
```


<a name="rabbitmq_stomp_spanish"></a> <a href="#rabbitmq_stomp_english">In english</a>

![Screenshot of RabbitMQ logo][rabbitmqlogo]

En el proyecto en el que estoy trabajando era necesario tener corriendo RabbitMQ con Stomp para poder hacer conexiones con Orbited, Orbited ya incluye MorbidQ, pero en este caso ya teníamos corriendo RabbitMQ para otros procesos y no era necesario tener un segundo sistema de queues.

Sorprendentemente, encontrar información de cómo agregar Stomp a RabbitMQ resultó algo doloroso, si había información pero como que no indicaba bien que pasos seguir. Entonces como parte de mis notas personales estoy agregando esta receta por si se llega a ofrecer de nueva cuenta.

*  Tener instalado RabbitMQ 1.6.0, [aquí hay unos pasos][steps] para instalarlo.
*  Tener instalado Mercurial para bajar el código de Stomp
*  Bajar el código de Stomp

```
hg clone -r rabbitmq_v1_6_0 http://hg.rabbitmq.com/rabbitmq-stomp
```

*  Compilar Stomp (apuntando al directorio de include donde se encuentra RabbitMQ, en Linux /usr/lib/erlang/lib/rabbitmq_server-1.6.0/include y en Mac yo lo tengo en /usr/local/lib/erlang/lib/rabbitmq_server-1.6.0/include)

```
cd rabbitmq-stomp
make RABBIT_SERVER_INCLUDE_DIR=/usr/lib/erlang/lib/rabbitmq_server-1.6.0/include
```

*  Copiar Stomp compilado al directorio de librerías de Erlang

```
sudo mkdir -p /usr/lib/erlang/lib/rabbitmq-stomp
sudo cp -R * /usr/lib/erlang/lib/rabbitmq-stomp
```

*  Agregar la configuración de Stomp a RabbitMQ (también apuntar al directorio correcto en la configuración, en Linux /usr/lib/erlang/lib/rabbitmq-stomp/ebin y en mi Mac /usr/local/lib/erlang/lib/rabbitmq-stomp/ebin)

```
sudo vim /etc/rabbitmq/rabbitmq.conf

#Add the lines below
NODENAME=rabbit
NODE_IP_ADDRESS=0.0.0.0
NODE_PORT=5672

LOG_BASE=/var/log/rabbitmq
MNESIA_BASE=/var/lib/rabbitmq/mnesia

SERVER_START_ARGS='
  -pa /usr/lib/erlang/lib/rabbitmq-stomp/ebin
  -rabbit
    stomp_listeners [{"0.0.0.0",61613}]
    extra_startup_steps [{"STOMP-listeners",rabbit_stomp,kickstart,[]}]'
```

Listo, si intentan arrancar RabbitMQ se verá que dice que está levantando los STOMP-Listeners (En Linux talvez necesiten terminar el proceso con sudo /etc/init.d/rabbitmq-server stop antes de correr lo siguiente, y en Mac para levantar el servidor yo ejecuto sudo /usr/local/lib/erlang/lib/rabbitmq_server-1.6.0/sbin/rabbitmq-server)

```
deploy@localhost:~$ sudo rabbitmq-server 
RabbitMQ 1.6.0 (AMQP 8-0)
Copyright (C) 2007-2009 LShift Ltd., Cohesive Financial Technologies LLC., and Rabbit Technologies Ltd.
Licensed under the MPL.  See http://www.rabbitmq.com/

node        : rabbit@localhost
log         : /var/log/rabbitmq/rabbit.log
sasl log    : /var/log/rabbitmq/rabbit-sasl.log
database dir: /var/lib/rabbitmq/mnesia/rabbit

starting database             ...done
starting core processes       ...done
starting recovery             ...done
starting persister            ...done
starting guid generator       ...done
starting builtin applications ...done
starting TCP listeners        ...done
starting STOMP-listeners      ...done

broker running
```

  [rabbitmqlogo]: http://c243421.r21.cf1.rackcdn.com/RabbitMQLogo-300x79.png
  [steps]: /2009/06/07/instalando-rabbitmq/
