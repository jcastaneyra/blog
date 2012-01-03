---
layout: post
title: "Testing range between xbee's"
date: 2012-01-03 15:16
comments: true
categories: Arduino Xbee
---

Como hace unos días mencioné de la adquisición de componentes para el Arduino, entre ellos unos Xbee´s de 2 mW y con la antena en chip (Serie 2), me hice a la tarea de probarlos y ver que tanto rango de alcance soportan.

<!-- more -->

## Configuración

### XBee's

Obviamente, para poder probar la comunicación entre Xbee's requerí de un par, uno configurado como Coordinator (el que estaba conectado al Arduino UNO) y el otro como Router (el que se conectó al Seeeduino Stalker), ambos Xbee's estaban configurados con el último firmware y con un baudrate de 57600 bps.

### Arduino UNO

Al Arduino UNO se le conectó el Xbee Coordinator por medio de un shield para Xbee de Seeedstudio.

{% img center http://c243421.r21.cf1.rackcdn.com/xbee_coordinator.jpg %}

El código es simple, y se ve a continuación.

{% gist 1556559 %}

### Seeeduino Stalker

Al Seeeduino Stalker se le conectó el Xbee Router, y el Buzzer al pin 11. El Stalker como trae su batería recargable y su celda solar, lo puedo tomar e irme a caminar sin ningún problema.

{% img center http://c243421.r21.cf1.rackcdn.com/xbee_router.jpg %}

El código de la función de buzz lo tomé de un sitio el cual no recuerdo :) , podríamos decir que estoy reutilizando código.

{% gist 1556573 %}

## Pruebas y Conclusiones

El alcance que me da es muy poco cuando existen paredes relativamente gruesas, y más aquí en México, que los materiales para paredes suelen ser ladrillo y cemento, al menos eso creo, ya que según pude ver, el alcance suele ser de unos 5 a 10 metros cuando hay paredes, y cuando si hay línea de vista, como de 20 a 30 metros.

<iframe width="560" height="315" src="http://www.youtube.com/embed/M3sFa7AnNn0" frameborder="0" allowfullscreen></iframe>

