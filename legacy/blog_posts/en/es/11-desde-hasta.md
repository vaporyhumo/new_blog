---
date: 2021-05-31 19:00:00
language: es
slug: desde-hasta
title: Envuelve un `desde-hasta` en una construcción genérica usando Sorbet
---

# Envuelve un `desde-hasta` en una construcción genérica usando Sorbet

Es común que en nuestras aplicaciones aparezcan construcciones del tipo
desde-hasta o desde-a. Algunos ejemplos podrían ser un rango de fechas, el
principio y fin de un viaje, o un intercambio de una divisa a otra.

Los métodos que ocupan este tipo de rangos, podrían verse así por ejemplo:

```ruby
def build_trip(from, to, other_options)
  do_something
  do_something_else(from, to)
end
```

En un contexto donde este tipo de llamadas abundan, donde dos parámetros solo
tienen sentido si van juntos y además suelen delegarse de un lado a otro siempre
juntos, vale la pena considerar usar alguna abstracción genérica para
simplificar la delegación de argumentos y agregar valor semántico al código.

Esta es mi forma de hacerlo, notar que uso las dependencias `sorbet-runtime` y
`sorbet-struct-comparable`:

```ruby
class AbstractValue < T::InexactStruct
  include T::Struct::ActsAsComparable
end

class FromTo < AbstractValue
  extend T::Generic

  Elem = type_member

  const :from, Elem
  const :to, Elem
end
```

Esto se ocuparía de la siguiente forma:

```ruby
stops = FromTo[String].new(from: 'santiago', to: 'lima')
build_trip(stops, other_options) # método con nuevo signature más simple
```

En la que destaca el hecho de que podemos generar instancias tipadas, y de que
estas instancias son inmutables y comparables.

Si no estamos usando sorbet podemos hacer una versión no tipada con un
comportamiento similar así:

```ruby
class FromTo
  def initialize(from, to)
    @from = from
    @to = to
  end

  attr_reader :from, :to

  def ==(other)
    from == other.from && to == other.to
  end

  def eql?(other)
    from.eql?(other.from) && to.eql?(other.to)
  end
end
```

Que luego se usaría:

```ruby
FromTo.new('santiago', 'lima')
```

Todo esto nos permite refactorizar los métodos en cuestión, simplificando sus
signatures:

```ruby
def build_trip(stops, other_options)
  do_something
  do_something_else(stops)
end
```

Este es un ejemplo sencillo de como se puede hacer el código más legible
agregando pequeñas abstracciones que representan patrones presente en él.
