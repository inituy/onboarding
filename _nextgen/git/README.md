# Git

Git es un sistema de control de version que tambien sirve para distribuir codigo. Usamos Git para empaquetar codigo segun tarea y funcion y para compatirlo entre las personas que estan trabajando en un proyecto.

### Funcion

Git funciona con pequeños paquetes de codigo llamados `commits` que aparecen ordenados en el tiempo en una especie de estructura de arbol. Idealmente el codigo dentro de cada commit esta relacionado a una misma tarea o a una misma funcion dentro del sistema.

El trabajo del programador es crear los commits y agregarlos al historial. Cuando mas de una persona trabaja en el mismo proyecto creamos una copia del historial para cada una. A estas copias se le llaman `branches`.

Al terminar la tarea los programadores tienen que combinar los historiales que partieron del mismo punto pero que ahora son diferentes. A este proceso se le llama `merge`. Cuando el `merge` esta completo terminamos con una sola `branch` que contiene el historial inicial, mas los `commit` que cada programador agrego.

Un `merge` puede generar un conflicto si los dos programadores pusieron codigo diferente en la misma linea del mismo archivo pero en distintas `branch`. El proceso de `merge` te da la oportunidad de solucionar los conflictos antes de terminar.

![git](./git.png)

Este es un glosario rapido para poder seguir con los comandos de Git:

1. *Repository*: Un repositorio es un proyecto en Git. Creamos un repositorio para cada aplicacion para mantener su historia separada de otros proyectos.
2. *Working directory*: Esta es la carpeta raiz del proyecto donde inicializamos el repositorio.
3. *Commit*: Es un paquete que contiene una lista de cambios al repositorio. Cuando se agrega, se modifica o se borra un archivo eso se guarda en el repositorio usando un `commit`.
4. *Branch*: Una lista de `commit` que funciona como historial de cambios hechos al repositorio. Todos los repositorios empiezan con una sola `branch`, pero se pueden tener mas de una.
5. *Remote*: Una version del mismo repositorio pero que esta en otra computadora. En casi todos los proyectos, el `remote` es la version del repositorio que esta en la nube.

### `git status`

El comando `git status` nos muestra el estado actual del working directory. Muestra que archivos cambiaron y cuales de esos cambios fueron agregados al siguiente commit.

```
$ git status

On branch banafederico
Your branch is behind 'origin/banafederico' by 2 commits, and can be fast-forwarded.
  (use "git pull" to update your local branch)

Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        new file:   spec/presentation/present_nothing_spec.js

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   app/data/find_or_save_user_by_email.js

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        app/data/save_session_request_for_user.js
        app/presentation/present_nothing.js
        spec/data/save_session_request_for_user_spec.js
```

### `git add`

Agrega uno de los archivos que cambiaron al siguiente commit. Se pueden agregar todos los archivos pasandole `-A` por parametro.

```bash
git add app/data/save_session_request_for_user.js # Agrega un archivo
git add spec/data/ # Agrega todos los cambios dentro del directorio al siguiente commit
git add -A # Agrega todos los cambios al siguiente commit
```

### `git commit`

Crea un commit. Si no se usa el parametro `-m` para agregarle el mensaje de commit, abre un editor de texto en la consola.

Con `-m` crea el commit sin mas interaccion:

```
$ git commit -m "Git updated picture"

[master cf3ad67] Git updated picture
 1 file changed, 0 insertions(+), 0 deletions(-)
 rewrite _nextgen/git/git.png (99%)
```

Sin `-m` abre el editor de texto en consola para agregarle el mensaje de commit:

```
$ git commit

  1 Git updated picture
  2 # Please enter the commit message for your changes. Lines starting
  3 # with '#' will be ignored, and an empty message aborts the commit.
  4 #
  5 # On branch master
  6 # Your branch is up to date with 'origin/master'.
  7 #
  8 # Changes to be committed:
  9 #&modified:   _nextgen/git/git.png

[master cf3ad67] Git updated picture
 1 file changed, 0 insertions(+), 0 deletions(-)
 rewrite _nextgen/git/git.png (99%)
```

### `git reset`

Este comando retrocede en la historia, borrando los commits pero sin modificar los archivos. Sin parametros, `git reset` borra los cambios que se agregaron al siguiente commit. Si se le pasa un ID de commit, `git reset` elimina commits del final de la lista hasta llegar al commit indicado.

Sin parametros funciona de esta manera:

```
$ git status
```

```
On branch master
Your branch is ahead of 'origin/master' by 2 commits.
  (use "git push" to publish your local commits)

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   _nextgen/README.md
        modified:   _nextgen/git/README.md

no changes added to commit (use "git add" and/or "git commit -a")
```

```
$ git add _nextgen/README.md
$ git status
```

```
On branch master
Your branch is ahead of 'origin/master' by 2 commits.
  (use "git push" to publish your local commits)

Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   _nextgen/README.md

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   _nextgen/git/README.md
```

```
$ git reset
```

```
Unstaged changes after reset:
M       _nextgen/README.md
M       _nextgen/git/README.md
```

```
$ git status
```

```
On branch master
Your branch is ahead of 'origin/master' by 2 commits.
  (use "git push" to publish your local commits)

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   _nextgen/README.md
        modified:   _nextgen/git/README.md

no changes added to commit (use "git add" and/or "git commit -a")
```