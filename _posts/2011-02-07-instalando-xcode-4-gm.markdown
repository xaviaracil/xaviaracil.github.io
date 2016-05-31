---
author: desarrolloenmac
comments: true
date: 2011-02-07 15:57:17+00:00
layout: single
slug: instalando-xcode-4-gm
title: Instalando Xcode 4 GM
wordpress_id: 21
categories:
- XCode
---

Para aquellos que tengáis acceso, Xcode 4 GM es realmente un gran producto. Eso sí, si ya teníais instalada alguna Developer Preview, cuidado, ya que la instalación de Xcode 4 GM se realiza en **/Developer**, no en **/Xcode4**, como las DPs.

Si eres como yo, que se puso el Xcode 4 DP en el Dock, seguramente todavía estarás arrancando éste en lugar de la versión GM. Eso si, como hay algunas utilidades que se instalan en ubicaciones comunes, no habrás notado ningún problema en, por ejemplo, subir aplicaciones a la Mac AppStore.

Mi solución: Desinstalar Xcode 4 DP6, desinstalar XCode 4 GM y volver a instalar Xcode4, _por si acaso_

PD: Para desinstalar una versión de Xcode basta con ejecutar en un terminal


    sudo /<Xcode Home>/Library/uninstall-devtools --mode=all


Donde Xcode Home es la carpeta donde está instalado el Xcode.

PD2: Será una funcionalidad menor, pero me gusta mucho el poder asignar una tarjeta de la agenda a un usuario de SVN o Git y ver las caras de los que hacen el commit.
