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

La firma de la función `it` podría ser de la siguiente manera.

```
Function(String, Function(Function))
```