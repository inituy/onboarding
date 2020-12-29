# Javascript

En esta parte del curso vamos a repasar algunos conceptos basicos de Javascript y otros no tan basicos. Javascript es nuestro lenguaje preferido porque es sencillo (a pesar de su sintaxis) y sirve tanto para frontend como para backend.

Vamos a empezar hablando de las funciones como el bloque fundamental del lenguaje sobre lo que vamos a construir todo lo demas.

### Funciones

Asi como un programa es un conjunto de sentencias que se ejecutan una despues de la otra, una funcion tambien se puede ver como una serie de sentencias que vamos a ejecutar.

Los elementos que componen una funcion son los siguientes:

1. *Parametros*: Son un conjunto de variables que reciben su valor cada vez que se llama la funcion. Por ejemplo, si tuvieramos una funcion para sumar dos numeros, los numeros serian parametros y cambiarian con cada ejecucion de la funcion.
2. *Cuerpo*: El cuerpo de una funcion es donde estan las sentencias que se ejecutan cuando se ejecuta la funcion.
3. *Retorno*: Es el resultado de ejecutar la funcion. En el ejemplo de la suma, el retorno de la funcion es el resultado de la suma.

![functions](./javascript_functions.png)

En Javascript, las funciones son objetos de "primera clase". Esto significa que una funcion en Javascript es un objeto propiamente dicho, que puede tener propiedades y hasta metodos.

No todos los lenguajes tienen esta caracteristica. En muchos lenguajes lo mas parecido a una funcion que podemos encontrar son los metodos, que solo pueden existir dentro de los objetos (en OOP) y nunca por si solos.

Como las funciones son objetos podemos guardarlas en variables y moverlas a distintas partes de nuestro programa. En la proxima parte vamos a ver como una funcion puede recibir a otra en sus parametros y que se puede hacer con eso.

### Callbacks

Se le llama "callback" a una funcion que se le pasa por parametro a otra funcion. Se llama asi porque cuando pasamos una funcion por parametro es para que se ejecute y recuperar el control de flujo.

![callbacks](./javascript_callbacks.png)

La palabra "call" significa "llamar" o "ejecuar" y "back" se puede traducir como "de vuelta" o "de regreso". En ingles decimos "call me back" cuando queremos que alguien nos "vuelva a llamar".

En codigo un callback se ve asi:

```javascript
function encontrarDato(callback) {
  // Ejecutar consulta...
  callback(resultadoDeLaConsulta);
}

function procesarDato(dato) {
  // Procesar dato...
}

// Ejecutamos la funcion "encontrarDato"
// y le pasamos "procesarDato" como parametro.
// Decimos que la funcion "procesarDato" es un
// "callback".
encontrarDato(procesarDato);

// Ya que "encontrarDato" recibe un callback,
// podemos pasarle otra funcion para
// procesar el dato de una manera distinta.
encontrarDato(function (dato) {
  // Procesamos el dato de otra manera diferente.
})
```