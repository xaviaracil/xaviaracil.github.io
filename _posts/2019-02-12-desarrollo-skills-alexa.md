---
title: "Desarrolo de skills para Alexa"
date: "2019-02-12 15:15:00 +0100"
excerpt_separator: <!--more-->
categories:
  - alexa
  - voice
  - aws
  - serverless
---

Alexa, si por un casual lleváis un par de años en alguna ermita solitaria, es el asistente de voz de Amazon. Este último año se ha puesto a la venta en España, añadiendo el castellano como lengua, por lo que ya se pueden desarrollar skills (o aplicaciones) para el público castellanohablante.

Vamos a ver una visión general de cómo desarrollar una skill.

<!-- more -->

Una Skill cuenta de 3 partes: la definición, el modelo, y el código. Como casi todo en Amazon Web Services, una skill se puede crear vía consola web o vía código + línea de comandos. En este caso, nos centramos en el segundo caso, ya que al tener código podemos tenerlo versionado y a buen recaudo en algún servidor git.

Así pues, necesitaremos la línea de comandos para crear y deployar una skill. Amazon nos proviene para ello con la [Alexa Skills Kit](https://developer.amazon.com/alexa-skills-kit) o ASK.  ASK os permite crear una skill a partir de una serie de plantillas y deployarla a nuestra cuenta de desarrollador de Alexa.

> Ojo, que si queremos desarrollar para Alexa necesitamos dos cuentas: una como desarollador Alexa para las skills y otra de AWS para el código de la skill, en forma de función Lambda

En [https://developer.amazon.com/alexa-skills-kit/tutorials/fact-skill-1](https://developer.amazon.com/alexa-skills-kit/tutorials/fact-skill-1) tenéis un tutorial básico de cómo crear una skill con ASK, empezando por cómo darse de alta en el portal de desarrolladores de Alexa. Os recomiendo que lo sigáis sólo para crear la cuenta de desarrollador en Alexa. Una vez dado de alta, os instaláis el ASK siguiendo las instrucciones de [https://developer.amazon.com/docs/smapi/quick-start-alexa-skills-kit-command-line-interface.html](https://developer.amazon.com/docs/smapi/quick-start-alexa-skills-kit-command-line-interface.html)

Una vez instalado y configurado, podéis crear vuestra primera skill a partir de una plantilla mediante `ask new`. Una vez seleccionado un par de parámetros (nombre, plantilla , lenguaje, etc) os montará la skill en un directorio.

[![Template Hello World](/images/2019-02-12-desarrollo-skills-alexa/hello-world.png)](/images/2019-02-12-desarrollo-skills-alexa/hello-world.png)

Allí podemos ver las tres partes de la skill, separadas en tres ficheros:

* skill.json
* index.js
* models/*.json

# Manifest de la skill (skill.json)

En este fichero puedes definir datos genéricos de la skill: nombre, descripción, frases de ejemplo, categoría, etc.

    {
      "manifest": {
        "publishingInformation": {
          "locales": {
            "en-US": {
              "summary": "Sample Short Description",
              "examplePhrases": [
                "Alexa open hello world",
                "hello",
                "help"
              ],
              "name": "desarrolloenmac",
              "description": "Sample Full Description"
            }
          },
          "isAvailableWorldwide": true,
          "testingInstructions": "Sample Testing Instructions.",
          "category": "KNOWLEDGE_AND_TRIVIA",
          "distributionCountries": []
        },
        "apis": {
          "custom": {
            "endpoint": {
              "sourceDir": "lambda/custom",
              "uri": "ask-custom-desarrolloenmac-default"
            }
          }
        },
        "manifestVersion": "1.0"
      }
    }

Aquí se definen también las apis que va a consumir la skill, normalmente funciones lambda.

> La referencia completa está en [https://developer.amazon.com/docs/smapi/skill-manifest.html](https://developer.amazon.com/docs/smapi/skill-manifest.html)

# Modelo

Dentro de la carpeta `models` deberéis definir el modelo de vuestra skill para cada idioma. Aquí es donde se definen los `intents`, las `utterances` y los `slots`. Pero vayamos paso a paso.

## Intents, Utterances y Slots

Podríamos definir los `intents` como las acciones que realiza una skill. Las frases que accionan los intents se llaman `utterances`.

Así, si queremos desarrollar una skill que nos cuente chistes, tendremos que definir un intent `ContarChiste`  con utterances como `cuéntame un chiste`,`hazme reír` o `entretenme payaso`.

Ahora, pensemos en ampliar nuestra skill de chistes para añadir categorías, como `cuéntame un chiste de abuelos`, etc. Esta categoría, una variable  que nuestro intent tendrá que procesar se llama `slot`.

## Definición del modelo

Ahora que ya tenemos los conceptos claros, es hora de definir nuestros intents en el modelo. Para cada locale soportado en nuestra skill, tendremos que definir los intents, dando utterances para que Alexa construya su modelo y definiendo slots.

Amazon nos proporciona una serie de intents y slots predefinidos (de hecho, en la skill de la captura anterior - creada a partir del template `HelloWorld`, podéis ver algunos).  En [https://developer.amazon.com/docs/custom-skills/standard-built-in-intents.html](https://developer.amazon.com/docs/custom-skills/standard-built-in-intents.html) tenéis el detalle de intents predefinidos.

> Más información sobre intents, utterances y slot en [https://developer.amazon.com/docs/custom-skills/create-intents-utterances-and-slots.html](https://developer.amazon.com/docs/custom-skills/create-intents-utterances-and-slots.html)

Sobre Slots hablaremos en próximos posts, cuando hablemos del `Dialog model`.

# Intent Handlers

El código de la función lambda tendrá nuestros `IntentHandlers`, trozos de código que se encargan de implementar los intents.
En realidad, un IntentHandler no es más que un objeto que debe implementar dos métodos:

* canHandle, que nos dice si el IntentHandler puede gestionar el intent.
* handle, que lo gestiona

Veamos un IntentHandler sencillo, de la plantilla `Hello World`:

    const LaunchRequestHandler = {
      canHandle(handlerInput) {
        return handlerInput.requestEnvelope.request.type === 'LaunchRequest';
      },
      handle(handlerInput) {
        const speechText = 'Welcome to the Alexa Skills Kit, you can say hello!';

        return handlerInput.responseBuilder
          .speak(speechText)
          .reprompt(speechText)
          .withSimpleCard('Hello World', speechText)
          .getResponse();
      },
    };

El método `handle` es que se encarga de realizar la acción del intent. La respuesta a enviar a Alexa consiste básicamente del texto que Alexa hablará, texto que se mostrará en aquellos dispositivos con pantalla. También se puede definir si se espera una respuesta, si se inicia un diálogo, etc. De todo esto hablaremos en detalle en próximas entradas.

Los IntentHandlers se deben registrar, de manera ordenada.

    exports.handler = skillBuilder
      .addRequestHandlers(
        LaunchRequestHandler,
        HelloWorldIntentHandler,
        HelpIntentHandler,
        CancelAndStopIntentHandler,
        SessionEndedRequestHandler
      )
      .addErrorHandlers(ErrorHandler)
      .lambda();

# Deployment

Una vez lo tenemos todo hay que deployar. Bastará con un `ask deploy` para que nuestra skill vaya a las máquinas de Amazon. Allí podremos testearla desde el navegador en la [Consola de Alexa](https://developer.amazon.com/alexa) o directamente en modo texto  vía `ask dialog --locale es-ES`

> OJO: ASK por defecto nos deploya la función lambda en la region de aws us-east-1 (Virginia). Si queremos que los usuarios de la península no tengan que esperar a que su petición cruce el charco tendremos que hacer algún truco para deployar la lambda también en alguna región europea (siendo Irlanda mi preferida)
> Visual Studio Code tiene una extensión para [ASK](https://github.com/alexa/ask-toolkit-for-vscode) totalmente recomendable. Permite el deploy desde el propio editor.

# Conclusiones y próximos pasos

Hemos visto que desarrollar una skill básica para Alexa es relativamente sencilla. En próximas entradas veremos skills más complejas, con diálogos para completar los slots, llamadas a apis externas, ya sean públicas o autorizadas mediante `oauth2`, etc.