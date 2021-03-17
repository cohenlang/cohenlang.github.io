---
title: Contratos inteligentes
permalink: /es/contratos-inteligentes
---

# Contratos inteligentes

Un **contrato inteligente** (*smart contract*) no es más que un tipo de aplicación para *blockchain*.
Representa una aplicación que puede desplegarse en la cadena de bloques tantas veces como sea necesario.

En **Algorand**, los contratos pueden ser de los siguientes tipos:

- Con estado, aquéllos que almacenan datos en la cadena de bloques.

- Sin estado, básicamente aquéllos que se usan para representar autoridades de firma.

## Contratos inteligentes con estado

Un **contrato inteligente con estado** (*stateful smart contract*) es aquél que tiene la capacidad de almacenar datos en la cadena de bloques.
Básicamente contiene:

- La lógica del contrato, que no es más que la parte algorítmica del contrato o programa.

- El estado del contrato, o sea, los datos almacenados en la cadena de bloques.

En **Cohen**, un contrato con estado se implementa mediante la sentencia ***contract***:

```
@stateful
export contract NombreContrato
  #campos del estado global
  #y métodos
```

### Estado de un contrato

El **estado** (*state*) de un contrato no es más que el conjunto de datos que mantiene para poder realizar su trabajo.
Es persistente y se almacena en la cadena de bloques.
En **Algorand**, el estado se divide en dos partes:

- El **estado global** (*global state*), aquél que se almacena en la cadena de bloques para todo el contrato, independientemente de sus suscriptores.

- El **estado local** (*local state*), aquél que se almacena en los registros de los suscritos al contrato.
  No se puede escribir en un registro si la cuenta no está suscrita al contrato.

Observe que el estado global es ajeno a los suscriptores, mientras que el local es específico a cada suscriptor.

#### Estado global

El estado global se almacena mediante campos definidos en el contrato.
Cuya sintaxis es como sigue:

```
var|const nombre: tipo
```

He aquí la lista de datos posibles:

Tipo de datos | Descripción
-- | --
**uint** | Un número entero sin signo.
**bool** | Un valor booleano.
**text** | Una cadena de texto.
**acct** | Una cuenta.
**asset** | Un activo de la cadena de bloques.
**txn** | Una transacción.
**ts** | Un *timestamp*.

Los campos del estado global se pueden acceder, en las funciones o métodos del contrato, mediante su **nombre**, si no entra en conflicto con el de otra variable o constante, o bien con **self.nombre** para dejar claro el acceso al estado global y, así, deshacer posibles conflictos de nombres.

#### Estado local

Como el estado local se almacena en los registros de los suscriptores, es necesario que la cuenta en la que se desea escribir se haya suscrito al contrato.

##### Acceso al estado local

El estado local de una cuenta se encuentra en su campo ***localState***.
He aquí un ejemplo ilustrativo:

```
@optedIn
entry proc vote(t: txn, vote: uint)
  #(1) pre
  if not voteInterval.includes(t.ts) then
    throw("Fuera del período de votación")
  
  if t.sender.localState.voted then
    throw("Ya ha votado.")
  
  #(2) voto
  if vote == 0 then votesAgainst += 1
  else votesInFavor += 1

  t.sender.localState.voted = true
```

##### Cuentas de aplicación

Durante una transacción, el contrato sólo puede escribir en el estado local del emisor de la transacción.
Aunque hay una extensión a esta regla.
Durante la creación del contrato, se puede añadir hasta cuatro cuentas adicionales.
En cualquiera de éstas, se podrá escribir también, siempre, recordemos, se hayan suscrito al contrato.

Estas cuatro cuentas se conocen formalmente como **cuentas de aplicación** (*app accounts*) y deben definirse en el contrato como campos constantes de tipo ***acct***, anotados con ***@app***.
Si aparecen más de cuatro definiciones de este tipo, se propagará un error en tiempo de compilación.
Sintaxis:

```
@app
const nombre: acct<localState: Tipo>

@app
const nombre: acct<localState: Tipo>
```

El estado local de una cuenta debe indicar la interfaz de su estado local, la cual indicará los nombres y tipos de sus datos.
Esto se indica mediante un sintaxis parecía a las plantillas o genéricos de otros lenguajes: `<localState: Tipo>`.

Su inicialización corre a cargo del comando `goal app`.
No se permitirá su escritura en el método constructor.

Si se desea, de las cuentas suscritas al contrato y no marcadas como de aplicación, se puede leer su estado local.
No se puede escribir en él, pero sí leerlo.
En estos casos, puede definirlas como variables o constantes, claro está, sin la anotación **@app**:

