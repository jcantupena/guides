### Flujo de trabajo en Git

El repositorio central tiene dos _branches_ con una linea de tiempo infinita:

* master
* develop

Consideramos __master__ para ser la _branch_ donde el codigo fuente siempre
refleja un estado listo para produccion.

Consideramos __develop__ como la _branch_ principal donde el codigo fuente
siempre reflaja el estado donde estan los ultimos desarrollos hechos listos para
el siguiente _release_.

Cuando el codigo fuente en __develop__ llega a un punto estable y esta listo
para hacer un _release_, todos los cambios deberian de unirse de vuelta a master
a traves de un _merge_. Asi, cada vez que un cambio se una a __master__, se
convierte en _release_ listo para prouduccion.

### Ejemplo de flujo de trabajo

Crear una _branch_ local desde __develop__ para el nuevo desarrollo.

```sh
git checkout develop
git pull origin develop
git checkout -b feature/mi-branch # Se utiiliza el prefijo feature
```

Haz tus cambios en la _branch_ y luego da _commit_ a ellos.

```sh
git status
git add -A
git commit
```

Escribe un buen mensaje de _commit_.

```
Un mensaje capitalizado, breve (de 50 caracteres o menos)

Una explicacion mas detallada si es necesaria. Con aproximadamente 72 caracteres
por linea.

Escribe tu mensaje en forma imperativav, ej: "Corrijir bug" y no "Se arreglo
bug". Esta convencion sigue el de los comandos de git merge y git revert.

Parrafos extra van despues de lineas en blanco.

* Tambien se pueden utilizar vinetas.
* Tipicamente, un guion o un asterisco se utilizan para la vineta, precedido de
  un solo espacio, sin lineas en blanco entre ellos, pero no hay una convencion
  exacta.
```

Agrega el numero de _issue_ al principio.

````
[#123] Mensaje de commit.
````

Subir tu _branch_.

````
git push origin feature/mi-branch
````

Abre un _Pull Request_ y avisa a los miembros del proyeccto para una revision de
codigo.

Utiliza _rebase_ frecuentemente para incorporar los cambios a __develop__.

````
git checkout develop
git pull origin develop
git checkout [branch]
git rebase develop
<resuelve los conflictos>
````

Tambien puedes utilizar _rebase_ para mantener un historial limpio.

Un buen _log_ de _commits_:

```
* Agregar los modelos y formas para las papas.
* Agregar el API para interactuar con las papas, junto con las pruebas unitarias.
* Agregar el dialogo de comentarios para revisar las papas.
```

Un mal _log_ de _commits_:

```
* Agregar los modelos y formas para las papas.
* Se decidio que el campo is_spud no era necesario y se removio.
* Olvide el parcial de la forma.
* Agregar el API para interactuar con las papas, junto con las pruebas unitarias.
* Una prueba fallo, se arreglo.
* Agregar el dialogo de comentarios para revisar las papas.
* Arreglo un typo.
* Arreglo otro typo.
```

Si todo esta bien, entonces haz _merge_ de tu _branch_ a __develop__ y actualiza
tus cambios.

````
git checkout develop
git merge [branch]
git push origin develop
````

Borra tu _branch_ local.

````
git branch -d [branch]
````

Borra en el repositorio remoto tambien.

````
git push origin :[branch]
````

### Preparar un release a produccion

Crear la _branch_ del _release_ desde __develop__.

```sh
# En la branch de develop
git checkout -b release-[YYYMMDD]-[HHMM]
```

Nombre de ejemplo de la branch: `release-20140325-1804`

Como lo dicta el flujo de git: Esta nueva _branch_ puede existir por un tiempo,
hasta que el _release_ sea lanzado definitivamente. Durante este tiempo, se
puede ccorrejir bugs en esta _branch_ (en vez que la de __develop__). Se
prohibe agregar nuevas funcionalidades. Estas deben de hacerse sobre __develop__
y por ende, esperar hasta el siguiente _release_.

Una vez que todo esta listo, haz _merge_ de la _branch_ del _release_ a __master__.

````
git checkout master
git merge release-[YYYYMMDD]-[HHMM]
````

Agregar el _tag_ de la version del _release_.

````
git tag -a [YYYYMMDD]-[HHMM]
````

Tambien actualiza __develop__ con la nueva _branch_.

````
git checkout develop
git merge release-[YYYYMMDD]-[HHMM]
````

Finalmente, actualiza el repositorio remoto.

````
git push origin master
git push origin develop
git push origin --tags
````
