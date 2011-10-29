---
layout: post
title: "Como crear un archivo zip con ruby"
date: 2009/11/16
comments: true
categories: 
---

Hace unos días me encontré con el problema de generar archivos Zip que contuvieran archivos de un curso de SCORM, ¿Cómo hacerlo?, lo primero que me vino a la mente fue Ruby!!!

<!-- more -->

La gema que me sirvió para esto fue rubyzip, y con el siguiente fragmento de código se realizaron los archivos zip:

{% gist 235735 %}

Espero que les sirva como a mi.

Links:

1.  [http://ruby-doc.org/core/classes/Dir.html](http://ruby-doc.org/core/classes/Dir.html)
2.  [http://ruby-doc.org/core/classes/FileUtils.html](http://ruby-doc.org/core/classes/FileUtils.html)
3.  [http://rubyzip.sourceforge.net/](http://rubyzip.sourceforge.net/)
