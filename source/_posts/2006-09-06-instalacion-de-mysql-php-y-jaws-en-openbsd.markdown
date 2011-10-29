---
layout: post
title: "Instalación de MySQL, PHP y Jaws en OpenBSD"
date: 2006/09/06
comments: true
categories: 
---

Muchos de los sitios en internet que hacen uso de páginas con contenido dinámico utilizan MySQL como base de datos y PHP como lenguaje de programación para habilitar dichas páginas dinámicas. Pero existen algunos sistemas de administración de contenidos (Content Management System), uno en partícular que ha sido creado por mexicanos (<a href="http://ion.gluch.org.mx/">Jonathan Hernández</a>), y es Jaws (<a href="http://www.jaws.com.mx" title="Jaws">http://www.jaws.com.mx</a>), éste hace uso de MySQL y de PHP. A continuación muestro un pequeño howto de como instalar MySQL, PHP y Jaws sobre OpenBSD.

<!-- more -->

Bien, la versión de OpenBSD sobre la cuál vamos a instalar estos paquetes es la 3.9, la guía de instalación de OpenBSD se encuentra aquí:

<a href="http://www.openbsd.org/faq/faq4.html" title="OpenBSD faq">http://www.openbsd.org/faq/faq4.html</a>

<b>Instalando MySQL y PHP.</b>

Para tener MySQL y PHP en nuestro sistema, necesitamos instalar algunos paquetes, los instalamos de la siguiente manera:
<pre># pkg_add ftp://ftp.openbsd.org/pub/OpenBSD/3.9/packages/i386/mysql-server-5.0.22.tgz</pre>
Instalamos PHP5:
<pre># pkg_add ftp://ftp.openbsd.org/pub/OpenBSD/3.9/packages/i386/php5-core-5.0.5.tgz</pre>
Habilitamos el módulo de PHP5:
<pre># /usr/local/sbin/phpxs -s

# cp /usr/local/share/examples/php5/php.ini-recommended /var/www/conf/php.ini</pre>
Instalamos y habilitamos el modulo de PHP para MySQL:
<pre># pkg_add ftp://ftp.openbsd.org/pub/OpenBSD/3.9/packages/i386/php5-mysql-5.0.5p0.tgz

# /usr/local/sbin/phpxs -a mysql</pre>
Instalamos y habilitamos el módulo de GD para PHP:
<pre># pkg_add ftp://ftp.openbsd.org/pub/OpenBSD/3.9/packages/i386/php5-gd-5.0.5p2-no_x11.tgz

# /usr/local/sbin/phpxs -a gd</pre>
<b>Configurando MySQL.</b>

Ahora necesitamos hacer algunas cuántas configuraciones de MySQL, para empezar arrancaremos el servidor de MySQL:
<pre># /usr/local/bin/mysqld_safe &amp;</pre>
Después de arrancar MySQL necesitamos darle un password al root de MySQL, ya que por defecto no tiene ningún password configurado, se lo asignamos de la siguiente manera:
<pre># /usr/local/bin/mysqladmin -u root password mipassword</pre>
Intentamos accesar a MySQL con root y su nuevo password:
<pre># mysql -u root -p</pre>
Y tenemos la siguiente salida:
<pre>Enter password:
Welcome to the MySQL monitor.  Commands end with ; or g.
Your MySQL connection id is 2 to server version: 5.0.22-log

Type 'help;' or 'h' for help. Type 'c' to clear the buffer.

mysql&gt;</pre>
Ya que estamos aquí crearemos la base de datos que usará jaws:
<pre>mysql&gt; create database jaws;</pre>
Podemos ver que la base de datos ha sido creada con:
<pre>mysql&gt; show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| test               |
+--------------------+
3 rows in set (0.04 sec)</pre>
Ya tenemos nuestra base de datos, ahora necesitamos crear un usuario para accesar a esta base de datos, y será con todos los prilegios sobre la misma:
<pre>mysql&gt; grant all privileges
    -&gt; on jaws.*
    -&gt; to 'jaws'@'localhost'
    -&gt; identified by 'mipassword';</pre>
Nos salimos de MySQL con:
<pre>mysql&gt; exit</pre>
Como Apache corre como chrooted en /var/www necesitamos hacer unos hard links para que pueda trabajar con los sockets de MySQL:
<pre># mkdir -p /var/www/var/run/mysql
# ln -f /var/run/mysql/mysql.sock /var/www/var/run/mysql/mysql.sock</pre>
<b>Configurando Apache y PHP.</b>

Necesitamos asegurarnos de que la siguiente línea se encuentre descomentada en /var/www/conf/httpd.conf:
<pre>AddType application/x-httpd-php .php</pre>
También que la línea de DirectoryIndex tenga lo siguiente:
<pre>DirectoryIndex index.html index.php</pre>
Hay que crear el directorio /var/www/tmp y darle permisos de escritura para todos los usuarios, este directorio puede ser usado por algunas aplicaciones que están en PHP (jaws por ejemplo):
<pre># mkdir /var/www/tmp

# chmod 777 /var/www/tmp</pre>
Ahora vamos a reiniciar nuestro servidor de apache:
<pre># apachectl stop
# apachectl start</pre>
Podemos probar que nuestro servidor de apache ya esté funcionando, creamos un archivo test.php en el directorio de documentos de http /var/www/htdocs, y metemos en este archivo lo siguiente:
<pre>&lt;?php
phpinfo();
?&gt;</pre>
La salida deberá ser algo así:

<a href="http://img89.imageshack.us/img89/3706/phptestsa3.jpg"><img src="http://img89.imageshack.us/img89/3088/phptestye8.jpg" alt="php test" height="242" width="320" /></a>

<b>Instalando y configurando Jaws.</b>

Para empezar debemos bajar la última versión de Jaws, para poder bajarlo utilizaremos wget, el cuál necesitamos instalarlo en nuestro sistema (a menos que ya se tenga instalado):
<pre># pkg_add ftp://ftp.openbsd.org/pub/OpenBSD/3.9/packages/i386/wget-1.10.2p0.tgz</pre>
Posteriormente bajamos la última versión de Jaws:
<pre>$ wget http://developer.novell.com/wiki/index.php/Special:File/jaws/jaws-0.6.2/jaws-complete-0.6.2.tar.gz</pre>
Descomprimimos el archivo:
<pre>$ tar xvzf jaws-complete-0.6.2.tar.gz</pre>
Podría describir todos los pasos aquí, pero es mejor seguir la guía de instalación que se encuentra aquí:
<a href="http://wiki.jaws-project.com/doku.php?id=jaws:v06:installation" title="Jaws installation"> http://wiki.jaws-project.com/doku.php?id=jaws:v06:installation</a>
Nada más aquí un detalle, hay que darle permisos de escritura y lectura a los directorios /var/www/htdocs/data y /var/www/htdocs/config:
<pre># chmod 777 /var/www/htdocs/data /var/www/htdocs/config</pre>
Una vez seguido el proceso de instalación de Jaws, ya tenemos un servidor OpenBSD corriendo MySQL, PHP y lo mejor de todo Jaws!!!
<a href="http://img168.imageshack.us/img168/9676/jawsrunningfh8.jpg"><img src="http://img89.imageshack.us/img89/6279/jawsrunningbl5.jpg" alt="Jaws running" height="242" width="320" /></a>
Referencias.
<a href="http://freeyourbox.org/tutorials/bsd/obsd3.8_apache_php_mysql.html"> http://www.nomoa.com/bsd/mysql.htm</a>

<a href="http://freeyourbox.org/tutorials/bsd/obsd3.8_apache_php_mysql.html">http://freeyourbox.org/tutorials/bsd/obsd3.8_apache_php_mysql.html</a>

<a href="http://dev.mysql.com/doc/refman/5.0/en/adding-users.html"> http://dev.mysql.com/doc/refman/5.0/en/adding-users.html</a>

<a href="http://wiki.jaws-project.com/doku.php?id=jaws:v06:installation"> http://wiki.jaws-project.com/doku.php?id=jaws:v06:installation</a>
