---
title: Estructuras
permalink: /es/estructuras
---

Una **estructura** (*struct*) es un tipo de datos especial, con las siguientes características:

- No puede definir un constructor explícitamente, pues lo hará el compilador implícitamente.

- Puede definir los campos de instancia del tipo.

Vamos a ver un ejemplo ilustrativo para ir abriendo boca:

```
struct Interval
  const begin: ts
  const end: ts
  fn includes(timestamp: ts) = timestamp between begin and end
```

Las estructuras definen un constructor implícito que obliga a tener que crear sus instancias a partir de un mapa u otro objeto con esos mismos campos:

```
Interval({begin = valor, end = valor})
Interval(begin = valor, end = valor)    #como siempre son mapas, se puede omitir {}
Interval(begin, end)                    #similar a: begin = begin, end = end
```

## Sintaxis de struct

La sintaxis de la sentencia **struct** es como sigue:

```
struct Nombre
  miembro
  miembro
  miembro
  ...
```

## Propiedades

Se puede definir una **propiedad** (*property*), esto es, un campo cuyo valor se calcula a partir de una expresión.
Para ello, tenemos dos sintaxis:

```
#Cuando la propiedad tiene varias proposiciones.
#Se indica el valor a devolver mediante un return.
@prop
fn nombre(): Tipo
  #código

#Para propiedades donde su cuerpo es una única expresión a evaluar.
#Se omite los paréntesis y el tipo, el cual se infiere de la expresión.
fn nombre = valor
```

## Uso de self

En caso de conflictos de nombre, puede utilizar el objeto **self** para indicar la instancia actual.
Ejemplo:

```
struct Interval
  const begin: ts
  const end: ts
  fn includes(timestamp: ts) = timestamp between self.begin and self.end
```