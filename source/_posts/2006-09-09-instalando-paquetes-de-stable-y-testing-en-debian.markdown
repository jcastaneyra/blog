---
layout: post
title: "Instalando paquetes de stable y testing en Debian"
date: 2006/09/09
comments: true
categories: 
---

Hace poco instalé la última versión stable de Debian, pero resulta que tenía interés de probar algunos paquetes más recientes de MySQL y de PHP, los cuáles se encontraban en la versión testing de los paquetes, así es que me puse a buscar en la red para saber como poder instalar desde la versión testing sin tener que mover todos mis paquetes a esta versión, o sin tener que estar modificando la lista de fuentes en sources.list a cada rato, y he aquí lo que se tiene que hacer.

Primero debemos tener la lista de fuentes de las versiones stable y testing en /etc/apt/sources.list. En mi caso apunto a los fuentes que están en un mirror de la UNAM:
<pre lang="bash"># sources for stable
deb http://nisamox.fciencias.unam.mx/debian/ stable main
deb-src http://nisamox.fciencias.unam.mx/debian/ stable main

deb http://security.debian.org/ stable/updates main

# sources for testing
deb http://nisamox.fciencias.unam.mx/debian/ testing main
deb-src http://nisamox.fciencias.unam.mx/debian/ testing main
#deb http://security.debian.org/ stable/updates main</pre>
Ahora tenemos que agregar a /etc/apt/apt.conf (crear el archivo si no existe) la siguiente línea:
<pre lang="bash">APT::Default-Release "stable";</pre>
si no agregamos esta línea a apt.conf, al momento de hacer apt-get upgrade el administrador de paquetes querrá actualizar los paquetes a testing.

Ya con esto podemos instalar MySQL y PHP de los fuentes de testing:
<pre># apt-get update
# apt-get -t testing install mysql-server-5.0
# apt-get -t testing install php5-mysql</pre>
