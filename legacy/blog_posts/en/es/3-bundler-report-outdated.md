---
date: 2021-02-01 16:36:00
language: es
slug: bundler-report-outdated
title: Cómo usar `bundler_report` para identificar gemas desactualizadas
---

# Cómo usar `bundle_report` para identificar gemas desactualizadas

La gema `next_rails` nos proporciona una herramienta llamada `bundle_report` que
podemos usar para saber cuales gemas dentro de nuestro *bundle* están
desactualizadas.

Para instalar la gema usamos el tradicional `gem install next_rails`.

Luego usamos `bundle_report` para obtener un resultado similar al siguiente:

```
$ bundle_report outdated

trollop 1.16.2:
  released almost 11 years ago
  (latest version, 2.9.10, released about 1 year ago)
jwt 1.5.6:
  released over 4 years ago
  (latest version, 2.2.2, released 6 months ago)
factory_bot 5.2.0:
  released 10 months ago
  (latest version, 6.1.0, released 7 months ago)
factory_bot_rails 5.2.0:
  released 10 months ago
  (latest version, 6.1.0, released 7 months ago)
rails 5.2.4.4:
  released 5 months ago
  (latest version, 6.1.1, released 30 days ago)
chunky_png 1.3.15:
  released about 2 months ago
  (latest version, 1.4.0, released about 1 month ago)
tzinfo 1.2.9:
  released about 2 months ago
  (latest version, 2.0.4, released about 2 months ago)

0 gems are sourced from git
17 of the 199 gems are out-of-date (9%)
```

En este reporte se listan todas las gemas que están desactualizadas, con la
fecha y versión del release en uso y del release actual.

Además de esto al final encontramos el total de gemas que se están obteniendo
directamente desde un repositorio y también la proporción de gemas
desactualizadas respecto del total.
