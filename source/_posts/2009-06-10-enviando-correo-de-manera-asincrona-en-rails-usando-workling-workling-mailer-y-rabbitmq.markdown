---
layout: post
title: "Enviando correo de manera asíncrona en Rails usando Workling, Workling-Mailer y RabbitMQ"
date: 2009/06/10
comments: true
categories: 
---

<a name="async_mailer_spanish"><strong>Enviando correo de manera asíncrona en Rails usando Workling, Workling-Mailer y RabbitMQ</strong></a> (<a href="#async_mailer_english">English</a>)

El trabajar con message queues es bastante interesante, ya que podemos mandar procesos al background y que estos sean procesados de manera asíncrona, un ejemplo podría ser el envío de correos, aunque también podría servir para realizar otras tareas, por ejemplo, como el envío de mensajes sms, generación de reportes, generación de pdf's, etc.

En esta ocasión les quiero presentar como enviar correos de manera asíncrona haciendo una aplicación sencilla haciendo uso de los puglins Workling y workling-mailer y del sistema RabbitMQ, en teoría, con esto se podría ajustar esta solución fácilmente a cualquier otro proceso que se quiera realizar de manera asíncrona.

<!-- more -->

Aplicación base
---------------

Se crea una aplicación básica con restful_authentication, y con la opción de activación.

Lo primero que hacemos es crear la aplicación en rails

```
rails mail_async_example -d mysql

rm public/index.html

script/plugin install git://github.com/technoweenie/restful-authentication.git

script/generate authenticated user sessions --include-activation
```

Creamos nuestra base de datos y migramos

```
rake db:create

rake db:migrate
```

Y hacemos unos pequeños cambios al código de la aplicación para enviar correo según las instrucciones de restful_authentication, así que agregamos lo suguiente:

``` ruby
#config/routes.rb
map.activate 'activate/:activation_code', :controller => 'users', :action => 'activate', :activation_code => nil

#config/environment.rb
config.active_record.observers = :user_observer
```

Con esto ya tendríamos nuestra aplicación básica la cuál podríamos probar de inmediato apuntando en el browser a http://localhost:3000/signup

Y agregando algunas cosillas extras, a app/controller/application_controller.rb agregamos la línea:

``` ruby
include AuthenticatedSystem
```

Esta misma línea que agregamos al Application controller la removemos de app/controller/sessions_controller.rb y de app/controllers/users_controller.rb

Y creamos un nuevo controlador

```
    script/generate controller portal index
```

Modificamos config/routes.rb para agregar al último como página root

``` ruby
    map.root :controller => "portal"
```

Por último agregamos un layout para nuestra aplicación:

``` erb
<!--app/views/layouts/application.html.erb--!>

<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"
       "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">

<html xmlns="http://www.w3.org/1999/xhtml" xml:lang="en" lang="en">
<head>
  <meta http-equiv="content-type" content="text/html;charset=UTF-8" />
  <title>Contracts: <%= controller.action_name %></title>
  <%= stylesheet_link_tag 'scaffold' %>
</head>
<body>
<% if current_user %>
   <%= link_to 'Logout', logout_path %>
<% else %>
   <%= link_to 'Login', login_path %>
   <%= link_to 'Sign up', signup_path %>
<% end %>

<% [:error, :warning, :notice].each do |level|
    if flash[level] -%>
      <div class="flash <%= level.to_s -%>"><%= flash[level] -%></div>
  <% end -%>
<% end -%>

<%= yield %>

</body>
</html>
```

Instalando amqp, workling y workling_mailer
-------------------------------------------

Ahora bien, antes de instalar Workling, para poder trabajar con RabbitMQ es necesario instalar la gema de amqp, esta gema requiere también de la gema de EventMachine, por lo que al instalar amqp también instalará EventMachine.

```
gem sources -a http://gems.github.com/
sudo gem install tmm1-amqp --no-rdoc --no-ri
```

Vemos nuestro listado de gemas

```
sudo gem list
```

Ahora si, a instalar workling y workling_mailer

