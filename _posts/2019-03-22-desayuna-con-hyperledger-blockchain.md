---
title: "Desayuna con Hyperledger Blockchain"
date: "2019-03-20 12:30:00 +0100"
excerpt_separator: <!--more-->
categories:
  - blockchain
---

Este viernes 22 de marzo he asistido al webinar [Desayuna con Hyperledger Blockchain](https://event.on24.com/eventRegistration/EventLobbyServlet?target=reg20.jsp&referrer=&eventid=1940584&sessionid=1&key=7B60492ACCD3643B462BDC76677D32D0&regTag=&sourcepage=register), presentado por IBM, y centrado en Blockchain y las soluciones proporcionadas por IBM para esta tecnología.

Hay que agradecer, antes de nada, que IBM me enviara un desayuno para degustar durante el webinar. Así da gusto.

[![Desayuno](/images/2019-03-22-desayuna-con-hyperledger-blockchain/IMG_2006.jpg)](/images/2019-03-22-desayuna-con-hyperledger-blockchain/IMG_2006.jpg)

Una vez saciada nuestra hambre, vamos con las notas del webinar. Si no sabes de que va blockchain, guarda esta entrada para más adelante. Si me animo, ya habrán otras entradas con lo que voy aprendiendo sobre esta tecnología.

La sesión, data por [Irene Gallego](https://twitter.com/IreneGallegoB) y [Ana López](https://twitter.com/AnaMancisidor) se centró en una introducción inicial corta y una demo handson de como crear una red blockchain en IBM Cloud e instanciar un  Smart Contract.

<!--more-->

[![Pantalla principal](/images/2019-03-22-desayuna-con-hyperledger-blockchain/webinar.png)](/images/2019-03-22-desayuna-con-hyperledger-blockchain/webinar.png)

# ¿Por dónde empezamos? Conociendo a IBM Cloud y Blockchain (Hyperledger)

Ana empieza con una introducción rápida a blockchain y a sus elementos principales. Básicamente, el objetivo principal de una red blockchain es la **confianza** en toda la red. Los elementos principales de una red blockchain son, básicamente:

- Ledger: Fuente de verdad
- Peer: Nodos que guardan una copia del ledger
- Smart Contract: las reglas de negocio y transaciones que se dan en una plataforma blockchain
- Consensus: ratificación de transacciones

> Ya ampliaremos esto en próximas entradas

Continua explicando la estrategia de IBM Cloud para blockchain, que se basa en redes privadas (objetivo de la sesión de hoy)

Las soluciones se basan en [Hyperledger](https://www.hyperledger.org), una comunidad Open Source sobre blockchain, albergado en la Linux Foundation. Entre sus diferentes proyectos está [Hyperledger Fabric](https://www.hyperledger.org/projects/fabric), una implementación de blockchain que proporciona un framework para el desarrollo de soluciones. Sobre esta implementación se ha basada IBM Cloud para ofrecer sus servicios blockchain (IBM Blockchain Platform).

Esta plataforma consta de 3 principios

- plataforma abierta (Hyperledger)
- plataforma híbrida
- plataforma escalable para producción (gobierno, sla, etc)

[Hyperledger Fabric](https://www.hyperledger.org/projects/fabric) tiene las siguientes características principales:

- Red provisionada y privada
- Identidad de los participantes acreditada
- Arquitectura modular
- Protocolo consenso y endorsement selectivo
- Confidencialidad
- Performance y escalabilidad

# Trabajando con Peers, Smart Contracts, Ledger y Canales en Hyperledger

En este apartado Ana nos da la terminología básica de una red blockchain. Así, tenemos:

- Peer: Mantienen una copia del ledger y ejecutan los smart contacts
- Ledger: Copia distribuida en cada peer. Fuente de verdad.
- Smart Contract: lógica de negocio, transacciones.
- Ordering: Ordena y valida las transacciones. Distribuye las actualizaciones en el ledger.
- CA: Entidad certificadora, administra identidades
- Canal: un ledger por cada canal

Los Smart contracts se ejecutan mediante la SDK. Un ledger tiene una copia de la cadena y un `world state` para saber el estado global y no tener que recorrer la cadena siempre.

> Para más info podéis consultar [Blockchain basics: Hyperledger Fabric](https://developer.ibm.com/articles/cl-blockchain-hyperledger-fabric-hyperledger-composer-compared/)

## Despliege

Hay varias maneras de desplegar una red blockchain de Fabric: manualmente via docker, como servicio (IBM Cloud), o como saas

## Componentes para el desarrollo y despliege de una red blockchain con Fabric

IBM Cloud proporciona dos componentes, la [IBM blockchain platform extensions para VS code](https://marketplace.visualstudio.com/items?itemName=IBMBlockchain.ibm-blockchain-platform) para desarrollo y test de smart contracts y el servicio IBM Blockchain Platform v2 de IBM Cloud para gobierno y operación de la red.

El servicio IBM Blockchain v2 tiene una interfaz de usuario para la gestión, similar a la del resto de servicios de IBM Cloud.

# Demo: Cómo crear y desplegar un Smart Contract en IBM Blockchain

Empezamos con la demo: Ana configura un red de Hyperledger Fabric, desarrolla un smart contract, lo instala en la red, lo instancia y lo invoca.

La demo se realiza sobre el VS Code y la consola web de IBM Blockchain Platform v2.

## Configuración de la red

Nos explica como crear una red mediante el servicio IBM Blockchain Platform v2. La creación de la red es sencillo: le das un nombre, le asignas una region donde desplegarlo, un grupo de recursos para agrupar lógicamente dentro de tu organización, tags para afinar esta agrupación lógica, y aceptar. Te aparece un wizard de configuración para acabar de definir el cluster de kubernetes donde desplegar la red, con opción para crear uno (incluso para evaluación, gratuito y que expira a los 30 días). Un click más y ya tenemos la red desplegada y aceso a la consola web para administrarla.

También os da acceso a la consola de kubernetes para una gestión del cluster.

La consola web te da una guía para crear la red: te ofrece opciones para crear, en este orden: CAs, organizaciones, nodos, orderers, canales. Detallando:

- Un CA - o Membership Service Provider (MSP) -para cada organización y otra para los orderers. Cada CA define usuarios autorizados para realizar las transaciones

- Un Peer para cada organización (como mínimo)

- Un servicio de ordering para añadir organizaciones al consorcio.

Ya podemos crear canales: Orderer, MSP, Identidad y operador, acls

Una vez creado el canal, ya podemos añadir peers.

> Próximamente habrá una api REST para la configuración de la red fuera de la consola web.

## Desarrollo del Smart Contract

Ya podemos desarrollar el smart contract (via VS Code y extensión). También se basa en wizards para desarrollar Smart Contracts. El wizard te genera un proyecto VS Code para desarrollar el smart contract.

Ana codifica un contrato de ejemplo, implementando dos métodos (instanciación y transación).

## Pruebas locales

La extensión de VS Code puede desplegar una red local para realizar pruebas. Así no tienes que ir empaquetando e instalando el smart contract en tu red.

## Empaquetado e instalación

La extensión de VS Code también permite empaquetar el smart contract para desplegarlo en la red que tenemos en el cloud. El empaquetado genera un fichero `cds` en tu . La instalación en nuestra red es tan sencilla como ir a la consola web de la red en IBM Cloud e importar este fichero `cds`.

> Próximamente habrá una api REST para la importación del smart contract fuera de la consola web.

## instanciación e invocación

La instalación no lo ejecuta. Se tiene que instanciar en un canal. Este proceso también se realiza mediante la consola. Para instanciarlo hay que definir una *Política de endorsement* (el nombre de la función de instanciación en nuestro smart contract). La instanciación se realiza en todos los peers del canal.

Una vez instanciado es turno de la invocación del contract desde una aplicación externa. La extensión de VS Code permite realizar esta invocación. Para ello, hay que realizar una serie de pasos:

- Tienes que bajarte el `connection profile` de la consola de IBM.
- Tienes que bajarte las claves pública y privada de un usuario autorizado. Para ello, Ana registra un nuevo usuario para la aplicación externa mediante la entidad certificadora (tan sencillo como indicar nombre, contraseña y  organización a la que pertenece). Después, realiza un `enroll` para obtener una clave pública y privada, que exportamos para añadirlo al Wallet (también desde la consola).

Una vez tenemos las claves, las exportamos en formato pem para importarlas en nuestro VS Code.

### Invocación

La extensión de VS Code nos permite conectarnos a la nuestra red. La conexión se realiza desde `Fabric Gateways`, donde hay que proporcionar el `connection profile` que hemos descargado anteriormente. Además, tenemos que crear un wallet con los pems del usuario autorizado que hemos exportado anteriormente.

Ya tenemos VS Code conectado a nuestra red en IBM Cloud como aplicación cliente. Nos aparece los canales y los smarts contracts instanciados en cada canal. Desde aquí podemos invocar (`submit`) la transacción del smart contract.

## Comprobación

Para comprovar que la transacción se ha realizado correctamos, tenemos que acceder al canal desde la consola web de nuestra red para ver que hay un nuevo bloque. Los detalles del bloque nos muestran que se trata de la transación que hemos invocado.

# Conclusiones

Muy buen webminar para aquellos que ya saben algo de blockchain. Demo sencilla pero completa que muestra como es el flujo para crear una red, desarrollar un smart contract, desplegarlo en la red e invocarlo desde el propio entorno de desarrollo del smart contract.

Los amigos de IBM nos han prometido cuentas de prueba para acceder a los clústeres efímeros de Kubernetes. A ver si puedo jugar con ellos y explicaros cómo van las pruebas.