---
title: Errores
permalink: /es/errores
---

**Cohen** proporciona soporte para errores, donde un **error** representa un evento anómalo que finaliza la ejecución del programa.
Para ello, proporciona la función reservada **throw()**.

## Propagación de error

Mediante la **propagación de error** (*error throwing* o *error raising*) se genera un error que finaliza el programa.
Se debe utilizar la función reservada **throw()**:

```
fn throw(error: text)
```

Ejemplos:

```
throw("Esto es el mensaje de error.")
```

Cada vez que se genera un error, finaliza el programa.