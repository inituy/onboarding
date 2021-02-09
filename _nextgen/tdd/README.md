# Test-driven development

TDD es una disciplina de desarrollo en el cual escribimos las pruebas primero y el codigo de la funcion despues. Como toda disciplina, TDD tiene reglas arbitrarias que cobran sentido una vez que las practicamos y sobre todo cuando lo compramos con el mecanismo opuesto de crear las pruebas al final.

[Las reglas de test-driven development](https://www.youtube.com/watch?v=qkblc5WRn-U) son tres. Son extremadamente simples aunque a simple vista parecen bastante ridiculas:

1. No se debe escribir codigo de produccion a menos que sea para satisfacer un *unit test* que esta fallando. Esto quiere decir que debemos empezar por escribir el *unit test* y no el codigo.
2. No se debe escribir del *unit test* de lo necesario para que este falle.
3. No se debe escribir mas codigo de produccion que el necesario para satisfacer el *unit test* que esta fallando.

Seguir estas reglas nos va a dejar dando vueltas en un ciclo que solamente termina cuando ambos el *unit test* y el codigo de produccion esta completo. Veamos un ejemplo.

###### Paso 1: Debemos escribir el *unit test* y parar ni bien el test falle:

```javascript
describe('funcion suma', function () {
  var suma = require('./suma.js');

  it('suma dos numeros', function () {
    var resultado = suma(1, 3);
  });
});
```

Terminamos de escribir esto y ya vemos que el test falla porque el archivo `suma.js` esta vacio y el test espera que sea una funcion.

###### Paso 2: Debemos escribir solo el codigo de produccion suficiente para hacer que el test deje de fallar.

```javascript
module.export = function suma() {};
```

Este codigo hace que el test deje de fallar. Fallaba porque `suma` no era una funcion y ahora si es.

###### Paso 3: Continuamos escribiendo el *unit test* y paramos ni bien falle de nuevo

```javascript
describe('funcion suma', function () {
  var suma = require('./suma.js');

  it('suma dos numeros', function () {
    var resultado = suma(1, 3);
    expect(suma).toBe(4);
  });
});
```

###### Paso 4: Agregamos solamente el codigo suficiente para que el test deje de fallar

```javascript
module.export = function suma() { return 4; };
```

Naturalmente el paso siguiente seria seguir escribiendo el *unit test* hasta que vuelva a fallar y volver a agregar solo el codigo necesario. Este ciclo termina cuando el test esta completo y no hay nada mas que agregar.

Tambien esto nos asegura que todo el codigo de produccion esta asociado con por lo menos un test. Cuando escribimos el codigo primero y las pruebas despues esto no es necesariamente cierto o, si lo fuera, no podriamos asegurarlo con confianza.

## Beneficios de practicar TDD

Sin duda el beneficio principal de TDD es el primero que describe [Robert C. Martin en su video The Three Rules of TDD](https://www.youtube.com/watch?v=qkblc5WRn-U).

En ese video nos invita a imaginarnos un grupo de personas que practica esta disciplina y sugiere que todas y cada una de esas personas ejecuto todos sus *unit test* y los vio completarse con exito hace menos de 10 minutos. Todos los miembros de ese equipo vieron que "todo funciona" hace menos de 10 minutos.

Otro de los beneficios es que al escribir un elemento de un programa practicando TDD terminamos con un archivo de prueba que nos muestra todas las posibles formas de usar ese elemento del programa. TDD nos da documentacion facil de entender y que se puede ejecutar para verificar que efecticamente funciona como dice que funciona.

Sin embargo ningun beneficio viene sin un costo. En el caso de TDD el costo es lo tedioso de practicar la disciplina. Practicar TDD es dificil, pero lo beneficios son tan fundamentales que es dificil creer que hay gente que no la usa.

El video de arriba muestra otros beneficios que no voy a poner aca. Te recomiendo que veas el video entero varias veces.

<hr />

[![Siguiente](../next.png)](../)