```
var|const nombre: acct<localState: Tipo>
```

### Comportamiento

El **comportamiento** (*behaviour*) consiste en las funciones o métodos del contrato que proporcionan su lógica.
Un contrato puede tener tantos métodos como sea necesario y se definen mediante sentencias **fn** o **proc** cuyas sintaxis son como sigue:

```
#función: debe devolver un valor
fn nombre(parámetros): Tipo

#procedimiento: no devuelve nada
proc nombre(parámetros)
```

Cuando se desea invocar un método de un contrato, desde una aplicación cliente, necesitaremos básicamente lo siguiente:

- El identificador de la instancia del contrato.
  Debe quedar claro que un contrato se puede desplegar tantas veces como sea necesario, pero cada una de ellas es independiente.
  Y a cada una de las instancias del contrato se le asocia un identificador de aplicación único.

- La función del contrato que se desea invocar o ejecutar.

- Los argumentos a pasar a la función.

En **Cohen**, *no* se puede sobrecargar las funciones.

#### Parámetros

Un **parámetro** (*parameter*) es una variable o constante cuyo dato se recibe de quien invoca la función.
He aquí su sintaxis:

```
#parámetros variables
nombre: tipo

#parámetros constantes, cuyo valor no podrá modificarse
#en el cuerpo de la función
const nombre: tipo
```

Actualmente, *no* es posible indicar valores predeterminados para los parámetros.

#### Funciones de entrada

De la misma manera que una clase puede tener accesibilidad para indicar que ciertos métodos son públicos y otros protegidos o privados, en los contratos de **Cohen** pasa lo mismo.
Un cliente sólo puede invocar las **funciones de entrada** (*entry functions*) o **métodos de entrada** (*entry methods*), aquéllas que en el contrato se definen como **entry**.
Son similares a las funciones externas de **Solidity** y **Vyper**.
Cualquier función no definida como punto de entrada no podrá ser invocada por los usuarios.

Es importante tener clara una cosa:
cuando se invoque un método de entrada, sus parámetros deben ser siempre de tipo básico, no es posible pasar una estructura como sí es posible en aquéllos que no son de entrada.
Pero como es posible que una secuencia de parámetros pueda asociarse a una interfaz o estructura, es posible definir parámetros como el siguiente:

```
#el uso de const está permitido
nombre: Tipo(nombre, nombre, nombre...)
```

En estos casos, cuando se indica un parámetro de este tipo, lo que se está indicando es un parámetro derivado cuyo valor procede de los indicados como tipo.
Por ejemplo, y sin entrar en programación específica de *blockchain*, supongamos la siguiente estructura de datos:

```
struct Interval
  begin: ts
  end: ts
```

La estructura se define como dos campos.
Pero en las funciones de entrada no se puede pasar datos complejos, sólo básicos.
Si lo deseamos, podríamos hacer lo siguiente:

```
entry proc __init__(t: txn, regInterval: Interval(start, end), voteInterval: Interval(start, end))
```

En esta definición, el método de entrada espera cinco argumentos:

- `t` que representa la transacción en curso.

- `begin` y `end` de un primer intervalo de tiempo.

- ` begin` y `end` de un segundo intervalo de tiempo.

Los parámetros se indican en orden de aparición y sus tipos se extraen de la definición de la estructura o interfaz.
Siguiendo con nuestro ejemplo, el primer argumento a pasar será `t`;
el segundo y el tercero, conformarán `regInterval`;
y el cuarto y el quinto, `voteInterval`.

En **Algorand**, existen algunas funciones de entrada especiales, que tienen un objetivo concreto para las cuales podemos definir su lógica, pero recordemos con un comportamiento y objeto determinado.
Éstas son las siguientes:

Nombre de la función | Tipo | Descripción
-- | -- | --
**\_\_init\_\_** | **proc** | Se invoca para inicializar el estado del contrato. Similar a la función homónima de **Vyper** o a los **constructor**es de **Solidity**.
**\_\_optIn\_\_** | **fn** | Suscripción al contrato por parte de la cuenta emisora de la transacción. Debe devolver **true** si se acepta la suscripción.
**\_\_closeOut\_\_** | **fn** | Solicitud de baja del contrato por parte de la cuenta emisora de la transacción.
**\_\_pay\_\_** | **proc** | Procesamiento de una transacción de pago.

#### Transacciones

