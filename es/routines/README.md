---
title: Rutinas
permalink: /es/rutinas
---

# Rutinas

Una **rutina** (*routine*) representa una secuencia de operaciones que puede invocarse en cualquier momento a través de un nombre y una lista de cero o más argumentos.
Se distingue entre funciones, aquéllas que devuelven un valor;
y procedimientos, aquéllos que no devuelven nada.

## Funciones

Una **función** (*function*) es una rutina que debe devolver un valor de un determinado tipo de dato.
Se definen mediante la sentencia **fn**:

```cohen
#Función que indica el tipo del dato a devolver
fn nombre(parámetros): Tipo
  #cuerpo de la función

#Función que indica que el valor a devolver
#será el de la variable indicada, a menos
#que se indique otro valor con un return
fn nombre(parámetros) -> nombre: Tipo
  #cuerpo de la función

#Ídem al anterior, pero la variable indica su valor inicial
fn nombre(parámetros) -> nombre := valor
  #cuerpo de la función

#Función que devuelve un determinado valor literal,
#si no se indica lo contrario explícitamente con un return
fn nombre(parámetros) -> valor
  #cuerpo de la función

#Función que devuelve el valor de una expresión
fn nombre(parámetros) = valor
```

### Sentencia return

Para indicar que finalice la invocación y se devuelva el valor, se usa la sentencia **return**:

```cohen
#devuelve el valor indicado en la definición de la función
return

#devuelve el valor indicado en la sentencia
return valor
```

Cuando una función se define con `->`, tenemos que tener en cuenta lo siguiente:

- Si no se indica ninguna expresión en el **return**, se devolverá el valor de la variable o expresión indicada en `->`.

- Si se indica una expresión en el **return**, se devolverá el valor de la expresión.

- Si se alcanza el fin de la función sin ejecutarse ningún **return**, se devolverá lo indicado en `->`.

He aquí unos ejemplos ilustrativos:

```cohen
fn suma(x: uint, y: uint, z: uint) -> resultado: uint
  resultado = x + y + z

#similar a:
fn suma(x: uint, y: uint, z: uint): uint
  return x + y + z

#similar a:
fn suma(x: uint, y: uint, z: uint) = x + y + z
```

## Parámetros

Un **parámetro** (*parameter*) es una variable o constante cuyo dato se recibe de quien invoca la rutina.
He aquí su sintaxis:

```cohen
#parámetros variables
nombre: tipo

#parámetros constantes, cuyo valor no podrá modificarse
#en el cuerpo de la función
const nombre: tipo
```

Actualmente, no es posible indicar valores predeterminados para los parámetros.

## Procedimientos

Un **procedimiento** (*procedure*) es una rutina que no devuelve nada.
Se definen mediante la sentencia **proc**:

```cohen
proc nombre(parámetros)
  #cuerpo del procedimiento
```

El cuerpo del procedimiento puede contener ***return***s, si fuera necesario.
En caso de contener una expresión, la expresión se evaluará, pero no devolverá su valor.

## Cuerpo de las rutinas

El **cuerpo** (*body*) de la rutina es la secuencia de proposiciones a ejecutar cada vez que se invoque la rutina.
Debe encontrarse sangrado una posición más que la sentencia de definición y cada proposición se separa de la siguiente por un salto de línea;
de manera similar a **Python**.
No se usa el punto y coma (`;`) como en otros lenguajes.

## Invocación de rutinas

Para invocar una rutina, usar el operador de llamada de manera similar a otros lenguajes:

```cohen
var resultado = suma(1, 2, 3)
```