```
script/plugin install git://github.com/purzelrakete/workling.git

script/plugin install git://github.com/langalex/workling_mailer.git
```

Para poder utilizar workling con RabbitMQ agregamos al final de config/environment.rb

``` ruby
config.after_initialize do
Workling::Remote.invoker = Workling::Remote::Invokers::EventmachineSubscriber
Workling::Remote.dispatcher = Workling::Remote::Runners::ClientRunner.new
Workling::Remote.dispatcher.client = Workling::Clients::AmqpClient.new
end
```

Posteriormente en app/models/user_mailer.rb añadimos

``` ruby
include AsynchMail
```

A la fecha de hoy el plugin de workling tiene un pequeño issue, para resolverlo hacemos lo siguiente en el archivo lib/workling/clients/memcache_queue_client.rb y verificar que contiene:

``` ruby
require 'memcache'
```

Al principio del archivo, si no lo tienes agrégalo.

Como el amqp corre sobre un ciclo de EventMachine es necesario correr nuestra aplicación con Thin, ya que este servidor soporta EventMachine sin ningún problema. Para instalar Thin corremos

```
sudo gem install thin --no-rdoc --no-ri
```

Instalando RabbitMQ
-------------------

Checar la instalación [aquí][instalando_rabbitmq]

A correr todos
--------------

Nos aseguramos de que el RabbitMQ esté corriendo.

Iniciamos el cliente de workling

```
script/workling_client start
```

E iniciamos el servidor thin

```
thin start
```

Apuntamos nuestro browser a http://localhost:3000 y nos registramos con un usuario, al verificar el log de desarrollo veremos que el correo ahora es enviado después de todas las últimas operaciones registradas en el log. Podemos probar quitando la línea de include AsynchMail del UserMailer y ver lo que hace, o bien haciendo nuevo proceso que sea pesado y que se realice en background.

Referencias:

