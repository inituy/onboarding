# Funciones en Javascript

En esta parte de la guía vamos a hablar sobre el uso de funciones en Javascript.

Aunque las nuevas versiones de Javascript vienen con otra sintaxis para crear funciones, nosotros vamos a crear nuestras funciones de la siguiente manera:

```
function nameOfTheFunction(param1, param2, param3) {
	// Contents of the function
}
```

La palabra clave `function` le indica al interprete que estamos definiendo una función. Seguido por `nameOfTheFunction`, el interprete va a entender que la función que estamos definiendo se llama de esa manera.

Los paréntesis después del nombre de la funcion encierran los parámetros de la función. Estos parámetros van a adoptar diferentes valores según como se use la función.

Al final están los corchetes `{}` que encierran el cuerpo de la función que esta compuesto de las sentencias que se van a ejecutar cuando ejecutemos nuestra función.

A partir de la declaración de la función en adelante, podemos usar el nombre de la función (en este caso `nameOfTheFunction`) seguido por paréntesis para ejectuar las sentencias dentro del cuerpo de la función, con los valores que queramos asignarle a los parametros.

Por ejemplo:

```
var result = nameOfTheFunction(1, '2', "tres");
```

## Funciones como parametros

Los parametros de una función pueden ser de cualquier tipo, incluso otras funciones.

En el paradigma de programación funcional, el que usamos nosotros, los programas se construyen principalmente combinando funciones de distintas maneras. Es por eso que en esta parte de la guía vamos a ver diferentes maneras de combinar funciones en Javascript.

### Callbacks

En algunas librerias [vamos a encontrar que las funciones aceptan un parametro llamado "callback"](https://nodejs.org/api/http.html#http_request_end_data_encoding_callback).

Los callbacks son funciones que se usan para notificar al que llama una funcion de que la tarea está terminada. En general la tarea en cuestión es asincrónica, o sea que se ejecuta en otro hilo para que el programa no pare mientras la tarea se completa.

Por ejemplo en NodeJS, la [siguiente función](https://nodejs.org/api/fs.html#fs_fs_readfile_path_options_callback) lee un archivo y ejecuta el callback cuando el contenido del archivo está disponible:

```
var fs = require('fs');

var callback = function (error, fileContents) {
	// fs.readFile will execute this function when
	// its done reading from the file system.
};

fs.readFile('../filename.txt', callback);
```

En algunos casos podemos encontrar funciones que reciben callbacks, que a su vez tambien reciben callbacks. Por ejemplo, [la función `it` del framework de testing Jasmine funciona de esa manera](https://jasmine.github.io/tutorials/async#callbacks).

```
it('does a thing', function(done) {
  someAsyncFunction(function(result) {
    expect(result).toEqual(someExpectedValue);
    done();
  });
});
```

`it` es una función cuyo segundo parámetro es un callback que se ejecuta cuando la herramienta está lista para correr los tests. Al mismo tiempo, este callback va a recibir por parametro otro callback para indicarle al framework que el test debe terminar.

La firma de la función `it` podría ser de la siguiente manera:

```
Function(String, Function(Function))
```

### Aplicación parcial

Aplicación parcial es una técnica que sirve para fabricar funciones de menor cantidad de parametros a partir de una funcion con mayor cantidad de parametros.

El ejemplo mas utilizado para explicar esta técnica son las operaciones matematicas. La siguiente función va a devolver la suma de dos numeros:

```
function sum(x, y) { return x + y; }
var three = sum(2, 1);
```

Podríamos utilizar aplicación parcial para resolver este problema, devolviendo una nueva función que tenga el primer parámetro fijo y espere solo un parámetro para completar la suma:

```javascript
function sum(x) {
	return function (y) {
		return x + y;
	}
}
var sumTwo = sum(2);
var three = sumTwo(y);
var five = sum(3)(2);
```

#### En el mundo real

La técnica de aplicación parcial se usa en el mundo real para lograr la [inyección de dependencias](https://en.wikipedia.org/wiki/Dependency_injection).

Por ejemplo, un programa del mundo real quiere guardar registros en una base de datos. Esta funcionalidad esta compuesta por las reglas de negocio (como debe guardarse el registro según el problema que el programa soluciona) y por el código de base de datos (las queries que hay que ejectura para que los datos se guarden).

La función `saveItem` que pertenece al modulo de las reglas de negocio no debe saber como persistir el registro en la base de datos. Entonces sus parámetros van a ser los datos del registro y una función que los persista en la base de datos:

```javascript
function saveItem(persistItem, itemData) {
	// Do stuff before persisting item
	persistItem(itemData);
}

saveItem(databasePersistItem, { id: 1 });
saveItem(databasePersistItem, { id: 2 });
saveItem(databasePersistItem, { id: 3 });
```

Podemos obtener una función `saveItem` que no necesite que la función de base de datos cada vez que se la llame utilizando aplicación parcial:

```javascript
function saveItem(persistItem, itemData) {
	// Do stuff before persisting item
	persistItem(itemData);
}

function saveItemToDatabase() {
	return function (itemData) {
		return saveItem(databasePersistItem, itemData);
	};
}

var saveItem2 = saveItemToDatabase();

saveItem2({ id: 1 });
saveItem2({ id: 2 });
saveItem2({ id: 2 });
```

En un programa real, la inyección de dependencia ocurre generalmente en el arranque. Usando aplicación parcial podemos definir todas las dependencias de nuestras funciones en el arranque del programa y utilizar las funciones parcialmente aplicadas de ahí en adelante:

```javascript
function saveItem(persistItem, itemData) {
	// Do stuff before persisting item
	persistItem(itemData);
}

function saveItemToDatabase(databaseConnection) {
	return function (itemData) {
		return saveItem(databasePersistItem(databaseConnection), itemData);
	};
}

function databasePersistItem(databaseConnection) {
	return function (itemData) {
		// Run queries using item data.
	};
}

function init() {
	var databaseConnection = new DatabaseConnection();
	return {
		saveItem: saveItemToDatabase(databaseConnection)
	};
}

var app = init();

app.saveItem({ id: 1 }) // Saves item to the database.
```

Esta complejidad agregada nos trae como beneficio la capacidad de desacoplar nuestro código, separando la lógica de negocio de la base de datos, y permitiendonos correr tests sobre la lógica de negocio sin utilizar la base de datos.

Consideremos el siguiente ejemplo en el cual creamos un test para la función `saveItem` sin tener que correr queries en la base de datos:

```javascript
describe('saveItem', function () {
	var saveItem = require('./save_item.js');
	var persistedItemData = null;

	function fakePersistItem(itemData) {
		persistedItemData = itemData;
	}

	it('persists item data', function () {
		var itemData = { id: Math.random() };
		saveItem(fakePersistItem, itemData);

		/* Did the function persist the data
		   using the function that we passed
		   as parameter? */
		expect(persistedItemData).toBe(itemData);
	});
})
```

Esto es todo por ahora en cuanto a funciones.

Para seguir con la guía, clic [acá](https://bitbucket.org/inituy/onboarding/src/master/javascript/parallelism.md).