---
layout: post
title: "Cambiar recursivamente permisos a archivos con extensión en Linux/Unix"
date: 2007/07/02
comments: true
categories: 
---

Hace unos días me encontré con el problema de que quería cambiar los permisos a todos los archivos con extensión .sh en un sistema HP-UX, pero me encontré con que el comando

<code>chmod -R 300 *.sh</code>

no funciona para estos casos, por lo que me puse a investigar un poquillo como poder hacer esto, y he aquí la solución que encontré.

<code>find . -type f -name '*.sh' -exec chmod 300 {} \;</code>

Un apunte más para recordar.
