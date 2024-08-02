---
date: 2021-02-02 16:49:00
language: es
slug: bundler-outdated
title: Cómo usar `bundler` para identificar gemas desactualizadas
---

# Cómo usar `bundler` para identificar gemas desactualizadas

*Bundler*, el gestor de librerías de *ruby* nos provee de una herramienta para
conocer cuales gemas no están en su última versión.

Al usar `bundle outdated` se obtiene un resultado similar al siguiente:
```s
$ bundle outdated

Gem                Current  Latest  Requested  Groups
actioncable        5.2.4.4  6.1.1
actionmailer       5.2.4.4  6.1.1
actionpack         5.2.4.4  6.1.1
actionview         5.2.4.4  6.1.1
activejob          5.2.4.4  6.1.1
activemodel        5.2.4.4  6.1.1
activerecord       5.2.4.4  6.1.1
activestorage      5.2.4.4  6.1.1
activesupport      5.2.4.4  6.1.1
chunky_png         1.3.15   1.4.0
factory_bot        5.2.0    6.1.0   ~> 5       default
factory_bot_rails  5.2.0    6.1.0   ~> 5       default
jwt                1.5.6    2.2.2
rails              5.2.4.4  6.1.1   ~> 5.2     default
railties           5.2.4.4  6.1.1
trollop            1.16.2   2.9.10
tzinfo             1.2.9    2.0.4
```

En este reporte se lista cada gema con su versión actual, su versión más
reciente, la versión restringida en el `Gemfile` y los grupos en los cuales
estas gemas están siendo requeridas.

Esto último puede ser de utilidad ya que las dependencias que no se requieran
para ambientes productivos suelen ser menos riesgosas de actualizar.
