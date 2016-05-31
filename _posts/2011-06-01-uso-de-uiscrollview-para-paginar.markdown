---
date: 2011-06-01 18:35:06+00:00
slug: uso-de-uiscrollview-para-paginar
title: Uso de UIScrollView para paginar
wordpress_id: 107
categories:
- Cocoa
- iOS
- Objective-C
---

Cuando uno mira la librería de componentes visuales para añadir en un fichero de interficie y encuentra a UIScrollView, lo más común es pensar que sirve para mostrar "algo", y que si ese "algo" ocupa más tamaño que el visible, aparecerá el scroll para mostrarlo.

Antes de empezar, el razonamiento anterior es totalmente válido. Si miramos la definición de UIScrollView en la documentación, encontraremos


<blockquote>The UIScrollView class provides support for displaying content that is larger than the size of the application’s window. It enables users to scroll within that content by making swiping gestures, and to zoom in and back from portions of the content by making pinching gestures.</blockquote>


Lo que nadie puede pensar, a priori, es que UIScrollView tiene dos formas de mostrar ese contenido extra: scroll y paginación. En este post hablaremos de cómo utilizar UIScrollView para paginar, consiguiendo comportamientos como el del tweets populares en la aplicación oficial de Twitter.

<!-- more -->![Aplicación de Twitter para iPhone](http://a1.mzstatic.com/us/r1000/028/Purple/de/d2/cc/mzl.tzgeibja.320x480-75.jpg)

UIScrollView muestra lo que tiene en contentView (un atributo de clase UIView) según si tiene activada la opción de scroll o de paginación. Para activar la paginación en una UIScrollView hay que activar los atributos Scrolling Enabled y Paging Enabled, tal y como muestra la figura.

[![](/images/2011-06-01-uso-de-uiscrollview-para-paginar/UIScrollView-paging.png)](/images/2011-06-01-uso-de-uiscrollview-para-paginar/UIScrollView-paging.png)

Y asignarle un contentSize adecuado. ¡Y ya está! Para demostrarlo crear un nuevo proyecto para iOS de tipo View-based Application

[![](/images/2011-06-01-uso-de-uiscrollview-para-paginar/UIScrollView-Paging-NewProject-300x204.png)](/images/2011-06-01-uso-de-uiscrollview-para-paginar/UIScrollView-Paging-NewProject.png)

Abrir el fichero XIB y poner una UIScrollView, de 300 píxels de ancho por 100 píxels de alto. Arrastrar 3 UILabels dentro del UIScrollView (aseguraos que están realmente dentro, lo podéis comprobar en la vista jerárquica, tal y como muestra la siguiente imagen).

![](/images/2011-06-01-uso-de-uiscrollview-para-paginar/UIScrollView-Paging-Frame.png)

![](/images/2011-06-01-uso-de-uiscrollview-para-paginar/UIScrollView-Paging-Outline.png)

Ajustad el texto de cada UILabel como "Texto 1", "Texto 2" y "Texto 3", respectivamente. Ajustad el tamaño a 300 x 100 en las tres UILabel y la posición x en 0.0, 300.0 y 600.0, respectivamente.

Por último, crear y asignad el outlet del UIScrollView en vuestro UIViewController (en XCode 4 es muy sencillo usando la vista de asistente).

![](/images/2011-06-01-uso-de-uiscrollview-para-paginar/UIScrollView-Paging-Outlet.png)[![](/images/2011-06-01-uso-de-uiscrollview-para-paginar/UIScrollView-Paging-Outlet2-300x187.png)](/images/2011-06-01-uso-de-uiscrollview-para-paginar/UIScrollView-Paging-Outlet2.png)

Una vez creado y asignado el outlet, descomentar el método viewDidLoad, añadiendo el siguiente trozo de código


        CGSize contentSize = CGSizeMake(3 *300.0, 100.0);
        [scrollView setContentSize:contentSize];


Como veis, hemos asignado el contentSize como 300 por el número de UILabels creadas.

Lo único que falta es el Build & Run!

El ejemplo demuestra lo fácil que es paginar con UIScrollView. En próximos posts os contaré como optimizar los recursos para no tener cargado en memoria todas las subviews e ir cargando únicamente la que se visualiza y sus "hermanas".
