---
title: "Publicar a GitHub Pages y Twittear 2: categorías"
date: "2017-04-20 15:09:00 +0200"
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

Vimos en {% post_url 2016-06-02-publicar-a-github-pages-y-twittear %} cómo twittear automáticamente los posts creados o editados al publicar nuestro site en GitHub Pages. Hoy vamos a ver cómo añadir las categorías del post como hashtags en el tweet. Es más fácil de lo que parece.

<!--more-->

Jekyll guarda las categorías de un post en la clave `categories` del hash `data`. Para obtener un string con los hashtags bastará con:

    hashtags = docs.data['categories'].map{|c| "#" + c.tr_s(" ", "_")}.join(' ')

Al array de categorías le aplicamos un `map`, que ejecuta el bloque de código para cada elemento del mismo. En nuestro caso, lo que queremos ejecutar es el código que genere un hashtag. Esto es devolver un string que empieze por `#` y que no contenga espacios `c.tr_s(" ", "_")`.

> **Actualización**: Resulta que con `tr_s` subtituimos únicamente los espacios, mientras que Twitter tampoco acepta `-` como parte del hashtag. Así, para que nos haga un hashtag correcto de `github-pages`, por ejemplo, habrá que modificar la llamada por:

    hashtags = doc.data['categories'].map{|c| "#" + c.gsub(/-|\s/, "_")}.join(' ')


Para añadir estos hashtags en nuestro tweet modificamos un poco la tarea `:tweet` nuestro `Rakefile`:

    site.posts.docs.each do |doc|
      if doc.relative_path == file
        hashtags = doc.data['categories'].map{|c| "#" + c.gsub(/-|\s/, "_")}.join(' ')
        bundle exec "t update \"#{hashtags} #{config['url']}#{doc.url}\""
      end
    end

Al tener varías líneas de código tenemos que hacer del `if doc.relative_path == file` un bloque.

Tenéis el `Rakefile` actualizado en <https://github.com/xaviaracil/xaviaracil.github.io/blob/master/Rakefile>
