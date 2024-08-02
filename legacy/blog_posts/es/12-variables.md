---
date: 2021-06-08 17:33:00
language: es
slug: variables
title: 'Rails: crear un objeto para las variables de ambiete'
---

# Rails: referenciar las variables de ambiente usando un inicializador con constantes

En la versión `2.10` de `rubocop-rails` se agrega un cop que aplica la
recomendación de no acceder directamente a `ENV` para referenciar las variables
de ambiente: `Rails/EnvironmentVariableAccess`.

La idea de este cop es prevenir potenciales desacuerdos de valores entre lo
indicado en las variables de ambiente con lo configurado en
`Rails.configuration`, que sería el lugar oficialmente recomendado para guardar
estos valores.

Otra característica de este cop es que no se aplica en `./config`, dando espacio
a que nosotros podamos agregar nuestras propias constantes en el proceso de
inicialización.

Aquí dejamos una propuesta distinta a la de usar el objeto de configuraciones de
`rails`. Lo que hacemos es crear un módulo que se cargará en la inicialización y
que contendrá solamente constantes creadas a partir de los valores encontrados
en `ENV`, o eventualmente de otros lados.

Primero, dónde lo vamos a ubicar? Simple, en `config/initializers`.

Segundo, cómo le vamos a poner? Aquí algunas sugerencias podrían ser:
`Environment`, `Configuration`, `World`. También pueden usar un nombre
único/original, en nuestro caso usamos la palabra euskera `Ingurune`, que
significa ambiente.

Tercero, porqué constantes? Queremos que los valores sean definidos una única
vez y lo antes posible; queremos que sean inmutables.

Entonces si juntamos todo esto nos puede quedar algo como esto:

```ruby
# config/initializers/ingurune.rb

module Ingurune
  # this is nilable
  DB_PASS = ENV['DB_PASS'].freeze
  # this has a default value
  ELASTIC_URL = ENV.fetch('ELASTIC_URL', 'localhost:9200').freeze
  # this will raise if missing
  TOP_SECRET = ENV.fetch('TOP_SECRET').freeze
end
```

Notar que aquí podemos separar las constantes en tres tipos, siendo el último de
ellos particularmente útil:

* Podemos usar `.[]` si es aceptable que una configuración no esté, es decir,
  que sea `nil`
* Podemos usar `.fetch` con dos argumentos si es que existe un valor por defecto
  en caso de que la configuración esté ausente.
* Podemos usar `.fetch` con un solo argumento si es que queremos imponer la
  necesidad de la presencia de esta configuración: el programa no correrá si es
  que no se encuetra.

Así podremos referenciar cualquiera de estos valores directamente en nuestro
código, agregando valor semántico (`Environment::ELASTIC_URL` vs
`ENV.fetch('ELASTIC_URL')`) y también reduciendo la complejidad aparente, ya que
no existen llamadas a métodos ni argumentos, solo referencia a constantes.

Si quieren saber como hacer esto *The Rails Way™* les recomendamos
[este artículo por Arkency][1].
[1]: https://blog.arkency.com/custom-application-configuration-variables-in-rails-4-and-5/
