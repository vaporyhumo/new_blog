---
date: 2021-05-18 00:41:00
language: es
slug: validation-exceptions
title: Prevenir excepciones al validar modelos de `ActiveRecord`
---

# Prevenir excepciones al validar modelos de ActiveRecord
## Retocando aplicaciones Rails Legacy

En algunas aplicaciones Rails, usualmente las que crecen sin una buena
planificación, se pueden encontrar registros **(Records)** con validaciones que
no manejan los errores de manera adecuada.

Un primer paso para evaluar la existencia de este tipo de validaciones en una
aplicación legacy puede ser un test que verifique que `.new.save` en los
distintos *records* no levante excepciones; recordemos que la intención de las
validaciones es agregar errores al modelo en cuestión.

Para esto podemos usar este pequeño snippet:

```ruby
RSpec.describe do
  ApplicationRecord.descendants.map do |klass|
    it "#{klass}.new.save should not raise an error" do
      expect { klass.new.save }.not_to raise_error
    end
  end
end

```

Si quisieramos asegurar este comportamiento modelo a modelo, podemos usar el
siguiente `shared_example`:

```ruby
RSpec.shared_examples 'new record does not raise on save' do
  it 'does not raise an error when saving an empty record' do
    expect { described_class.new.save }.not_to raise_error
  end
end
```

Recomiendo usar estas técnicas en proyectos legacy para identificar potenciales
modelos descuidados, y corregir los eventuales errores que pueden existir en las
validaciones de estos.
