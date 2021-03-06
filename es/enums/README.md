---
title: Enumeraciones
permalink: /es/enumeraciones
---

# Enumeraciones

Una **enumeración** (*enumeration*) es un tipo que lista un posible conjunto explícito de valores.
Se definen mediante la sentencia **enum**:

```cohen
enum Nombre
  Elemento
  Elemento
  Elemento
  ...
```

Cada elemento está formado por dos componentes, el nombre y su valor:

```cohen
Nombre = Valor
```

El valor debe ser un literal numérico

He aquí un ejemplo de una enumeración predefinida de **Cohen**:

```cohen
/**
 * Los posibles tipos de transacción que pueden emitirse.
 */
enum TxnType
  Unknown = 0
  Payment = 1
  KeyRegistration = 2
  AssetConfig = 3
  AssetTransfer = 4
  AssetFreeze = 5
  AppCall = 6
```

## Operadores de comparación

Para comparar el valor de una variable o constante con un elemento de la enumeración, se puede usar los operadores `==`, `!=`, `==~` y `!=~`:

```cohen
t.type == TxnType.AppCall
t.type != TxnType.AppCall

t.type ==~ AppCall
t.type !=~ AppCall
```

## Operadores de asignación

Para cambiar el valor de una variable de tipo enumeración, se puede usar los operadores `=` y `=~`:

```cohen
var txnType: TxnType

txnType = TxtType.Payment
txnType =~ Payment
```

## Sentencia with..do

En una sentencia `with..do` podemos indicar tanto el tipo y el elemento como sólo el elemento:

```cohen
with t.type do
  if TxnType.Unknown then
    #...

  if Payment then  #similar a: TxnType.Payment
    #...

  else
    #...
```