---
title: "Dando contexto a nuestra skill - Slots"
date: "2019-03-14 14:13:00 +0100"
excerpt_separator: <!--more-->
categories:
  - alexa
  - voice
  - aws
  - serverless
---

Siguiendo con esta miniserie de artículo sobre Alexa, llegamos al apartado sobre `Slots`. De artículos anteriores sabemos que una slot permite dar información adicional a un `intent` (lo podríamos llamar variables del `intent`).

A continuación intentaré explicar cómo se definen estos intent, cómo se procesan, y cómo se van pasando entre Alexa y nuestra Skill hasta que el `intent` se haya completado.

<!--more-->

# Definición

Las `slots` se definen en el fichero de definición del modelo.  Allí es donde tenemos que indicar su nombre, el tipo, ejemplos de `utterances`, etc.

Para indicar que un intent tiene una slot, tendréis que definir la slot  en el campo `slots` del `intent`. Así, si tengo un intent llamado `GetMenuIntent` con dos slots llamadas `menuDate` y `dish`, tendría que definirlo tal que así:

    {
      "interactionModel": {
        "languageModel": {
          ...
          "intents": [
            {
              "name": "GetMenuIntent",
              "slots": [
                {
                  "name": "menuDate",
                  "type": "AMAZON.DATE"
                },
                {
                  "name": "dish",
                  "type": "MENU_DISH"
                }
              ],
              "samples": [
                "que hay de menú {menuDate}",
                "que hay de comer {menuDate}",
                "que hay de {dish} el {menuDate}",
                "que hay {dish} {menuDate}",
                "que hay {dish} el {menuDate}",
                "que hay {menuDate}",
                "qué hay de menú el {menuDate}",
                "qué hay de menú {menuDate}",
                ...
              ]
            }
        ...
    }

Por suerte, Amazon nos proporciona un [conjunto de tipos ya predefinidos](https://developer.amazon.com/docs/custom-skills/slot-type-reference.html)para nuestra slot. Así, si lo que queremos es una fecha, un lugar, etc, no hace falta que lo definamos nosotros desde cero.  En el ejemplo anterior, `menuDate` es una slot de tipo predefinido fecha, mientras que `dish` es de tipo propio `MENU_DISH`.

> Podéis encontrar más información en [https://developer.amazon.com/docs/custom-skills/create-intents-utterances-and-slots.html](https://developer.amazon.com/docs/custom-skills/create-intents-utterances-and-slots.html)

## Tipos propios

Si no tenemos suerte y nuestra slot es de un tipo no soportado por Amazon, nos tocará definirlo.  Para ello necesitamos identificar los posibles valores y sus sinónimos. Esto se realiza en el campo `types` del modelo. 

Así, si seguimos con el ejemplo anterior, tenemos tipo propio llamado `MENU_DISH` donde queremos definir tipos de platos en un menú (básicamente primero, segundo o principal, y postre). La definición se basa, pues, en identificar los posibles valores, asignándoles un identificador, y añadiendo posibles sinónimos. En nuestro caso:

    {
      "interactionModel": {
        "languageModel": {
          ...
          "types": [
            {
              "name": "MENU_DISH",
              "values": [
                {
                  "id": "first",
                  "name": {
                    "value": "primer plato",
                    "synonyms": [
                      "de primero",
                      "para empezar"
                    ]
                  }
                },
                {
                  "id": "main",
                  "name": {
                    "value": "plato principal",
                    "synonyms": [
                      "de segundo",
                      "principal",
                      "para continuar",
                      "para seguir"
                    ]
                  }
                },
                {
                  "id": "dessert",
                  "name": {
                    "value": "postre",
                    "synonyms": [
                      "de postre",
                      "para acabar"
                    ]
                  }
                }
              ]
            }
          ]
        },
    ...
    }

# Proceso

Alexa intenta, a partir del modelo de nuestra skill, resolver automáticamente las slots, pasándolas a nuestro intent como atributos de la `request`.  Obtener estos valores puede ensuciar el código, así que gracias a [Germán Viscuso](https://twitter.com/germanviscuso) por darnos el siguiente [método](https://github.com/germanviscuso/skill-sample-nodejs-dogmatch-spanish/blob/master/lambda/custom/index.js) para generar un diccionario con los valores de cada slot de nuestro intent:

    function getSlotValues(filledSlots) {
      const slotValues = {};

      //console.log(`The filled slots: ${JSON.stringify(filledSlots)}`);
      Object.keys(filledSlots).forEach((item) => {
        const name = filledSlots[item].name;

        if (filledSlots[item] &&
          filledSlots[item].resolutions &&
          filledSlots[item].resolutions.resolutionsPerAuthority[0] &&
          filledSlots[item].resolutions.resolutionsPerAuthority[0].status &&
          filledSlots[item].resolutions.resolutionsPerAuthority[0].status.code) {
          console.log('Could find ER: ' + filledSlots[item].name);
          switch (filledSlots[item].resolutions.resolutionsPerAuthority[0].status.code) {
            case 'ER_SUCCESS_MATCH':
              slotValues[name] = {
                synonym: filledSlots[item].value,
                resolved: filledSlots[item].resolutions.resolutionsPerAuthority[0].values[0].value.name,
                id: filledSlots[item].resolutions.resolutionsPerAuthority[0].values[0].value.id,
                isValidated: true,
              };
              break;
            case 'ER_SUCCESS_NO_MATCH':
              slotValues[name] = {
                synonym: filledSlots[item].value,
                resolved: filledSlots[item].value,
                isValidated: false,
                id: null
              };
              break;
            default:
              break;
          }
        } else {
          console.log('Could NOT find ER: ' + filledSlots[item].name);
          slotValues[name] = {
            synonym: filledSlots[item].value,
            resolved: filledSlots[item].value,
            isValidated: false,
            id: null
          };
        }
      }, this);
      return slotValues;
    }

Con esta `function`,  obtener los valores de cada slot es tan sencillo como, dentro del `handler`:

    const GetMenuIntentHandler = {
      handle(handlerInput) {
        const filledSlots = handlerInput.requestEnvelope.request.intent.slots;

        const slotValues = getSlotValues(filledSlots);
        const date = moment(slotValues.menuDate.resolved, "YYYY-MM-DD", true)
    ....
      }
    }

> Fijaos que en `filledSlots` nos proporciona en `id` el identificador del tipo, si la slot es de un tipo propio. Así, en nuestro ejemplo, `slotValues.dish.id` tendríamos `first`, `main` o `dessert`, en función de lo que haya resuelto alexa.

No obstante, puede que el usuario no haya indicado un valor para la slot, o que ésta tenga que confirmarse. Así, entramos en lo Amazon llama [Dialog Model](https://developer.amazon.com/docs/custom-skills/define-the-dialog-to-collect-and-confirm-required-information.html), quizá una de las partes más complicadas del desarrollo de una skill.

## Volvemos a la configuración

El `Dialog Model`  también requiere de una configuración que también se realiza en el fichero de definición del modelo.  Allí tenemos que definir, para cada slot, características como, por ejemplo, si es obligatoria (o `elicit`) y hasta que no esté resulta no se puede finalizar el diálogo, si requiere de confirmación por nuestra parte o no (`confirmationRequired` ), ejemplos de preguntas para mostrar al usuario. 

Siguiendo con el ejemplo, supongamos que en nuestro intent `menuDate` es obligatoria, mientras que `dish` no lo es. Para mejorar la skill, supongamos también que definimos tres preguntas para obtener la fecha (Alexa escogerá una para mostrar al usuario).  Nuestra configuración del diálogo sería:

    {
      "interactionModel": {
        ...
        "dialog": {
          "delegationStrategy": "SKILL_RESPONSE",
          "intents": [
            {
              "name": "GetMenuIntent",
              "confirmationRequired": false,
              "prompts": {},
              "slots": [
                {
                  "name": "menuDate",
                  "type": "AMAZON.DATE",
                  "elicitationRequired": true,
                  "confirmationRequired": false,
                  "prompts": {
                    "elicitation": "Elicit.Intent-GetMenuIntent.IntentSlot-menuDate"
                  }
                },
                {
                  "name": "dish",
                  "type": "MENU_DISH",
                  "elicitationRequired": false,
                  "confirmationRequired": false
                }
              ]
            }
          ]
        },
        "prompts": [
          {
            "id": "Elicit.Intent-GetMenuIntent.IntentSlot-menuDate",
            "variations": [
              {
                "type": "PlainText",
                "value": "Dime el día que quieres consultar, por favor"
              },
              {
                "type": "PlainText",
                "value": "De qué día quieres saberlo?"
              },
              {
                "type": "PlainText",
                "value": "Tengo el plato ({dish}), pero ¿De qué día quieres saberlo?"
              }
            ]
          }
        ]
      }
    }

# Inicio del diálogo

Mientras Alexa no acaba de completar y resolver las slots, marcará el intent como no completado. Para continuar con nuestro diálogo, tendremos que añadir un nuevo intent que, básicamente, continúe con el diálogo.

Siguiendo nuestro ejemplo:

    const InProgressGetMenuIntentHandler = {

      canHandle(handlerInput) {
        return handlerInput.requestEnvelope.request.type === 'IntentRequest'
          && handlerInput.requestEnvelope.request.intent.name === 'GetMenuIntent'
          && handlerInput.requestEnvelope.request.dialogState !== 'COMPLETED';
      },

      handle(handlerInput) {
        const currentIntent = handlerInput.requestEnvelope.request.intent;

        return handlerInput.responseBuilder
          .addDelegateDirective(currentIntent)
          .getResponse();
      }
    }

> No os olvidéis de registrar este nuevo intent *antes* que el intent relativo al diálogo completo.

    exports.handler = skillBuilder
      .addRequestHandlers(
        LaunchRequestHandler,
        InProgressGetMenuIntentHandler,
        GetMenuIntentHandler
      )
      .addRequestInterceptors(DialogManagementStateRequestInterceptor)
      .lambda();

# Manteniendo parámetros

Si tenemos más de una slot y las estamos completando/confirmando una a una, Alexa únicamente nos envía información sobre la slot en cuestión. Para mantener los valores de las slots anteriores, tendremos que guardarlas en la sesión.

Para ello,  debemos implementar un `Interceptor` que se encargue de ello, también gracias a [Germán](https://github.com/germanviscuso/skill-sample-nodejs-dogmatch-spanish/blob/master/lambda/custom/index.js):

    const DialogManagementStateRequestInterceptor = {

      process(handlerInput) {

        const currentIntent = handlerInput.requestEnvelope.request.intent;

        console.log('Interceptor got Intent: ' + JSON.stringify(currentIntent));

        console.log('Dialog state: ' + handlerInput.requestEnvelope.request.dialogState);

        if (handlerInput.requestEnvelope.request.type === "IntentRequest"
          && handlerInput.requestEnvelope.request.dialogState !== "COMPLETED") {
          console.log('Interceptor detected dialog');
          const attributesManager = handlerInput.attributesManager;
          const sessionAttributes = attributesManager.getSessionAttributes();

          // If there are no session attributes we've never entered dialog management
          // for this intent before.
          if (sessionAttributes[currentIntent.name]) {
            console.log('Interceptor found stored Intent');
            let savedSlots = sessionAttributes[currentIntent.name].slots;
            console.log('Stored slots are: ' + JSON.stringify(savedSlots))
            for (let key in savedSlots) {
              // we override with a saved slot value only if the current intent does not
              // have it and it's present in the saved slots
              console.log('Interceptor processing slot: ' + key);
              if (!currentIntent.slots[key].value && savedSlots[key].value) {
                console.log('Replacing: ' + JSON.stringify(currentIntent.slots[key]));
                console.log('...with: ' + JSON.stringify(savedSlots[key]));
                currentIntent.slots[key] = savedSlots[key];
              }
            }
          }
          console.log('Saving for next round: ' + JSON.stringify(currentIntent));
          sessionAttributes[currentIntent.name] = currentIntent;
          attributesManager.setSessionAttributes(sessionAttributes);
        }
      }
    };

> No olvidéis registrar en interceptor. En el trozo de cogido anterior, donde se registraba el handler, está la llamada para registrar también el interceptor.

# Finalizando el diálogo

Cuando todas las slots están completadas, Alexa nos envía el diálogo como completado.

Es el momento de hacer nuestro negocio. No os olvidéis de limpiar la sesión, finalizando el diálogo, añadiendo `.withShouldEndSession(true)` cuando construyáis la respuesta, tal que así:

    const response = handlerInput.responseBuilder
                .speak(speechText)
                .withSimpleCard(`Menú Joan Coret para ${date.format('L')}`, menuText)
                .withShouldEndSession(true)
                .getResponse();

# Recapitulando

Probablemente, los diálogos son lo más complejo con lo que me he encontrado en este corto viaje desarrollando skills para Alexa. Además, la documentación puede llegar a ser un poco confusa.

Pero gracias a estas funciones auxiliares, que nos permiten obtener y mantener los valores de las slots durante todo el diálogo, y a *pegarte* haciendo un tipo propio, se puede llegar a mejorar tu skill significativamente.