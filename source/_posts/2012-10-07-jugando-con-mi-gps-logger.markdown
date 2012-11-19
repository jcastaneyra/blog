---
layout: post
title: "Jugando con mi gps logger"
date: 2012-10-07 22:06
comments: true
categories: Arduino
---

Tenía rato que no tocaba mis arduinos, ¡Desde enero! Pero recientemente
en el trabajo empezamos a platicar sobre la plataforma de Arduino, y de
como a algunos les llamaba la atención, por lo que de regreso en casa,
agarré mis arduinos, los desempolvé y los llevé al trabajo para
mostrarlos.

![gps
logger](https://s3-us-west-1.amazonaws.com/jcastaneyra-blog/images/logger.jpg)

Pero precisamente ahora, que estaba por viajar, traté de hacer mi
logger, digo, en realidad es simple lo que hace en general, pero la
diversión ha sido grande.

<!-- more -->

## Materiales y código

Lo que he usado ha sido:

1. Seeeduino stalker v2.1
2. GPS bee de Seeedstudio
3. Lithium Ion Battery de 980 mAh
4. Solar panel
5. Micro SD card de 2Gb
6. Seeeduino stalker enclosure

![componentes](https://s3-us-west-1.amazonaws.com/jcastaneyra-blog/images/logger_components.jpg)

El código después de un rato de haber hecho pruebas parece simple, pero
en realidad me encontré con algunos problemas que fui resolviendo sobre
la marcha, problemas, que si me remonto a enero ya había visto y que no
tenía ni idea de como resolver.

El programa utiliza las librerías de TinyGPS, Wire, DS3231 y SD.

La librería de TinyGPS la uso para ayudarme a parsear las sentencias
NMEA que va arrojando el gps, en un principio pensé que esta librería no
me estaba ayudando, ya que el gps tardaba mucho en arrojar sentencias
correctas, claro todas mis pruebas habían sido cerca de la ventana por
lo que el gps no agarraba bien, ahora las pruebas fueron fuera de la
ventana, y puedo decir que funciona muuuuy bien. Algo curioso fue que
estuvo guardando coordenadas mientras estaba dentro del metro,
interesante.

Con la librería de Wire, la verdad no se para que sirve, la habré sacado
de algún sketch que vi en la red y ya no tuve tiempo para probar que
pasaba si la quitaba de mi programa.

La librería DS3231 la utilizo para poderme comunicar con el chip que
trae el seeeduino stalker, este chip es de tiempo real y además de
temperatura, la librería ya la había modificado para que funcionara con
Arduino 1.0, no se si Seeedstudio ya tenga una nueva librería, por lo
que esta vez me fui rápido por lo que ya sabía que funcionaba, la
librería modificada se encuentra
[aquí](https://github.com/jcastaneyra/ds3231_library).

Y por último, la librería SD la utilicé para ir escribiendo a la sd
card.

Algo con lo que me había encontrado también, era que no tenía ni idea de
como convertir en Arduino un flotante a string, para así poderlo mandar
por Serial, con este frágmento de código pude hacerlo:

```
float current_lat = -99.123456;
static char dtostrfbuffer[8];

// convert float to array of charts
dtostrf(current_lat,8, 6, dtostrfbuffer);
```

Aquí está el código completo.

<script src="https://gist.github.com/3845285.js"> </script>

El enclosure al final se veía así.

![enclosure](https://s3-us-west-1.amazonaws.com/jcastaneyra-blog/images/logger_enclosure.jpg)

## Resultados

Algunos de los datos que arrojó (fecha, tiempo, temperatura, latitude y
longitud):

51012,22431200,28.50,19.389139,-99.187347
51012,22432300,28.50,19.389179,-99.187309
51012,22433300,28.50,19.389219,-99.187302
51012,22434300,28.50,19.389210,-99.187302
51012,22435300,28.50,19.389210,-99.187309
51012,22440300,28.50,19.389191,-99.187279
51012,22441400,28.50,19.389210,-99.187271
51012,22442400,28.50,19.389219,-99.187309

También vi, que para poder mostrar estos datos en un mapa no era
necesario crear un programa, hay una página
[http://www.gpsvisualizer.com/](http://www.gpsvisualizer.com/), en donde
subes tu archivo con coordenadas, aquí también me topé con un problema,
de que mi archivo lo estaba creando con datos de fecha y temperatura al
principio de la línea y las coordenadas al final, gpsvisualizer.com
requiere que el archivo tenga las coordenadas al principio de la línea
(latitud, longitud).

![map](https://s3-us-west-1.amazonaws.com/jcastaneyra-blog/images/gps_logger_map.png)

## Final

Al final no salieron todos las coordenadas hasta el final de mi viaje, y
eso fue porque la batería del seeeduino se acabó, pero aún así, ha sido
muy divertido ver esto funcionando, yo quería añadirle un xbee para que
en cuanto este se conectara con el xbee coordinador pudiera transmitirle
los datos o de menos su posición actual, pero no lo pude echar a andar y
creo saber porqué, en el código, interactúo con el pin 10 para poder
echar a andar la SD, y ahí precisamente es donde se conecta el xbee
shield, por lo que creo que eso afectaba a que funcionara, por lo que va
a ser necesario conectar el xbee a otros pines, pero eso será para
después.


