---
author: desarrolloenmac
comments: true
date: 2013-05-15 09:07:35+00:00
layout: single
slug: couchbase-introduccion
title: 'Couchbase: Introducción'
wordpress_id: 244
categories:
- Couchbase
---

En la primera sesión de [Couchbase Barcelona Developers Day]({% post_url 2013-05-15-en-el-couchbase-barcelona-developer-day %}) nos han hecho una introducción de Couchbase. Para el que no lo sepa, Couchbase es una base de datos Not Only SQL. Que qué significa esto? Básicamente, y resumiendo mal y rápido, que es una base de datos sin schema, por lo que cada registro de una tabla (hablando en términos SQL) puede tener las columnas que quiera, no está ligado a un schema que te defina las columnas que tiene cada tabla.

Dentro del abanico de base de datos NoSQL, están las que se basan en Key-Value (como Redis) y los que se basan en Document (como mongodb). Couchbase permite los dos tipos.

Las funcionalidades que ofrece y que me han llamado la atención:




  * Cluster de nodos donde los datos se reparten automáticamente en los nodos. Cuando añades un nuevo nodo (cosa que se hace sin tener que parar la base de datos y de forma muy sencilla), los datos se vuelven a repartir de forma automática sobre todos los nodos (rebalancing).




Lo mejor de esto es que el cliente tiene un mapa de los nodos existentes en tu cluster (llamado cluster map), lo que hace que independientemente del documento que estás obteniendo la latencia de red es la misma, ja que el cliente calcula a qué nodo del cluster hacer la petición.







  * Eventually persistence: Couchbase tiene montando una memcached antes de la persistencia en disco, así que cuando escribes un documento en la base de datos, **se escribe en cache y devuelve el control al cliente**. Esto significa que cuando el cliente recibe el ok de la escritura, puede ser que los datos no estén en disco. La persistencia se realiza de forma asíncrona, escribiendo en el disco del nodo y enviando el documento para actualizar las réplicas del documento en los diferentes nodos del cluster.




  * Failover : Cuando un nodo cae (ya sea por problemas de red, memoria, etc), al tener una copia de los documentos que este nodo tiene en las réplicas en los otros nodos del cluster, la gestión del failover es tan sencilla como activar esta réplicas (marcarlas como los documentos activos) y/o hacen un rebalance de los documentos en los nodos restantes del cluster.


Ahora toca la instalación y puesta en marcha del servidor.
