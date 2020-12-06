# Arquitectura

Para diseñar la arquitectura de nuestros programas usamos algunos principios de Clean Architecture segun los explica [Robert C. Martin](http://cleancoder.com).

Aunque a veces estamos limitados por el lenguaje de programacion que usamos, usamos el paradigma de programacion funcional. Cuando usamos lenguajes donde las funciones no son "first-class citizens" usamos el patron [Command](https://en.wikipedia.org/wiki/Command_pattern) para crear objetos ejecutables que funcionen de manera parecida funciones.

Del paradigma funcional tambien tomamos los patrones de diseño que nos permiten entender y razonar mejor nuestros programas.

### Clean code

Los principios que tomamos de Clean Architecture son los siguientes.

#### Architecture is about intent

Sabemos que tenemos una buena arquitectura cuando podemos entender lo que hace un programa rapidamente, idealmente sin tener que leer el codigo.

Nuestros programas estan diseñados de manera que leer los nombres de los archivos ya da una idea de para que se escribio el programa. Por ejemplo, si el programa tiene un sistema de usuarios vas a encontrar un archivo llamado `add_user.js` o parecido. Si es un sistema bancario vas a encontrar archivos llamados `open_bank_account.js` y `close_bank_account.js`.

La base de datos que el programa usa o el mecanismo que el usuario usa para darle input al programa estan desacoplados de la logica de negocio y en general este tipo de detalles (irrelevantes entender que es lo que hace el programa) estan definidos en algun archivo que no esta a primera vista.

Para lograr una arquitectura limpia y facil de entender, todos nuestros programas tienen en su interfaz publica o API una serie de "casos de uso" o "acciones" que el programa puede llevar a cabo.

Todas estas funciones se pueden encontrar en la misma carpeta.

```
./app/
	./app/actions/
		./app/actions/add_user.js
		./app/actions/open_bank_account.js
		./app/actions/close_bank_account.js
./spec/
		./app/actions/add_user_spec.js
		./app/actions/open_bank_account_spec.js
		./app/actions/close_bank_account_spec.js
```

Las acciones o casos de uso determinan en su codigo que otras funciones se usan y en que orden. De esta manera logramos que entender un caso de uso del programa sea casi tan sencillo para leer instrucciones.

```javascript
function addUser(payload) {
	return Promise.resolve(payload)
		.then(validateNewUser)
		.then(validateNewUserDoesntAlreadyExist)
		.then(saveNewUser)
		.then(saveNewUserSession)
		.then(presentNewUser);
}
```

Esto tambien nos permite organizar la logica de negocio en capas: Leer el nombre de los archivos dentro de la carpeta `actions` nos da una idea al mas alto nivel de lo que hace el programa. Leer el codigo de una accion nos da una vision a alto nivel de cual es la logica detras de esa accion.

De ahi en adelante podemos ver con lujo de detalles como funciona cada parte del sistema mirando el codigo del resto de las funciones. Las dependencias (como la conexion a la base de datos) se inyectan al inicializar el programa y no se encuentran en el codigo de logica de negocio.

#### Clear boundaries

Nuestros programas son simples y faciles de entender en tanto los distintos modulos de nuestro programa no se mezclen.

Podemos determinar las modulos de nuestro programa segun su proposito: Las acciones o casos de uso son un modulo, las funciones que validan input de usuario pertenecen a otro modulo, las funciones que persisten datos a otro y las funciones que preparan el output a otro. Pueden haber mas modulos segun la funcionalidad que el programa necesite, por ejemplo si necesita enviar emails.

Las funciones dentro de un modulo se involucran lo menos posible con las funciones de otros modulos y cada funcion se limita a lo que le corresponde segun el modulo en el que esta. De hecho, las unicas funciones que tienen permitido llamar funciones de otros modulos son las acciones y tambien es su unica responsabilidad.

Cosas como la conexion a la base de datos y otros third party estan completamente fuera del programa y se inyectan al utilizarlo. Por ejemplo, quien invoque la funcion que persiste un usuario debe tambien indicarle como guardarlo en la base de datos:

```javascript
function saveNewUser(dependencies) {
	return function (payload) {
		dependencies
			.persistUser({
				email: payload.user.email,
				password: payload.user.password
			})
			.then(function () {
				// Do whatever after saving
			});
	}
}

describe('saveNewUser', function () {
	var userDataUsed;

	var fn = saveNewUser({
		persistUser: function (userData) {
			userDataUsed = userData;
			return Promise.resolve();
		}
	});

	it('persists data', function (done) {
		var payload = {
			user: {
				email: '...',
				password: '...'
			}
		};
		fn(payload).then(function () {
			expect(userDataUsed).toEqual({
				email: payload.user.email,
				password: payload.user.password
			});
			done();
		});
	});
});
```

Las acciones tampoco deben saber sobre la base de datos. Eso quiere decir que la funcionalida de base de datos siempre viene desde afuera de la fachada de nuestro programa (en un programa en NodeJS, este seria el archivo `index.js`).

#### I/O as a plugin / Delayed decisions

En general, todo lo que no es logica de negocio es I/O. Leer una request HTTP, enviar un email o guardar datos en la base de datos son procesos de input/output.

Nuestro programa no debe tener ninguna pista de I/O en su logica de negocio. Como se explico en la seccion anterior, la funcionalidad de I/O viene desde fuera de los limites de nuestra logica de negocio.

El beneficio que recibimos de organizar nuestro codigo de esta manera es que la gran mayoria de las decisiones, como que persistencia vamos a usar, de donde va a venir el input de nuestro programa o que servicio de emails vamos a usar, pueden tomarse despues de terminar con toda la logica de negocio.

Si miramos el codigo de mas arriba, podemos ver como hariamos para crear la logica de negocio que persiste un usuario de nuestro sistema sin saber que base de datos vamos a usar (o si vamos a usar una).

La decision de que base de datos usar se puede tomar al final del desarrollo. Incluso podemos trabajar simultaneamente en la logica de negocio y en la persistencia. De hecho, podriamos delegar completamente el moduloe de persistencia a un equipo de trabajo distinto.

### Pipeline pattern