---
date: 2011-03-30 07:35:34+00:00
slug: actualizar-rails-a-3-0-5
title: Actualizar Rails a 3.0.5
wordpress_id: 94
categories:
- Ruby On Rails
---

Snow Leopard viene con Rails 2.3.5 instalado, cuando la versión más reciente, a fecha de hoy, es la 3.0.5.



    $ rails -v
    Rails 2.3.5





Para actualizar la versión de Rails no basta con actualizar únicamente Rails, también hay que actualizar RubyGems, el sistema de gemas o packages de ruby.


<!-- more -->


## RubyGems




La actualización de RubyGems es tan sencilla como abrir un terminal y ejecutar




    sudo gem update --system




El sistema os pedirá la contraseña de un usuario administrador. El resultado tiene que ser algo como esto:




    Updating RubyGems
    Updating rubygems-update
    Successfully installed rubygems-update-1.6.2
    Updating RubyGems to 1.6.2
    Installing RubyGems 1.6.2
    RubyGems 1.6.2 installed

    === 1.6.2 / 2011-03-08

    Bug Fixes:

    * require of an activated gem could cause activation conflicts.  Fixes
      Bug #29056 by Dave Verwer.
    * `gem outdated` now works with up-to-date prerelease gems.

    ------------------------------------------------------------------------------

    RubyGems installed the following executables:
    	/System/Library/Frameworks/Ruby.framework/Versions/1.8/usr/bin/gem






## Rails



Una vez actualizado RubyGems, para actualizar Rails bastará con ejecutar, en el mismo terminal



    sudo gem install rails




Si hace mucho que habéis ejecutado la llamada anterior o estáis en otro terminal es posible que os vuelvan a pedir la contraseña de administrador. El resultado tiene que ser algo como esto:





    Fetching: activesupport-3.0.5.gem (100%)
    Fetching: i18n-0.5.0.gem (100%)
    Fetching: activemodel-3.0.5.gem (100%)
    Fetching: rack-1.2.2.gem (100%)
    Fetching: rack-test-0.5.7.gem (100%)
    Fetching: rack-mount-0.6.14.gem (100%)
    Fetching: tzinfo-0.3.25.gem (100%)
    Fetching: abstract-1.0.0.gem (100%)
    Fetching: erubis-2.6.6.gem (100%)
    Fetching: actionpack-3.0.5.gem (100%)
    Fetching: arel-2.0.9.gem (100%)
    Fetching: activerecord-3.0.5.gem (100%)
    Fetching: activeresource-3.0.5.gem (100%)
    Fetching: polyglot-0.3.1.gem (100%)
    Fetching: treetop-1.4.9.gem (100%)
    Fetching: mail-2.2.15.gem (100%)
    Fetching: actionmailer-3.0.5.gem (100%)
    Fetching: thor-0.14.6.gem (100%)
    Fetching: railties-3.0.5.gem (100%)
    Fetching: bundler-1.0.10.gem (100%)
    Fetching: rails-3.0.5.gem (100%)
    Successfully installed activesupport-3.0.5
    Successfully installed i18n-0.5.0
    Successfully installed activemodel-3.0.5
    Successfully installed rack-1.2.2
    Successfully installed rack-test-0.5.7
    Successfully installed rack-mount-0.6.14
    Successfully installed tzinfo-0.3.25
    Successfully installed abstract-1.0.0
    Successfully installed erubis-2.6.6
    Successfully installed actionpack-3.0.5
    Successfully installed arel-2.0.9
    Successfully installed activerecord-3.0.5
    Successfully installed activeresource-3.0.5
    Successfully installed polyglot-0.3.1
    Successfully installed treetop-1.4.9
    Successfully installed mail-2.2.15
    Successfully installed actionmailer-3.0.5
    Successfully installed thor-0.14.6
    Successfully installed railties-3.0.5
    Successfully installed bundler-1.0.10
    Successfully installed rails-3.0.5
    21 gems installed
    Installing ri documentation for activesupport-3.0.5...
    Installing ri documentation for i18n-0.5.0...
    Installing ri documentation for activemodel-3.0.5...
    Installing ri documentation for rack-1.2.2...
    Installing ri documentation for rack-test-0.5.7...
    Installing ri documentation for rack-mount-0.6.14...
    Installing ri documentation for tzinfo-0.3.25...
    Installing ri documentation for abstract-1.0.0...
    Installing ri documentation for erubis-2.6.6...
    Installing ri documentation for actionpack-3.0.5...
    Installing ri documentation for arel-2.0.9...
    Installing ri documentation for activerecord-3.0.5...
    Installing ri documentation for activeresource-3.0.5...
    Installing ri documentation for polyglot-0.3.1...
    Installing ri documentation for treetop-1.4.9...
    Installing ri documentation for mail-2.2.15...
    Installing ri documentation for actionmailer-3.0.5...
    Installing ri documentation for thor-0.14.6...
    Installing ri documentation for railties-3.0.5...
    Installing ri documentation for bundler-1.0.10...
    Installing ri documentation for rails-3.0.5...





Si todo ha ido bien, rails -v os mostrará la versión correcta.





    $ rails -v
    Rails 3.0.5
