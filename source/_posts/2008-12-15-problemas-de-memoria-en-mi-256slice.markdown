---
layout: post
title: "Problemas de memoria en mi 256slice"
date: 2008/12/15
comments: true
categories: 
---

No se si ya había comentado, pero mi blog lo tengo en un VPS de slicehost y tengo una <a href="http://www.slicehost.com">256slice</a> es decir una partición virtual con 256 Mb de memoria, y recientemente debido al monto de memoria limitado (talvez debería pensar seriamente en subir el monto de memoria por unos cuántos dolares más al mes, ¿debería?) he experimentado algunos problemas ya que mi servidor se había estado muriendo, y en los logs estaba la evidencia:


    Dec 15 08:27:59 ubuntu kernel:  [<ffffffff8025d987>] out_of_memory+0x2e/0x187


Por lo que me puse a investigar y encontré unos ajustes que se le tienen que hacer a la configuración del servidor apache2:

    <IfModule mpm_prefork_module>
        StartServers          3
        MinSpareServers       3
        MaxSpareServers       3
        ServerLimit          50
        MaxClients           50
        MaxRequestsPerChild   1000
    </IfModule>

Además, también instalé el plugin de wordpress <a href="http://wordpress.org/extend/plugins/wp-super-cache/">WP Super cache</a> para bajarle un poco al proceso de php con la base de datos.

Espero que esto funcione, estoy intentando esto antes de subir a 512 Mb de memoria, y digo ojalá que esta experiencia pueda ayudar a alguien más que se encuentre en un problema parecido.
