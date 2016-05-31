---
date: 2013-05-16 09:59:30+00:00
slug: test-unitarios-en-javascript
title: Test unitarios en Javascript
wordpress_id: 251
categories:
- Javascript
- Test
---

Recientemente he descubierto [QUnit](http://qunitjs.com) para la realización de test unitarios en Javascript. La instalación consiste en añadir un js y un css a tu página web.

Para definir y ejecutar un test bastará con llamar a la función _test(name, function)_, pasándole una función con los resultados esperados. El ejemplo más sencillo (sacado de su página de documentación) sería:





`test( "hello test", function() {`










` ok( 1 == "1", "Passed!" );`










`});`







De momento me funciona muy bien para los test unitarios. Ahora estoy investigando como realizar test impliquen llamadas asíncronas. Buscando por ahí he encontrado [Jquery Spy](http://www.jqueryspy.com), pero me da que sólo funciona para las llamadas AJAX de Jquery.
