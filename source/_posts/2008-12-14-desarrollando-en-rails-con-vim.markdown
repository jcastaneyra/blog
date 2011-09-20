---
layout: post
title: "Desarrollando en Rails con Vim"
date: 2008/12/14
comments: true
categories: 
---

Cuando empecé a meterme con Rails me encontré con que había unos cuantos IDE's para desarrollar en este framework y algunos editores, digo como desarrollador de Java era obvio que esperaba un IDE y los disponibles eran o son <a href="http://www.netbeans.org">Netbeans</a>, <a href="http://www.aptana.com">Aptana</a>, <a href="http://www.activestate.com/Products/komodo_ide/index.mhtml">Komodo</a> y editores como <a href="http://www.jedit.org">JEdit</a>, <a href="http://macromates.com">TextMate</a>, <a href="http://www.vim.org">Vim</a>, <a href="http://www.gnu.org/software/emacs/">Emacs</a>, de los cuales el que de plano vi que era muy usado era el TextMate, en cada screencast que me encontraba veía que lo usaban, sólo había un problema, y digo un problema para mi, era que no era gratuito.

Al ver esto me preguntaba porque era más usado un editor de texto turbocargado con comandos en la consola y no un IDE, y la respuesta de los expertos era que con la consola de comandos y un editor se tiene más control sobre el proyecto además de la experiencia que esto te deja en lugar de dejar a que el IDE automatice todo.

Pues bien, debido a una cuestión de $$ me puse a usar un rato Aptana y JEdit, con los cuales tuve buenas experiencias, pero recientemente vi el <a href="http://weblog.jamisbuck.org/2008/10/10/coming-home-to-vim">post</a> de un experto en Rails, creador de <a href="http://www.capify.org/">Capistrano</a>, Jamis Buck y que venía de trabajar con TextMate durante algunos años pero que previamente había trabajado con Vim, y que ahora estaba moviéndose de nueva cuenta a Vim. Jamis Buck como experto en Rails y TextMate, ahora que se estaba moviendo creó un plugin para Vim para tener ciertas funciones que TextMate tiene. Así es que cuando vi este post dije "de aquí soy".

Vim siempre ha sido un editor muy poderoso, el cual he usado por años pero no como un usuario experto, porque la verdad tiene un buen de comandos, pero ahora estoy tratando de subirme en él para los desarrollos en Rails que estaré haciendo.

Con el post de Jamis Buck y el plugin que hizo (<a href="http://github.com/jamis/fuzzyfinder_textmate/tree/master">FuzzyFinder_TextMate</a> que extendió de <a href="http://www.vim.org/scripts/script.php?script_id=1984">FuzzyFinder</a>) y a todas las respuestas que recibió en su blog, es como he levantado mi ambiente con Vim, en este caso lo he hecho en la Mac, pero en Linux deber ser casi igual.

{% img left http://c243421.r21.cf1.rackcdn.com/imagen-1-300x277.png %}

En Leopard primero que nada me instalé MacVim bajándolo de <a href="http://code.google.com/p/macvim/">http://code.google.com/p/macvim/</a> y después con ayuda de los posts de Jamis Buck y de los comments ahí puestos logré levantar mi ambiente (<a href="http://weblog.jamisbuck.org/2008/10/10/coming-home-to-vim">Coming home to Vim</a> y <a href="http://weblog.jamisbuck.org/2008/11/17/vim-follow-up">Vim Follow-up</a>), también en los comments de estos posts encontré a una persona que puso en github su configuración de vim al igual que sus plugins de vim, que por cierto fue de bastante ayuda (<a href="http://github.com/manalang/vim-config/tree/master">http://github.com/manalang/vim-config/tree/master</a>). Se que en internet hay muchos recursos sobre comandos de Vim, pero aquí está uno que me encontré :) <a href="http://rayninfo.co.uk/vimtips.html">http://rayninfo.co.uk/vimtips.html</a>

En resumen, Vim es un editor muy potente, tan potente que se pueden hacer cosas que talvez en un IDE no se puedan hacer, para muestra este video.

<iframe width="420" height="315" src="http://www.youtube.com/embed/pCiVCiku3cM" frameborder="0" allowfullscreen></iframe>
