---
date: 2021-02-04 21:48:00
language: es
slug: flay
title: Cómo usar `flay` para identificar código duplicado o similar
---

# Cómo usar `flay` para identificar código duplicado o similar

**Flay** es una librería que nos permite identificar código que está repetido en
*distintas partes de nuestra aplicación, o código que es tan similar que es
*candidato a refactor.

El uso más sencillo de es esta herramientas es ejecutarlo sin flags en la
carpeta raíz de tu proyecto.

```s
flay .

Total score (lower is better) = 59181

...

23) Similar code found in :iter (mass = 412)
  ./spec/requests/passenger_data_segments_matrix_controller_spec.rb:6
  ./spec/requests/segment_types_matrix_controller_spec.rb:6

24) IDENTICAL code found in :iter (mass*2 = 400)
  ./spec/requests/trip_segments_controller/upcoming_spec.rb:238
  ./spec/requests/trip_segments_controller_spec.rb:279
```

Esto te muestra un puntaje total, en donde menos es mejor; y luego una lista de
casos de código similar o **IDENTICO**.

En cada item de este listado hay 4 partes de interés, eventualmente...

Primero podemos distinguir si acaso el item es código similar o es código
idéntico. Generalmente los del segundo tipo pueden ser más fáciles de extraer o
refactorizar.

Luego podemos ver qué tipo de código está repedito, puede ser una llamada a una
función, un bloque, una definición, una asignación, etc. Este es el símbolo que
en el ejemplo aparece como `:iter` y por lo que entiendo está relacionado con el
árbol sintáctico.

En tercer lugar está la masa, o puntaje de la incidencia. Este se calcula según
la complejidad misma del código duplicado y la cantidad de lugares en que este
código se encuentra. El puntaje se duplica cuando hablamos de código
**identico** en vez de similar.

Por último, cada incidencia cuenta con una lista de lugares en donde se
encuentra este código similar o repetido. Esto es archivo y linea de código.
Vamos a ver caso en donde el mismo código se encuentra repetido incluso dentro
del mismo archivo.

### Cuándo o cómo lo uso?

Hay tres casos en los que creo que puede venir buen usar una herramienta como
esta, detallo a continuación.

#### Cómo métrica general

Si corremos flay a nivel de aplicación, podemos obtener una noción de cuántos
patrones se están usando en el código y cuán bien se están implementando.

Quiero decir, una aplicación que aplica patrones consistentes y se refactoriza a
menudo tendrá menos código duplicado que una en la que no se usan patrones.

La proporción entre este dato y la complejidad misma de la aplicación nos puede
servir para comparar entre distintas aplicaciones, por ejemplo *flay score/flog
score*. Ya hablaremos de `flog` en otro momento.

#### Para refactorizar una parte de la aplicación

Si ya estás (o con tu equipo están) en una etapa en donde se aplican patrones,
pero aún existe mucho código legacy, o están tratando de uniformar el uso de
ciertos patrones, se puede usar `flay` para identificar código repetido solo en
una subsección del proyecto.

En Rails es muy fácil ejemplificarlo, si por ejemplo juntas todos tus **query
objects** en la carpeta `app/queries`, correras `flay` en ese directorio.

```s
flay app/queries
```

El resultado tendrá exactamente el mismo formato, pero una menor cantidad de
incidencias; todas estas incidencias están de alguna manera más conectadas.

Particularmente he notado que gran cantidad del código duplicado se presenta de
esta forma, en archivos que coinciden en forma o patrón; pero no siempre es así,
y hay patrones que se presentan a lo largo de toda la aplicación.


#### Para refactorizar una tarea en específico

Si estás trabajando en una tarea en particular, puedes usar `flay` cuando tus
pruebas ya estén listas y pasando y llegue la hora del refactor.

Puedes correr flay sobre todos tus archivos nuevos y/o modificados, para tratar
de eliminar todo el código repetido antes de solicitar que tus cambios se
mezclen al proyecto.

Un ejemplo de esto podría ser el siguiente:

```sh
flay $(git diff branch1 branch2 --name-only)
```

Usar consistentemente `flay` de esta manera te puede impulsar a aumentar la
calidad de tu código de sobremanera, ya que vas a empezar a familiarizarte con
los patrones subyacentes en tu código, y además te aconstumbrarás a hacer clases
y módulos más pequeños y reutilizables, cada uno con una única responsabilidad.

Te recomiendo hacer el ejercicio de levantar algunos PRs con *flay score* de
**0**, te aseguro que aprenderás una cosa o dos que antes no sabías.
