---
title: "Visual Studio Code"
date: "2018-03-15 12:00:00 +0100"
excerpt_separator: <!--more-->
header:
  teaser: 2018-03-15-visual-studio-code/code-teaser.png
  image: 2018-03-15-visual-studio-code/code.png

categories:
  - visual studio code
  - editores
  - ide

---

Posiblemente el editor de textos es uno de las aplicaciones más personales de cada desarrollador. Estamos hablando de una aplicación que nos permita a los desarrolladores editar rápida y eficazmente un texto o código sin que cargue mucho la máquina (que la RAM la tenemos reservada para los IDE's y compiladores varios).

En los últimos años he pasado por algunos editores, donde dos han tenido más relevancia: TextMate y [Atom](https://atom.io/). [Textmate](https://macromates.com) lo usaba para editar texto (no código) mientras que Atom lo tenía para textos, proyectos con git (esta web, por ejemplo) y código simple.

<!-- more -->

Últimamente he decidido probar [Visual Studio Code](https://code.visualstudio.com), el editor de Microsoft. Atom comía muchos recursos de la máquina y no acababa de estar cómodo, mientras que Textmate está desaparecido desde hace muchos años (la versión 2 nunca ha llegado a salir, la última beta és para macOS 10.8).

De filosofía similar a Atom, he encotrado Visual Studio Code más ágil y rápido. Además, las modificaciones sobre texto usando expresiones regulares (para lo que usaba Textmate) son igual de rápidas que en Textmate y más amigables. También la conversión de encodings (para lo que usaba Textmate también) és sencilla, aunque para archivos grandes - de centenares de Mbs - lo más rápido sigue siendo usar `iconv` en el Terminal.

# Extensiones a tener en cuenta

Además del soporte a diferentes lenguajes, las extensiones permiten ampliar las funcionalidades del editor. Durante este (poco) tiempo en el que llevo usando Visual Studio Code, hay un conjunto de extensiones que puedo recomendar. Como por ejemplo...

## Gitlens

El soporte de Visual Studio Code para Git es muy bueno. Permite realizar las operaciones habituales (`commit`, `pull`, `push`, `diff`, `log`, etc...) desde el mismo editor. [Gitlens](https://github.com/eamodio/vscode-gitlens) es una extensión que añade funcionalidades como:

- Buscar commits
- Ver el commit donde se modificó por última vez la línea de código (o texto) actual
- etc.

## GitLab Workflow

En mi trabajo usamos [GitLab](https://gitlab.com) como repositorio de Git. De hecho, lo usamos para mucho más, pero esto es otro tema. [GitLab Workflow](https://gitlab.com/fatihacet/gitlab-vscode-extension) permite gestionar, entre otras cosas:

- Ver el estado del pipeline en el propio Visual Studio Code
- Gestionar las Merge Request, issues, etc... relacionadas con el proyecto.
- Crear snippets, etc

## Docker

[Docker](https://github.com/microsoft/vscode-docker) permite arrancar containers desde el propio Visual Studio Code. Más útil, permite ver los logs de los containers que se están ejecutando. Todo sin salir de Visual Studio Code.

## Deploy

[Deploy](https://github.com/mkloubert/vs-deploy) permite deployar el proyecto a diferentes entornos de forma automática. Por ejemplo, para este site tengo definido un deployment que arranca el jekyll cuando guardo un documento markdown.

# Conclusión

Tengo que reconocer que ser un producto de Microsoft me tiraba para atrás. Pero después de un tiempo hay que decir que Visual Studio Code es una maravilla. Atom y Textmate ya están en la basura.