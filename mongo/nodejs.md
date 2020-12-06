# MongoDB en NodeJS

Hay dos cosas que es necesario mencionar para trabajar con MongoDB en NodeJS: Conexiones y ObjectIds

### Conexiones

En general creamos Singletons que nos permiten mantener la menor cantidad de conexiones abiertas con el servidor de MongoDB.

En NodeJS la funcion que nos da la conexion activa se ve asi:

```javascript
var MongoClient = require('mongodb').MongoClient;

function connect() {
	var user = process.env.MONGO_USER;
	var pass = encodeURIComponent(process.env.MONGO_PWD);
	var host = process.env.MONGO_HOST;
	var name = process.env.MONGO_NAME || 'test';

	// If MONGO_HOST is not defined we connect
	// to the localhost.
	var url = process.env.MONGO_HOST
		? `mongodb+srv://${user}:${pass}@${host}/${name}?retryWrites=true&w=majority`
		: 'mongodb://localhost:27017';

	// Returns a promise because connecting to the
	// database is an async task.
	return Promise.resolve()
		.then(function () {
			// Reuse the connection if already exists.
			// In Javascript you can set properties
			// to functions and here we use it to attach
			// the client to the `connect` function.
			if (connect.client) return connect.client;

			// `MongoClient.connect` returns a promise
			// because its an async task. This comes from
			// the official MongoDB package.
			return MongoClient.connect(url, {
				useNewUrlParser: true,
				useUnifiedTopology: true
			});
		})
		.then(function (newClient) {
			// Here `newClient` will either be the
			// result of `MongoClient.connect`` or the
			// already existing client stored in
			// `connect.client`.
			connect.client = newClient;

			// Select the database. This may be unnecessary
			// if we're using the full database URL.
			return connect.client.db(name);
		})
		.catch(function (error) {
			// Capture and print the error raised
			// by `MongoClient.connect`.
			console.log('MONGO CONNECTION ERROR', error);
		});
};

// This runs only once on NodeJS and then `connect`
// remains the same throughout the execution of the
// program. That's why we can store the client to it
// and not worry about it getting lost and a new
// connection being created every time.
module.exports = connect;
```

Conectarse a la base de datos es una tarea asincronica y por lo tanto la funcion `connect` devuelve una promise.

```javascript
var mongoConnect = require('...');

mongoConnect().then(function (mongoConnectionClient) {
	// `findOne` returns a promise that
	// we can work with in the next `then`.
	return mongoConnectionClient
		.collection('animals')
		.findOne({ name: 'Ronco' })
});
```

La sintaxis del cliente de MongoDB en NodeJS es diferente a la que encontramos cuando corremos consultas directamente en la consola.

### ObjectIds

MongoDB asigna un campo `_id` a cada documento. Este campo es de un tipo en particular y puede confundirse con un string si no se presta atencion.

Por ejemplo, si tuvieramos un documento `{ "_id": ObjectId("asd123") }` en la coleccion de animales, la siguiente consulta no funcionaria:

```javascript
mongoConnect()
	.then(function (db) {
		return db
			.collection('animals')
			.findOne({ _id: "asd123" });
	})
	.then(function (result) {
		// Here `result` is null.
	});
```

Debemos usar la funcion `ObjectId` para transformar el string `asd123` en una ID de documento valida y que el motor de busqueda encuentre lo que estamos buscando.

```javascript
var objectId = require('mongodb').ObjectId;

mongoConnect()
	.then(function (db) {
		return db
			.collection('animals')
			.findOne({ _id: objectId("asd123") });
	})
	.then(function (result) {
		// Here `result` is the document we
		// were looking for.
	});
```

[Clic aca para seguir con la guia.](/architecture/index.md)