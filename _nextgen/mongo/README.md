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

### `deleteOne` &