# Paralelismo

En este caso la palabra paralelismo se refiere a la capacidad de ejecutar mas de un proceso a la vez al mismo tiempo.

Cuando un programa corre en un unico procesador, cada sentencia se ejecuta al terminar la anterior. Pero cuando usamos mas de un procesador el programa necesita indicarle a un procesador explicitamente que espere la señal del otro.

Para hacer esto en lenguajes como Java se usa una abstraccion llamada `Thread`. Un thread representa un procesador que puede enviar y recibir mensajes para ejecutar o comunicar el resultado de un proceso.

Consideremos el siguiente ejemplo: Tenemos una coleccion de numeros y nuestro programa debe mostrar el doble de cada numero en la coleccion. Para resolver esto podemos hacer una multiplicacion a la vez, en un solo procesador, hasta que hayamos recorrido toda la collection o podemos darle a cada procesador una cantidad igual de numeros, esperar que cada procesador termine de multiplicar, y juntar los resultados.

![paralelismo](/javascript/parallelism.jpg)

Siempre que un problema se pueda resolver ejecutando procesos en paralelo, resolverlo con un solo procesador es teoricamente menos eficiente, excepto en el caso que el problema sea tan sencillo que sea mas el esfuerzo de correrlo en mas de un procesador que en uno solo.

## Event-driven Javascript

Javascript fue originalmente creado para paginas web, aunque ahora tambien se usa para otras cosas.

A las acciones de un usuario en un navegador web, Javascript las llama eventos. Cada vez que apretamos una tecla o movemos el mouse o hacemos click el Javascript del navegador crea y procesa un evento.

Un programa en Javascript corriendo en un navegador web puede suscribirse a estos eventos por ejemplo para saber cuando el usuario mueve el mouse, o cuando apreta Enter en un formulario.

Javascript administra la reaccion a estos eventos usando una cola. Cuando un evento se dispara y causa la ejecucion de una funcion, esta funcion se agrega al final de la cola y se ejecuta cuando terminan todas las funciones que llegaron antes.

Esto genera otro problema con las funciones que corren por mucho tiempo, ya que todas las demas tienen que esperar que termine para que puedan ejecutarse. Para evitar este problema, Javascript permite demorar la ejecucion de una sentencia dentro de una funcion enviandola al final de la cola.

En los siguientes ejemplos tenemos dos funciones que imprimen "Hola mundo" en la consola infinitamente. Una de ellas detiene el programa y la otra no. La que no detiene el programa lo logra enviando cada `console.log` al final de la cola de ejecucion, permitiendole a otras funciones ejecutar.

```javascript
function printForever() {
	while (true) { console.log("Hola mundo"); }
}

function printForever() {
	function print() { console.log("Hola mundo"); }
	setInterval(print, 1)
}
```

La funcion `setInterval` es una de las funciones que permite enviar ejecuciones al final de la cola. En el caso de `setInterval` se envia la funcion en el primer parametro al final de la cola de ejecucion cada 1 milisegundo.

Otra funcion es `setTimeout` que funciona de la misma manera que `setInterval`, pero ejecuta la funcion una sola vez.

## Promises

Promises es otra de las formas que Javascript nos permite ejecutar funciones en el futuro sin bloquear el programa entero.

Las promises funcionan muy parecido a los threads, y para todos los usos practicos son lo mismo: Se inicializan con una tarea (funcion) especifica y comunican una respuesta al finalizarla.

Una promise es un objeto que se crea a partir de una funcion y permite asignar funciones para recibir el resultado de la ejecucion de dicha funcion.

```javascript
var promise = new Promise(function () {
	// Code to execute asynchronously.
});

promise.then(function () {
	// Code to execute when the promise finishes
	// executing successfully.
});

promise.catch(function () {
	// Code to execute when the promise finishes
	// executing with error
});
```

