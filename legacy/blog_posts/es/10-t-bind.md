---
date: 2021-05-31 17:22:00
language: es
slug: t-bind
title: 'Sorbet: Cómo especificar en qué contexto se ejecuta un bloque'
---

# Sorbet: Cómo especificar en qué contexto se ejecuta un bloque

Cuando tienes un bloque que se va a ejecutar en otro contexto, sorbet
probablemente no encontrará los métodos referenciados, ya que de hecho no
existen en el contexto en el cuál son llamados.

Puedes especificar el contexto tanto en la definición de la función que recibe
el bloque, como en el llamado mismo usando `T.bind`, así:

```ruby
class ApplicationController < ActionController::API
  rescue_from ActiveRecord::RecordNotFound do |e|
    T.bind(self, ApplicationController)

    render_errors([message], status: :not_found)
  end
end
```

De esta forma podemos subir algunos archivos más complicados a `typed: strict`.

Para aprender mejor a usar bloques la lectura recomendada es
[la documentación misma](https://sorbet.org/docs/procs).
