---
title: Interfaces
permalink: /es/interfaces
---

# Interfaces

Una **interfaz** (*interface*) declara la estructura de datos de un objeto.
Es similar a las interfaces de otros lenguajes, pero en **Cohen** sólo indica los campos que un objeto puede tener.
Básicamente, se utilizan para declarar los campos que necesitamos tenga un determinado objeto para poder operar con él.

## Definición de interfaces

Para definir una interfaz, hay que utilizar la sentencia **intf**:

```cohen
#sin interfaz madre
intf Nombre
  Campos

#con interfaz madre
intf Nombre: Nombre
  Campos
```

Los campos se definen como sigue:

```cohen
nombre: tipo
```

Ejemplo:

```cohen
intf Interval
  begin: ts
  end: ts
```