---
title: "Swift Playground to iOS Project"
date: "2017-04-18 16:40:13 +0200"
excerpt_separator: <!--more-->
header:
  teaser: 2017-03-30-swift-playground/playground_book-150x150.png
  image: 2017-03-30-swift-playground/playground_book.png

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

Si abrimos el contenido del paquete del playground disponible en https://developer.apple.com/swift/blog/?id=38 veremos algo así como la imagen siguiente:

![playground book](/images/2017-03-30-swift-playground/playground_book.png)

Parece sencillo, ¿verdad? un par de apuntes:

- `Chapters` contiene los capítulos o apartados del _book_. Dentro esta el fichero `Manifest.plist` con los metadatos del _book_ (capítulos, etc.).
- `Resources` contiene los recursos generales de todo el _book_ (imágenes, vídeos, etc.).

Cada capítulo es una carpeta con:

- Carpeta `Pages` con carpetas para cada página del capítulo.
- Carpeta `Sources` con código fuente común al capítulo (_aka las librerías_).
- Fichero `Manifest.plist` con metadatos del capítulo (páginas, etc).

Cada página es una carpeta con:

- Fichero `Contents.swift` con el código que se visualiza (y ejecuta) en la página.
- Carpeta `Resources` con recursos de la página (imágenes, vídeos, etc.).
- Fichero `Manifest.plist` con metadatos de la página (cómo debe visualizarse, etc.).

> La especificación completa del formato la podréis encontrar en  https://developer.apple.com/library/content/documentation/Xcode/Conceptual/swift_playgrounds_doc_format/index.html

Así pues, todo lo que hay en la/s carpeta/s `Sources` y en los ficheros `Contents.swift` conforma el _codebase_ del proyecto. Para nuestro objetivo, la _chicha_ se encuentra en el capitulo `Sphero.playgroundchapter`.

> **Licencia**

> El código del playground tiene licencia "AS IS", así que en principio se puede usar. De todas maneras, si hay alguno experto en derecho es un buen momento para manifestarse.


# Extra

Hemos visto cómo está formado un playground. Alguno de vosotros habrá pensado en crear sus propios playgrounds (_total, son cuatro ficheros con un par de metadatos_). Si estaís interesados, os recomiendo el post de Ash Furrow [Building Swift Playground Books](https://ashfurrow.com/blog/building-swift-playground-books/)

# Siguientes pasos

Una vez tenga claro el aspecto legal (básicamente si la licencia permite usar el código fuente, que creo que _sí_) adaptaré el código en un proyecto iOS.