En los contratos, las llamadas a los métodos de entrada se realiza mediante transacciones.
Si un método, lo más habitual, necesita acceder a los datos de la transacción en curso, debe indicar como primer parámetro uno con nombre **t** y tipo **txn**.
En estos casos, el compilador le pasará los datos de la transacción en curso.
No existe una variable global a través de la cual acceder a la transacción en curso como ocurre en otros lenguajes.
Hay que indicarlo explícitamente en la lista de parámetros o no se tendrá acceso a sus datos.

Cualquier otra función puede recibir el objeto transacción con cualquier nombre, pero respetando, claro está, el tipo.

Recordemos:

```
entry proc __init__(t: txn, ...)
  #...
```

#### Anotaciones

A continuación, se muestra algunas anotaciones del compilador para proporcionar mayor seguridad a las funciones:

Anotación | Descripción
-- | --
**@view** | Indica que no puede modificar el estado global del contrato. Similar a las funciones **@view** de **Vyper** o al modificador **const** de **Solidity**.

A diferencia de otros lenguajes, *no* existe anotaciones para:

- Definir métodos puros.
  Cualquier función que no necesite acceder al estado, por buenas prácticas, se debe definir fuera del contrato, ya sea en el propio módulo del contrato u otro distinto.

## Ejemplos

### Sistema controlado de votación

Como primer ejemplo, vamos a definir un sistema de votación para aprobar o rechazar una propuesta.
Para ello:

- Sólo se podrá votar durante un intervalo de tiempo.

- Los votantes deben registrarse para así poder votar y deberán hacerlo durante un intervalo de tiempo.
  Esto lo harán suscribiéndose al contrato.

- Los votantes sólo podrán votar una vez y no podrán cambiar su voto.

```
/**
 * Un intervalo de tiempo.
 */
struct Interval
  /**
   * Inicio del intervalo de tiempo.
   */
  const begin: ts

  /**
   * Fin del intervalo de tiempo.
   */
  const end: ts

  /**
   * Comprueba si un timestamp se encuentra dentro del intervalo.
   */
  fn includes(timestamp: ts) = timestamp between begin and end

/**
 * Estructura de los datos almacenados en el estado local de los votantes.
 */
intf LocalState
  /**
   * ¿Ha votado ya?
   */
  voted: bool

/**
 * Una aplicación de votación regulada.
 */
@stateful
export contract PermissionedVotingApp
  /**
   * Cuenta propietaria del contrato.
   */
  @owner
  const owner: acct

  /**
   * Votos a favor.
   */
  var votesInFavor: uint

  /**
   * Votos en contra.
   */
  var votesAgainst: uint

  /**
   * Inicializa el contrato teniendo en cuenta las fechas de inicio y fin de
   * los intervalos de registro y votación.
   */
  entry proc __init__(t: txn, regInterval: Interval(start, end), voteInterval: Interval(start, end))
    self.{
      owner = t.sender
      regInterval
      voteInterval
      votesInFavor = 0
      votesAgainst = 0
    }

  /**
   * Registra al emisor de la transacción para la votación.
   * Debe estar dentro del período de registro.
   */
  entry fn __optIn__(t: txn) = regRound.includes(t.ts)

  /**
   * Solicita darse de baja del proceso de votación.
   * Sólo podrá hacerlo fuera del período de votación.
   * En caso de haber votado, su voto no se dará de baja.
   */
  @optedIn
  entry fn __closeOut__(t: txn) = not voteInterval.includes(t.ts)

  /**
   * Lleva a cabo el voto por parte del emisor de la transacción.
   * Debe estar dentro del período de votación.
   *
   * Consideramos el valor 0 como en contra;
   * cualquier otro valor como a favor de la propuesta que se está votando.
   */
  @optedIn
  entry proc vote(t: txn<localState: LocalState>, vote: uint)
    #(1) pre
    if not voteInterval.includes(t.ts) then
      throw("Fuera del período de votación")
    
    if t.sender.localState.voted then
      throw("Ya ha votado.")
    
    #(2) voto
    if vote == 0 then votesAgainst += 1
    else votesInFavor += 1

    t.sender.localState.voted = true
```

Para compilarlo:

```shell
#genera PermissionVotingApp.teal,
#si queremos otro nombre usar la opción -o
cohen teal PermissionedVotingApp.chn
```

Para obtener una clase de **JavaScript** para interactuar fácilmente con el contrato:

```shell
#genera PermissionVotingApp.js,
#si queremos otro nombre usar la opción -o
cohen js PermissionedVotingApp.chn
```