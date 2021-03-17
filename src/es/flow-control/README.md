---
title: Control de flujo
permalink: /es/control-de-flujo
---

El **control de flujo** (*flow control*) consiste en las sentencias con las que regular el flujo de ejecución de un bloque de proposiciones.

## Sentencia if

Mediante la **sentencia if** (*if statement*), se elige una de varias opciones de flujo atendiendo a unas expresiones de comparación:

```
if condición then
  #código
else if condición then
  #código
else if condición then
  #código
else
  #código
```

Se puede definir un dato en la cláusula **if** que puede utilizarse dentro de toda la sentencia, tanto en las condiciones como en los cuerpos de las demás cláusulas.
Para ello, se puede usar la siguiente sintaxis:

```
#definición de variable en línea con tipo inferido
if nombreVariable := expresión; condición then
  #...

#definición de constante en línea con tipo inferido
if nombreVariable ::= expresión; condición then
  #...
```

He aquí un ejemplo ilustrativo:

```
if x ::= valor; x == 0 then
  #...
else if x == 1 then
  #...
else
  #...

#similar a:
const x = valor

if x == 0 then
  #...
else if x == 1 then
  #...
else
  #...
```

## Sentencia with..do

La **sentencia with..do** (*with..do statement*) se puede usar para seleccionar entre varias opciones:

```
with valor do
  if comparación1 then
    #código
  if comparación2 then
    #código
  else
    #código
```

Las cláusulas **if**s pueden indicar varias opciones separándolas por comas.
He aquí un ejemplo:

```
with x do
  if 0 then #...
  if 1, 2 then #...
  else #...
```

Al igual que en la sentencia **if**, se puede definir una variable (`:=`) o constante (`::=`) en línea para contener el valor de la comparación,
el cual se puede usar en los cuerpos de las cláusulas **if**.
Ejemplo:

```
with nombre ::= expresión do
  #...
```

## Sentencia for each

La **sentencia for each** (*for each statement*) se puede utilizar para iterar por una lista con un tamaño fijo de elementos:

```
#definición de bucle con variable de iteración
for each nombre in lista do
  #...

#definición de bucle con constante de iteración
for each const nombre in lista do
  #...
```

Ejemplo:

```
for each const i in [1, 2, 3] do
  #...
```

## Sentencia break

Mediante la **sentencia break** (*break statement*), se puede finalizar un bucle:

```
break
```

## Sentencia next

Mediante la **sentencia next** (*next statement*), se puede saltar a la siguiente iteración del bucle:

```
next
```