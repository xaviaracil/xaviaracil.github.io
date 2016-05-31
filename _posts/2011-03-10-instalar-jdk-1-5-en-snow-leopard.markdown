---
date: 2011-03-10 12:31:07+00:00
slug: instalar-jdk-1-5-en-snow-leopard
title: Instalar JDK 1.5 en Snow Leopard
wordpress_id: 83
categories:
- Java
---

Snow Leopard viene únicamente con la versión 1.6 de la JDK de Java. Aunque parezca que tenga la JDK 1.5 si miramos en /System/Library/Frameworks/JavaVM.framework/Versions, en realidad es un softlink hacia la JDK 1.6

[![](/images/2011-03-10-instalar-jdk-1-5-en-snow-leopard/java15_main-300x256.png)](/images/2011-03-10-instalar-jdk-1-5-en-snow-leopard/java15_main.png)




Si queremos trabajar con la JDK 1.5 hay que hacer un pequeño proceso que mostramos a continuación.

<!-- more -->

Primero, id a la carpeta /System/Library/Frameworks/JavaVM.framework/Versions y moved las carpetas 1.5.0 y 1.5 la papelera. Os pedirá la contraseña de administración.




Bajad la última actualización de la JDK 1.5 para Leopard (en el momento de la redacción de esta entrada es la [Java 1.5 Update 9](http://support.apple.com/kb/DL1359)).




Una vez bajado, montad el DMG haciendo doble click sobre el fichero JavaForMacOSX10.5Update9.dmg y abrid el fichero JavaForMacOSX10.5Update9.pkg con la herramienta [Pacifist](http://www.charlessoft.com/), tal y como muestra la imagen

[![](/images/2011-03-10-instalar-jdk-1-5-en-snow-leopard/java15_install_pacifist-300x157.png)](/images/2011-03-10-instalar-jdk-1-5-en-snow-leopard/java15_install_pacifist.png)




Una vez en Pacifist, navegad hasta Contents of JavaForMacOSX10.5Update9.pkg/Contents of JavaForMacOSX10.5Update9.pkg/System/Library/Frameworks/JavaVM.framework/Versions, seleccionad 1.5 y 1.5.0 y con el botón secundario del ratón seleccionad Install to Default Location.

[![](/images/2011-03-10-instalar-jdk-1-5-en-snow-leopard/java15_install_inside_pacifist-263x300.png)](/images/2011-03-10-instalar-jdk-1-5-en-snow-leopard/java15_install_inside_pacifist.png)




Os pedirá confirmación para instalarla. Comprobad que el checkbox Use Administrator Privileges está marcado y clickar Install. Os pedira la contraseña de administrador.

[![](/images/2011-03-10-instalar-jdk-1-5-en-snow-leopard/java15_install_pacifist_confirm-300x123.png)](/images/2011-03-10-instalar-jdk-1-5-en-snow-leopard/java15_install_pacifist_confirm.png)




Y ya está. Para usar la JDK 1.5 por defecto añadir las siguientes líneas en el fichero .profile de vuestra carpeta de usuario (si no existe podéis crearlo).



    export JAVA_HOME=/System/Library/Frameworks/JavaVM.framework/Versions/1.5.0/Home
    export PATH="JAVA_HOME/bin:$PATH"



Como prueba, abrid un nuevo terminal y ejecutad el comando:



    	java -version



Si todo ha ido bien os mostrará:



    	java version "1.5.0_26"
    	Java(TM) 2 Runtime Environment, Standard Edition (build 1.5.0_26-b03-376-10M3326)
    	Java HotSpot(TM) Client VM (build 1.5.0_26-156, mixed mode, sharing)






[![](/images/2011-03-10-instalar-jdk-1-5-en-snow-leopard/java15_confirm-300x191.png)](/images/2011-03-10-instalar-jdk-1-5-en-snow-leopard/java15_confirm.png)





Un consejo: tened una copia a mano de JavaForMacOSX10.5Update9.dmg ya que cada vez que actualizeis la JDK tendreis que repetir este proceso.
