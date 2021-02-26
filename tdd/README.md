# Test-driven development (En construccion)

TDD es una disciplina que consiste de crear pruebas automatizadas (tests) antes de crear el codigo de aplicacion. Practicar esta disciplina nos obliga a encarar el desarrollo desde otro punto de vista y nos da beneficios que no tendriamos de otra manera, como por ejemplo, poder confiar que cuando nuestras pruebas pasan el codigo esta pronto para produccion.

### Que es un test

Una *test suite* es un programa que escribimos en paralelo con el codigo de aplicacion que se encarga de usar el programa principal y evaluar su funcionamiento.

La test suite deberia interactuar con todos los elementos de la aplicacion mas de una vez, de todas las maneras posibles y desde varios niveles de abstraccion. Cuando practicamos test-driven development correctamente, podemos estar seguros que nuestros tests cubren todo el codigo de aplicacion.

Cuando escribimos una prueba automatizada tenemos que dejar de lado la creatividad y ver el codigo con ojo critico. Crear un test no es un ejercicio de creatividad sino de control de calidad.

El cambio de perspectiva de tester a developer y viceversa puede ser particularmente dificil, y quizas sea la parte mas dificil de practicar test-driven development.

#### Composicion

Un test tiene tres etapas que van en este orden: Precondicion, ejecucion y evaluacion.

![Test](./tdd_1.png)

La etapa de pre-condicion la usamos para crear el contexto necesario para poder ejecutar la funcion en cuestion de manera significativa. Por ejemplo, si estuvieramos probando una funcion que busca un usuario en la base de datos, usariamos la etapa de pre-condicion para insertar ese usuario. Si no tenemos el usuario en la base de datos, ejecutar la funcion no nos seria util ya que no podriamos evaluar si funciona.

En la segunda etapa vamos a ejecutar la funcion con los parametros que correspondan.

La ultima etapa es la de evaluacion. En base a los parametros que le pasamos en la etapa de ejecucion, vamos a verificar que el resultado de la funcion es lo que esperabamos. Siguiendo con el ejemplo de la funcion que busca el usuario, en esta etapa vamos a confirmar que la funcion encontro el usuario.

En un mismo test vamos a repetir estas tres etapas tantas veces como casos de uso tenga la funcion.

Nuestro ejemplo tiene por lo menos dos casos de uso: El caso en el que el parametro coincide con un ID de un usuario en la base de datos y el caso en el que no.

```javascript
/* Usamos Jasmine para crear tests en
   Javascript. Las funciones `describe`,
   `beforeAll` e `it` vienen definidas
   en la libreria. */
describe('buscarUsuario', function () {
  var usuario;

  /* `beforeAll` se va a ejecutar antes
     que los tests. Aca vamos a definir
     nuestras pre-condiciones. */
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

A veces tambien es importante agregar una cuarta etapa, que podemos llamar post-condicion, para quitar los efectos secundarios del test. En este caso podriamos usar la post-condicion para dejar la base de datos vacia como estaba antes de empezar.

### Tipos de test

Un programa se puede probar de muchas maneras diferentes. Podemos crear pruebas que simulen ser personas reales que usan el programa desde la interfaz de usuario, que interactuen con el servidor HTTP o que trabajen sobre una sola funcion.

El tipo de test va a depender de la parte del programa con la que interactua y del objetivo del test. Como las "capas" de un programa son muchas y hay muchas razones diferentes para probar cada una, los tipos de test que podemos escribir son muchos tambien.

En este onboarding nos vamos a enfocar en tres tipos de test: Unitarios, de integracion y de punto a punto.

#### Unit testing

![Unit testing](./tdd_2.png)

Llamamos *unit test* al que prueba una sola funcion de nuestro sistema. El concepto de "unidad" puede cambiar segun cada programa, paradigma o equipo de trabajo. En nuestro caso le vamos a llamar unidad a cada `function`.

Los unit tests nos permiten empezar a probar nuestro sistema desde temprano y dan granularidad tal que podemos asegurar de manera individual y aislada que cada elemento en el nivel mas bajo del programa funciona bien.

#### Integration testing

Un *integration test* prueba mas de una funcion en conjunto. El nivel de integracion que probamos depende mucho de las opiniones del equipo. En nuestro caso vamos a probar la integracion entre servidor HTTP, aplicacion (logica de negocio) y base de datos.

![Integration testing](./tdd_3.png)

Un test de interaccion desde el servidor HTTP nos ayuda a probar los puntos de entrada de nuestra API tal cual lo haria un usuario y tambien sirve de documentacion.

La [explicacion de Martin Fowler](https://martinfowler.com/bliki/IntegrationTest.html) es interesante y vale la pena leer. Tambien incluye *contract testing* que es lo que vamos a ver mas abajo. Lo que hacemos nosotros es lo que el describe como "broad" integration testing.

Los tipos de tests que siguen tambien se pueden considerar tests de integracion porque prueban mas de una funcion interactuando entre ellas. Pero cuando hablemos de "integration testing" vamos a estar refiriendonos solamente a los tests que hacemos a traves del servidor HTTP.

#### End-to-end testing

Los tests *end-to-end* son los que simulan una persona real usando el sistema. Idealmente se ejecutan en un ambiente lo mas similar posible al de produccion y sin tomar atajos en cuanto a la interfaz de usuario.

![End-to-end testing](./tdd_4.png)

Estos tests pueden ser los que mas valor agregan al test suite pero son costosos, tanto para ejecutar como para desarrollarlos.

Cuando hacemos tests que van de una punta tenemos que considerar desde el CSS hasta los servicios externos. Por ejemplo, algunas herramientas para testing en el browser fallan si un boton que queremos cliquear no esta visible en la pantalla. Tambien podemos cometer el error de llamar una API que usa un servicio externo que cobra por uso y generar un gasto cada vez que corremos los tests.

Al mismo tiempo precisa de muchas cosas funcionando a la vez y eso hace que que ejecutar los tests lleve mucho tiempo y recursos. Por eso es que este tipo de test no es parte del dia a dia del programador, sino que se ejecutan de vez en cuando.

### Contract testing y test doubles 

<hr />

[![Ver en YouTube](../youtube.png)](https://www.youtube.com/watch?v=qs6LNiqffGk)

[![Siguiente](../next.png)](../)