1. [http://github.com/christospappas/workling/commit/063b9849b32e0c6140a5ecdb254357770d9bd28f][workling1]
2. [http://github.com/purzelrakete/workling/][workling2]
3. [http://github.com/langalex/workling_mailer/][workling_mailer]
4. [http://github.com/tmm1/amqp/][amqp]
5. [http://github.com/technoweenie/restful-authentication/][restful_auth]
6. [http://www.rabbitmq.com][rabbitmq]
7. [http://code.macournoyer.com/thin/][thin]
8. [Instalando RabbitMQ][instalando_rabbitmq]


<a name="async_mailer_english"><strong>Sending mails in asynchronous way in Rails using Workling, Workling-Mailer and RabbitMQ</strong></a> (<a href="#async_mailer_spanish">Español</a>)

Working with message queues is so interesting, since we can put processes in the background in order to be processed in asynchronous way, imagine this to send several mails, or maybe for heavy tasks as sms messages, report generation, pdf generation, etc.

So, with this post I want you to show how to send mails in asynchronous way, we are going to make a small application using Workling, Workling-mailer and as message queue RabbitMQ, actually we could adjust this solution to any other problem with async process requirement.

Base application
----------------

We create a simple application with restful_authentication and activation option.

First of all we create the rails app

```
rails mail_async_example -d mysql

rm public/index.html

script/plugin install git://github.com/technoweenie/restful-authentication.git

script/generate authenticated user sessions --include-activation
```

Now with rake we create and migrate DB

```
rake db:create

rake db:migrate
```

Following the restful_authentication instructions to send mail we add this:

``` ruby
#config/routes.rb
map.activate 'activate/:activation_code', :controller => 'users', :action => 'activate', :activation_code => nil

#config/environment.rb
config.active_record.observers = :user_observer
```

Our base app is working now, so to test it we get the url http://localhost:3000/signup in the browser.

Add this in app/controller/application_controller.rb:

``` ruby
include AuthenticatedSystem
```

The same line added to Application controller is removed from app/controller/sessions_controller.rb and app/controllers/users_controller.rb

We create a new controller for portal index

```
script/generate controller portal index
```

And modify config/routes.rb to add portal as root

``` ruby
map.root :controller => "portal"
```

And last we create a layout for our application:

``` erb
<!--app/views/layouts/application.html.erb--!>

<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"
       "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">

<html xmlns="http://www.w3.org/1999/xhtml" xml:lang="en" lang="en">
<head>
  <meta http-equiv="content-type" content="text/html;charset=UTF-8" />
  <title>Contracts: <%= controller.action_name %></title>
  <%= stylesheet_link_tag 'scaffold' %>
</head>
<body>
<% if current_user %>
   <%= link_to 'Logout', logout_path %>
<% else %>
   <%= link_to 'Login', login_path %>
   <%= link_to 'Sign up', signup_path %>
<% end %>

<% [:error, :warning, :notice].each do |level|
    if flash[level] -%>
      <div class="flash <%= level.to_s -%>"><%= flash[level] -%></div>
  <% end -%>
<% end -%>

<%= yield %>

</body>
</html>
```

Installing amqp, workling and workling_mailer
---------------------------------------------

Now, before using Workling, we need RabbitMQ and amqp is needed, this gem also requires EventMachine gem, so, both gems are installed when amqp is installed.

```
gem sources -a http://gems.github.com/
sudo gem install tmm1-amqp --no-rdoc --no-ri
```

List your installed gems

```
sudo gem list
```

Now, install workling and workling_mailer

```
script/plugin install git://github.com/purzelrakete/workling.git

script/plugin install git://github.com/langalex/workling_mailer.git
```

Add this to config/environment.rb to allow workling to use RabbitMQ

``` ruby
config.after_initialize do
Workling::Remote.invoker = Workling::Remote::Invokers::EventmachineSubscriber
Workling::Remote.dispatcher = Workling::Remote::Runners::ClientRunner.new
Workling::Remote.dispatcher.client = Workling::Clients::AmqpClient.new
end
```

To app/models/user_mailer.rb add this

``` ruby
include AsynchMail
```

Until today I've seen a little issue with workling, to solve it we do the folloging in this file lib/workling/clients/memcache_queue_client.rb; verify that you have:

``` ruby
require 'memcache'
```

Add it if you don't have it.

Amqp runs in an EventMachine cycle, so it is needed to run our app with Thin, since Thin supports EventMachine without any problem. To install Thin run

```
sudo gem install thin --no-rdoc --no-ri
```

Installing RabbitMQ
-------------------

Follow the instructions here [aquí][instalando_rabbitmq]

Everybody runs
--------------

Cool, naw we ensure that RabbitMQ is running.

Start workling client

```
script/workling_client start
```

And start Thin server

```
thin start
```

Point with your browser to http://localhost:3000 and sign up a new user, then verify your log and will see that email is sent and registered to the end of log after all operations. We can test removing include AsynchMail from UserMailer and see what happens, it would be great if you test some application with a heavy process working in the background.

Links:

1. [http://github.com/christospappas/workling/commit/063b9849b32e0c6140a5ecdb254357770d9bd28f][workling1]
2. [http://github.com/purzelrakete/workling/][workling2]
3. [http://github.com/langalex/workling_mailer/][workling_mailer]
4. [http://github.com/tmm1/amqp/][amqp]
5. [http://github.com/technoweenie/restful-authentication/][restful_auth]
6. [http://www.rabbitmq.com][rabbitmq]
7. [http://code.macournoyer.com/thin/][thin]
8. [Instalando RabbitMQ][instalando_rabbitmq]


[instalando_rabbitmq]: /2009/06/07/instalando-rabbitmq/
[workling1]: http://github.com/christospappas/workling/commit/063b9849b32e0c6140a5ecdb254357770d9bd28f
[workling2]: http://github.com/purzelrakete/workling/
[workling_mailer]: http://github.com/langalex/workling_mailer/
[amqp]: http://github.com/tmm1/amqp/
[restful_auth]: http://github.com/technoweenie/restful-authentication/
[rabbitmq]: http://www.rabbitmq.com
[thin]: http://code.macournoyer.com/thin/


