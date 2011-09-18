---
layout: post
title: "Ya quedó instalado Ubuntu 8.10 (actualizado sobre 8.04)"
date: 2008/11/09
comments: true
categories: 
---

La semana pasada, instalé Ubuntu 8.10, bueno más bien actualicé sobre mi versión de 8.04, sólo tuve un pequeño problema, que cuando arrancaba no me reconocía la tarjeta de video, y después de un rato me di cuenta, que al hacer la actualización hay ciertos archivos que no sobreescribe, incluso te saca mensajes de que los archivos los va a dejar intactos, uno de esos archivos fue el /boot/grub/menu.lst, por lo que la actualización a 8.10 traía un kernel nuevo, y este kernel nuevo no se encontraba en la lista de kerneles a arrancar.

Así pues, la solución fue agregar ese nuevo kernel al menu.lst, y con esto quedó solucionado el problema de que no me reconocía la tarjeta de video.
