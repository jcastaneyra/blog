---
layout: post
title: "Como tener un router linksys con un modem infinitum alámbrico"
date: 2007/03/07
comments: true
categories: 
---

Bueno, realmente no se si esto sea un howto o un tutorial, pero al menos para mi si será un registro más que me ayudará a recordar cómo está esto del router linksys con el modem infinitum alámbrico, he aquí a lo que me refiero.

Sucede que donde estoy viviendo no tengo la posibilidad de <img src="http://jcastaneyra.files.wordpress.com/2007/03/wrt54g.jpg" alt="wrt54g.jpg" align="right" />contratar una conexión broadband de internet (por cuestiones de arrendamiento), por lo que me di a la tarea de contactar con los vecinos y ver si alguien me podría apoyar, y obviamente yo apoyar con los gastos de la conexión, para mi buena suerte, las vecinas del departamento de al lado, que son sobrinas de una amiga de la familia de mi flaca, tienen internet con infinitum, pero sólo tienen el modem alámbrico, así es que tuve que comprar un router inalámbrico, el equipo que compré fue un linksys WRT54G.

Al primer intento de instalación del router, éste no jaló, después de analizar bien la situación e investigando un poco, me di cuenta que el modem alámbrico de infinitum proporciona ip's dinámicas en el rango de ip's privadas 192.168.1.0 y que el router inalámbrico linksys por defecto también proporciona en ese mismo rango por lo que la conexión a internet de buenas a primeras no es posible.

Después de haber dado una leída en algunos foros de discusión en internet, muchas soluciones eran dadas, pero la más fácil de todas (para mi) es cambiar la dirección IP local del router en la configuración básica a 192.168.2.1, guardar los cambios, apagar y volver a prender el router wireless, de esta manera el modem alámbrico infinitum maneja el rango de ip's 192.168.1.0 y el router 192.168.2.0, evitando el problema de choque con estos dos grupos de ip's entre los dos equipos.
<p style="text-align:center;"><a href="http://img104.imageshack.us/img104/2839/linksysgy7.jpg"><img src="http://img104.imageshack.us/img104/3127/linksysmb4.jpg" alt="" /></a></p>

Y así, con ese cambio que no lleva más de 5 minutos se tiene funcionando un router linksys inalámbrico detrás del modem alámbrico de infinitum, es más creo que me llevó más tiempo conectar los equipos.

<strong>UPDATE (2008/06/17)</strong>

Se me había pasado poner este update, pero nuestro buen Kique, nos mandó lo siguiente, es una solución bastante sencilla a este problema:
<blockquote>
<div class="comment-content">

Hola a todos, encontre una solución aún MAS FACIL:

Primero te aseguras de que tu modem 2WIRE tenga internet (osea que tenga todas las luces del mismo color y ninguna sin parpadear, esto es para descartar problemas con tu conexión.

Luego conectas un cable de red sencillo desde el puerto Ethernet 1 del 2WIRE al puerto Ethernet 1 de tu router Linksys (osea, hacer una conexión de ethernet a ethernet entre los dos aparatos).

Listo, ya tienes internet y solo debes configurar el WIFI.</div>
Nota: esto te dejaría con solo 3 puertos ethernet en tu Linksys, pero es una solución sencilla para los que no quieren meterle mano a la configuración.</blockquote>
