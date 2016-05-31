---
author: desarrolloenmac
comments: true
date: 2011-06-08 18:37:11+00:00
layout: single
slug: obtener-datos-json
title: Obtener datos JSON
wordpress_id: 130
categories:
- Cocoa
- JSON
- Objective-C
---

El formato JSON (JavaScript Object Notation) es muy común en servicios web ligeros. Cocoa contiene una serie de clases para la conectividad con internet (sobre todo en Foundation), pero no tiene soporte (aún) para JSON.

Para rellenar este hueco existe [SBJson](https://github.com/stig/json-framework), antes conocida como json-framework. Es una framework muy fácil de usar, basta con bajar el código, añadir todos los ficheros de la carpeta Classes en nuestro proyecto XCode e importar el fichero "SBJson.h".

Este framework contiene una categoría de NSString con el método - (id)JSONValue; (si no sabes qué es una categoría, echa un vistazo a la entrada [Usando categorías para evitar duplicar código]({% post_url 2011-02-21-usando-categorias-para-evitar-duplicar-codigo %})). Así, todos los objetos NSString responden al mensaje - (id)JSONValue, que parsea el texto como JSON y lo transforma a un objeto Cocoa.

Como ejemplo, vamos a utilizar la [search API](http://dev.twitter.com/doc/get/search) de Twitter, que nos proporciona una serie de URL's que devuelven objetos en formato JSON. El siguiente código obtiene la representación JSON de los tweets que contienen el texto que hay en un UITextField llamado textField.



        // cargar tweets de la twitter search API (http://dev.twitter.com/doc/get/search)    
        NSString *queryURL = [NSString stringWithFormat:@"http://search.twitter.com/search.json?q=%@", textField.text];
        NSURL *url = [NSURL URLWithString:queryURL];

        NSString *jsonString = [NSString stringWithContentsOfURL:url encoding:NSUTF8StringEncoding error:nil];

        // Parseo JSON
        id object = [jsonString JSONValue];




- (id)JSONValue devuelve o un NSDictionary o un NSArray, dependiendo de si el objeto JSON representa un único objeto o un array de objetos. Así, con el object del código anterior haremos lo siguiente:



        if ([object isKindOfClass:[NSDictionary class]]) {
            NSDictionary *objectAsDictionary = (NSDictionary *) object;
            id results = [objectAsDictionary valueForKey:@"results"];
            if ([results isKindOfClass:[NSArray class]]) {
                self.timelineArray = results;
            }        
        }
        if ([object isKindOfClass:[NSArray class]]) {
            self.timelineArray = object;
        }



Lo que hace el código anterior es asignar a la property NSArray *timelineArray el array de tweets, para mostrarlos posteriormente. Pero eso será en el siguiente post.
