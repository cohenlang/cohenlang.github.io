---
title: Contenedores de datos
permalink: /es/contenedores-de-datos
---

Un **contenedor de datos** (*data container*) es un recipiente para almacenar algo.
Se distingue entre variables y constantes.

## Variables

Una **variable** es un contenedor de datos que puede cambiar su valor almacenado en cualquier momento.
Para definir variables, se usa la sentencia **var**:

```
#una variable por sentencia
var nombre: Tipo
var nombre: Tipo = valor
var nombre = valor        #tipo inferido del valor

#varias variables por sentencia
var (
  nombre: Tipo
  nombre: Tipo = valor
  nombre = valor          #tipo inferido del valor
  ...
)
```

Ejemplos:

```
var x = 123
var y: text = "ciao mondo!"

var (
  x = 123
  y: text = "ciao mondo!"
)
```

## Constantes

Una **constante** (*constant*) es un contenedor que no permite modificar su valor.
Por esta razón, deben inicializarse siempre.
Se definen mediante la sentencia **const**:

```
#una única constante
const nombre: Tipo = valor
const nombre = valor        #tipo inferido del valor

#varias de una vez
const (
  nombre: Tipo = valor
  nombre = valor            #tipo inferido del valor
  ...
)
```

Veamos un ejemplos:

```
const x = 123
const y: text = "ciao mondo!"

const (
  x = 123
  y: text = "ciao mondo!"
)
```

## Identificadores

Un **identificador** (*identifier*) es una palabra que identifica una cosa.
Se distingue entre nombres y palabras reservadas.
Un **nombre** (*name*) es aquél que identifica una función, un tipo, una variable, etc.
Mientras que una **palabra clave** (*keyword*), aquél reservado por el lenguaje y que, en principio, no puede utilizarse como nombre.

Los identificadores deben comenzar por un subrayado o una letra, seguido de cero, uno o más caracteres (letras, dígitos o subrayados).
He aquí unos ejemplos ilustrativos:

```
x
_x
x123
x_12_34
```

Las palabras reservadas del lenguaje son:

```
abstract  and     as
between   break
const
do
each      else    entry   enum  export
false     fn      for
if        in      intf    intl
native    next    nop     not
or
proc      pub     pvt
return
self      struct  super
then      throw   true    type
use
var       virtual
with
```