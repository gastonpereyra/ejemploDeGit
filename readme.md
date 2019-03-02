# GIT
## Ejemplo de Git

<img src="https://git-scm.com/images/logo@2x.png">

## Antes de Empezar

Tener en cuenta Git no es Github, y Github usa Git.Esto quiere decir que podes usar Git sin usar Github.

Github, Gitlab, Bitbucket y otras son **Repositorios Remotos**, Tienen Interfaces graficas que ayudan mucho al manejo simple de los mismos.

PERO si bien en estos se pueden hacer muchas cosas equivalentes a los comandos en GIT, no es posible hacer TODO lo que se quiere. Para eso se necesitan herramientas adicionales, por ejemplo Github provee de Github desktop, que es una interfaz grafica para usar Git de manera local en las maquina propias, y resolver algunas problemas por ejemplo volver a versiones previas.

Por esta razón lo que voy a explicar a continución es para usar GIT, en la consola de comandos, y asi poder usarlo para cualquier repo remoto, o local, en cualquier lado.

Otra cosa, tienen que tener instalado GIT en sus maquinas, si no lo tienen pueden fijarse como hacerlo, es simple

Aca les dice los pasos segun el SO

<https://git-scm.com/book/es/v1/Empezando-Instalando-Git>

Aca para configurar la SSH para usarlo con github

<http://www.webtutoriales.com/articulos/configurar-git-para-trabajar-con-github>

O buscar donde quieran como hacerlo, no es dificil

