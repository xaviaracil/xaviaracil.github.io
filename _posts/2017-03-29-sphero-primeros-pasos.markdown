---
title: "Sphero: Primeros pasos"
date: "2017-03-29 15:52:17 +0200"
excerpt_separator: <!--more-->
categories:
  - swift
  - learning
  - sphero
---

Siguiendo con la serie iniciada en {% post_url 2016-11-27-sphero-para-iniciar-a-la-programacion--side-project %}, lo primero que hice fue ver qué posibilidades de programación me daba el juguetito.

Si leísteis el anterior post ({% post_url 2016-11-27-sphero-para-iniciar-a-la-programacion--side-project %}), había tres opciones (en realidad hay más, pero la opción _CoreBluetooth_ a pelo la descarto de momento):

- Código del [Swift Playground](https://developer.apple.com/swift/blog/?id=38)
- [Sphero iOS SDK](https://github.com/orbotix/Sphero-iOS-SDK)
- [SPRK Lightning Lab - Programming for Sphero Robots](http://www.sphero.com/education)

Vamos a ver qué ofrece cada uno.

<!--more-->
## Swift Playground

El playground contiene un conjunto de clases que se comunica son el Sphero via _CoreBluetooth_. Proporciona cuatro funcionalidades:

* `public func setColor(_ color: UIColor)` para cambiar el color
* `public func setBackLightBrightness(_ brightness: UInt8)` para cambiar el brillo de la luz posterior (la que indica la dirección del robot)
* `public func roll(speed: UInt8, heading: UInt16)` para moverse
* `public func heading(_ heading: UInt16)` para actualizar la dirección

## Sphero iOS SDK

SDK oficial aunque un poco anticuado (el último commit es de hace más de un año). Además, esta en Objective C, lenguaje vigente pero que no me molestaría en aprender (el futuro es Swift).

En cambio, proporciona más funcionalidades que el playground:

* Habilitar sensores y recibir eventos
* Upload de programas en [oval](http://sdk.sphero.com/robot-languages/oval-language/), un lenguaje en C.

## SPRK Lightning Lab

Es una app para programar el Sphero. Similar a [Scratch](https://scratch.mit.edu), permite el desarrollo de pequeños programas que se ejecutan en el Sphero. Perfecta para hacer las primeras pruebas y para dejar a los pequeñ@s, ya que el interfaz es muy claro: las instrucciones son bloques que vas juntando para _montar_ el programa.

![](http://a1.mzstatic.com/us/r30/Purple71/v4/e8/46/e2/e846e2e3-60f1-c11b-8986-ab69f0a77302/screen696x696.jpeg)

# Conclusiones y próximos pasos

Personalmente he empezado por dejar a las niñas que jueguen con la app mientras yo hago alguna prueba con el código del playground. También se me ha pasado por la cabeza probar el SDK (hasta migrarlo a Swift), pero no creo que tenga tanto tiempo.

En próximos posts contaré mis aventuras con el playground.
