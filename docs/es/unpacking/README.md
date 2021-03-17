---
title: Desempaquetado
permalink: /es/desempaquetado
---

El **desempaquetado** (*unpacking*) es una operación que permite extraer fácilmente objetos de listas, mapas o estructuras.

## Expresiones de desempaquetado

Tenemos un tipo especial de expresión con la que se puede asignar elementos de una lista, mapa o estructura a varios contenedores de datos.

### Expresión de desempaquetado de lista

Por un lado, tenemos el desempaquetado de una lista.
Consiste en asignar a una, dos o más variables los elementos consecutivos de una lista.
Cada variable recibe el elemento de la lista que le corresponda por su posición.
Así pues, la primera variable recibirá el elemento 0; la segunda, el 1; y así sucesivamente.
Su sintaxis es como sigue:

```
[variable, variable, variable...] = Exp
```

También se puede definir contenedores de datos a partir de listas:

```
var [variable, variable, variable...] = Exp
const [variable, variable, variable...] = Exp
```

No hace falta indicar los tipos, porque se inferirán del tipo de la lista.

## Expresión de desempaquetado de mapa

También es posible desempaquetar campos de un mapa u objeto.
En este caso, se usa las llaves (`{}`):

```
{variable, variable, variable...} = Exp
```

Ejemplo:

```
const {begin, end} = voteRound
```