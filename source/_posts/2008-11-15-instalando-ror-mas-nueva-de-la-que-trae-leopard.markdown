---
layout: post
title: "Instalando RoR 2.2 RC2 en Leopard"
date: 2008/11/15
comments: true
categories: 
---

Al día de hoy no tengo una computadora a mi disposición, y como no tengo una, de vez en cuando hago cosas en la compu del trabajo, que no debería, y también a veces me prestan la macbook air, y en la macbook puedo trabajar un poco más, que me gustaría trabajar mucho más en la macbook, pues bien, hace rato ya había trabajado con Ruby on Rails con la versión que trae el leopard, pero hace unos días me dieron ganas de probar la RC de RoR la 2.2 (ya la RC2), para instalarlo tuve que investigar un poquito, ya que no es un proceso de instalación sencillo, tiene algunos trucos, para esto me basé en el post de <a href="http://danbenjamin.com/articles/2008/02/ruby-rails-leopard">Dan Benjamin</a>, pero para variar me hago un resumen a mi mismo, para cuando se me vuelva a ofrecer, así que para más detalle visitar el post, ahí si mencionan que sólo por las dudas sería bueno respaldar la información, yo no lo hice, y todo salió bien.

Pues bien, para tener instalada una de las últimas versiones de RoR y diferente a las que está instalada por defecto en leopard es necesario tener instalado el XCode 3.0 o superior, este puede ser bajado de developer.apple.com o bien instalándolo del disco que viene con la mac, la verdad no se que disco es, pero con la mac debe de venir un disco (o más no recuerdo).

<strong>Primeros pasos</strong>

Primero que nada hay que configurar el path, para esto habrá que abrir una terminal (en este caso con el bash shell), y verificar que esté en mi directorio home, o bien esto puedo realizarse usando el comando:
<pre lang="bash">cd</pre>
Habrá que editar el archivo .bash_login, yo lo hice con el vi, es posible que este archivo no exista, si no existe lo vamos a crear, y si existe pos editaremos la parte del path, entonces para editarlo o crearlo es con el siguiente comando:
<pre lang="bash">vi .bash_login</pre>
Al final del archivo se deberá introducir la siguiente línea:
<pre lang="bash">export PATH="/usr/local/bin:/usr/local/sbin:/usr/local/mysql/bin:$PATH"</pre>
Lo siguiente es ejecutar el cambio:
<pre lang="bash">. .bash_login</pre>
Al path se agregó la ruta donde se encuentra MySQL, para la instalación de MySQL en leopard recuerdo que sólo bajé el paquete directamente de www.mysql.com y seguí las instrucciones que vienen con dicho paquete.

Se tiene que crear ahora un directorio donde vamos a bajar Ruby y posteriormente compilarlo, entonces habrá que ejecutar los siguientes comandos:
<pre lang="bash">sudo mkdir -p /usr/local/src

sudo chgrp admin /usr/local/src

sudo chmod -R 775 /usr/local/src

cd /usr/local/src</pre>
<strong>Ruby</strong>

Para bajarlo y compilarlo ejecutamos los siguiente:
<pre lang="bash">curl -O ftp://ftp.ruby-lang.org/pub/ruby/1.8/ruby-1.8.6-p287.tar.gz

tar xvzf ruby-1.8.6-p287.tar.gz

cd ruby-1.8.6-p287

./configure --enable-shared --enable-pthread CFLAGS=-D_XOPEN_SOURCE=1

make

sudo make install

cd ..</pre>
Para verificar que ruby se instaló en el path correcto, escribir:
<pre lang="bash">which ruby</pre>
Se debería ver:
<pre lang="bash">/usr/local/bin/ruby</pre>
<strong>RubyGems</strong>

El siguiente paso es instalar RubyGems (que por cierto Ruby on Rail 2.2 RC2 requiere de RubyGems 1.3.1):
<pre lang="bash">curl -O http://files.rubyforge.mmmultiworks.com/rubygems/rubygems-1.3.1.tgz

tar xvzf rubygems-1.3.1.tgz

cd rubygems-1.3.1

sudo /usr/local/bin/ruby setup.rb

cd ..</pre>
<strong>Ruby on Rails</strong>

Para instalar la versión estable de RoR se ejecuta:
<pre lang="bash">sudo gem install rails</pre>
También instalamos mongrel y capistrano, por si las moscas:
<pre lang="bash">sudo gem install mongrel

sudo gem install capistrano</pre>
Ahora si, para instalar la versión 2.2 RC2 de RoR ejecutar:
<pre lang="bash">sudo gem install rails -s http://gems.rubyonrails.org</pre>
Y por último instalamos la gem para mysql
<pre lang="bash">sudo gem install mysql -- --with-mysql-dir=/usr/local/mysql</pre>
Recursos:

<a href="http://danbenjamin.com/articles/2008/02/ruby-rails-leopard">http://danbenjamin.com/articles/2008/02/ruby-rails-leopard</a>

<a href="http://weblog.rubyonrails.com/">http://weblog.rubyonrails.com/</a>

<a href="http://dev.mysql.com/get/Downloads/MySQL-5.0/mysql-5.0.67-osx10.5-x86_64.dmg/from/pick#mirrors">http://dev.mysql.com/get/Downloads/MySQL-5.0/mysql-5.0.67-osx10.5-x86_64.dmg/from/pick#mirrors</a>
