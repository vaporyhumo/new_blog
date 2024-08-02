---
date: 2021-02-03 17:18:00
language: es
slug: libyear-outdated
title: Cómo usar `libyear-bundler` para identificar gemas desactualizadas
---

# Cómo usar `libyear-bundler` para identificar gemas desactualizadas

Existe una herramienta llamada `libyear-bundler` que nos permite obtener
distintos reportes acerca de la desactualización de nuestras dependencias.

Tiene distintos formatos y aquí solamente les voy a mostrar dos de ellos, el que
muestra todo y el que es un resumen.

Para instalar la gema solamente usamos `gem install libyear-bundler`.

Si la ejecutamos en alguno de nuestros proyectos, obtendremos un resultado
similar al siguiente:

```s
$ libyear-bundler --all

activesupport    5.2.4.4   2020-09-09   6.1.1   2021-01-07   23  [1, 0, 0]   0.3
chunky_png        1.3.15   2020-12-15   1.4.0   2020-12-28    1  [0, 1, 0]   0.0
factory_bot        5.2.0   2020-04-24   6.1.0   2020-07-08    4  [1, 0, 0]   0.2
factory_bot_rails  5.2.0   2020-04-26   6.1.0   2020-07-08    2  [1, 0, 0]   0.2
jwt                1.5.6   2016-09-19   2.2.2   2020-08-18    7  [1, 0, 0]   3.9
rails            5.2.4.4   2020-09-09   6.1.1   2021-01-07   23  [1, 0, 0]   0.3
railties         5.2.4.4   2020-09-09   6.1.1   2021-01-07   23  [1, 0, 0]   0.3
trollop           1.16.2   2010-04-06   2.9.10  2019-11-25    7  [1, 0, 0]   9.6
tzinfo             1.2.9   2020-12-16   2.0.4   2020-12-16    5  [1, 0, 0]   0.0
ruby               2.6.6                3.0.0                 4  [1, 0, 0]   0.0

System is 17.6 libyears behind
Total releases behind: 283
Major, minor, patch versions behind: 17, 1, 0
```

Algunas gemas se remueven del ejemplo para hacerlo más pequeño.

Para cada gema tenemos de izquierda a derecha:
- su nombre;
- la versión en uso, con la fecha del release;
- la versión más reciente, con su fecha;
- la cantidad de releases desde la versión en uso;
- una tupla con la cantidad de `[MAJOR, MINOR, PATCH]` versiones de
  desactualización, por ejemplo de `rails 5.2.4.4` a `6.1.1` hay un `(1)`
  `MAJOR`;
- la cantidad de años o *libyears* de desactualización de dicha gema, con un
  solo decimal.

Finalmente un resumen de:
- la cantidad total de libyears de deuda,
- la cantidad total de releases de deuda,
- la cantidad de `[MAJOR, MINOR, PATCH]` versiones de deuda.

Por otra parte, si solo nos interesan los datos totales, por ejemplo si estamos
tomando estadísticas, podemos agregar la opción `--grand-total`.

```s
$ libyear-bundler --all --grand-total

17.6
283
[17, 1, 0]
```

O si usamos `--grand-total` sin `--all` se obtiene solo el total de *libyears*.

```s
$ libyear-bundler --grand-total

17.6
```

Estos son, por supesto, los mismo números al final del reporte completo.
