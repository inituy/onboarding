# Test-driven development (En construccion)

TDD es una disciplina que consiste de crear pruebas automatizadas (tests) antes de crear el codigo de aplicacion. Practicar esta disciplina nos obliga a encarar el desarrollo desde otro punto de vista y nos da beneficios que no tendriamos de otra manera, como por ejemplo, poder confiar que cuando nuestras pruebas pasan el codigo funciona.

### Que es un test

Una *test suite* es un programa que escribimos en paralelo con el codigo de aplicacion que se encarga de usar el programa principal y evaluar su funcionamiento.

![Test](./tdd_1.png)

Cuando escribimos una prueba automatizada tenemos que dejar de lado la actitud creativa que nos acompaña cuando buscamos la solucion a un problema y empezar a ver el codigo desde un punto de vista critico. Crear un test no es un ejercicio de creatividad sino de control de calidad.

Cambiar de tester a developer y viceversa puede ser particularmente dificil, y quizas sea la parte mas dificil de practicar test-driven development.

#### Composicion

Un test tiene tres etapas que van en este orden: Precondicion, ejecucion y evaluacion.

La etapa de precondicion se usa para crear el contexto necesario para poder ejecutar la funcion en cuestion de manera significativa. Por ejemplo, si estuvieramos probando una funcion que busca un usuario en la base de datos, usariamos la etapa de precondicion para insertar ese usuario. Si no tenemos el usuario en la base de datos, ejecutar la funcion no nos seria util ya que no podriamos evaluar si funciona como queremos o no.

En la segunda etapa vamos a ejecutar la funcion. Siguiendo con el ejemplo de la funcion que busca el usuario, este es el momento en el que llamamos a la funcion con los parametros que corresponda.

La ultima etapa es la de evaluacion. En base a los parametros que le pasamos en la etapa de ejecucion, vamos a verificar que el resultado de la funcion es lo que esperabamos. En el caso de nuestro ejemplo vamos a ver si la funcion encontro el usuario.

En un mismo test vamos a repetir estas tres etapas tantas veces como casos de uso tenga la funcion.

Nuestro ejemplo tiene por lo menos dos casos de uso: El caso en el que la ID corresponde con un usuario en la base de datos y el caso en el que no.

```javascript
/* Usamos Jasmine para crear tests en
   Javascript. Las funciones `describe`,
   `beforeAll` e `it` vienen definidas
   en la libreria. */
describe('buscarUsuario', function () {
  var usuario;

  /* `beforeAll` se va a ejecutar antes
     que los tests. Aca vamos a definir
     nuestras precondiciones. */
  beforeAll(function () {
    usuario = { id: 123, email: 'usuario@servicio.com' };
    baseDeDatos.insertar(usuario); // Asumimos que funciona!
  });

  /* `it` se usa para expresar el caso de
     uso. Aca vamos a ejecutar la funcion
     y evaluar el resultado. */
  it('encuentra el usuario', function () {
    /* Ejecutamos la funcion con el ID de
       usuario correcto y verificamos
       que devuelva el usuario. */
  });

  it('no encuentra el usuario', function () {
    /* Ejecutamos la funcion con el ID de
       usuario incorrecto y verificamos
       que devuelva `null`. */
  });
});
```

### Tipos de test

Un programa se puede probar de muchas maneras diferentes. Podemos crear pruebas que simulen ser personas reales que usan el programa desde la interfaz de usuario, que interactuen directamente con el servidor HTTP o que trabajen sobre una unica funcion.

El tipo de test va a depender del objetivo del test y de la parte del programa con la que interactua. Como las "capas" de un programa son muchas y hay muchas razones diferentes para probar cada una, los tipos de test que podemos escribir son muchos tambien.

En este onboarding nos vamos a enfocar en tres tipos de test: Unitarios, de integracion y de punto a punto.

#### Unit testing

#### Integration testing

#### End-to-end testing

### Contract testing y test doubles 

<hr />

[![Ver en YouTube](../youtube.png)](https://www.youtube.com/watch?v=qs6LNiqffGk)

[![Siguiente](../next.png)](../)
