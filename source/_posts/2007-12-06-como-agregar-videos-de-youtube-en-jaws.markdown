---
layout: post
title: "Cómo agregar videos de youtube en Jaws"
date: 2007/12/06
comments: true
categories: 
---

Hace unos días me volví a meter, después de varios meses, a un <a href="http://oax.homeunix.org">servidor</a> que tengo con unos amigos, el cuál tiene <a href="http://www.jaws-project.com">Jaws</a> instalado, y pues le hice algo de mantenimiento, migré la versión que tenía 0.6.3 a 0.7.3, instalé algunos parches, le di algo de mantenimiento a la base de datos y saqué algunos respaldos.

<!-- more -->

Ya después, me puse a jugar un rato con mi nuevo Jaws y quise agregar un video de youtube, y me puse a buscar en la red, y busqué, y busqué ... y no encontré nada (me pregunto que habrá pasado con la página de www.jaws-user.com), entonces me puse a probar y probar en la administración de jaws y haciendo posts de videos hasta que di con el problema y pude postear un video. Y ahora aquí quiero compartir como fue que se solucionó, es una solución sencilla, pero que me llevó un buen rato encontrarla.

El problema estaba en que en los ajustes avanzados del Panel de control tenía como editor el editor de tipo word/OpenWriter, pero este editor al querer agregar el código embebido de el video de youtube como que trunca el código.

<a href="http://img364.imageshack.us/img364/4836/jawswordeditorxx4.jpg"><img src="http://img125.imageshack.us/img125/4620/jawswordeditorvt6.jpg" /></a>

La solución estuvo en cambiar este editor de tipo word/OpenWriter al editor clásico de Jaws, y con esto el código embebido de youtube no será truncado.

<a href="http://img468.imageshack.us/img468/3858/jawseditorxe3.jpg"><img src="http://img444.imageshack.us/img444/3015/jawseditorxg8.jpg" /></a>

Ahora bien, cuando me refiero al código embebido del video de youtube, es al código que viene en la parte inferior a la etiqueta que dice Embed.

<a href="http://img511.imageshack.us/img511/3758/youtubeembedle3.jpg"><img src="http://img134.imageshack.us/img134/1317/youtubeembedzu5.jpg" /></a>

Y listo, con esto tendríamos un video de youtube en jaws ;)
