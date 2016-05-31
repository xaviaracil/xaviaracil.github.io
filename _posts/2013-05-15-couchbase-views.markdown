---
author: desarrolloenmac
comments: true
date: 2013-05-15 14:36:43+00:00
layout: single
slug: couchbase-views
title: 'Couchbase: Views'
wordpress_id: 249
categories:
- Couchbase
---

Seguimos con la serie dedicada al Couchbase Barcelona Developer Day. Una vez explicado el servidor, las operaciones básicas hemos instalado el SDK ([http://www.couchbase.com/develop](http://www.couchbase.com/develop)) y jugado con él.

La instalación del SDK para [ruby](http://www.couchbase.com/develop/ruby/current) ha sido sencilla. El único problema es que el SDK depende de una libreria en C ([libcouchbase](http://www.couchbase.com/develop/c/current)), que en Mac OS X se instala usando homebrew, y yo tenía instalada una versión no actualizada. Por lo demás _bufar i fer ampolles_.

Una vez esto nos hemos metido con la chicha de verdad: las views. No son más que indices que creamos sobre la base de datos usando map-reduce. Mediante el map definimos la clave y el resultado de cada elemento en el índice y con reduce obtenemos la función a aplicar al conjunto de resultados para diferentes cálculos (contar, sumar, etc). Así podemos obtener listados de la base de datos a partir de campos de los documentos que tenemos, y no de las keys que los identifican.

Es más sencillo de lo que parece. En [http://hardlifeofapo.com/basic-couchbase-querying-for-sql-people/](http://hardlifeofapo.com/basic-couchbase-querying-for-sql-people/) podréis ver una analogía entre lo que sería una query SQL en Couchbase Views.
