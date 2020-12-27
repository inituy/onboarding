# MongoDB

MongoDB es una base de datos no relacional. En MongoDB guardamos los datos en forma de *documentos* que se organizan en *colecciones* y creamos *indices* para mejorar el rendimiento de las consultas.

### Funcion

MongoDB es una herramienta super flexible gracias a que es no relacional y no requiere que definamos el schema de antemano. Nos permite persistir datos rapidamente para crear tests automatizados y nos da una API para consultarlos que es mucho mas accesible que SQL.

![mongo](./mongo.png)

### `insertOne` & `insertMany`

Para agregar un nuevo documento a una coleccion usamos `insertOne`. Podemos usar `insertMany` si tenemos mas de una documento y queremos agregarlos todos al mismo tiempo.

No necesitamos hacer nada antes de poder agregar un documento a una coleccion, incluso si la coleccion no existe, MongoDB la crea automaticamente.

Desde el cliente para consola:

```javascript
> db.users.insertOne({ email: 'random3@email.com' })
{
        "acknowledged" : true,
        "insertedId" : ObjectId("5fe802e9538755d73dbb3ca2")
}
```

```javascript
> db.users.find()
{ "_id" : ObjectId("5fe7fad934b392788c01df72"), "email" : "random1@email.com" }
{ "_id" : ObjectId("5fe7fb9534b392788c01df73"), "email" : "random2@email.com" }
{ "_id" : ObjectId("5fe802e9538755d73dbb3ca2"), "email" : "random3@email.com" } 
```

Desde Javascript:

```javascript
var user = { email: 'random3@email.com' };
var connection;

connectToMongo()
  .then(function (c) {
    connection = c;
  })
  .then(function () {
    return connection
      .collection('users')
      .insertOne(user);
  })
  .then(function () {
    console.log(user);
    // { _id: ObjectId("5fe802e9538755d73dbb3ca2"), email: 'random3@email.com' }
  })
  .then(function () {
    return connection
      .collection('users')
      .find()
      .toArray();
  })
  .then(function (documents) {
    console.log(documents);
    // [
    //   { _id: ObjectId("5fe7fad934b392788c01df72"), email: "random1@email.com" },
    //   { _id: ObjectId("5fe7fb9534b392788c01df73"), email: "random2@email.com" },
    //   { _id: ObjectId("5fe802e9538755d73dbb3ca2"), email: "random3@email.com" }
    // ]
  })
```

La firma de la funcion `insertMany`, es apenas diferente. Recibe un array en vez de un objeto.

```javascript
> db.users.insertMany([ { email: 'random4@email.com' }, { email: 'random5@email.com' } ])
{
        "acknowledged" : true,
        "insertedIds" : [
                ObjectId("5fe80501531d44e7d62f5c74"),
                ObjectId("5fe80501531d44e7d62f5c75")
        ]
}
```

```javascript
> db.users.find()
{ "_id" : ObjectId("5fe7fad934b392788c01df72"), "email" : "random1@email.com" }
{ "_id" : ObjectId("5fe7fb9534b392788c01df73"), "email" : "random2@email.com" }
{ "_id" : ObjectId("5fe802e9538755d73dbb3ca2"), "email" : "random3@email.com" }
{ "_id" : ObjectId("5fe80501531d44e7d62f5c74"), "email" : "random4@email.com" }
{ "_id" : ObjectId("5fe80501531d44e7d62f5c75"), "email" : "random5@email.com" }
```

### `deleteOne` & `deleteMany`

Estas funciones eliminan documentos de una coleccion. Igual que con `insertOne` e `insertMany`, la diferencia entre `deleteOne` y `deleteMany` esta en que el segundo recibe un array.

Lo que recibe por parametro es un objeto que MongoDB va a comparar con los documentos de la coleccion. Cualquier documento que tenga el mismo valor para cada una de las claves es candidato para la operacion. Por supuesto, tambien existen operadores que modifican este comportamiento y permiten ejecutar consultas mas complejas.

Por ejemplo, vamos a llamar la funcion `deleteOne` con `{ _id: ObjectId("5fe7fad934b392788c01df72") }` como parametro. Como el proposito de la funcion es eliminar un solo documento, MongoDB se va a quedar con el primero que encuentra que cumpla con las condiciones. En este caso, la condicion es que el campo `_id` debe ser `ObjectId("5fe7fad934b392788c01df72")`.

Desde la consola es asi:

```javascript
> db.users.deleteOne({ _id: ObjectId("5fe7fad934b392788c01df72") })
{ "acknowledged" : true, "deletedCount" : 1 }
```

