---
layout: post
title: "Como obtener una imagen ISO de un CD/DVD en Mac"
date: 2009/01/30
comments: true
categories: 
---

Hace poco un compañerito de la comunidad de [México on Rails][railsmx] mencionó que tener una Mac es como tener lo mejor de ambos mundos (de Windows y Linux), y he aquí el porqué, resulta que quería obtener una imagen ISO de cierto DVD para respaldarlo y posiblemente después quemarlo, y aquí les muestro al más puro estilo Unix como se puede hacer.

<!-- more -->

Primero que nada hay que insertar el CD/DVD a extraer (duh! pero alguien podría no saber :) )

Y ejecutando el siguiente comando obtenemos el status del CD/DVD a quemar:

    jcastaneyra$ drutil status
    Vendor   Product           Rev
    OPTIARC  DVD RW AD-5630A   1CHQ

    Type: DVD-ROM              Name: /dev/disk1
    Sessions: 1                  Tracks: 1
    Overwritable:   00:00:00         blocks:        0 /   0.00MB /   0.00MiB
    Space Free:   00:00:00         blocks:        0 /   0.00MB /   0.00MiB
    Space Used:  330:41:69         blocks:  1488144 /   3.05GB /   2.84GiB
    Writability:
    Book Type: DVD-ROM (v1)

Posteriormente desmontamos el CD/DVD:

    jcastaneyra$ diskutil unmountDisk /dev/disk1
    Unmount of all volumes on disk1 was successful

Y ahora obtenemos la imagen con la utilidad dd:

    jcastaneyra$ dd if=/dev/disk1 of=vista.iso bs=2048
    1488144+0 records in
    1488144+0 records out
    3047718912 bytes transferred in 1209.311307 secs (2520210 bytes/sec)

Después para expulsar el CD/DVD lo que se hace es volverlo a montar (jejeje, talvez puede haber otra forma, posiblemente haya un comando eject, ya no lo probé):

    jcastaneyra$ diskutil mountDisk /dev/disk1
    Volume(s) mounted successfully

Y por último probamos la imagen ISO:

    jcastaneyra$ hdid vista.iso
    /dev/disk2                                                 /Volumes/VISTA_32_BUSINESS

Y para poder quemar la imagen ISO se podría utilizar la aplicación "Utilidad de Discos"

[railsmx]: http://www.mexicoonrails.com.mx
