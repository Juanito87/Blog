---
title: "Manejo de Dotfiles"
date: 2021-02-16T09:15:18-03:00
draft: false
tags: ['dotfiles', 'linux', 'git']
---

# Dotfiles
## Qué son y para qué respaldarlos

Si son usuarios de sistemas \*nix, alguna vez al menos se cruzaron con un archivo de configuración, con el formato .app (por ejemplo .bashrc).
Este tipo de archivos, se encargar de almacenar las personalizaciones que uno va realizando para las aplicaciones (por gui o cli).
Y mantenerlos, dependiendo del método, puede tornarse engorroso.
Yo pase por:

- Recrear las configuraciones cada vez que las necesitaba (cosas recordables por gui)
- Hacer una copia del original antes de probar algo nuevo (empezar a modificar archivos con algún editor de texto)
- Respaldar a drive de manera manual (no querer perder los cambios si formateaba)
- Manejar un repositorio separado de la configuración, copiando y pegando cambios (no querer perder mucho tiempo recuperando la configuración de una prueba fallida)
- Y un repositorio git bare (viendo si aprendo algo)

Este camino, marca un poco como fue avanzando la complejidad de mi configuración, a medida de que empezaba a pasar más tiempo en la terminal y menos en la gui.
Esta manera de gestionar los archivos de configuración encuentro que es la más cómoda.
No solo es más fácil de mantener y recuperar cuando alguna prueba rompe algo; si no que también facilita la gestión de cualquier herramienta disponible en git.

# Qué es un repositorio git bare

La respuesta consta de dos partes, por un lado un repositorio git, es la unidad de almacenamiento para un proyecto, dentro del sistema de control de versiones git (github y gitlab son algunos ejemplos).
El control de versiones permite llevar un histórico de los cambios que se van realizando en la estructura y contenido del proyecto.
Esto permite no solo recuperar la configuración de manera sencilla luego de una prueba fallida, si no que permite revisar versiones anteriores, para buscar algo que se rompió tiempo atrás cuando lo notas, o que borraste porque ya no lo usabas, y es necesario nuevamente.
Sabiendo que es un repositorio git, ahora hay que aclarar la diferencia entre un repositorio normal y un repositorio bare.
Si clonamos o creamos un repositorio normal, se crea todo lo necesario para trabajar en el directorio donde se crea el repo:

- Archivos de administración (los archivos que git usa para configurar el comportamiento del repositorio y mantener las versiones)
- Directorio de trabajo (acá se realiza el desarrollo)
- Los archivos en los que se trabaja

En cambio un repositorio bare, es solo los archivos de administración, de acá podemos sacar los archivos que necesitamos, pero no se crea un directorio de trabajo, y por ende, tampoco se hace el checkout, como cuando se clona un repositorio normal.
Esto nos va a permitir:

1. Definir la carpeta de configuración del repo (.cfg por ejemplo)
2. Definir nuestro $HOME (donde viven todos los archivos de configuración que nos interesan) como el directorio de trabajo

La otra herramienta que podemos utilizar son los submódulos de git.
De esta manera, podemos tener repositorios adentro del repositorio, lo cual nos otorga más flexibilidad y modularidad para mantener el repositorio, incluyendo repositorios de otras personas dentro de nuestro repo.

# Como estructurar los archivos de configuración

La idea de gestionar los archivos de configuración usando git y submódulos, es que la configuración sea más sencilla de segmentar.
Al estar segmentada es más fácil mantener y encontrar la configuración que estamos buscando.
En mi caso mi estructura es:

 * dotfiles que llaman repos
    * repo con configuración de app como submódulo
         * plugins para la app como submódulos

Al usar está estructura mi home solo tiene archivos con pocas líneas que llaman al archivo que consolida la configuración de todo el repo. Por ejemplo, mi .bashrc:

```
##!/bin/bash
## shellcheck disable=1090
source ~/.shell_config/bash/bashrc
```

El archivo llamado arriba se encarga de cargar:

* Alias
* Configuraciones de comandos
* Comandos personalizados
* Variables de entorno
* Configuración del prompt
* Etc

Esto se puede hacer con todas las aplicaciones. Teniendo una configuración más segmentada, facilitando encontrar la configuración especifica que buscamos.
Esto también ayuda a que sea más difícil romper la configuración, y más sencillo debbugearla.
Otro beneficio de está segmentación es la facilidad de reutilizar código si usamos más de una terminali (bash, zsh, etc), o queremos migrar entre ellas.

# Y cómo hacemos?

El proceso es bastante sencillo. Con git ya instalado iniciamos el repositorio ejecutando:

`git init --bare $HOME/.cfg`

