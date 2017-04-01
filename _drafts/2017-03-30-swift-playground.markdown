---
title: "Swift Playground to iOS Project"
date: "2017-03-30 15:16:07 +0200"
excerpt_separator: <!--more-->
categories:
  - swift
  - learning
  - sphero
---
En {% post_url 2017-03-29-sphero-primeros-pasos %} os comenté que _jugaría_ con el playground disponible en https://developer.apple.com/swift/blog/?id=38 para ver cómo se comunicaba con el Sphero. Para ello, mi idea es crear una app en iOS con el código del playground, ya que:

* Tengo ganas de ver el código.
* Mi iPad no puede ejecutar [Swift Playgounds](http://www.apple.com/swift/playgrounds/)

Así pues, vamos a crear una app con el código del playground.

<!--more-->

# Bundle

Un playground no es más que un [bundle](https://developer.apple.com/library/content/documentation/General/Conceptual/DevPedia-CocoaCore/Bundle.html), un directorio con un conjunto de recursos que se muestra por el Finder como un fichero simple. Una aplicación en macOS, por ejemplo, es un bundle.

> Un truco para saber si un ítem del Finder es un bundle es comprobar si aparece _Mostrar contenido del paquete_ en el menú contextual, tal y como muestra la imagen

![show package contents](/images/2017-03-30-swift-playground/bundle.png)

# Formato del Playground Book

![playground book](/images/2017-03-30-swift-playground/playground_book.png)


> La especificación completa del formato la podréis encontrar en  https://developer.apple.com/library/content/documentation/Xcode/Conceptual/swift_playgrounds_doc_format/index.html

# Extra

Hemos visto cómo está formado un playground. Alguno de vosotros habrá pensado en crear sus propios playgrounds (_total, son cuatro ficheros con un par de metadatos_). Si estaís interesados, os recomiendo el post de Ash Furrow [Building Swift Playground Books](https://ashfurrow.com/blog/building-swift-playground-books/)
