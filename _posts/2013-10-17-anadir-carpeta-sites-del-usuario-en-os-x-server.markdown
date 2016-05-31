---
date: 2013-10-17 07:48:34+00:00
slug: anadir-carpeta-sites-del-usuario-en-os-x-server
title: Añadir carpeta Sites del usuario en OS X Server
wordpress_id: 295
categories:
- OS X
- Teletrabajo
---

A partir de Lion Compartir Web dejó de estar accesible en la preferencia del sistema Compartir. No obstante, OS X seguía viniendo con Apache, por lo que se podía activar el Compartir Web siguiendo tutoriales como el de [http://reviews.cnet.com/8301-13727_7-57481978-263/how-to-enable-web-sharing-in-os-x-mountain-lion/](http://reviews.cnet.com/8301-13727_7-57481978-263/how-to-enable-web-sharing-in-os-x-mountain-lion).

Si eres miembro del [Mac Developer Program](https://developer.apple.com/devcenter/mac/index.action) tienes acceso a una versión de [Mac OS x Server](https://www.apple.com/osx/server/), que contiene, entre otros, un servidor web (Apache) y, lo más importante, una aplicación para configurarlo de manera amigable (esto es, no tener que abrir el terminal y empezar a editar /etc/apache2/httpd.conf). El problema que hay es que el Apache que instala OS X Server es diferente al que viene por defecto en OS X, y cuando instalas OS X Server éste deshabilita el Apache por defecto (básicamente lo elimina de LaunchDaemons para evitar que se arranque cuando se inicia el sistema). Así que la configuración del tutorial anterior no nos sirve para activar Compartir Web con OS X Server.

Mirando la documentación de Apple al respecto (concretamente en [https://help.apple.com/advancedserveradmin/mac/10.8/#apd9eb9f4ab-1377-47e6-a2c4-1311e25a74df](https://help.apple.com/advancedserveradmin/mac/10.8/#apd9eb9f4ab-1377-47e6-a2c4-1311e25a74df)) vemos dónde se encuentran los directorios de configuración para poderles meter mano, pero lo que queremos es hacerlo sin abrir el Terminal, ¿verdad? Si queréis hacerlo via Terminal dejad un comentario y lo explico.

Para habilitar Compartir Web con OS X Server tendremos que:


## 1. Abrir la aplicación Server


Se encuentra en /Applications. Además seleccionar el servidor que queremos configurar (en nuestro caso el propio ordenador).


## 2. Seleccionar el Sitio Web que queremos configurar


Normalmente el sitio web HTTP, puerto 80.


[![SitiosWeb](/images/2013-10-17-anadir-carpeta-sites-del-usuario-en-os-x-server/SitiosWeb-300x194.png)](/images/2013-10-17-anadir-carpeta-sites-del-usuario-en-os-x-server/SitiosWeb.png)




Editar el sitio web, haciendo clic donde indica el cursor en la imagen anterior.


## 3. Editar los alias


Un alias no es más que un path que el servidor web resuelve una carpeta diferente a la que tocaría. Esto es justo lo que queremos, ya que queremos que http://localhost/~xavi se sirva desde /Users/xavi/Sites y no desde /Library/Server/Web/Data/Sites/Default/~xavi (que sería la carpeta que tocaría).

Editar los alias del sitio web, haciendo clic donde indica el cursor en la siguiente imagen.


## [![SitioWeb](/images/2013-10-17-anadir-carpeta-sites-del-usuario-en-os-x-server/SitioWeb-300x194.png)](/images/2013-10-17-anadir-carpeta-sites-del-usuario-en-os-x-server/SitioWeb.png)4. Añadir el alias


Click en + y añadir el alias. Acordaos de poner el "~".


[![alias](/images/2013-10-17-anadir-carpeta-sites-del-usuario-en-os-x-server/alias-300x153.png)](/images/2013-10-17-anadir-carpeta-sites-del-usuario-en-os-x-server/alias.png)




Finalmente aceptarlo todo y ya lo tendréis. Si tenéis más de un usuario tendréis que añadir un alias para cada usuario.

Espero que os sea de utilidad
