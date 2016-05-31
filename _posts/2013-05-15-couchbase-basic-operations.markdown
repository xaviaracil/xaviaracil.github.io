---
date: 2013-05-15 11:10:46+00:00
slug: couchbase-basic-operations
title: 'Couchbase: Basic Operations'
wordpress_id: 247
categories:
- Couchbase
---

Siguiendo la agenda de la [Couchbase Barcelona Developer Day](http://eventbrite.com/event/6310267179), ahora ha tocado las operaciones básicas.

Couchbase se comporta como una hash, donde cada key tiene asociado un valor, valor que puede ser del tipo que se quiera: integer, JSON, binary, etc. Las Las limitaciones son: key tiene que ser única y el valor no puede pasar de 20Mb de tamaño. Parece suficiente, no?

Las operaciones básicas son get, set, add, replace y cas. La diferencia entre set y add es que set hace un override de los datos (si ya existían) mientras que add lanza una excepción si los datos ya existían en  base de datos. Estas operaciones son muy rápidas, por lo que te recomiendan abusar de ellas en vez de montarte una join descomunal.

La clave aquí es modelar el documento correctamente. Tu documento puede ser un JSON con todos los datos referentes a tu modelo (aunque implique duplicidad de datos) o múltiples documentos que se referencian unos con otros (mediante "jugar" con la key de los documentos). La ventaja es que este modelado se puede modificar cada vez que se desee sin perjudicar a la base de datos.

Seguimos.
