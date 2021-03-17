---
title: Typedefs
permalink: /es/typedefs
---

Un **typedef** es un alias para un tipo.
A veces, necesitamos asignar otro nombre a un tipo o bien definir un tipo para una lista.
Para estos casos, podemos usar la siguiente sintaxis de la sentencia **type**:

```
#alias de otro tipo
type Nombre = Tipo

#definición de una lista con tipo específico
type Items = Tipo[tamaño]
```

Ejemplos:

```
type Length = uint
type Items = uint[2]
```