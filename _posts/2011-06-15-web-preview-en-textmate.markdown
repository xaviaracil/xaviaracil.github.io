---
date: 2011-06-15 09:28:42+00:00
slug: web-preview-en-textmate
title: Web Preview en Textmate
wordpress_id: 161
categories:
- HTML
- Navegadores
- Textmate
---

Ya sabéis que opino que [Textmate](http://macromates.com/) es un gran editor de código fuente. Tuve la suerte que obtener una licencia con la primera versión del [MacHeist](http://macheist.com/) y no puedo estar más contento.

Entre sus muchas funcionalidades está la previsualización _live_ del fichero HTML que estás escribiendo, disponibles desde la opción de menu Window -> Show Web Preview o la combinación de teclas Ctrl + Option + Command + p. Esta previsualización se hace dentro del propio Textmate, pero ¿que pasa si queremos previsualizar nuestro HTML en un navegador web? Textmate nos ofrece esa posibilidad.

<!-- more -->

Buscando en internet cómo abrir Textmate desde el Terminal después de una restauración del sistema (no os preocupéis, era para dividir el disco duro de mi MacBookPro en dos particiones, una para la developer preview de Lion) he encontrado la página [http://wiki.macromates.com/Main/Howtos](http://wiki.macromates.com/Main/Howtos), donde hay una serie de trucos para Textmate.

El que nos ocupa está disponible en [http://wiki.macromates.com/Main/Howtos#SafariPreview](http://wiki.macromates.com/Main/Howtos#SafariPreview). Resumiendo, para hace un _live preview_ de nuestro HTML en Safari (o otro navegador web) hay que realizar los siguientes pasos:




  * Crear un fichero de script. Podéis utilizar el propio Textmate para hacerlo. El contenido del script es el siguiente:



    #!/bin/bash --posix
    tee /tmp/saf_tmp.html

    osascript -e "tell application "Safari" to open location "file:///tmp/saf_tmp.html""


No estamos haciendo más que guardar la salida estándar en el fichero /tmp/saf_tmp.html y ejecutar un [Applescript](http://developer.apple.com/applescript) que indica a Safari que abra el fichero /tmp/saf_tmp.html. SI en vez de Safari queréis usar Firefox o Chrome sólo hay que modificar el nombre de la aplicación.


  * Guardar el fichero de script en una carpeta. Sencillo, no? A modo de ejemplo, yo lo he guardado en ~/Soft/TextmateAddons/safari_preview.sh


  * Abrir un fichero HTML en Textmate (o crear uno nuevo) y abrir el Web Live Preview. Ya sabéis, mediante la opción de menu o Ctrl + Option + Command + p


  * Activar el check Show options si no está activado. Se encuentra en la parte inferior de la Web Live Preview.


  * En el Drawer que se muestra, activar el check Pipe text through, y en campo de text escribir sh el_path_a_vuestro_script. En mi caso, el texto a poner sería ~/Soft/TextmateAddons/safari_preview.sh


  * Y ya está. Si además, tenéis activado el check Refresh after change, cada cambio hecho en Textmate se verá reflejado en Safari de forma inmediata.




En la captura de pantalla podéis ver como queda el tinglado.
[![](/images/2011-06-15-web-preview-en-textmate/textmate_safari-300x187.png)](/images/2011-06-15-web-preview-en-textmate/textmate_safari.png)
