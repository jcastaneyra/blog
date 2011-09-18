---
layout: post
title: "Bundler gem and Daemons error (The default Gemfile was not found)"
date: 2010/02/12
comments: true
categories: 
---

<a name="bundler_gem_daemons_error_english"></a> (<a href="#bundler_gem_daemons_error_spanish">En español</a>)

Some days ago we deployed an application with rails, in this application we are using some daemons (with daemons gem) to take care about some business processes, and recently to have more control with all gems used by our application we started to use bundler.

Almost all was done, so, at the moment to start the daemons, we get the following error:

```
/usr/lib/ruby/gems/1.8/gems/bundler-0.9.3/lib/bundler.rb:122:in `default_gemfile': The default Gemfile was not found (Bundler::GemfileNotFound)
	from /usr/lib/ruby/gems/1.8/gems/bundler-0.9.3/lib/bundler.rb:64:in `setup'
```

So, looking for this error in google we didn't get anything, and looking into the bundler gem code the solution to this problems was:

``` ruby
ENV['BUNDLE_GEMFILE'] ||= File.join(Dir.pwd, 'Gemfile')
```

The complete daemon code here:

{% gist 302390 %}

<a name="bundler_gem_daemons_error_spanish"></a> (<a href="#bundler_gem_daemons_error_english">In english</a>)

Hace algunos días hicimos el deployment de una aplicación con rails, con esta aplicación corremos algunos demonios (con la gema daemons) que se hacen cargo de algunas operaciones de negocio, y recientemente para tener más control de las gemas que utilizamos empezamos a usar bundler.

Ya se tenía todo listo, al momento de correr los demonios nos encontramos con el siguiente error:

```
/usr/lib/ruby/gems/1.8/gems/bundler-0.9.3/lib/bundler.rb:122:in `default_gemfile': The default Gemfile was not found (Bundler::GemfileNotFound)
	from /usr/lib/ruby/gems/1.8/gems/bundler-0.9.3/lib/bundler.rb:64:in `setup'
```

Cabe mencionar que buscando en google no se encontró nada de como resolver el problema, así que indagando un poco en el código de bundler, llegamos a la siguiente solución, que fue agregar la siguiente línea al código que levanta nuestro demonio.

``` ruby
ENV['BUNDLE_GEMFILE'] ||= File.join(Dir.pwd, 'Gemfile')
```

El código completo del demonio es el siguiente:

{% gist 302390 %}