*Nota:* los nombres que voy a utilizar como .cfg, pueden ser modificados. Mantuve el nombre de la carpeta del [artículo](https://www.atlassian.com/git/tutorials/dotfiles) que leí, pero cambie el alias por algo que tenía más sentido para mi.

Antes de poder utilizar debemos realizar algunas configuraciones:

*Evitemos loops en el versionamiento*
`echo ".cfg" >> ~/.gitignore`

*Configuremos el alias de manera temporal para facilitar el trabajo en el repo*
`alias gitdot='/usr/bin/git --git-dir=$HOME/.cfg/ --work-tree=$HOME'`

*Nota:* el alias también puede persistirse en el .bashrc

*No mostramos los archivos que no están siendo versionados*
`gitdot config --local status.showUntrackedFiles no`

Como el repositorio no tiene nada debemos agregar los archivos que deseemos al repositorio.
Este proceso consta de 3 pasos:

* Agregamos archivos al area de staging (listo para integrarse al repositorio)
    * Agregamos submódulos (si los necesitamos)
* Realizamos el commit (para generar la versión)
* Enviamos los cambios a un repositorio remoto

Para esto:

```
gitdot add arhivo1 archivo2 ...
gitdot submodule add <url-del-repo> ./ruta/del/repo
gitdot commit -m Que cambio estamos implementando
gitdot push origin master
```

De esta manera ya queda el flujo de trabajo armado. Solo queda persistir el alias, eso va a depender de tu configuración.

## Exportar el repositorio

Una vez creado el repositorio, podemos clonarlo desde cualquier equipo en el lo deseemos.
Para eso debemos:

`git clone --bare --depth 1 --recurse-submodules --shallow-submodules url-repo $HOME/.cfg`

De esta manera podemos clonar el repositorio en formato bare, con los submódulos.
Trayendo solo el último commit de cada repositorio. Esto nos permite ahorrar espacio y aumentar la velocidad de descarga, ya que no necesitamos el histórico completo del proyecto en el repositorio local.
Una vez que hemos recreado el repositorio, debemos hacer el checkout del mismo, para poder recrear los archivos.
Este proceso hay que hacerlo en varias partes, considerando:

* Pueden ya existir archivos de configuración en esa ruta, debemos respaldarlos y moverlos
* Si usamos submódulos hay que iniciarlos y actualizarlos
* Debemos aplicar la configuración

```
alias gitdot='/usr/bin/git --git-dir=$HOME/.cfg/ --work-tree=$HOME'
mkdir -p .config-backup && \
gitdot checkout 2>&1 | egrep "\s+\." | awk {'print $1'} | \
xargs -I{} mv {} .config-backup/{}
gitdot checkout
gitdot submodule init
gitdot submodule update
source .bashrc
```

*Nota:* si los submódulos tienen también submódulos, se debe repetir el proceso de iniciar y actualizar.

## Trabajar en el repositorio

Una vez creado el repositorio, las tareas usuales van a ser:

*Versionar achivos en el repo*
Agregar un archivo al control de versiones.
`gitdot add archivo`

*Elimiar archivos en el repo*
La primer opción permite mantener la copia local, la segunda borra ambas.
```
gitdot rm --cached archivo
gitdot rm archivo
```

*Recuperar los archivos despues de una prueba fallida*
Antes de realizar un commit, deberíamos validar que el cambio funciona.
Si no lo hace, con el siguiente comando podemos revertir el cambio a la última versión commiteada.
`gitdot reset ruta/archivo`

*Recuperar un archivo versionado que borramos*
`gitdot restore archivo`

*Persistir cambios al repo*
El mensaje del commit nos ayuda a rastrear los cambios que fuimos haciendo.
Esto es útil a la hora de buscar funcionalidades o cambios que hayan roto algo.
`gitdot commit -m "Explico lo que hice"`

*Traer actualizaciones desde el repositorio central*
`gitdot pull`

*Enviar actualizaciones al repositorio central*
`gitdot push`

*Agregar submodulos*
```
gitdot submodule add url-repo ruta/al/repo
gitdot submodule init
gitdot submodule update
```

*Actualizar submodulos*
`git submodule update --remote --merge`

*Eliminar submodulos*
```
gitdot submodule deinit ruta/al/submodulo
gitdot rm --cached ruta/al/submodulo
rm -Rf .cfg/modules/ruta/al/submodulo
```

# Conclusión

Este proceso me resulta bastante flexible y más veloz que los que utilizaba antes.
El alias y las carpetas de destino pueden modificarse a gusto, para que resulte más fácil de recordar.
Si les interesa ver mis dotfiles los pueden encontrar [aquí](https://github.com/Juanito87/dotfiles.git) , pero no tiene archivo de instalación.
Como bonus, pueden agregar a su configuración lo siguiente, para que el alias gitdot funcione con autocompletado:

```
if [ -f "/usr/share/bash-completion/completions/git" ]; then
      . /usr/share/bash-completion/completions/git
fi
__git_complete gitdot __git_main
```