Los metodos `then` y `catch` nos permite decidir que hacer con la respuesta en caso de exito o error. La responsabilidad de decidir si la tarea fue ejecutada con exito o no es de la funcion interna de la promise.

```javascript
new Promise(function (resolve, reject) {
	if (/* check for success */) {
		resolve(); // Task finished successfully.
	}
	else {
		reject(); // Task finished not successfully.
	}
})
```

La funcion con la que inicializamos la promise recibe dos funciones por parametros: Una para finalizar la tarea existosamente y otra para finalizarla con error. Los errores que se disparan dentro de la funcion tambien finalizan la tarea.

```javascript
new Promise(function (resolve, reject) {
	// The following is equivalent to
	// calling the reject function.
	throw new Error('Oops!');
})
```

Las funciones `resolve` y `reject` a su vez reciben parametros. Estos parametros van a ser el resultado de la ejecucion de la funcion dentro de la promesa y son los que luego estaran disponibles dentro de `then` o `catch`.

```javascript
var promise = new Promise(function (resolve, reject) {
	if (/* check for success */) {
		resolve(2+2);
	}
	else {
		reject('Oops!');
	}
});
promise.then(function (result) {
	// result would be 2+2
});
promise.catch(function (result) {
	// result would be 'Oops!'
});
```

### Paralelismo en Javascript

Especialmente en Nodejs se encuentra funciones que deben ser ejecutadas asincronicamente, o sea en paralelo al proceso principal. Todas las tareas relacionadas a I/O como leer un archivo o ejecutar una consulta a una base de datos son tareas asincronicas.

En estos casos podemos encontrar dos alternativas: Callbacks o promises. Casi todas las librerias ofrecen la opcion de usar promises, y si no la ofrecen permiten pasar funciones por parametro para ser ejecutadas al terminar la tarea (callbacks).

```javascript
// Read a file (callbacks)

var fs = require('fs');

fs.readFile('filename.txt', function (error, contents) {
	// contents is a string containing the
	// contents of the file.
})
```

```javascript
// MongoDB query

var connection = connectToMongo();

connection
	.collection('items')
	.findOne({ available: true }) // Query returns promise
	.then(function (result) {
		// result is whatever item
		// was found in the database.
	})
```

Es una convencion nuestra hacer que todas las funciones devuelvan una promise siempre. De esta manera estamos cubiertos cualquiera sea la tarea que una funcion debe cumplir, independientemente de si es asincronica o no.

```javascript
function sum(x, y) {
	return Promise.resolve(x+y);
}

sum(2, 3).then(function (result) {
	// result is 5
});
```

## Chaining

Es posible encadenar promesas para coordinar la ejecución de tareas asincrónicas. Esto se puede hacer utilizando la promise que devuelve ejectuar la función `then`.

La función `then` se ejecuta cuando la promise se completa, y a su vez devuelve una promise. Esto nos permite agendar una tarea asincrónica para ejecutarse al terminar la tarea anterior.

```javascript
var promise1 = new Promise(function (resolve) {
	// Run async operation 1
});

var promise2 = promise1.then(function () {
  // Run async operation 2
})
 
var promise3 = promise2.then(function () {
  // Run async operation 3
})
```

No es recomendable asignar mas de una función a la misma promise usando `then`, es confuso y puede generar bugs.

```javascript
/* DO NOT DO IT LIKE THIS! */

var promise1 = new Promise(function (resolve) {
	console.log('Task 1');
	resolve();
});

var promise2 = promise1
  .then(function () {
    console.log('Task 2');
  })
  .then(function () {
  	console.log('Task 3');
  })

var promise3 = promise1
  .then(function () {
	console.log('Task 4');
  })
  .then(function () {
  	console.log('Task 5');
  });

// Prints:
// - Task 1
// - Task 2
// - Task 4
// - Task 3
// - Task 5
```

Para seguir con la guía, clic [acá](/inituy/onboarding/src/master/mongo/basics.md).