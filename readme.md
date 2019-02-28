# GIT
## Ejemplo de Git

<img src="https://git-scm.com/images/logo@2x.png">


[Guia](https://git-scm.com/book/en/v2)

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