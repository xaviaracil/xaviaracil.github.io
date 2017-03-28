---
title: "Visualise, Document & Explore your Software Architecture"
date: "2017-03-28 11:45:53 +0200"
header:
  image: http://storage.pardot.com/210132/15813/goto_simon_brown_facebook.png
excerpt_separator: <!--more-->

---

Curiosa presentación realizada en el pasada GOTO 2016 sobre visualización de la arquitectura del software. Me ha gustado mucho la analogia mapa <--> diagrama y su propuesta de detalle que detalla como *C4*.

![](https://images.contentful.com/emmiduwd41v7/2XwNQZQnWoC4eE4kioCSy2/ea2791d4cd4daa09ff91aec9a6726020/Image18.JPG)

> Diagramas are maps

¿Quién no se ha encontrado nunca con este **problemón**? Es **muy** costoso mantener diagramas actualizados con el código fuente porque no hay herramientas que te ayuden. Normalmente estos diagramas se realizan con herramientas especializadas en diagramas tipo Visio, OmniGraffle, etc. Herramientas muy buenas, sí, pero no tiene conexión con el código, lo que implica que:
* tienes que hacer el diagramas a mano
* El mantenimiento es manual: si modificas el código, tienes que modificar el diagrama (a mano, claro)

Vale, también tenemos herramientas que analizan el código y te dibujan un diagrama. Suena genial, pero normalmente el diagrama resultante es **inútil**: No hay separaciones por capas lógicas, hay mucha información de detalle, etc.

<!--more-->

En la presentación se menciona la herramienta [Structurizr](https://www.structurizr.com/) a la que habrá que seguir.

> Who still uses UML?

UML, eso que aprendimos en la carrera pero que raramente usamos. Por qué? porque es muy abstracto? porque _component_ significa una cosa para mi equipo y otra distinta para el tuyo?

La podéis encontrar gracias a Realm en [GOTO 2016 • Visualise, Document & Explore your Software Architecture • Simon Brown](https://realm.io/news/gotocph-simon-brown-visualize-document-explore-your-software-architecture/?utm_source=ios-list&utm_medium=email&utm_content=ios-content)
