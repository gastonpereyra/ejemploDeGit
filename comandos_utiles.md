# GIT
## Comandos Utiles

[Volver a Readme](https://github.com/gastonpereyra/ejemploDeGit/blob/master/readme.md)

<img src="https://git-scm.com/images/logo@2x.png">

## Configuración inicial

Para guardar los cambios es necesario registrar un nombre de usuario y un email.

Puede ser "local" (default) para el proyecto, "global" para el usuario del SO, y "system" para todos los usuarios.

```git
git config --local user.name "Gaston"
git config --local user.email "unMail@correo.com"
git config --global user.name "Gaston"
git config --global user.email "unMail@correo.com"
git config --system user.name "Gaston"
git config --system user.email "unMail@correo.com"
```

Se pueden configurar otro monton de cosas.

Para ver lo configurado

`git config --list`

## Iniciar

Para iniciar un repositorio, en la carpeta donde se encontrará.

`git init`

De esta manera esta listo para empezar a usarse, crea la carpeta `.git/` donde se guardara la configuración y los archivos.
Crea la rama `master`.

## Clonar

Se puede iniciar clonando un repositorio existente, importando el estado de ese directorio.

`git clone [url]`

[url] = dirección del repositorio a clonar.

Este pasara a ser "propiedad" del usuario local y poder modificarlo.

## Cambios

Si en el directorio de trabajo modificamos, agregando archivos, o modificando los mismos, git detecta esos cambios. Y la diferencias en los archivos. 

Para ver el estado de estos cambios 

`git status`

Aporta información tanto en que archivos fueron cambiados, y no notificados, ni guardados como en que rama se esta trabajando.

## Agregar al Stage

Para agregar archivos al Stage y prepararlos para guardarlos se usa

* Para agregar 1 archivo

```
git add [nombre de archivo]
git add index.html 
```

* Para agregar patrones de archivos 

```
git add [patron]
git add *.html 
```

En este caso agrega todos los archivos (que ya no estuvieran) que tengan de extensión ".html"

* Para agregar todos

```
git add .
```

### Eliminar Archivos / renombrar

Se usa para Eliminar

`git rm [archivo, patron o directorio]`

Para renombrar

`git mv [archivo] [nuevo nombre]`

### Sacar del Stage

PAra sacar del Stage 

`git reset HEAD <archivo>`

## Guardar cambios

Para guardar los cambios en el repositorio

`git commit`

Esto lleva a un editor (que se puede configurar previamente). 

La manera recomendada:

`git commit -m "[mensaje]"`

Si se creo un archivo y es la primera vez que lo vamos a guardar esta es la forma. Pero si ya tuvo otros commits se puede evitar el paso de agregar asi:

`git commit -am "[mensaje]"`

Si se agrega `-v` se agrega al commit las diferencias en los archivos

## Ver Historial de cambios

Para ver el historial de commits

`git log`

Se muestra los commits, su identificador, quien los hizo, timestamp y la referencias.

Para verlo comprimido

`git log --oneline`

Se ve identificador, las referencias (Head, branches), y el mensaje del commit.

`git log --oneline --graph`

`git log --oneline --graph --all`

Se ve en forma de grafo, arbol, las ramificaciones y merges.

## Moverse entre los commits

Para poder ir a otros commits anteriores o posteriores se usa

`git checkout [identificador del commit]`

Tambien se puede usar para moverse hacia una rama, o etiqueta.

Pero si no estamos sobre una la cabecera de una rama los cambios guardados se perderan.

## Etiquetas

Para agregar una etiqueta, otro marcardor en el Tree. (usado en general para marcar versiones). Ya sea en la posicion actual o en un commit pasado.

```
git tag -a [nombre] -m [mensaje]
git tag -a [nombre] -m [mensaje] [identificado de commit]
git tag -a v1.0.0 -m "Version Estable"
```

Para ver las etiquetas

`git tag`

Para buscar versiones

`git tag -l [nombre o patron]`

## Agregar Remoto

Para agregar un repo remoto se necesita un alias y una url, en general la rama principal (master) va con el alias origin

```
git remote add [alias] [url]
git remote origin http:github.com/esejemplo/ejemplo1.git
```
Para ver los remotos

`git remote -v`

Para ver los datos del remoto

`git remote show [nombre]`

## Fetch

Para recuperar la información del remoto y traerla al directorio local (sin unirla)

`git fetch [nombre]`

## Pull

Para unir los cambios de remoto en el directorio local

`git pull [nombre]`

## Push

Para unir el repo local con el remoto

``` 
git push [alias] [branch]
git push origin master
```

Para subir al remoto las etiquetas

``` 
git push [alias] [etiqueta]
git push [alias] --tags
git push origin --tags
```

Se puede borrar una rama

` git branch -d [nombre]`

<img src="https://github.com/gastonpereyra/ejemploDeGit/blob/master/imagenes/arbol-sin-merge.png">

## Merge

Sirve para fusionar 2 ramas. Para eso se usa:

`git merge [nombre]`

### Union Lineal

Si tenemos 2 ramas, la principal y la secundaria, la secundaria avanza y una vez estando seguros que esta todo bien queremos que la principal tmb avance, nos posamos sobre la principal y hacemos merge con la secundaria. Entonces la rama principal hace un fast-forward.

```
$ git checkout master
$ git merge dev
Updating f42c576..3a0874c
Fast-forward
```

<img src="https://github.com/gastonpereyra/ejemploDeGit/blob/master/imagenes/arbol-con-merge-lineal.png">

### Union no-lineal

Si tenemos muchas ramas que se diverficaron, se pueden usar varios tipos de estrategias para que converjan. 

* Recursión, nos situamos en la rama principal y le decimos que una (merge) con la rama que queres, esto crea una nueva instancia superior a ambas.

```
$ git checkout master
Switched to branch 'master'
$ git merge dev -m "Unir 2 ramas"
Merge made by the 'recursive' strategy.
```

* Rebase, la estrategia del rebase es convertir las bifurcaciones en uniones lineales, nos situamos en la rama secundaria, y hacemos el rebase con la rama principal, luego hacemos una union lineal.

```
git rebase [rama]
git checkout dev
git rebase master
git checkout master
git merge dev
```

<img src="https://github.com/gastonpereyra/ejemploDeGit/blob/master/imagenes/arbol-con-merge-recursivo.png">

### Ramas Remotas

Se pueden configurar para pushear a un repo remoto de la misma manera que la rama principal

```
# asigno secundaria al repo remoto
git remote secundaria http:github.com/esejemplo/ejemplo1.git
# hago push de la rama dev al repositorio llamado secundaria, la primera vez
git push -u secundaria dev
# Hago otras modificaciones
# Hago commits
# pusheo otra vez
git push secundaria
```

El `-u` se usa para que git automaticamente asocie una rama con un alias de un repo remoto. Entonces la siguiente vez de "push" solo usas el alias.

- - - -

### Revertir / Resetear

Para volver atras por si hubo algun error hay varias formas, y muchas otras estrategias

* Revertir: Cuando usas un repo remoto, donde trabajan varias personas, usas este metodo para no romper el historial de cambios, y que la gente q usa el repo remoto no tenga problemas de sincronia.

`git revert [identificador] -m [mensaje] `

En este caso el revert crea un nuevo commit, osea avanza, dejo todo lo que se hizo en el historial, pero el estado actual es anterior, seria como borrar a mano y commitear.

* Reset: Reset es lo contrario, reset vuelve commits atras, los borra del historial. Por lo que si volves atras antes de lo que trajiste del remoto puede haber conflictos en el resto de del equipo
    * soft: `git reset --soft [identificador]` | vuelve al commit [identificador] pero no borra nada, deja todo como estaba listo para commitear.
    * mixed: `git reset --mixed [identificador]` | vuelve al commit [identificador] pero no borra nada, y deja no esta en en Stage Area.
    * hard: `git reset --hard [identificador]` | vuelve al commit [identificador] en el estado en el que estaba el commit en ese tiempo.



## Para saber más

[Guia](https://git-scm.com/book/en/v2)