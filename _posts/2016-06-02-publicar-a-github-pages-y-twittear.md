---
title: 'Publicar a GitHub Pages y twittear de forma automática'
header:
  teaser: 2016-06-02-publicar-a-github-pages-y-twittear/pre-push-150x150.png
  image: 2016-06-02-publicar-a-github-pages-y-twittear/pre-push.png
excerpt_separator: <!--more-->
categories:
- jekyll
- github-pages
- git
- rakefile
---

{% include toc %}
El blog está ahora alojado en [GitHub Pages](https://pages.github.com) usando [jekyll](https://jekyllrb.com) como generador del blog. Más adelante hablaré sobre las ventajas de esta solución sobre la anterior (Wordpress hospedado en un servidor), pero ahora toca hablar de cómo notificar vía Twitter que hay un nuevo post en el blog.

Ésto que con Wordpress es tan sencillo como instalar un plugin, con jekyll + GitHub Pages la cosa se complica un poco.
<!--more-->

Mi solución ha sido aprovechar un hook de git para obtener la URL de los nuevos posts y publicarlos de forma automática usando rake y [Twitter CLI](http://sferik.github.io/t/). Os la comento:

# Git commits
La clave en la solución es realizar commits con el mensaje `New post` cuando dentro del commit se encuentre el nuevo post. Esta cadena es la que buscaremos posteriormente para obtener las URL de los posts y notificar a Twitter.

# Git Hook

[Git](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks) permite una serie de hooks, que te permite añadir funcionalidad propia dentro del workflow propio de git. Por ejemplo: podemos ejecutar los test antes de hacer un push para evitar subir algo al repositorio que rompa el proyecto.

En nuestro caso, definiremos un hook `pre-push` para obtener los commits pertenecientes al push que contengan ficheros en la carpeta `_posts` (donde jekyll guarda los posts) y que cumplan la expresión regular `^New post` (que comiencen por New post).

    commit=`git rev-list --grep '^New post' "$range" _posts`

Con esta lista de commits obtenemos los posts pertenecientes al commit.

	files=`git diff-tree --no-commit-id --name-only -r $line | grep '^posts/'`

Ya sólo nos falta crear la URL y llamar al notificador

	for file in $files; do
		name=`echo "$file" | cut -d/ -f2| cut -d. -f1 | cut -d- -f3-`
		rake tweet "http://www.desarrolloenmac.com/$name"
	done

El `hook` completo (a guardar como `.git/hooks/pre-push`) es:

	#!/bin/sh

	# An example hook script to verify what is about to be pushed.  Called by "git
	# push" after it has checked the remote status, but before anything has been
	# pushed.  If this script exits with a non-zero status nothing will be pushed.
	#
	# This hook is called with the following parameters:
	#
	# $1 -- Name of the remote to which the push is being done
	# $2 -- URL to which the push is being done
	#
	# If pushing without using a named remote those arguments will be equal.
	#
	# Information about the commits which are being pushed is supplied as lines to
	# the standard input in the form:
	#
	#   <local ref> <local sha1> <remote ref> <remote sha1>
	#
	remote="$1"
	url="$2"
	z40=0000000000000000000000000000000000000000
	while read local_ref local_sha remote_ref remote_sha
	do
	if [ "$local_sha" = $z40 ]
	then
		# Handle delete
		:
	else
		if [ "$remote_sha" = $z40 ]
		then
			# New branch, examine all commits
			range="$local_sha"
		else
			# Update to existing branch, examine new commits
			range="$remote_sha..$local_sha"
		fi

		# Check for "New post" commit
		commit=`git rev-list --grep '^New post' "$range" _posts`
		for line in $commit; do
			files=`git diff-tree --no-commit-id --name-only -r $line | grep '^posts/'`
			for file in $files; do
				`rake tweet "$file"``
			done
		done
		exit 0
	fi
	done
	exit 0

**Por qué `pre-push`?** cuando se ejecuta el hook `pre-push` los objetos todavía no se han enviado al servidor (en nuestro caso GitHub) por lo que si el push falla ya habremos enviado los tweets. El caso es que no hay hooks `post-push` para cliente (existe un `post-receive` en el servidor, pero no tenemos el código de GitHub, verdad? ). Otra solución sería utilizar los [Webhooks de GitHub](https://developer.github.com/webhooks/), pero creo que complicaría la cosa más.
Además, los casos en los que un push así falle serían que alguien hubiera hecho un push al repositorio o que la comunicación falle (*con mi **gran** conexión casera podría ser*)
{: .notice--info}

# Twitter CLI

Para publicar tweet usaremos [Twitter CLI](http://sferik.github.io/t/). Si tenemos XCode instalado en nuestro mac (que lo tenemos, verdad!) la instalación es tan sencilla como:

	gem install t -n /usr/local/bin

Este `-n /usr/local/bin` es porque en El Capitán /usr/bin está fuera del sandbox (más info en <http://stackoverflow.com/questions/31972968/cant-install-gems-on-os-x-el-capitan>).
{: .notice--info}

Para configurarlo hay que seguir la guía de <http://sferik.github.io/t/>, que resumiendo sería:

* Crear app en Twitter
* Autorizar

Lo mejor de todo es la que la propia herramienta te guía en el proceso. Para ello, abre un terminal y ejecuta:

	t authorize

Y sigue los pasos.

# Rakefile

Rakefile es, resumiendo mucho, un makefile para ruby (el lenguaje de jekyll). Como makefile, sirve para automatizar las tareas.

Para automatizar la publicación de un tweet, definimos en el `Rakefile` la tarea:

	task :tweet do
            require 'jekyll'
            require 'yaml'
            ARGV.shift
            file = ARGV.first
            user_config = YAML.load_file('_config.yml')
            config = Jekyll::Configuration.from user_config
            site = Jekyll::Site.new config
            site.read
            site.posts.docs.each do |doc|
                bundle exec "t update #{config['url']}#{doc.url}" if doc.relative_path == file
            end
            # By default, rake considers each 'argument' to be the name of an actual task.
            # It will try to invoke each one as a task.  By dynamically defining a dummy
            # task for every argument, we can prevent an exception from being thrown
            # when rake inevitably doesn't find a defined task with that name.
            ARGV.each do |arg|
                task arg.to_sym do ; end
            end
       end

Este `rake tweet url` será el que llame el `hook` `pre-push`
El Rakefile completo lo podéis encontrar en <https://github.com/xaviaracil/xaviaracil.github.io/blob/master/Rakefile>

# Conclusión

A priori parece que nos estamos complicando un poco, pero mejor invertir un poco de tiempo ahora que tener que publicar los tweets de forma manual. Ya sabéis, somos *vagos* y queremos que la máquina trabaje para nosotros.

![twitter_cards](/images/2016-06-02-publicar-a-github-pages-y-twittear/twitter_cards.png){: .align-center}

Si queréis que el tweet salga chulo chulo echadle un vistazo a [Twitter Cards](https://dev.twitter.com/cards/overview). El tema que uso en jekyll ([Minimal Mistakes](https://mmistakes.github.io/minimal-mistakes/)) los soporta y el tweet queda muy bien
{: .notice--info}
