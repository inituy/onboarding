# Vagrant

## Qué es

Vagrant es una herramienta para crear y administrar máquinas virtuales. Nosotros la usamos en conjunto con VirtualBox ya que es de las herramientas mas conocidas para crear máquinas virtuales, aunque Vagrant funciona con varias otras plataformas.

Vagrant permite especificar las carácterisiticas de una máquina virtual en un archivo, y que de esa manera podamos replicar la máquina virtual y trabajar todos sobre el mismo ambiente de desarrollo. También podemos usar esta máquina virtual para crear el ambiente de producción.

## Cómo crear tu máquina virtual

Podés encontrar nuestro archivo Vagrant [acá](https://bitbucket.org/inituy/vagrant/src/master/). Mantenemos actualizado este archivo para que funcione con la mayor cantidad de proyectos posible, aunque puede haber algunas excepciones en las que haya que hacer configuración extra.

Asumiendo que ya tenés VirtualBox y Vagrant instalados, el siguiente paso es abrir una terminal de Windows, navegar hasta el directorio donde bajaste los archivos y correr:

```
vagrant up
```

Este comando va a crear la máquina virtual (si no fue creada antes) y va a ejecutar los archivos que están incluídos en el repositorio, que a su vez van a instalar las herramientas que usamos en los proyectos.

Con la máquina virtual activa, podemos ejecutar:

```
vagrant ssh
```

Esto va a crear una conexión SSH con la máquina virtual y vas a poder correr comandos de Ubuntu. En esta terminal es donde vamos a hacer la gran mayoria del trabajo para el cual necesitamos Ubuntu.

La máquina virtual va a quedar levantada hasta que la apagues con:

```
vagrant halt
```

Para seguir con la guía [hacé clic acá](https://bitbucket.org/inituy/onboarding/src/master/javascript/functions.md).