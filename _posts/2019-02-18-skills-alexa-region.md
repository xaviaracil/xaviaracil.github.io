---
title: "¿Cómo deployar tu skill en Europa?"
date: "2019-02-18 18:15:00 +0100"
excerpt_separator: <!--more-->
categories:
  - alexa
  - voice
  - aws
  - serverless
---

Ya tienes tu skill (o una incipiente versión de ella) que has deployado usando [ASK](https://developer.amazon.com/alexa-skills-kit). Como eres buen desarrollador y piensas en tus usuarios, quieres que tu skill sea lo más rápida posible. Como, además, eres curioso, miras con detalle qué clase de magia negra ha realizado tu `ask deploy` y compruebas, con horror, que ha deployado tu skill en la region `us-east-1`. ¡Pobres usuarios españoles! Su petición tiene que cruzar el charco! ¡Y la respuesta también! Veamos cómo podemos arreglarlo.

<!--more-->

A día de hoy, ask deploya en `us-east-1` por narices. Es más, los típicos parámetros de otras herramientas CLI de Amazon para especificar la region con `ASK` no funcionan. Pero no desesperéis, deployar nuestra skill en otras regiones que no Virginia es más fácil de lo que parece. Para ello, necesitamos modificar el fichero `skill.json` de nuestra skill.

> A día de hoy, las funciones lambda para nuestra skill se pueden deployar en las regiones de Tokio, Irlanda, Virginia y Oregon. Más info en [https://developer.amazon.com/docs/custom-skills/host-a-custom-skill-as-an-aws-lambda-function.html#about-lambda-functions-and-custom-skills](https://developer.amazon.com/docs/custom-skills/host-a-custom-skill-as-an-aws-lambda-function.html#about-lambda-functions-and-custom-skills)

# skill.json una vez deployada

Si os fijáis en el fichero `skill.json` una vez hayáis deployado la skill, veréis que es diferente respecto a la versión que teníais antes. Mientras que inicialmente tenemos

```
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
```

Cuando deployamos la skill tenemos

```
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
```

Nótese la aparición de un atributo `apis` en el json. En este campo se definen los servicios que va a consumir nuestra skill, en nuestro caso, la función lambda.

```
    "apis": {
      "custom": {
        "endpoint": {
          "sourceDir": "lambda/custom",
          "uri": "ask-custom-desarrolloenmac-default"
        }
      }
    }
```

Será en este atributo donde realizaremos las modificaciones.

# Modificaciones a skill.json

En [https://developer.amazon.com/docs/custom-skills/host-a-custom-skill-as-an-aws-lambda-function.html#about-lambda-functions-and-custom-skills](https://developer.amazon.com/docs/custom-skills/host-a-custom-skill-as-an-aws-lambda-function.html#about-lambda-functions-and-custom-skills) hemos visto que si queremos deployar la lambda en Europa, no queda otra que hacerlo en Irlanda.  Así pues, vamos a indicar a la skill que queremos que también se ejecute en Irlanda.

Para ello, bastará con modificar el atributo `apis` de nuestro `skill.json`, añadiendo la region `EU`:

```
    "apis": {
      "custom": {
        "endpoint": {
          "sourceDir": "lambda/custom",
          "uri": "ask-custom-desarrolloenmac-default"
        },
        "regions": {
          "EU": {
            "endpoint": {
              "sourceDir": "lambda/custom",
              "uri": "ask-custom-desarrolloenmac-default"
            }
          }
        }
      }
    }
```

Fijaos que la `uri` de la función lambda es la misma que ya teníais.

> El schema de `skill.json` lo tenéis disponible en [https://developer.amazon.com/docs/smapi/skill-manifest.html](https://developer.amazon.com/docs/smapi/skill-manifest.html). Allí, podréis ver ,  en [regions](https://developer.amazon.com/docs/smapi/skill-manifest.html#regions), que el valor para Europa es `EU`

Así, el fichero `skill.json` modificado queda de la siguiente manera:

```
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
        },
        "regions": {
          "EU": {
            "endpoint": {
              "sourceDir": "lambda/custom",
              "uri": "ask-custom-desarrolloenmac-default"
            }
          }
        }
      }
    },
    "manifestVersion": "1.0"
  }
}
```

Si deployáis ahora, podréis comprobar que se crean dos funciones lambda, una en casa región: `us-east-1` y `eu-west-1`. En mi caso `arn:aws:lambda:eu-west-1:613409542017:function:ask-custom-desarrolloenmac-default` y `arn:aws:lambda:us-east-1:613409542017:function:ask-custom-desarrolloenmac-default`

```
[xavi@mbp-xavi desarrolloenmac (master)]$ ask deploy --profile "default" --target "all"
Profile for the deployment: [default]
-------------------- Update Skill Project --------------------
Skill Id: amzn1.ask.skill.4c6a5528-55db-40d0-aa05-0d8b7bcb5b74
Skill deployment finished.
Model deployment finished.
[Warn]: No runtime and handler settings found for alexaUsage "custom/EU" when creating Lambda function. CLI will use "nodejs8.10" and "index.handler" as the Runtime and Handler to create Lambda. You can update the runtime and handler for the target Lambda in the project config and deploy again if you want to set differently.
Lambda deployment finished.
Lambda function(s) created:
  [Lambda ARN] arn:aws:lambda:eu-west-1:613409542017:function:ask-custom-desarrolloenmac-default
Lambda function(s) updated:
  [Lambda ARN] arn:aws:lambda:us-east-1:613409542017:function:ask-custom-desarrolloenmac-default
[Info]: No in-skill product to be deployed.
Your skill is now deployed and enabled in the development stage. Try simulate your Alexa skill skill using "ask dialog" command.
```

# Recap

Mientras esperamos que los desarrolladores de Amazon añaden el soporte multiregion a `ASK`, tendremos que realizar este pequeño ajuste manual para disponer de nuestras skills (es decir, las funciones lambda que las gestionan) en otras regiones fuera de `us-east-1`. 

Todo lo que sea ajuste manual no gusta a amantes de la automatización como nosotros, aunque este ajuste sea puntual y no muy costoso. Al menos, en este caso hemos tenido suerte de *gastar* un par de minutos en realizarlo.
