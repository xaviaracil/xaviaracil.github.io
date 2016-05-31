---
date: 2013-10-02 12:16:35+00:00
slug: http-status-en-rails
title: HTTP status en Rails
wordpress_id: 291
categories:
- Ruby On Rails
---

A veces hay que retornar diferentes status HTTP en las peticiones Rails. El típico rails generate scaffold... genera una action como la siguiente:


      # POST /courses
      # POST /courses.json
      def create
        @course = Course.new(params[:course])
        respond_to do |format|
          if @course.save
            format.html { redirect_to @course, notice: 'Course was successfully created.' }
            format.json { render json: @course, status: :created, location: @course }
          else
            format.html { render action: "new" }
            format.json { render json: @course.errors, status: :unprocessable_entity }
          end
        end
      end







Cuando la llamada a save falla y el formato de salida es json, se devuelven los errores en formato json y con el estado HTTP :unprocessable_entity. Pero, ¿de donde sale este símbolo?




Buscando en internet he encontrado la página [http://www.codyfauser.com/2008/7/4/rails-http-status-code-to-symbol-mapping](http://www.codyfauser.com/2008/7/4/rails-http-status-code-to-symbol-mapping), donde indica que los diferentes status HTTP están definidos en el fichero actionpack/lib/action_controller/status_codes.rb. A partir de Rails 3 estos códigos se han movido a Rack. Ahora los tenéis disponibles en rack/utils.rb, en la constante HTTP_STATUS_CODE. Si tenéis arrancado el servidor gem (sudo gem server) podéis ver la documentación en [http://0.0.0.0:8808/doc_root/rack-1.5.2/rdoc/Rack/Utils.html](http://0.0.0.0:8808/doc_root/rack-1.5.2/rdoc/Rack/Utils.html)
