# Arquitectura

El concepto de arquitectura de software es muy frecuentemente ignorado por los programadores cuando empiezan a trabajar en un nuevo proyecto. O si se habla sobre arquitectura, es un concepto que se toma como resuelto hace años y que no vale la pena intentar mejorar.

En esta parte de curso vamos a ver como la arquitectura del software que construimos no solo es algo que podemos controlar, sino que es importante hacerlo para que no sea un infierno mantener neustro codigo en el futuro.

### Proposito de una arquitectura

[Segun Wikipedia](https://en.wikipedia.org/wiki/Software_architecture), la arquitectura de un software es una abstraction inteligible de un sistema sistema complejo que nos permite tomar decisiones que luego de implementadas seria muy costoso volverlas para atras.

> Software architecture is an "intellectually graspable" abstraction of a complex system.

> Software architecture is about making fundamental structural choices that are costly to change once implemented.

Por otro lado, [Robert Martin en su presentacion Clean Architecture](https://www.youtube.com/watch?v=Nsjsiz2A9mg) insiste en que la arquitectura de software no deberia explicar *de que* esta hecho el software sino *para que* fue hecho. Tambien propone que el proposito de una arquitectura bien hecha es atrasar la toma de decisiones, no tomarlas de antemano.

> Architecture is about intent.

> The purpose of a good architecture is to defer decisions.

A nosotros nos parece mas practico la definicion que nos da Robert Martin.

En los proyectos donde aplicamos estos conceptos pudimos ver como los resultados son exactamente los que propone. Si pudieras ver el codigo de uno de los proyectos en los que trabajamos serias capaz de entender que tipo de software es y para que fue creado, gracias a que diseñamos su arquitectura con ese proposito.