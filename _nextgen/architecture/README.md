# Arquitectura

El concepto de arquitectura de software es muy frecuentemente ignorado por los programadores cuando empiezan a trabajar en un nuevo proyecto. O si se habla sobre arquitectura, es un concepto que se toma como resuelto hace años y que no vale la pena intentar mejorar.

En esta parte de curso vamos a ver como la arquitectura del software que construimos no solo es algo que podemos controlar, sino que es importante hacerlo para que no sea un infierno mantener nuestro codigo en el futuro.

### Proposito de una arquitectura

[Segun Wikipedia](https://en.wikipedia.org/wiki/Software_architecture), la arquitectura de un software es una abstraction inteligible de un sistema sistema complejo que nos permite tomar decisiones que luego de implementadas seria muy costoso volverlas para atras.

> Software architecture is an "intellectually graspable" abstraction of a complex system.

> Software architecture is about making fundamental structural choices that are costly to change once implemented.

Por otro lado, [Robert Martin en su presentacion Clean Architecture](https://www.youtube.com/watch?v=Nsjsiz2A9mg) insiste en que la arquitectura de software no deberia explicar *de que* esta hecho el software sino *para que* fue hecho. Tambien propone que el proposito de una arquitectura bien hecha es atrasar la toma de decisiones, no tomarlas de antemano.

> Architecture is about intent.

> The purpose of a good architecture is to defer decisions.

> A good architecture maximizes the number of decisions not made.

A nosotros nos parece mas practica la definicion que nos da Robert Martin.

En los proyectos donde aplicamos estos conceptos pudimos ver como los resultados son exactamente los que propone. Si pudieras ver el codigo de uno de los proyectos en los que trabajamos serias capaz de entender que tipo de software es y para que fue creado, gracias a que diseñamos su arquitectura con ese proposito.

Veamos cuales son las caracteristicas de nuestras arquitecturas que nos permiten expresar el "para que" del software.

#### [Use-case driven design](https://en.wikipedia.org/wiki/Use_case) y estructura de archivos

En la proxima seccion vamos a entrar en detalle sobre que es un caso de uso y como lo ponemos dentro del software en forma de codigo.

Por ahora es necesario entender que nuestros proyectos estan organizados alrededor de las acciones que un usuario puede llevar a cabo con nuestro software. A esas acciones les llamamos "casos de uso".

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

Como todos los casos de uso (o "acciones") estan a la vista inmediatamente en el nombre de los archivos, no es necesario revisar el codigo ni ir mucho mas lejos para saber cual es el proposito de nuestro software. Si se tratara de un software para administrar inmuebles veriamos casos de usos como `agregar_inmueble` y si fuera para subir fotos habria un caso de uso llamado `subir_foto` o similar.

Adentro de los archivos tambien vamos a encontrar una estructura de codigo que nos permite a simple vista entender de que se trata el caso de uso. Para eso vayamos a la siguiente seccion donde vamos a hablar sobre casos de uso en detalle.

### [Use-case driven design](https://en.wikipedia.org/wiki/Use_case)

Un caso de uso es una lista de acciones que describen una interaccion del usuario con nuestro sistema. El usuario lleva a cabo un caso de uso para obtener cierto resultado del sistema. Un caso de uso tambien puede tener efectos secundarios sobre el sistema, como un cambio en la base de datos.

Hacer "use-case driven design" significa crear nuestro software basandonos en los casos de uso que los componen. Esto puede ser opuesto a pensar nuestro software como elementos que interactuan entre ellos en un contexto determinado, como se da cuando diseñamos software para la facultad.

Diseñar nuestro software alrededor de los casos de uso nos permite enfocarnos mucho mas en las necesidades de nuestro usuario. Las necesidades de nuestros usuarios se pueden traducir casi directamente a casos de uso y luego a codigo, si diseñamos nuestra arquitectura de esta manera.

Aparte de organizar y nombrar los archivos de nuestro programa basandonos en los casos de uso, el codigo dentro de los archivos tambien toma la forma de caso de uso y describe los pasos de la interaccion. Un caso de uso en nuestro programa se veria algo asi:

```javascript
function createSession(payload) {
  return Promise.resolve(payload)
    .then(findUserByEmailAndPassword)
    .then(saveNewSession)
    .then(presentNewSession);
}
```

Como podes ver en el ejemplo de arriba, el codigo en el nivel mas alto de nuestro software tambien fue diseñado para que exprese el "para que" del sistema. En este caso del caso de uso en particular.