# MongoDB

MongoDB es una base de datos no relacional. No tiene tablas como las bases de datos relacionales sino que tiene colecciones.

Estas colecciones no estan relacionadas entre ellas como lo estan las tablas en las bases de datos relaciones. En MongoDB (como en otras bases de datos no relacionales) no podemos hacer `JOIN` entre las colecciones.

### Documentos

Dentro de las colecciones, MongoDB guarda documentos. Los documentos se guardan en formato JSON y funcionan de la misma manera. Eso quiere decir que podemos guardar todos los tipos de datos primitivos pero no funciones (que en Javascript es lo que transforma un diccionario o hash en algo que podriamos llamar on objeto con metodos).

Tambien podes guardar documentos anidados y arrays de documentos dentro de un documento.

```javascript
db.myIncredibleCollection.insertOne({
	itemName: '...',
	numberValue: 123,
	arrayOfNumbers: [4, 5, 6],
	arrayOfThings: [
		{ nameOfTheThing: '...' },
		{ nameOfTheThing: '...' },
	]
});
```

### Schema

Tambien a diferencia de las bases de datos relacionales, el schema no esta determinado en la base de datos ya que es posible agregar y quitar pedazos de informacion a los documentos, independientemente de los otros documentos en la coleccion y del estado anterior del mismo documento.

```javascript
db.myIncredibleCollection.updateOne(
	{ 
		_id: ObjectId('...') 
	},
	{
		$set: {
			whateverIWant: 899
		}
	}
);
```

Vale la pena notar que el schema no desaparece, ya que es necesario que los datos tengan una estructura en particular para poder trabajar con ellos.

Por ejemplo, si tuviera una coleccion de animales con su nombre de la siguiente manera:

```javascript
$ db.animals.find()
{ "name": "Fido" }
{ "name": "Pluto" }
```

Mi codigo necesita saber que los documentos dentro de la coleccion de animales tiene un campo `name` para poder imprimir el nombre de cada animal:

```javascript
animals.forEach(function (animal) {
	// Here's where you specify the schema.
	// Documents in the animals collection must
	// have the "name" field.
	console.log(animal.name);
})
```

Por lo tanto el schema existe, pero no se especifica en la base de datos sino en el codigo.

Este tipo de base de datos nos da mayor flexibilidad en el desarollo a cambio de mayor dificultad en la organizacion de los datos. Pero rara vez encontramos un programa en el cual el acceso a los datos es un problema.

### Indices

Los indices tienen el mismo rol en MongoDB que en las bases de datos relacionales: Mapean los valores de cada documento y le facilitan la tarea al motor de busqueda.

Si tuvieramos 5 millones de animales en nuestra coleccion y quisieramos buscar uno en particular por nombre, esa consulta llevaria mucho tiempo en resolverse sin un indice.

```
db.animals.findOne({ name: 'Ronco' });
```

Para encontrar ese animal sin un indice, MongoDB debe recorrer la coleccion entera con sus 5 millones de documentos y comparar el campo `name` de cada documento con el filtro que especificamos en la consulta. Las consultas tambien pueden tener filtros mucho mas complejos, como expresiones regulares o multiples filtros, que pueden hacer que sea todavia mas lenta.

Para crear un indice tenemos que indicar uno o mas campos y la direccion en la cual queremos aplicar el orden para cada campo.

```
db.animals.createIndex({
	name: 1,
	age: -1
})
```

El indice anterior va a tener los documentos de la coleccion de animales ordenados por nombre y por edad (cuando los nombres sean iguales). El indice tambien va a decirle al motor de busqueda en que lugar de la coleccion se encuentra cada valor.

Por ejemplo, si vuelvo a buscar animales con el nombre "Ronco", MongoDB va a saber que esta cerca del final de la coleccion y no va a comparar contra los primeros 3.5 millones de documentos.

El indice le va a indicar que los documentos cuyo nombre empiezan con "R" estan ubicados entre el documento numero 3.500.000 y 4.200.000.

[Clic para ir al siguiente articulo](/mongo/nodejs.md)