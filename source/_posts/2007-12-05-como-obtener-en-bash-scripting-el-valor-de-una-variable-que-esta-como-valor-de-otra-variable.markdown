---
layout: post
title: "Como obtener en bash scripting el valor de una variable que está como valor de otra variable"
date: 2007/12/05
comments: true
categories: 
---

Si, así como lo leen, ¿Cómo obtener el valor de una variable cuyo nombre a su vez es el valor de otra variable?, ¡pero que demon...! ¿Quien querría hacer algo así y con bash scripting? La verdad no se, yo lo iba a utilizar en el trabajo pero ya no fue así, la cuestión es que voy a dejar esto plasmado por si después se me vuelve a ofrecer o igual si a alguien más se le ocurre hacer algo como esto.

Supongamos que yo quiero ejecutar el comando de hostname para obtener el nombre de mi servidor, pero a su vez yo tengo una variable en mi script cuyo nombre es el mismo resultado del comando hostname (que igual y podría ser cualquier otro comando, o cualquier otro resultado, la idea sería tener algo así como variables anidadas).

<!-- more -->

Pues bien, al ejecutar el comando hostname, el resultado de este comando sería "gandalf":
<pre lang="bash">jcastaneyra@gandalf:~/My documents$ hostname
gandalf</pre>
Y mi script tendría algo así:
<pre  lang="bash">jcastaneyra@gandalf:~/My documents$ cat test.sh
#!/bin/bash
gandalf="El nombre de mi server esta chido"

echo "Ejecutando el comando hostname"
result=`hostname`
echo ${result}
otro=`eval echo '$'${result}`
echo $otro</pre>
Y al ejecutar mi script obtendría:
<pre  lang="bash">jcastaneyra@gandalf:~/My documents$ ./test.sh
Ejecutando el comando hostname
gandalf
El nombre de mi server esta chido</pre>
Analizando el script, tenemos que se define una variable gandalf con la cadena "El nombre de mi server esta chido", al ejecutar el comando hostname, el resultado es guardado en la variable result y después es desplegada, con la línea
<pre  lang="bash">`eval echo '$'${result}`</pre>
se evalúa la expresión que hace un echo de la variable
<pre  lang="bash">result</pre>
que a su vez tiene un valor
<pre  lang="bash">gandalf</pre>
y que a su vez tiene la cadena "El nombre de mi server esta chido".

Con esto obtenemos el valor de la variable gandalf que a su vez es el valor de la variable result, como dije en un principio algo así como variables anidadas en bash scripting.
