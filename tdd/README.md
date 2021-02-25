# Test-driven development (En construccion)

TDD es una disciplina que consiste de crear pruebas automatizadas (tests) antes de crear el codigo de aplicacion. Practicar esta disciplina nos obliga a encarar el desarrollo desde otro punto de vista y nos da beneficios que no tendriamos de otra manera, como por ejemplo, poder confiar que cuando nuestras pruebas pasan el codigo funciona.

### Que es un test

Una *test suite* es un programa que escribimos en paralelo con el codigo de aplicacion que se encarga de usar el programa principal y evaluar su funcionamiento.

![Test](./tdd_1.png)

Cuando escribimos una prueba automatizada tenemos que mirar el codigo desde el punto de vista de un tester ya que no estamos creando el codigo de aplicacion. Esto puede ser particularmente dificil cuando practicamos test-driven development, ya que tenemos que cambiarnos de programador a tester en ciclos muy cortos.

#### Composicion

Un test tiene tres etapas que van en orden: Precondicion, ejecucion y evaluacion.

La etapa de precondicion se usa para crear el contexto necesario para poder ejecutar y evaluar el funcionamiento de lo que estamos probando. Por ejemplo, si estuvieramos probando una funcion que buscar un registro en la base de datos, usariamos la etapa de precondicion para insertar ese registro. Si no tenemos el registro en la base de datos, no podemos probar la funcion que lo encuentra.

En la segunda etapa vamos a ejecutar la funcion que queremos probar. Siguiendo con el ejemplo de la funcion que busca el registro, este es el momento en el que llamamos a la funcion con los parametros que llevan (`buscarRegistro(idDeRegistro)`).

La ultima etapa es la mas importante. En base a los parametros que le pasamos en la etapa de ejecucion, vamos a verificar que la funcion haya devuelto lo que tenia que devolver. En el caso de nuestro ejemplo vamos a ver si la funcion encontro el registro.

En un mismo test vamos a repetir estas tres etapas tantas veces como casos de uso tenga la funcion que estamos probando. Nuestro ejemplo de la funcion que busca el registro en la base de datos tenemos por lo menos dos casos de uso: El caso en el que la ID corresponde con un registro en la base de datos y el caso en el que no. En el primer caso la funcion debe encontrar el registro y en el segundo no.


```javascript
/* Usamos Jasmine para crear tests en
   Javascript. Las funciones `describe`,
   `beforeAll` e `it` vienen definidas
   en la libreria. */
describe('buscarRegistro', function () {
  var registro;

  /* `beforeAll`` se va a ejecutar antes
     que los tests. Aca vamos a poner
     nuestras precondiciones. */
  beforeAll(function () {
    registro = { id: 123 };
    baseDeDatos.insertar(registro); // Asumimos que existe!
  });

  /* `it` se usa para expresar el caso de
     uso. Aca vamos a ejecutar la funcion
     y evaluar el resultado. */
  it('encuentra el usuario', function () {
    /* Ejecutamos la funcion con el ID de
       registro correcto y verificamos
       que devuelva el registro. */
  });

  it('no encuentra el usuario', function () {
    /* Ejecutamos la funcion con el ID de
       registro incorrecto y verificamos
       que devuelva `null`. */
  });
});
```

### Tipos de test

<hr />

[![Ver en YouTube](../youtube.png)](https://www.youtube.com/watch?v=qs6LNiqffGk)

[![Siguiente](../next.png)](../)
