---
title: "Swift Playground to iOS Project II: iOS Project"
date: "2017-04-18 16:42:08 +0200"
excerpt_separator: <!--more-->
header:
  teaser: 2017-03-30-swift-playground/playground_book-150x150.png
  image: 2017-03-30-swift-playground/playground_book.png

categories:
  - swift
  - learning
  - sphero

---

Seguimos con la serie Sphero, cuyo último post fue {% post_url 2017-04-18-swift-playground-to-ios-project %}. Ahora que ya tenemos localizado el código fuente del playground es hora de crear nuestro proyecto iOS para jugar con BB-8 desde nuestro iPhone.

<!--more-->

# Preparando el proyecto

Tenemos que crear un nuevo proyecto de tipo *iOS -> Single View App* en Xcode. Una vez creado el proyecto, y de cada a tener organizado las fuentes del mismo, podemos copiar el contenido que nos interesa del playground dentro de un nuevo grupo del proyecto Xcode.

![new xcode project ](/images/2017-04-18-swift-playground-to-ios-project-ii-ios-project/xcode_project.png)

Para ello, tenemos que añadir al proyecto los ficheros que hay en la carpeta `Contents/Chapters/Sphero.playgroundchapter/Sources`. Estos ficheros contienen lo necesario para conectarse al Sphero y enviarle algunos comandos básicos.

![playground files ](/images/2017-04-18-swift-playground-to-ios-project-ii-ios-project/playground_files.png)

Si hemos añadido los ficheros correctamente, tendremos una estructura de ficheros en nuestro proyecto Xcode parecida a la mostrada en la siguiente imagen:

![Project structure ](/images/2017-04-18-swift-playground-to-ios-project-ii-ios-project/project_file_structure.png)

# Comandos básicos

Los comandos que nos permite enviar al Sphero 
