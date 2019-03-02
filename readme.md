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

## Empezando

Hay varias maneras de empezar

Van al repo remoto, por ejemplo Github, y en sus cuentas crean un nuevo repositorio

<img src="https://github.com/gastonpereyra/ejemploDeGit/blob/master/imagenes/github-repoNuevo.png">

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

<img src="https://github.com/gastonpereyra/ejemploDeGit/blob/master/imagenes/status-vacio.png">

Cuando hiciste cambios asi:

<img src="https://github.com/gastonpereyra/ejemploDeGit/blob/master/imagenes/status-cambios.png">

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

<img src="https://github.com/gastonpereyra/ejemploDeGit/blob/master/imagenes/status-SA.png">

### Guardar los cambios

Cuando lo queres guardar se usa el siguiente comando

```
# git commit -m [mensaje]
git commit -m "agregar index.html"
```

Si solo pones "git commit" inicia el editor, que puede ser un dolor de cabeza si no lo sabes usar o no lo configuraste para uno que si sepas.

Asi esta guardado.

### Guardar modificaciones

Si ya tenias guardado un archivo y lo modificas, hay una forma facil de volverlo a guardar

```
# git commit -am []
```

