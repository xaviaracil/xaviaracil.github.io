---
author: desarrolloenmac
comments: true
date: 2011-02-28 15:27:26+00:00
layout: single
slug: subversion-como-ignorar-ficheros-de-forma-global
title: 'Subversion: cómo ignorar ficheros de forma global'
wordpress_id: 76
categories:
- Java
- Subversion
tags:
- Eclipse
- Java
---

Tengo una cantidad de proyectos que usan [Subversion](http://subversion.apache.org/) para el control de versiones, la mayoría proyectos en Java con [Maven](http://maven.apache.org) y [Eclipse](http://www.eclipse.org). Al utilizar estos sistemas, me encuentro que tienen una serie de ficheros comunes que no quiero que esten bajo Subversion, a saber:




  * .project: Fichero project de Eclipse


  * .classpath: Fichero classpath de Eclipse


  * .settings: Fichero settings de Eclipse


  * target: Carpeta con los resultados de la compilación en Maven







Para no tener que ejecutar svn propset svn:ignore con cada uno de los ficheros lo mejor es hacer que el Subversion los ignore globalmente. Para ello, basta con editar el fichero ∼/.subversion/config y añadir la línea:





    	global-ignores = .classpath .project .settings target .DS_Store





En concreto, además de ignorar los ficheros mencionados anteriormente, también he añadido el fichero .DS_Store. La única pega es que no podrás tener ningun fichero llamado target en el Subversion.