(Mas Info de los comandos)[https://github.com/gastonpereyra/ejemploDeGit/blob/master/comandos_utiles.md]


## Contenido

* Empezando
    * Clonar
    * Init
    * Configurar Usuario local
    * Configurar Repositorio Remoto
* Modificando cosas
    * WD / SA
    * Status
* Add
* Commit
* Log
    * Graph
* Checkout Commits
* Push
    * Push -u
* Etiquetas
* Ramas
* Checkout Ramas y Etiquetas
* Merge
    * Fast-forward
    * Recursivo
    * Rebase
* Volver atras
    * Revert
    * Reset
* Fork
* Pull Request

## Empezando

Hay varias maneras de empezar

Van al repo remoto, por ejemplo Github, y en sus cuentas crean un nuevo repositorio

<img src="https://github.com/gastonpereyra/ejemploDeGit/blob/master/imagenes/github-newRepo.png">

Le ponen un nombre, no le acepten que se les cree un readme.md, publico o privado elección de uds. Aceptan y ven algo asi:

<img src="https://github.com/gastonpereyra/ejemploDeGit/blob/master/imagenes/github-repoCreado.png">

Ahi les da toda la info necesaria.

Arriba les da la dirección para del repo, incluso da lineas de comando para ni siquiera tener que pensar.

### Opcion 1 - Clonar

La mas facil talves. Van a la carpeta donde quieren que ESTE LA CARPETA del repositorio. Digamos al directorio padre.

```
git clone [direcion del repo]
```

Y asi crea una nueva carpeta con el nombre del repositorio, trae el contenido del repositorio, lo copia y queda listo para usar. Tiene configurado donde esta el remoto, y otro monton de cosas. Esto funciona tanto si esta vacio o es un proyecto empezado hace rato. Pero habra cosas a configurar si no estan ya hechas como el usuario (que lo vemos a continuación)

### Opcion 2 - Inicio Manual

Aca creamos la carpeta donde vamos a trabajar, y ponemos

```
git init
```

Fin.

No mentira, asi iniciamos pero no tiene nada, la configuramos un poco. 

Si cuando instalamos Git en nuestras maquinas no lo hicimos podemos configurar de todo, desde nuestros nombres a que editor usar. Pero el nombre y el email son obligatorios, sino no es posible hacer la gran mayoria de cosas.

#### Configurar Usuario

Simple, no hace falta que sea una cuenta en ningun lado, solo datos en lo posible reales para que si es necesario alguien te contacte.

```
git config [scope] user.name "[nombre]"
git config [scope] user.email "[email]"
```

Que es scope ? a que ambito pertenece el usuario, y esto se debe a que si estan en una pc compartida pueden no querer dejar que su usuario lo use todo el mundo. Hay 3 tipos
    - `--local`, por defecto (si no ponen nada) que es solo para el proyecto, afuera de la carpeta no es posible usarlo
    - `--global`, es para toda la sesion del SO, por lo que se puede seguir usando en otras carpetas
    - `--system`, es para todo el SO, es decir que si el SO tiene varios usuarios todos pueden o van a usar el mismo nombre y email para Git.

Ejemplo:

```
git config --global user.name "Gaston"
git config --global user.email "esteNoEs@miEmail.com"
```

#### Configurar Remoto

Para poder subir a Github o cualquiera hay que indicar a Git a donde hay que subir los archivos. Esto se hace asi:

```
git remote add [alias] [url]

# Ejemplo

git remote origin http:github.com/esejemplo/ejemplo1.git
```

ALIAS, es un nombre de fantasia para asociar la URL, asi no hay que escribir todo el tiempo tanto. SE pueden poner la cantidad de Alias que se quieran incluso a la misma URL, esto es para usar las ramas.

"origin" es el nombre mas comun para el repositorio principal , para usar la rama principal, si clonan uno desde github se llama asi.

### Listo ?

Una vez hecho lo anterior, aparece una carpeta ".git" (que es oculta), donde Git guarda todo su manejo interno (que es para otro tema), es importante no tocarlo si no sabes bien que haces.

Cuando creas un repo en Git, lo que haces es crear la carpeta, mas `git init` + `git remote add origin [url]`, y alojarlo en la nube.

Y asi esta listo para empezar.

## Modificando cosas

La idea de Git es ir guardando versiones de las archivos, versiones que le vamos indicando cuando guardarlas, e ir poniendo marcadores para en caso de ser necesario volver atras, crear nuevas versiones y comparar y otras muchas cosas. Por lo que este guardado **no es automatico** (salvo que estemos usando algun plugin o entorno que lo haga por nosotros como pasa en Glitch).

Hay **3 areas principales** en *GIT*
    * **WD**: Working directory, osea el area de trabajo, lo que vos estas editando, los archivos en si mismo.
    * **SA**: Stage Area, es un lugar donde ponemos los archivos antes de confirmarlos para guardar
    * **Commits**: (no se llama asi pero para explicarlo ahora lo voy a llamar asi), que es la confirmación para guardar el archivo, aca git guarda una "foto" del archivo o los archivos, y le asigna un identificador (un codigo en SHA-1) y queda en un **"arbol"** (osea todos los posibles caminos que se fueron guardando). Pasan mas cosas pero es para otro tema.

Otra cosa importante hay marcadores especiales que se pueden ir dejando por el arbol de git, uno es que indica donde VOS estas parado que es el **HEAD** (lo vas a ver varias veces), otro son las *ramas* (branches), siempre hay al menos 1 sino no se pueden guardar los cambios, la default es **master** (que es la principal casi siempre), se pueden crear mas, y uno puede ir de una rama a la otra (y a las versiones de lso archivos en ellas), y otro marcador son las etiquetas (**tags**), es un tipo especial no existe uno por default, hay que crearlos y sirven para crear la versiones recomendadas de tus programas, en Github aparecen en la parte de *releases*, es lo que en general nos dan como el programa v1.1.2 o la v1.1.20-beta. 

En fin, modificas tus archivos, creas, y borrar. Lo que necesites y llegaste a un punto que te parece correcto guardar.

### Verifcar estado

Para verificar el estado actual de git

`git status`

Cuando no pasa nada especial se ve asi:

<img src="https://github.com/gastonpereyra/ejemploDeGit/blob/master/imagenes/status_vacio.png">

Cuando hiciste cambios asi:

<img src="https://github.com/gastonpereyra/ejemploDeGit/blob/master/imagenes/status_cambios.png">

Aca te muestra en que ramas estas en **On branch **

Te muestra si desde la ultima vez que reviso el repositorio esta al dia, o avanzaste o retrocediste.

Y te dice si existen archivos que ya alguna ves incluiste en el arbol de git (**not staged**) y los que nunca estuvieron (**untrecked**). Aparecen en Rojo

Ademas te muestra también si estan en el SA (**changes to be committed**). Aparecen en Verde.

Esto sirve para ir chequeando si tenes algo por guardar o no.

### Agregas archivos

Digamos que creas el archivo `index.html`. No le pones nada, pero lo queres dejar registrado. Para la primera vez, osea cuando creas el archivo y nunca lo guardaste en GIT si o si

```
# git add [nombre de archivo]
git add index.html 
```

Y con `git status` se ve algo aasi:

<img src="https://github.com/gastonpereyra/ejemploDeGit/blob/master/imagenes/status_SA.png">

### Guardar los cambios

Cuando lo queres guardar se usa el siguiente comando

```
# git commit -m [mensaje]
git commit -m "agregar index.html"
```

Si solo pones "git commit" inicia el editor, que puede ser un dolor de cabeza si no lo sabes usar o no lo configuraste para uno que si sepas.

Asi esta guardado.

<img src="https://github.com/gastonpereyra/ejemploDeGit/blob/master/imagenes/add_commit_nuevo.png">


### Guardar modificaciones

Si ya tenias guardado un archivo y lo modificas, hay una forma facil de volverlo a guardar. Digamos que agregue contenido a index.hmtl

```
# git commit -am [mensaje]
git commit -am "agregar contenido a index"
```

### De a Muchos

Con `git add ` se pueden agregar de a muchos archivos, no solo de a uno, usando patrones o bien `git add .` para agregar todos. Y luego hacer un commit en general.

Digamos que creamos mas archivos `contact.html`, `info.html`, `style.css`, `script.js`.

Entonces podemos hacer:

```
# Guardar los HTML
git add *.html
git commit -m "Crear info y contact"

# guardar el resto
git add .
git commit -m "agregar archivo CSS y JS"
```

## Ver los cambios

Con `git log` o mejor con `git log --oneline --graph --all` se pueden ver el historial de cambios y los identificadores de los commits (los 4 primeros numeros sirven para por ejemplo ir de un lado a otro)

<img src="https://github.com/gastonpereyra/ejemploDeGit/blob/master/imagenes/arbol-sin-merge.png">

## Moverte entre commits

Con el identificador podes moverte entre un commit y otro usando

```
# git checkout [identificador]
# Ir al commit cuyo primeros 4 caracteres de identificacion son 39ab
git checkout 39ab 
```

Eso si como nos va a estar parado en ninguna rama, hacer un cambio aca se puede perder a futuro.

## Subir a un repo remoto

Es tan simple como 

```
# git push [alias] [rama]
git push -u origin master
```

Ese `-u` se puede poner la primera vez, y es para que desde ahora se acuerdo que cada push a origin corresponde a la rama master. Entonces la proxima vez basta con hacer `git push origin`

- - - 

Esto es lo basico.

Pero vamos a lo importante.

## Version estable

Si tu programa llega a un punto de version estable, podes usar **tags** para darle una marcación especial

Se crean asi:

```
# git tag -a [nombre] -m [mensaje]
# Crear un tag en la posición actual
git tag -a v1.0.0 -m "Version Estable"

# git tag -a [nombre] -m [mensaje] [identificado de commit]
# crear un tag en un commit en particular
git tag -a v1.0.0-beta -m "Version Beta" 39ab

```

De esta manera siempre podes volver a una version estable. Ya que no se mueve.

Y para subirlo al repo hay que hacer

```
# git push [alias] [nombre de tag]
git push origin v1.0.0

# para subir todos los tags de una
# git push [alias] --tags
git push origin --tags

```

## Ramas

Esto es escencial, tener varias ramas sirve para que por ejemplo:

- dejas una rama principal (master) con codigo que anda bien y funciona
- usas ramas secundarias para modificar ciertas partes, y cuando estas funcionan bien, la unis o a otras ramas secundarias, o a la principal. Asi podes tener una rama de Desarrollo, y una de Produccion, la de Produccion es el programa que distribuye al publico y el desarrollo es la que usa el equipo para mejorar el programa.
- trabajar en equipo, todos suben a un repositorio central, pero cada uno trabaja en una rama separada para no afectar el trabajo del otro.

Se crean facil

```
# git branch [nombre]

# abrimos ramas para modificar el archivo contact
git branch contact

# abrimos ramas para modificar el archivo info
git branch info

# abrimos ramas para modificar el archivo css
git branch css
```

### Moverse Entre ramas y etiquestas

Para ir de una rama a otra o a una etiqueta, es como moverse entre commits

```
# git checkout [nombre]
# vamos a contact
git checkout contact
```

Hacemos git status y vemos que dice que estamos en contact

### Modificar las ramas

Podemos hacer las modificaciones de archivos que queramoas en cada rama, y al cambiar vemos que para las otras ramas es como si eso no pasara. Asi cuando nos equivocamos no rompemos todo.

Eso si, cuidado con modificar en 2 ramas diferentes el mismo archivo, cuando las queramos unir va a lanzar problemas y hasta que no resolvamos no va a dejar avanzar (por ejemplo haciendo un commit nuevo en el archivo con problemas con una nueva version del mismo).

Digamos que en nuestro caso modicamos el archivo contact en la rama de su nombre (que ya estabamos)

```
# en la rama contact
# modificamos y guardamos
git commit -am "Modificamos Contact"
```

Vamos a la rama info y modificamos info.html

```
# vamos a la rama antes de modificarlo
git cheackout info
# modicamos el archivo
# guardamos
git commit -am "Modificamos info"
```

Lo mismo con Style.css

```
# vamos a la rama antes de modificarlo
git cheackout css
# modicamos el archivo style.css
# guardamos
git commit -am "Modificamos style.css"
```

Si hacemos un `git log --oneline --graph --all`

<img src="https://github.com/gastonpereyra/ejemploDeGit/blob/master/imagenes/arbol-sin-merge.png">

## Unir ramas

Esto es un tema largo. Es facil pero hay varias formas.

### Fast-Forward

Es la facil, esto pasa cuando por ejemplo la rama principal quedo atras, y avanzamos en una secundaria.

```
# git merge [rama]
# vamos a la que esta atras
git checkout master
# le decimos que avance por la rama CSS
git merge css 
```

Asi no hay problemas solo cambia la referencia.

<img src="https://github.com/gastonpereyra/ejemploDeGit/blob/master/imagenes/arbol-con-merge-lineal.png">

### Recursiva

En este caso tenemos varias ramas que tomaron caminos diferentes, no hay 1 que este atras, entonces hay que unir las ramas y todos los archivos casi 1 a 1, pero eso git lo hace automaticamente. Y chequea se hay errores, por ejemplo que las 2 modificaran el mismo archivo. Pero si no pasa nada, crea un commit nuevo, y las 2 ramas van hacia ese commit

```
# git merge [rama] -m [mensaje]
# vamos a la principal
git checkout master
# le decimos que avance por la rama info
git merge info -m "Unir 2 ramas"
```

<img src="https://github.com/gastonpereyra/ejemploDeGit/blob/master/imagenes/arbol-con-merge-recursivo.png">

### Reformulando.. gira a ..

También en el caso anterior se puede simular que siempre tuvimos el caso de una lineal. Hay un comando especial para esto, y lo que hace es poner el primer commit de una rama(1) como el siguiente de la rama(2), y luego se avanza con un merge lineal.

```
# git rebase [rama]

# vamos a la rama que queremos poner "adelante"
git checkout contact

# hacemos rebase con la rama que queremos que quede "atras" (en general la principal)
git rebase master

# Vamos a la que quedo atras
git checkout master

# Avanzamos hacia la que esta "adelante"
git merge contact
```

<img src="https://github.com/gastonpereyra/apuntes/blob/master/images/arbol-con-merge-rebase.png">

### Actualicemos el repo

Creemos una nueva etiqueta

`git tag -a v1.1.0 -m "Nueva Version"` 

Y actualizamos el repo

`git push origin`

TAMBIËN, se puede "pushear" cada rama al repo remoto y otro dia hacer todo esto de merge, digamos que creamos un alias para las ramas `git push -u devContact contact` (y asi con todas, repito crear los alias antes).

Y subimos los tags

`git push origin --tags`

- - - -

## Fetch y Pull

Digamos que pasan los dias y queremos ver si alguien estuvo modificando el repo 

`git fetch origin`

Aca git se fija si nuestro repositorio en la maquina es el mismo que en el remoto, pero ojo no lo actualiza de entrada para eso

`git pull origin`

Es bueno hacerlo antes de empezar a modificar, si trabajamos con otros

- - - -

## Hay una mosca en mi repo

Digamos que rompimos todo.

* Si usamos ramas, es un alivio porque si algo pasa en una rama que no es la principal no jodimos a los que usan nuestro programa.
* Si usamos etiquetas, lo que distribuimos son las versiones de las etiquetas, y de ultimo el codigo principal lo podemos volver a una de ellas.

Hay formas de **volver atras**

### Revert

Podemos usar REVERT, lo que hace revert es crear un commit nuevo usando el estado de un commit viejo. Entonces no elimina el historial de cambios, y eso hace que si trabajamos con un repo remoto no le genere lios al resto. 

`git revert [id del commit al cual queremos volver] -m [mensaje]`

### Reset

Otra forma son los reset, aca volvemos atras en el historial, todos los commits que vamos volviendo van desapareciendo o juntandose (segun la opcion).

* soft: `git reset --soft [identificador]` | vuelve al commit [identificador] pero no borra nada, deja todo como estaba listo para commitear.
* mixed: `git reset --mixed [identificador]` | vuelve al commit [identificador] pero no borra nada, y deja no esta en en Stage Area.
* hard: `git reset --hard [identificador]` | vuelve al commit [identificador] en el estado en el que estaba el commit en ese tiempo.

### Recapitulando

Si le erraste podes hacer alguna de estas cosas para volver a un lugar seguro y arreglarlo. Eso si siempre usa status para fijarte que estes sobre una rama, y que esta en el SA, que no esta siendo seguido y cosas asi.

- - - -

## FORK

Fork es cuando copias un proyecto de otro repositorio al tuyo y poder modificarlo como quieras, pero guarda una referencia a ese repo original.

Aca es mas facil usarlo en la interfaz de la pagina de Github, es un boton arriba a la derecha.

## PULL REQUEST

Pull request se puede usar para actualizar tu fork, y para sugerir cambios al repositorio original.

Si hacemos una modificación en nuestro repo (forkeado) y apretamos el boton de "pull request" en la pagina de github (devuelta es mas facil asi), hace una serie de chequeos, te pide de dejar un comentario (sino es automatico) y envia la petición, y el dueño del repo original se encarga de aceptar o no, hacerle modificaciones o lo que crea conveniente, si te acepta te convertis en **colaborador**. 

En esos chequeos que hace, se fija que este todo al dia, y si no lo une, hace un "merge", por esa razón si haces un **pull request vacio** (osea sin modificaciones) lo unico que hace es actualizar tu repo local. No hace falta pedir permiso ni nada.

## Hecho Por Gastón Pereyra

Cualquier sugerencia, cambios o correciones avisar.
