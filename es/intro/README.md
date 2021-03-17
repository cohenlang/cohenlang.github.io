---
title: Introducción
permalink: /es/introduccion
---

# Introducción

**Cohen** es un lenguaje de programación para el desarrollo de *smart contracts* o *DApps* para [**Algorand**](https://algorand.foundation).
Sus principales objetivos son:

- Desarrollo de DApps seguras.

- Facilidad de uso, entendimiento y aprendizaje.

- Extensibilidad a otras plataformas o *blockchains* como, por ejemplo, **Ethereum**.

- Soporte de módulos y herencia para facilitar la reutilización de código y la mejora de la productividad.

Y proporciona las siguientes herramientas:

- **cohen**, el compilador de **Cohen**.

*Roadmap*:

- Soporte de herencia de simple.

- Soporte para **TypeScript**, **Dart**, **Python** y **Go**.

- Generación de **Markdown** a partir de los comentarios de documentación.

## Instalación

**Cohen** se ha desarrollado para la plataforma [**Node.js**](https://nodejs.org) con [**Dogma**](https://dogmalang.com).
Las herramientas de **Cohen** se pueden instalar como sigue:

```shell
npm i -g cohen
```

Finalizada la instalación, se recomienda comprobar el acceso a las aplicaciones:

```shell
$ cohen help
```

### Extensión de VS Code

Se encuentra disponible la extensión **Cohenlang** para **VS Code**.

## Comentarios

Un **comentario** (*comment*) es un texto usado principalmente para explicar algo.
En **Cohen**, los comentarios comienzan por una almohadilla `(#`):

```
#esto es un comentario hasta fin de línea

#[esto es un comentario delimitado por corchetes
que puede expandirse en varias líneas
si así lo deseamos]
```

Observe que cuando un comentario comienza por `#[` se cierra con `]`.
Así se permite los comentarios multilínea en línea.

### Comentarios de documentación

Un **comentario de documentación** (*doc comment*) se utiliza para describir detalladamente un objeto como, p.ej., un tipo o una función.
Se delimitan como sigue:

```
/**
 * comentario
 */
```

## Compilación

La **compilación** (*compilation*) es el proceso u operación mediante el cual transformamos código de **Cohen** a otro lenguaje.
En caso de **Algorand**, a **TEAL**.

### Directivas de compilación

Una **directiva de compilación** (*compilation directive*) es un comando que permite especificar fragmentos de código que sólo deben compilarse bajo determinadas condiciones.

Las directivas se referencian mediante `#!`.

#### Directiva #!if

Mediante la directiva **#!if** se puede compilar determinados fragmentos de código.
Su sintaxis es como sigue:

```
#!if [not] Nombre then
código
#!else if Nombre then
código
#!else
código
#!end
```

Actualmente, sólo se puede indicar una cláusula **else if** y/o una cláusula **else**.
Como nombre hay que indicar una variable directiva de compilación que son siempre booleanas.

### Compilación a TEAL

Para compilar de **Cohen** a **TEAL**, hay que usar el comando **cohen teal**.
He aquí un ejemplo ilustrativo mediante el cual se indica que compile el código, dejando los archivos generados en build, definiendo la variable directiva *xyz*:

```
cohen teal -d=xyz -o=build src
```

### Comprobación sintáctica

Mediante **cohen check** se comprueba si el contenido de uno o más archivos son correctos.
El siguiente ejemplo muestra cómo comprobar la sintaxis de los archivos ubicados en la carpeta **src**:

```
cohen check src
```