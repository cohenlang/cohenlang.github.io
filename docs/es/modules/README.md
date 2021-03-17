---
title: Módulos
permalink: /es/modulos
---

Un **módulo** (*module*) es un archivo de código, con extensión **.chn**, que contiene objetos reutilizables como, por ejemplo, funciones, estructuras, enumeraciones, interfaces o contratos.

Los módulos tienen miembros.
De manera predeterminada, son privados.
Para poder reutilizarlos, hay que usar las palabras reservadas **export** o **pub**.

Un módulo puede exportar los siguientes objetos:

- Funciones.

- Tipos (enumeraciones, interfaces, estructuras...).

- Constantes.
  No se puede definir variables a nivel de módulo, pero sí constantes.

## Sentencia export

La **sentencia export** (*export statement*) puede exportar un objeto.
Su sintaxis es la siguiente:

```
export objeto
```

He aquí unos ejemplos:

```
export fn suma(x: uint, y: uint) = x + y
```

No hay que olvidar que un módulo sólo puede tener un único **export** y, en caso de hacerlo, no podrá tener **pub**s.

## Sentencia pub

Cuando se desea que el módulo exporte varios objetos, debemos usar **pub**.

```
pub objeto
```

Veamos un ejemplo ilustrativo:

```
pub fn suma(x: uint, y: uint) = x + y
pub fn resta(x: uint, y: uint) = x - y
```

## Importaciones

La **importación** (*import*) es la operación mediante la cual usamos uno o más objetos de un determinado módulo.
Deben aparecer al comienzo de un módulo y se realizan mediante una sentencia **use**:

```
use (
  ítem
  ítem
  ítem
  ...
)
```

Los ítems pueden estar disponibles en el SDK de **Cohen**, ser locales al proyecto o bien externos (importados con alguna herramienta).
Para indicar su procedencia, hay que indicar el protocolo de acceso:

- **cohen:**, indica el SDK de **Cohen**.

- **pkg:**, indica un módulo de un paquete.

- **Nada**, cuando no se indica nada, se asume el proyecto actual, concretamente, el directorio actual.
  Cuando la ruta comienza por dos puntos (`..`), se buscará en el directorio padre al actual.
  En cambio, cuando comienza con un identificador en el directorio en curso.

Se usa la barra invertida (`/`) para navegar dentro de los directorios o paquetes.

He aquí unos ejemplos:

```
use (
  cohen:txn/TxnType #importa la enumeración TxnType del SDK de Cohen
  pkg:math          #importa el paquete math
  math              #importa el módulo math del directorio actual
  ../math           #importa el módulo math del directorio padre al actual
)
```

Si se importa un módulo que publica varios objetos, se accederán por el operador punto (`.`):

```
math.sum(12, 34)
```

Si se importa un módulo pero sólo se quiere hacer uso de uno o más objetos, usar la siguiente sintaxis:

```
{objeto, objeto...} = ítem
```

Ejemplo:

```
use (
  {suma, resta} = math
)
```

Si se desea renombrar un objeto, por ejemplo, para evitar conflictos de nombres, usar el operador **as**:

```
use (
  {sum as suma, sub as resta} = math
)
```

Para importar un módulo con otro nombre, usar el operador **=**:

```
use (
  m = math
)
```

Cuando se importa un módulo que exporta un único objeto, el propio objeto es lo que se importará.

## Sintaxis de un módulo

Todo módulo debe llevar la extensión **.chn** y tener la siguiente sintaxis:

```
#opcional
use (
  importaciones
)

#Definiciones de objetos, tanto exportables como privados.
#Recordemos que para poder reutilizarlos, habrá que usar export o pub.
#Los módulos pueden tener constantes, pero no variables.
#Y en caso de definir una constante, habrá que indicar un valor literal.
```

## Comprobación de sintaxis

Los módulos, salvo que sean de contratos, no tienen que compilarse.
Lo harán cuando se usen en los contratos y sólo de aquellos objetos que se usen en el contrato.
Pero si se desea comprobar que la sintaxis de un módulo es correcta, se puede usar el comando **cohen check**.
Ejemplo:

```
cohen check módulo.chn
```