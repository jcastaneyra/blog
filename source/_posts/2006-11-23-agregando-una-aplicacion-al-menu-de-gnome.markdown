---
layout: post
title: "Agregando una aplicación al menú de Gnome"
date: 2006/11/23
comments: true
categories: 
---

En esta ocasión voy a agregar una aplicación al menú de Gnome en Ubuntu, ya que hace unos días bajé el Azureus y para no tener que estar corriendolo desde línea de comandos a cada rato, me puse a investigar como meter la aplicación en el menú y así facilitarme la tarea de correrlo.

Seguramente esto debe ser algo muy sencillo de hacer para algunos, pero en mi caso servirá de recordatorio para cuando vuelva a querer agregar una aplicación al menú.

Bueno, en mi caso ya había bajado el azureus, pero para los que no lo hayan bajado puede ser bajado de <a href="http://azureus.sourceforge.net/download.php">http://azureus.sourceforge.net/download.php</a>.

Este se puede descomprimir y colocar en /opt, es recomendable que tenga los permisos del usuario que normalmente lo corre en ubuntu, por eso de los updates.

Una vez descomprimidos crear el siguiente archivo /usr/share/applications/azureus.desktop con el contenido siguiente:

<pre>
[Desktop Entry]
Name=Azureus
Comment=P2P Client
Exec=/opt/azureus/azureus
Icon=/opt/azureus/Azureus.png
Terminal=false
Type=Application
Categories=Application;Network;
</pre>

Y ser vería más o menos así:

<img src="http://c243421.r21.cf1.rackcdn.com/menu_azureus.jpg" alt="Azureus en el menú" />

Y si se quieren hacer más cosas para poner a punto Ubuntu, he aquí una muy buena guía:

<a href="http://cmaverick.wordpress.com/2006/10/29/edgy-optimizado/">http://cmaverick.wordpress.com/2006/10/29/edgy-optimizado/</a>
