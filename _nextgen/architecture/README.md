# Arquitectura

El concepto de arquitectura de software es muy ignorado por los programadores cuando empiezan a trabajar en un nuevo proyecto. O si se habla sobre arquitectura, es un concepto que se toma como resuelto hace años y que no vale la pena intentar mejorar.

En esta parte de curso vamos a ver como la arquitectura del software que construimos es algo que podemos controlar y que es importante hacerlo para que no sea un infierno mantener nuestro codigo en el futuro.

### Proposito de una arquitectura

[Segun Wikipedia](https://en.wikipedia.org/wiki/Software_architecture), la arquitectura de un software es una abstraction inteligible de un sistema complejo que nos permite tomar decisiones que luego de implementadas seria muy costoso volverlas para atras.

> Software architecture is an "intellectually graspable" abstraction of a complex system.

> Software architecture is about making fundamental structural choices that are costly to change once implemented.

Por otro lado, [Robert Martin en su presentacion "Clean Architecture"](https://www.youtube.com/watch?v=Nsjsiz2A9mg) insiste en que la arquitectura de software no deberia explicar *de que* esta hecho el software sino *para que* fue hecho. Tambien propone que el proposito de una arquitectura bien hecha es atrasar la toma de decisiones, no tomarlas de antemano.

> Architecture is about intent.

> The purpose of a good architecture is to defer decisions.

> A good architecture maximizes the number of decisions not made.

En los proyectos donde aplicamos la arquitectura como lo explica Robert Martin pudimos ver como los beneficios que propone estan ahi. Si pudieras ver el codigo de uno de estos proyectos podrias entender que tipo de software es y para que fue creado, gracias a que diseñamos su arquitectura con ese proposito.

Veamos cuales son las caracteristicas de nuestras arquitecturas que nos permiten expresar el "para que" del software.

#### [Use-case driven design](https://en.wikipedia.org/wiki/Use_case) y estructura de archivos

En la seccion que viene vamos a entrar en detalle sobre que es un caso de uso y como lo ponemos dentro del software en forma de codigo. Por ahora es necesario entender que nuestros proyectos estan organizados alrededor de las acciones que un usuario puede llevar a cabo con nuestro software y que a esas acciones les llamamos "casos de uso".

Por ejemplo, un usuario podria iniciar sesion, ver su perfil y actualizarlo. Eso se veria reflejado en los archivos de nuestro software, ya que cada una de esas acciones se transformaria en un archivo:

```
app/
  actions/
    create_session.js
    get_user_profile.js
    update_user_profile.js
  data/
  validation/
  presentation/
database/
http/
spec/
```

Como todos los casos de uso (o "acciones") estan a la vista inmediatamente, no necesitas revisar el codigo ni ir mucho mas lejos para saber cual es el proposito de nuestro software. Si se tratara de un software para administrar inmuebles veriamos casos de usos como `agregar_inmueble` y si fuera para subir fotos habria un caso de uso llamado `subir_foto` o similar.

Adentro de los archivos tambien vamos a encontrar el codigo organizado de una manera que nos permite entender de que se trata a simple vista.

### [Use-case driven design](https://en.wikipedia.org/wiki/Use_case)

Un caso de uso es una lista de acciones que describen una interaccion del usuario con nuestro sistema. Cada caso de uso le da al usuario un resultado especifico y tambien puede tener efectos secundarios sobre el sistema, como un cambio en la base de datos.

Hacer "use-case driven design" significa diseñar nuestro software empezando por y enfocandonos principalmente en los casos de uso. Esto puede ser opuesto a pensar nuestro software como elementos que interactuan entre ellos, como cuando diseñamos software para la facultad usando UML y programacion orientada a objetos.

Diseñar nuestro software alrededor de los casos de uso nos permite enfocarnos mucho mas en las necesidades de nuestro usuario. Las necesidades de nuestros usuarios se pueden traducir casi directamente a casos de uso y luego a codigo, si diseñamos nuestra arquitectura de esta manera:

```javascript
function createSession(payload) {
  return Promise.resolve(payload)
    .then(findUserByEmailAndPassword)
    .then(saveNewSession)
    .then(presentNewSession);
}
```

Un caso de uso se traduce a una funcion cuya responsabilidad es ordenar los pasos del caso de uso, que tambien son funciones.

### [Railway oriented programming](https://fsharpforfunandprofit.com/posts/recipe-part2/)

Un nivel mas adentro, la funcion `saveNewSession` representa un paso en la interaccion entre el usuario y el sistema. Esta funcion forma parte de un [pipeline pattern](../javascript#pipeline-pattern) y por lo tanto su *input* va a ser el *output* de la funcion anterior.

"Railway oriented programming" es una idea divertida [que presento Scott Wlaschin en el 2014](https://www.youtube.com/watch?v=E8I19uA-wGY) y que explica bastante bien lo que pasa con este patron pipeline. Funciona asi:

![Railway oriented programming](./railway_oriented_programming.png)

Nuestro caso de uso es una via por donde va a pasar un tren. Ese tren es una estructura de datos que tiene la informacion que precisamos para ejecutar el caso de uso.

Si queremos crear una nueva sesion, ese tren tiene el email y password del usuario. Para actualizar el perfil del usuario, el tren lleva la sesion y la informacion para actualizar el perfil.

Las funciones dentro del pipeline pueden ver lo que esta en el tren y pueden agregarle cosas. Las funciones que vienen despues ven el tren como quedo despues de pasar por la ultima estacion. Si aparece un error en alguna de las estaciones, el tren toma el camino rojo y no para en ninguna estacion mas.

Esto implica que las funciones que forman parte del pipeline tienen un requerimiento fijo y es que deben recibir al tren como input, y el tren debe salir como output. Debe ser el mismo tren, con la excepcion que lo que tiene adentro puede ser distinto.

Una funcion del pipeline se ve asi:

```javascript
/* La funcion recibe "payload" que es
   la estructura de datos que tiene la
   informacion para ejecutar el caso
   de uso. */
function saveNewSession(payload) {
  /* Las funciones del pipeline siempre
     devuelven una promesa para complementar
     con las funciones que son asincronicas. */
  return persistSession(payload.user)
    .then(function (result) {
      // Retorna el mismo objeto que recibio.
      return payload;
    });
}
```

### I/O as a plugin