```javascript
> db.users.find()
{ "_id" : ObjectId("5fe7fb9534b392788c01df73"), "email" : "random2@email.com" }
{ "_id" : ObjectId("5fe802e9538755d73dbb3ca2"), "email" : "random3@email.com" }
{ "_id" : ObjectId("5fe80501531d44e7d62f5c74"), "email" : "random4@email.com" }
{ "_id" : ObjectId("5fe80501531d44e7d62f5c75"), "email" : "random5@email.com" }
```

Desde Javascript:

```javascript
var query = {
  _id: mongoObjectId('5fe7fad934b392788c01df72')
};

connectToMongo().then(function (connection) {
  return connection
    .collection('users')
    .deleteOne(query);
})
```

### `findOne`

Esta funcion es la que usamos cuando queremos consultar la base de datos y conseguir un solo documento. Recibe un objeto de la misma forma que usamos arriba con la funcion `deleteOne` y devuele el primer documento que encuentra.

Desde la consola:

```javascript
> db.users.findOne({ email: 'random2@email.com' })
{
        "_id" : ObjectId("5fe7fb9534b392788c01df73"),
        "email" : "random2@email.com"
}
```

Desde Javascript:

```javascript
var query = { email: 'random2@email.com' };

connectToMongo()
  .then(function (connection) {
    return connection
      .collection('users')
      .findOne(query);
  })
  .then(function (result) {
    // ...
  })
```

### `find`

La funcion `find` es diferente a `findOne` porque no devuelve un documento sin un cursor. Para no cargar todos los posibles resultados de una consulta todos a la vez, MongoDB devuelve este cursor que sabe cuales son los resultados y le permite al usuario navegarlos.

Desde la consola:

```javascript
> db.users.find()
{ "_id" : ObjectId("5fe7fb9534b392788c01df73"), "email" : "random2@email.com" }
{ "_id" : ObjectId("5fe802e9538755d73dbb3ca2"), "email" : "random3@email.com" }
{ "_id" : ObjectId("5fe80501531d44e7d62f5c74"), "email" : "random4@email.com" }
{ "_id" : ObjectId("5fe80501531d44e7d62f5c75"), "email" : "random5@email.com" }
```

En este caso son pocos los resultados y entonces el cursor imprime todos los resultados y desaparece. Si fueron mas resultados, tendriamos que correr `it` para seguir iterando sobre el cursor para conseguir la siguiente pagina de resultados.

Desde Javascript:

```javascript
connectToMongo()
  .then(function (connection) {
    return connection
      .collection('users')
      .find();
  })
  .then(function (cursor) {
    cursor.forEach(function (user) {
      // ...
    });
  })
```

`find` tambien recibe un objeto como parametro que funciona igual que `findOne` y `deleteOne`.

### `updateOne` & `updateMany`

Estas funciones se usan para modificar documentos. El primer parametro de ambas funciona igual que en `deleteOne` y `deleteMany`, con la misma diferenciacion entre `One` y `Many`.

`updateOne` y `updateMany` tambien reciben un segundo parametro que le va a explicar a MongoDB como debe cambiar los documentos a medida que los encuentra. Este segundo parametro es un objeto y sus claves (en el primer nivel) deben ser las que corresponden a las operaciones de MongoDB.

En el siguiente ejemplo vamos a actualizar uno de los usuarios y le vamos a agregar un nombre `{ name: 'Random User 3' }` usando la operacion `$set`:

Desde la consola:

```javascript
> db.users.updateOne({ email: 'random3@email.com' }, { $set: { name: 'Random User 3' } })
{ "acknowledged" : true, "matchedCount" : 1, "modifiedCount" : 1 }
```

```javascript
> db.users.find()
{ "_id" : ObjectId("5fe7fb9534b392788c01df73"), "email" : "random2@email.com" }
{ "_id" : ObjectId("5fe802e9538755d73dbb3ca2"), "email" : "random3@email.com", "name" : "Random User 3" }
{ "_id" : ObjectId("5fe80501531d44e7d62f5c74"), "email" : "random4@email.com" }
{ "_id" : ObjectId("5fe80501531d44e7d62f5c75"), "email" : "random5@email.com" }
````

Desde Javascript:

```javascript
var query = { email: 'random3@email.com' };
var update = { $set: { name: 'Random User 3' } };

connectToMongo().then(function (connection) {
  return connection
    .collection('users')
    .updateOne(query, update);
})
```

### Object IDs

...