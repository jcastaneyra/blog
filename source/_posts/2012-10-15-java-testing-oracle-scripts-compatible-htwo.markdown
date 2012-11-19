---
layout: post
title: "Java Testing: Scripts hechos para oracle compatibles con H2"
date: 2012-10-15 17:30
comments: true
categories: java testing
---

{% img right https://s3-us-west-1.amazonaws.com/jcastaneyra-blog/images/jenkins_ci.png 320 224 %}

En nuestro desarrollo trabajamos con Oracle, y un tiempo estuvimos
probando directamente con Oracle desde maven con JUnit y Mockito, todo
esto corriendo en un Jenkins, pero
las pruebas tomaban mucho tiempo, varios minutos de hecho, pero en ese
momento tuvimos que hacerlo para validar también los cambios que se
hacían a base de datos, aunque a estas fechas no se bien si esto fue una
buena decisión, posiblemente se pudieron también haber hecho pruebas
sobre estos scripts en una base de datos de oracle aparte, e
independiente de los tests del código.

Después de un rato estas pruebas fueron ligeramente abandonadas, por lo
que tiene meses que no han vuelto a estar en verde, entonces ahora es
necesario retomarlas, pero el hecho de que tome bastante tiempo para
realizarlas y el de tener una BD de Oracle real corriendo en donde se
ejecutan las pruebas, en este caso un servidor de integración, lo hace
un proceso algo engorroso, entonces ahora estamos buscando volver a
trabajar con una DB en memoria para bajarle el tiempo a las pruebas, antes
usábamos hsqldb, y ahora queremos probar H2.

<!--more-->

Para que los scripts hechos para Oracle puedan ser compatibles con H2,
tuvimos que cambiar lo siguiente:

1. VARCHAR2 => VARCHAR
2. NUMBER => NUMERIC
3. Un alter table drop con múltiples campos no funciona, por lo que de una
   sóla línea de drop borrando varios campos se tuvieron que hacer
   varias donde sólo borraran un campo.
4. CHAR(1) para datos booleanos, para h2 tuvimos que cambiarlo a BOOLEAN.
5. También, si se hacen foreign keys, los campos con los que se hacen
   deben ser no nulos, esta validación la hace H2, curiosamente en
   Oracle pasa bien.
6. El data source quedó de la siguiente manera, pero el truco acá es que
   como nosotros hacemos uso de un schema por default, el schema se crea
en la url y ahí se define como schema por default.

```
<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource"
      p:driverClassName="org.h2.Driver"
      p:url="jdbc:h2:mem:test;DB_CLOSE_DELAY=-1;MODE=Oracle;INIT=CREATE
SCHEMA IF NOT EXISTS MYSCHEMA\>
      p:username="sa"
      p:password="" />
```

La pregunta es ¿Porqué no hicimos compatibles los scripts desde un
principio? Y creo que si lo plantié desde el principio, pero por alguna
razón no le dimos el control adecuado a esto, aunque me parece que
todavía podemos enderezar un poco el camino. La recomendación aquí es,
que mientras más estándar sean los scripts, mejor la adaptación con
cualquier otro motor de BD, incluso para casos como este, de testing.

Por otra parte, todo el set de queries que se tienen que correr, tales
como creación de catálogos, se podrían correr en la url de la configuración de H2,
dentro del parámetro INIT, pero si lo hacíamos así, los queries
empezaban a fallar porque las tablas ya estaban creadas o bien porque se
violaban algunos contraints (los registros ya estaban insertados con una
primer ejecución con alguna conexión previa dentro de las mismas pruebas). Para resolver esta parte creamos un bean de
inicialización.

El bean de inicialización se ve así:

<script src="https://gist.github.com/3862707.js"> </script>

Y del lado de la configuración de data source y de beans:

```
<bean id="dataSourcePopulator" class="util.DataPopulator">
        <property name="dataSource" ref="dataSource" />
    </bean>
```

Con esto, las pruebas han vuelto a correr rápidamente, ahora están más
cerca del status verde.


