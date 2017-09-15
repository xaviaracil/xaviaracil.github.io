---
title: "Swift Playground to iOS Project II: iOS Project"
date: "2017-09-15 18:12:00 +0200"
excerpt_separator: <!--more-->
header:
  teaser: 2017-09-15-swift-playground-to-ios-project-ii-ios-project/xcode_project.png
  image: 2017-09-15-swift-playground-to-ios-project-ii-ios-project/xcode_project.png

categories:
  - swift
  - learning
  - sphero

---

Seguimos con la serie Sphero, cuyo último post fue {% post_url 2017-04-18-swift-playground-to-ios-project %}. Ahora que ya tenemos localizado el código fuente del playground es hora de crear nuestro proyecto iOS para jugar con BB-8 desde nuestro iPhone.

La idea es replicar el comportamiento de una página del playground en una aplicación iOS. La página que queremos replicar es la página `Sphero` del capítulo `Sphero`. En ella, se muestra una especie de joystick que controla los movimientos del Sphero a la vez que permite cambiar el color del robot seleccionándolo entre un conjunto de colores mostrados.

<!--more-->

# Preparando el proyecto

Tenemos que crear un nuevo proyecto de tipo *iOS -> Single View App* en Xcode. Una vez creado el proyecto, y de cada a tener organizado las fuentes del mismo, podemos copiar el contenido que nos interesa del playground dentro de un nuevo grupo del proyecto Xcode.

![new xcode project ](/images/2017-09-15-swift-playground-to-ios-project-ii-ios-project/xcode_project.png)

Para ello, tenemos que añadir al proyecto los ficheros que hay en la carpeta `Contents/Chapters/Sphero.playgroundchapter/Sources`. Estos ficheros contienen lo necesario para conectarse al Sphero y enviarle algunos comandos básicos.

![playground files ](/images/2017-09-15-swift-playground-to-ios-project-ii-ios-project/playground_files.png)

Si hemos añadido los ficheros correctamente, tendremos una estructura de ficheros en nuestro proyecto Xcode parecida a la mostrada en la siguiente imagen:

![Project structure ](/images/2017-09-15-swift-playground-to-ios-project-ii-ios-project/project_file_structure.png)

# Comandos básicos

Los comandos que tenemos disponibles para enviar al Sphero se encuentran implementados en el fichero `Sphero+Commands.swift`. Son los siguientes:

- `setColor` para definir el color del sphero.
- `setBackLightBrightness`
- `roll` para moverse
- `heading` para definir la dirección de movimiento

No son muchos, pero para lo básico nos sobra y basta.

# ViewController + CollorWell

Podemos copiar el fichero del playground `Contents/Chapters/Sphero.playgroundchapter/Pages/Sphero.playgroundpage/Sources/SpheroViewController.swift` en nuestro proyecto. Este fichero contiene dos clases:

## CollorWell.swift
Vista para mostrar un círculo de un determinado color y que ejecuta una acción cuando el usuario hace tap dentro de ese círculo. Esta acción viene definida por el parámetro `action` del constructor `init(color: UIColor, action: @escaping (ColorWell) -> Void)`.

## SpheroViewController.swift
UIViewController principal. Crea la jerarquía de vistas en `viewDidLoad` y recoge los eventos de tap dentro del joystick para calcular la dirección y velocidad y mover al robot (funciones `    touchesBegan(_ touches: Set<UITouch>, with event: UIEvent?)`, `touchesMoved(_ touches: Set<UITouch>, with event: UIEvent?)` y `touchesEnded(_ touches: Set<UITouch>, with event: UIEvent?)`).

Cabe decir que este controller no envia ningún comando al robot. Contiene las variables `public var joystickMoved: ((_ angle: Double, _ magnitude: Double) -> Void)?` y `public var colorSelected: ((UIColor) -> Void)?` que se invocan cuando tocan (movimiento del joystick y selección de color). Es faena nuestra el implementar los métodos correspondientes.

Por suerte, en el fichero `Contents/Chapters/Sphero.playgroundchapter/Pages/Sphero.playgroundpage/Contents.swift` del playground tenéis el código de estas dos funciones:


    // A Sphero object which is automatically set to the nearest device.
    var sphero: Sphero?

    //#-editable-code
    let viewController = SpheroViewController()

    viewController.joystickMoved = { angle, magnitude in
      let rollForce = UInt8(magnitude * 0.5 * Double(UInt8.max))
      let rollAngle = UInt16(radiansToDegrees(angle))

      sphero?.roll(speed: rollForce, heading: rollAngle)    
    }

    viewController.colorSelected = { color in
      sphero?.setColor(color)
    }

Este código se puede reutilizar, añadiendo el mismo a nuestro controller, por ejemplo.

### Cómo me connecto al robot

El código para conectarse al propio robot está en el mismo fichero `Contents.swift` del playground:

    DispatchQueue.main.async {
      sphero = Sphero.nearest()
    }

`Sphero` es una clase que se define en `Sphero.swift`, copiado en el paso anterior. Fijaos que la llamada se realiza en otro thread para no bloquear el thread principal.

### Recursos

`ViewController` crea dos `UIImage` con las imagenes `Boundary` y `Notches`. Estas imágenes se encuentran en la carpeta `Contents/Resources` del playground. Tendremos que añadirlas a los assets de nuestro proyecto Xcode.

# Resumen

Dependiendo de la versión de Xcode y de la versión de Swift que tengas seguramente os tocará modificar algo el código. Poca cosa la verdad.

No subo el proyecto de ejemplo al GitHub porque no estoy seguro si la licencia de las librerías de Apple lo permiten. En todo caso, si tenéis cualquier tipo de problema no dudéis en comentarlo por aquí.
