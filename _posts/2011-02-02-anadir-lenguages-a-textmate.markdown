---
author: desarrolloenmac
comments: true
date: 2011-02-02 18:00:46+00:00
layout: single
slug: anadir-lenguages-a-textmate
title: Añadir lenguages a TextMate
wordpress_id: 201
categories:
- Textmate
---

[Textmate](http://macromates.com/) es un gran editor de texto para desarrollar, sobre todo para lenguages como Ruby on Rails, PHP, Python, etc.

Aunque tiene soporte para gran cantidad de bundles (lenguajes y aplicaciones relacionadas con el desarrollo, como SVN, FTP), sólo los más comunes vienen con la distribución. Para añadir un nuevo bundle (como, por ejemplo, el soporte al lenguaje Pascal) basta con tener instalado el SVN (me parece que se instala automáticamente con el XCode) y hacer lo siguiente en una ventana del terminal:



    	mkdir -p /Library/Application Support/TextMate/Bundles
    	cd /Library/Application Support/TextMate/Bundles
    	svn co http://svn.textmate.org/trunk/Bundles/Pascal.tmbundle






[![Installing pascal bundle](/images/2011-02-02-anadir-lenguages-a-textmate/installing_pascal_bundle_for_textmate.jpg)](/images/2011-02-02-anadir-lenguages-a-textmate/installing_pascal_bundle_for_textmate.jpg)En [http://svn.textmate.org/trunk/Bundles](http://svn.textmate.org/trunk/Bundles) tenéis la lista de bundles disponibles.

Fuente: [http://manual.macromates.com/en/bundles#getting_more_bundles](http://manual.macromates.com/en/bundles#getting_more_bundles)
