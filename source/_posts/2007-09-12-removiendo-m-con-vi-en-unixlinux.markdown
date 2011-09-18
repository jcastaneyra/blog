---
layout: post
title: "Removiendo ^M con VI en Unix/Linux"
date: 2007/09/12
comments: true
categories: 
---

Después de un rato de estar ausente (por múltiples ocupaciones), sólo me tomo un pequeño momento para postear algo que puede ayudarnos a salir del hoyo cuando tengamos problemas con scripts y con los carácteres ^M al final de cada línea. El comando de VI para solucionar esto sería:

<code>:%s/^M$//g</code>

Hay que asegurarse que para obtener ^M se utilicen la conbinación de teclas CONTROL + V y CONTROL + M. Con este comando se reemplazan todos los ^M por nada.

Referencia:

http://www.vim.org/tips/tip.php?tip_id=26
