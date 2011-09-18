---
layout: post
title: "Instalación de OpenBSD, Apache y ddclient (dyndns.org)"
date: 2006-09-05
comments: true
categories: 
---

OpenBSD es uno de los sistemas open source tipo unix más seguros que existen, y ¿porqué es uno de los mas seguros?, es porque la seguridad viene por default ("secure by default"), es decir, en la instalación por default se tiene un sistema minimalista con tan sólo los servicios mínimos requeridos y eso lo hace muy seguro, es más, en la página de OpenBSD (<a href="http://www.openbsd.org" title="OpenBSD">http://www.openbsd.org</a>) aseguran que sólo han tenido un hueco remoto en la instalación por default en más de 8 años. Y claro, algo muy importante es que lo puedo instalar en mi máquina pentium a 200 Mhz, si lo se, corre muy lento, pero creo que es la mejor opción.

Bien, para instalar openbsd hay que seguir la guía que está aquí:

<a href="http://www.openbsd.org/faq/faq4.html" title="OpenBSD faq 4">http://www.openbsd.org/faq/faq4.html</a>

Una vez que tengamos instalado OpenBSD habrá que crear un primer usuario, y como queremos que este usuario pueda acceder al comando <i>su</i> para realizar tareas administrativas como root se tendrá que agregar el usuario al grupo <i>wheel</i>. Se ejecuta el siguiente comando como root:
<pre># adduser</pre>
Cómo es la primera vez en ejecutarse adduser, se desplegará lo siguiente, a lo cual daremos &lt;Enter&gt; a todas las preguntas de configuración, con esto habilitaremos las opciones por defecto:
<pre>Couldn't find /etc/adduser.conf: creating a new adduser configuration file
Reading /etc/shells
Enter your default shell: csh ksh nologin sh [ksh]:
Your default shell is: ksh -&gt; /bin/ksh
Default login class: daemon default staff [default]:
Enter your default HOME partition: [/home]:
Copy dotfiles from: /etc/skel no [/etc/skel]:
Send message from file: /etc/adduser.message no [no]:
Do not send message
Prompt for passwords by default (y/n) [y]:
Default encryption method for passwords: auto blowfish des md5 old
[auto]:
Use option ``-silent'' if you don't want to see all warnings and questions.

Reading /etc/shells
Check /etc/master.passwd
Check /etc/group

Ok, let's go.
Don't worry about mistakes. I will give you the chance later to correct any input.</pre>
Posteriormente llenamos los datos del usuario de la siguiente manera:
<pre>Enter username []: jcastaneyra
Enter full name []: Jose Castaneyra
Enter shell csh ksh nologin sh [ksh]:
Uid [1000]:
Login group jcastaneyra [jcastaneyra]:
Login group is ``jcastaneyra''. Invite jcastaneyra into other groups: guest no
[no]: wheel
Login class daemon default staff [default]:
Enter password []:
Enter password again []:

Name:        jcastaneyra
Password:    ****
Fullname:    Jose Castaneyra
Uid:         1000
Gid:         1000 (jcastaneyra)
Groups:      jcastaneyra wheel
Login Class: default
HOME:        /home/jcastaneyra
Shell:       /bin/ksh
OK? (y/n) [y]: y
Added user ``jcastaneyra''
Copy files from /etc/skel to /home/jcastaneyra
Add another user? (y/n) [y]: n
Goodbye!</pre>
Ahora bien, habrá que probar nuestro nuevo usuario, le damos el comando <i>exit</i> y hacemos login con el usuario creado, y veremos que podemos acceder al root con el comando <i>su</i>.

Ya como root podemos iniciar nuestro servidor apache.
<pre># apachectl start
/usr/sbin/apachectl start: httpd started</pre>
Para que nuestro servidor de apache inicie cada vez que arranque OpenBSD deberemos meter las siguientes líneas a <i>/etc/rc.conf.local</i>:
<pre>httpd_flags=YES</pre>
Para poder tener corriendo el ddclient con un servicio de dominios dinámicos debemos darnos de alta con un proveedor de estos servicios, para este caso escogí dyndns, nos damos de alta y damos de alta el dominio que queramos (uno de los dominios gratuitos, o bien comprar uno, pero eso nos costaría), en este caso agregamos oax.homeunix.org.

<a href="http://img432.imageshack.us/img432/919/dyndnsmg5.jpg"><img src="http://img459.imageshack.us/img459/4291/dyndnszb6.jpg" alt="Dyndns" height="242" width="320" /></a>

Ahora bien, para que nuestra ip dinámica pueda ser actualizada frecuentemente con el dominio que registramos en dyndns, tenemos que instalar el ddclient, y lo instalamos de la siguiente manera:
<pre># pkg_add ftp://ftp.openbsd.org/pub/OpenBSD/3.9/packages/i386/ddclient-3.6.6.tgz
ddclient-3.6.6: complete</pre>
Editamos el archivo <i>/etc/ddclient/ddclient.conf</i>, y en nuestro caso nos aseguramos de que las líneas siguientes estén en el archivo:
<pre>use=web, web=checkip.dyndns.org/, web-skip='IP Address' # found after IP Address

login=jcastaneyra                               # default login
password=mipassword                             # default password

##
## dyndns.org dynamic addresses
##
## (supports variables: wildcard,mx,backupmx)
##
 server=members.dyndns.org,
 protocol=dyndns2
 oax.homeunix.org</pre>
Y ejecutamos el ddclient con el comando <i>/usr/local/sbin/ddclient</i>.

Ahora bien, para que el ddclient se ejecute cada vez que arranque nuestra máquina hay que agregar las siguientes líneas a <i>/etc/rc.local</i> antes de <i>echo '.'</i>
<pre>if [ -x /usr/local/sbin/ddclient ]; then
echo -n ' ddclient';       /usr/local/sbin/ddclient
fi</pre>
¡A probar nuestro servidor de web con dominio dinámico! (dominio sobre una ip dinámica)

<a href="http://img459.imageshack.us/img459/7516/apacheoaxpd8.jpg"><img src="http://img367.imageshack.us/img367/6486/apacheoaxcl1.jpg" alt="Web corriendo" height="242" width="320" /></a>

Y listo, ya tenemos un servidor OpenBSD con web y dominio corriendo :D
