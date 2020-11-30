# Funciones en Javascript

En esta parte de la guía vamos a hablar sobre el uso de funciones en Javascript.

Aunque las nuevas versiones de Javascript vienen con otra sintaxis para crear funciones, nosotros vamos a crear nuestras funciones de la siguiente manera:

```
function nameOfTheFunction(param1, param2, param3) {
	// Contents of the function
}
```

La palabra clave `function` le indica al interprete que estamos definiendo una función. Seguido por `nameOfTheFunction`, el interprete va a entender que la función que estamos definiendo se llama de esa manera.

Los paréntesis después del nombre de la funcion encierran los parámetros de la función. Estos parámetros van a adoptar diferentes valores según como sea use la función.

Al final están los corchetes `{}` que encierran el cuerpo de la función que esta compuesto de las sentencias que se van a ejecutar cuando ejecutemos nuestra función.

A partir de la declaración de la función en adelante, podemos usar el nombre de la función (en este caso `nameOfTheFunction`) para ejectuar las sentencias dentro del cuerpo de la función, con los valores que queramos asignarle a los parametros.

Por ejemplo:

```
var result = nameOfTheFunction(1, '2', new Tres());
```

## 