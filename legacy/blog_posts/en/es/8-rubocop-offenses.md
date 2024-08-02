---
date: 2021-02-06 23:09:00
language: es
slug: rubocop-offenses
title: Cómo identificar las ofensas de `rubocop` con más ocurrencias
---

# Cómo identificar las ofensas de `rubocop` con más ocurrencias

Básicamente, con este formato puedes encontrar cuales son las ofensas más
comunes en un proyecto, lo que te puede dar indicios de qué es lo que falta
aprender o profundizar como equipo o como individuos.

```sh
rubocop --format offenses
```

Y la lista se verá así:

```s
1765  RSpec/MultipleMemoizedHelpers
1226  RSpec/NestedGroups
463   RSpec/MultipleExpectations
180   RSpec/ExampleLength
...
2     Style/IfWithBooleanLiteralBranches
1     Lint/SafeNavigationChain
1     Performance/CollectionLiteralInLoop
--
4068  Total
```

Donde cada ofensa se lista con su cantidad de ocurrencias de mayor a menor, y
abajo el total de ofensas.

Al igual que los otros formatos, este puede ser mezclado con el resto de
opciones, como por ejemplo para correrlo solo en algunos archivos, y también
puede ser mezclado con otros formateadores.

En el caso particular de la aplicación que usé para el ejemplo, se puede ver que
la mayor parte de las ofensas son del mismo departamento. Este es un indicio
clarísimo de que algo anda mal, y en efecto, esta aplicación tiene una suite de
pruebas con mucho coverage, pero que demora 40 minutos en correr, y los archivos
pueden tener hasta miles de lineas de código.

Al igual que en el caso anterior, partir por las de abajo será mucho más
sencillo de trabajar que partir por las de arriba, pero en este caso cada ofensa
tiene su propio grado de dificultad; muchas de ellas son autocorregibles, y
otras no requieren más que unos minutos en arreglar.

Por otro lado las ofensas de `Metrics` o de `RSpec` pueden ser mucho más
complejas de trabajar, pero son por lejos las de mayor impacto.

Si usas a menudo este formatter tendrás siempre una noción de cuál es la parte
más débil de  tus desarrollos, o cuales son las áreas que tu equipo no maneja
muy bien, y donde vale la pena invertir más tiempo.

## TL;DR

```sh
rubocop -fo
```
