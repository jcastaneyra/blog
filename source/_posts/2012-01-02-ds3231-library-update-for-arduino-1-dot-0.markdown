---
layout: post
title: "DS3231 Library update for Arduino 1.0"
date: 2012-01-02 20:06
comments: true
categories: Arduino
---

{% img right http://c243421.r21.cf1.rackcdn.com/ds3231.jpg %}
Ya les había mencionado de mi compra del Seeeduino Stalker, y obviamente empecé a hacer mis primeras pruebas.

Este clon de Arduino trae un RTC DS3231 y en Seeedstudio.com puedes descargar la librería del mismo, pero resulta que con los cambios que se hicieron en el IDE 1.0 de Arduino los sketches que vienen con la librería no funcionan, así que tuve que hacer unos cambios, los cambios en específico sólo se hicieron al archivo DS3231.cpp.

<!--more-->

Toda la librería la subí aquí:

[https://github.com/jcastaneyra/ds3231_library](https://github.com/jcastaneyra/ds3231_library)

Referencias:

[http://www.seeedstudio.com/wiki/Seeeduino_Stalker_v2.1](http://www.seeedstudio.com/wiki/Seeeduino_Stalker_v2.1)
[http://blog.makezine.com/archive/2011/12/arduino-1-0-is-out-heres-what-you-need-to-know.html](http://blog.makezine.com/archive/2011/12/arduino-1-0-is-out-heres-what-you-need-to-know.html)

