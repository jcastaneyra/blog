---
layout: post
title: "Raspberry PI primer contacto"
date: 2012-11-19 12:06
comments: true
categories: raspberry
---
{% img right https://s3-us-west-1.amazonaws.com/jcastaneyra-blog/images/rpi_02.jpg 260 180 %}
Por azares del destino tuve un primer contacto con la Raspberry PI, de
hecho yo quería comprarme una, pero no la había encontrado disponible, y
de repente se me aparece una en las manos.

<!--more-->

## Características

### Primero, ¿Qué es una Raspberry PI?

De la página de [raspberrypi.org](http://www.raspberrypi.org/faqs):

> The Raspberry Pi is a credit-card sized computer that plugs into your
TV and a keyboard. It’s a capable little PC which can be used for many
of the things that your desktop PC does, like spreadsheets,
word-processing and games. It also plays high-definition video. We want
to see it being used by kids all over the world to learn programming.

[{% img https://s3-us-west-1.amazonaws.com/jcastaneyra-blog/images/Raspi_Iso_Blue.png %}](https://s3-us-west-1.amazonaws.com/jcastaneyra-blog/images/Raspi_Iso_Blue.png)

### ¿Qué SoC (System on chip) se usa?

> The SoC is a Broadcom BCM2835. This contains an ARM1176JZFS, with
floating point, running at 700Mhz, and a Videocore 4 GPU. The GPU is
capable of BluRay quality playback, using H.264 at 40MBits/s. It has a
fast 3D core accessed using the supplied OpenGL ES2.0 and OpenVG
libraries.

### ¿Qué tan poderoso es?

> The GPU provides Open GL ES 2.0, hardware-accelerated OpenVG, and
1080p30 H.264 high-profile decode.

> The GPU is capable of 1Gpixel/s, 1.5Gtexel/s or 24 GFLOPs of general
purpose compute and features a bunch of texture filtering and DMA
infrastructure.

> That is, graphics capabilities are roughly equivalent to Xbox 1 level of
performance. Overall real world performance is something like a 300MHz
Pentium 2, only with much, much swankier graphics.

### Además

* 512MB RAM
* Arranca desde una SD card, corriendo una distribución de Linux
* 10/100 BaseT Ethernet socket
* Tiene GPIO (General purpose input/output). Son unos pines para poder
programarlos y que interactúen con otros dispositivos, sensores por
ejemplo.

[{% img https://s3-us-west-1.amazonaws.com/jcastaneyra-blog/images/rpi_02.jpg %}](https://s3-us-west-1.amazonaws.com/jcastaneyra-blog/images/rpi_02.jpg)

## Conectando y arrancando la Raspberry PI

### Lista de dispositivos y cables.

1. Obvio una Raspberry PI
2. SD card con distribución de Linux para RPI (esta ya venía incluída)
3. Teclado y mouse usb
4. Cable de red
5. Cable hdmi
6. Cable de USB a micro USB
7. Macbook para compartir internet

### Prueba

En mi caso, yo sólo lo quería echar a andar, ya venía con una SD card
con la distribución de linux de Raspberry precargada, por lo que me tuve
que saltar el paso de cargarsela, todo fue más simple. El mouse y
teclado usb que encontré lo tenía guardado entre las cosas viejas que
tenía, en las fotos verán que están todavía con polvo.

[{% img https://s3-us-west-1.amazonaws.com/jcastaneyra-blog/images/rpi_mouse.jpg %}](https://s3-us-west-1.amazonaws.com/jcastaneyra-blog/images/rpi_mouse.jpg)

La alimentación y la red se la proporcioné desde la macbook, por usb y
compartiendo la red inalámbrica.

[{% img https://s3-us-west-1.amazonaws.com/jcastaneyra-blog/images/rpi_con.jpg %}](https://s3-us-west-1.amazonaws.com/jcastaneyra-blog/images/rpi_con.jpg)

Por medio de hdmi conecté la TV de la sala para poder ver la salida, y
así se ve cuando está iniciando la raspberry.

[{% img https://s3-us-west-1.amazonaws.com/jcastaneyra-blog/images/rpi_init.jpg %}](https://s3-us-west-1.amazonaws.com/jcastaneyra-blog/images/rpi_init.jpg)

La primera vez que arranca aparece un menú para configurar ciertas
cosas, a ese menú no le tomé foto pero de las cosas que puedes
configurar es, el layout de tu teclado, habilitar o deshabilitar el
arranque de servidor de ssh, el password de usuario pi, etc. Después de
configurar la Raspberry se reinició.

Luego me apareció el login (el usuario es pi y el password por default
es raspberry) y a lo que inmediatamente después vi el
espacio libre de la SD.

[{% img https://s3-us-west-1.amazonaws.com/jcastaneyra-blog/images/rpi_sdsize.jpg %}](https://s3-us-west-1.amazonaws.com/jcastaneyra-blog/images/rpi_sdsize.jpg)

Inicialmente la compartición de la red no la configuré bien, por lo que
no tenía salida a internet.

[{% img https://s3-us-west-1.amazonaws.com/jcastaneyra-blog/images/rpi_error_net.jpg %}](https://s3-us-west-1.amazonaws.com/jcastaneyra-blog/images/rpi_error_net.jpg)

Una vez que configuré bien la compartición de internet en la macbook, lo
que hice en la consola de la raspberry fue correr el comando.

```
sudo dhclient eth0
```

Con este comando adquirió una IP dinámica del servidor de DHCP que se
tiene corriendo en la macbook para compartir internet.

[{% img https://s3-us-west-1.amazonaws.com/jcastaneyra-blog/images/rpi_dhclient.jpg %}](https://s3-us-west-1.amazonaws.com/jcastaneyra-blog/images/rpi_dhclient.jpg)

Una vez hecho esto, ya pude navegar.

[{% img https://s3-us-west-1.amazonaws.com/jcastaneyra-blog/images/rpi_browsing.jpg %}](https://s3-us-west-1.amazonaws.com/jcastaneyra-blog/images/rpi_browsing.jpg)

## Final

Es impresionante lo que han hecho los de Raspberry, tener una
computadora de ese tamaño capaz de reproducir video con calidad de
blueray y además de poder ejecutar juegos que requieran de alto
desempeño gráfico, y por un precio bajo.

La otra parte interesante es para poder progamarla e interfazarla con
dispositivos externos o bien como para enseñar a niños a usar y a
programar una computadora, ahora si no creo que haya tanto problema cuando nos
digan "tu hijo tiró la computadora, y luego la metio al agua".

## LINKS

[Raspberry FAQs](http://www.raspberrypi.org/faqs)


