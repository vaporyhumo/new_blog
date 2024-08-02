---
date: 2021-02-05 22:46:00
language: es
slug: rubocop-worst
title: Cómo identificar los archivos con más ofensas de `rubocop`
---

# Cómo identificar los archivos con más ofensas de `rubocop`

Si usas `rubocop` en tus proyectos, existe una alta probabilidad de que tu
código aún tenga una buena cantidad de ofensas.

Supongo, ya que generalmente las aplicaciones no suelen comenzar con procesos
automatizados para el control de este tipo de cosas, que es la única manera
efectiva de implementar este tipo de herramientas *as far as I know*.

Entonces?

```sh
rubocop --format worse
```

Y la respuesta? Una lista interminable con la cantidad de cops por archivos y al
final el total

```s
283   spec/requests/trips_controller_spec.rb
228   spec/requests/tickets_controller_spec.rb
183   spec/models/ticket_spec.rb
...
1     setup/settings.rb
1     setup/seeds.rb
--
4068  Total
```

Obviamente esto es compatible con el resto de argumentos de `rubocop` y se puede
usar en conjunto con otros formatos también.

Este formato puede servir como estrategia para priorizar esfuerzos.

Puedes seleccionar por la parte inferior los archivos que tienen muy pocas
ofensas, estos serán los más fáciles de *limpiar*.

Si comienzas por la parte superior encontrarás lo que probablemente sea la parte
más deplorable de tu aplicación. Gran cantidad de ofensas en primer lugar
probablemente signifique gran cantidad de código; esto es algo que queremos a
evitar a toda costa dentro de un archivo. Para *limpiar* aquí necesitarás más
tiempo y más esfuerzo.

## TL;DR

Esta es la versión corta.

```sh
rubocop -fw
```
