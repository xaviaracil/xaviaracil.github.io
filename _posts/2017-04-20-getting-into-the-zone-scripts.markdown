---
title: "Getting into the Zone: Scripts"
date: "2017-04-20 13:17:44 +0200"
excerpt_separator: <!--more-->
header:
  teaser: getting-into-the-zone/script.png
  image: getting-into-the-zone/script.png

categories:
  - scripts
  - lifehacking
  - productividad

---

Nuestro día a día es un continuo de interrupciones. La *Zona* es aquel espacio de tiempo donde no estamos para nadie, eliminando cualquier distracción posible para centrarnos en producir:

- Mail
- Mensajeria instantánea
- Lector de feeds
- Notificaciones del móvil

No es un concepto nuevo (me acuerdo de haber leído al respecto en [Rework: Change the Way You Work Forever](http://amzn.to/2o6XyNd) allá por el ¿2008?) y intenté automatizarlo por entonces vía acción del malogrado QuickSilver.

> QuickSilver me permitió, entre otras cosas, descubrir [Think Wasabi](http://thinkwasabi.com/), un blog que pivotó a hablar de productividad bastante recomendable

De vuelta a la actualidad, vamos a intentar automatizarlo de nuevo.
<!--more-->

## TL;DR

Bajate los scripts de <https://github.com/xaviaracil/zone-scripts> y copia los ficheros `Getting in the Zone.scpt` y `Back from Zone.scpt` a la carpeta `~/Library/Scripts` (si no existe la puedes crear).

Para acceder a los scripts abre la aplicación `Editor de Scripts` y activa `Mostrar el menú Scripts en la barra de menús` en las preferencias.

Tendréis los scripts disponibles en el menú de scripts.

 ![script](/images/getting-into-the-zone/menu-scripts.png)

# Automatizando la entrada y salida (al menos del mac)

Usaremos AppleScript para crear un par de scripts que nos permitan entrar y salir de la *Zona*. Los scripts son muy sencillos: cierran o abren un conjunto de aplicaciones.

> [AppleScript](https://developer.apple.com/library/content/documentation/AppleScript/Conceptual/AppleScriptX/AppleScriptX.html) es un lenguaje de scripts para mac OS que permite interactuar con aplicaciones y el propio mac OS de forma automática.

![script](/images/getting-into-the-zone/script.png)

Las aplicaciones a cerrar/abrir las definimos en la variable `appList`. En mi caso tengo definidas:

- Mail
- Skype
- Telegram
- Mensajes
- Leaf

El cógido para cerrar estas aplicaciones es tan simple como:

	set appList to {"Mail", "Skype", "Telegram", "Messages", "Leaf"}
	repeat with theApp in appList
      tell application theApp to quit
	end repeat

El código está disponible en el repo  <https://github.com/xaviaracil/zone-scripts> de GitHub.

## Poniendo los scripts en su sitio

Los scripts deben estar en la carpeta `~/Library/Scripts`. Si la carpeta no existe la tendréis que crear.

![folder](/images/getting-into-the-zone/folder-scripts.png)

## Acceso a los scripts

Los scripts estarán disponibles en el menú de scripts.

![script](/images/getting-into-the-zone/menu-scripts.png)

### ¿Menú de scripts? ¡No lo veo!

Ningún problema: abre la aplicación `Editor de Scripts` (en `/Applications/Utilities`) y asegúrate de tener activado `Mostrar el menú Scripts en la barra de menús` en las preferencias.

![prefs](/images/getting-into-the-zone/pref.png)


# Extra

Podéis crear un servicio en Automator que ejecute cada uno de los scripts. Parece una tontería, pero los servicios pueden tener asociado una combinación de teclas, permitiendo la ejecución de los scripts sin tener que soltar las manos del teclado. Si estáis interesados, os explico cómo en otro post.

# Mejoras

1. Se podría integrar el código de <http://stackoverflow.com/questions/25210120/is-it-possible-to-turn-on-off-do-not-disturb-for-os-x-programmatically> para activar/desactivar la funcionalidad de "No Molestar" del Centro de Notificaciones.
2. Se podría _recordar_ de alguna manera al usuario para que: deshabilite las notificaciones de su smartphone, o al menos le dé la vuelta y lo ponga boca abajo.
