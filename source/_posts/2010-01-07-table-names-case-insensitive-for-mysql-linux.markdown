---
layout: post
title: "Table names case insensitive for MySQL Linux"
date: 2010/01/07
comments: true
categories: 
---

Hola a todos, feliz año 2010, que tal se pasaron estas fechas? descansaron bastante? Pues yo más o menos, unos ratos descansaba y otros trabajaba, y para variar me encontré con esto en el trabajo.

Resulta que estaba montando un servidor de Weblogic y MySQL en Linux, para instalar una aplicación que originalmente estaba con Weblogic y Oracle, y resulta que al cargar las tablas de esta aplicación en MySQL empecé a encontrarme problemas con los nombres de las tablas (entre otras cosas, como los tipos de datos entre Oracle y MySQL), yo no sabía que MySQL por default en Linux es case sensitive, por lo que me di a la tarea de investigar y buscar como deshabilitar esta función.

Es necesario editar el archivo my.cnf que se puede encontrar en /etc o bien en /etc/mysql, e inmediatamente después de la sección \[mysqld\], agregar lo siguiente:

```
lower_case_table_names=1
```

Guardamos el archivo y reiniciamos MySQL, con esto al momento de crear nuevas tablas estas se harán en minúsculas, y ya no tendremos problemas de si son mayúsculas o minúsculas.
