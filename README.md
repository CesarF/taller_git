# Taller de git

Pasos esenciales para el manejo de git con CLI.

Prueba de PR

## Antes de empezar

1. Instale [git](https://git-scm.com/)
2. Configure un usuario para git
```
git config --global user.name "nombre"
git config --global user.email "correo@ejemplo.com"
```

## Como iniciar un proyecto git

### Opción 1:
Ubíquese en la carpeta donde se albergará su proyecto.

```
git init
git remote add https://github.com/CesarF/taller_git.git
git fetch origin
git pull
git status
```
### Opción 2:
Ubíquese en la carpeta donde se albergará su proyecto.

```
git clone https://github.com/CesarF/taller_git.git
git status
```

## Como crear ramas 
Revise en cual rama está ubicado:
```
git status
```
Puede revisar que ramas tiene en su entorno local:
```
git branch
```
Cree una nueva rama
```
git checkout -b nombre_rama
git status
git branch
```

## Moverse entre ramas
Para moverse entre ramas digite
```
git checkout nombre_rama
```

## Manejar los cambios
Para agregar los cambios realizados al área de stage use:
```
git add .
```
Para crear un nuevo commit en el historial de su repositorio use:
```
git commit -m "título del commit"
```
Estos cambios quedarán en el histórico de su repositorio local, para propagarlos al repositorio remoto use:
```
git push
# o
git push origin nombre_rama
```

## Mezclar cambios
Suponiendo que usted tiene dos ramas en el repositorio remoto y que esas dos ramas tienen distintos cambios que desea mezclar, la forma correcta sería manejar esa mezcla en su repositorio local.

Supongamos que tiene dos ramas (rama_a, rama_b) y ambas ramas están en el repositorio remoto. rama_a tiene cambios que desean queden en la rama_b, para ello usted debe:

* validar que tiene ambas ramas localmente
```
git branch
```
* Si no tenemos la rama rama_a
```
git checkout -b rama_a origin/rama_a
```
* Si la tenemos
```
git checkout rama_a
```
* Validar su estado
```
git fetch
git status
```
* Bajar cambios que están en el repositorio remoto si se requiere
```
git pull origin rama_a
```
* Ahora repetimos los pasos para la rama rama_b
* Para mezclar nos ubicamos en la rama que deseamos quede con los cambios mezclados, en este caso rama_b y digitamos.
```
git merge rama_a
git status
```

# Solucionando conflictos
Supongamos que la mezcla anterior generó conflictos en algunos archivos. Es decir que habían cambios en ambas ramas que modificaron la misma línea de código. La mejor forma de solucionarlo es con su editor de texto. 

Abra el documento en su editor y encontrará algo similar a lo siguiente:
```
<<<<<<< HEAD
Acá está mi cambio
=======
Acá está el cambio en rama_a
>>>>>>> rama_a
```
Es su deber editar el documento, quitar las etiquetas ___<<<<<<< HEAD___, ___=======___, ___>>>>>>> rama_a___ y remover el cambio que ya no es válido, dejar ambos, o remover ambos.

Cuando termine de editarlo guarde sus cambios como un nuevo commit:
```
git add .
git commit -m "merge rama_a and rama_b branches"
```

# Mover cambios si moverlos al stage ni hacer commit.
Es posible que en ocasiones por varias razones usted termine modificando archivos en una rama que no los debe tener. Supongamos que son varios archivos y quiere mover los cambios a otra rama sin afectar la historio de la actual. Para ello usted debe:
```
git status
git stash
git checkout rama_b
git stash pop
git status
```
En este caso, los cambios fueron movidos de la rama rama_a a la rama rama_b sin afectar la historio de rama_a.

# Como deshacer los cambios de un último commit
Si quieres mantener los cambios. En este caso los cambios volverán al área de trabajo pero no estarán en stage o histórico.
```
git reset --soft HEAD~1
```
Si quiere eliminar los cambios.
```
git reset --hard HEAD~1
```

# Como devolver los cambios de mi repositorio.
No es lo ideal hacer este proceso, pero entendemos que puede suceder. Supongamos que de forma tardia nos hemos dado cuenta que tenemos un problema en una rama en el repositorio remoto y queremos devolver los cambios a un commit antes del commit donde agregamos el error. Para ello usted debe buscar cual es el commit adonde quiere regresar y anotar su identificador.

```
git checkout rama_a_arreglar
git pull origin rama_a_arreglar
git reset --hard commit_id
git push -f origin rama_a_arreglar
```
Este conjunto de comandos puede ser riesgoso para su repositorio, depende del estado de las demás ramas y puede estar eliminando cosas por error. Tenga mucha precaución en su uso.
