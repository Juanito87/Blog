---
title: "Vim"
date: 2021-03-24T23:24:31-03:00
draft: false
---

# Qué es VIM?

Vim es el mejor editor de texto, aunque haya gente que opine algo distinto.
Una de las grandes ventajas de vim es su capacidad de adaptarlo a lo que necesites, siempre que estés dispuesto a dedicarle tiempo.
Si bien la curva de aprendizaje inicial es relativamente compleja, una vez que te acostumbras, no hay vuelta atrás.
Para mi caso particular, uso la versión de terminal de vim (hay una versión con gui incluso para windows).
Una de las características más importantes de vim más allá de su capacidad de adaptación; es que por defecto incluye varias herramientas.
Las mismas son de fácil acceso desde el teclado, por lo que su uso es más veloz que por gui.

# Como salir de vim

Una de las preguntas más buscadas en plataformas como stackoverflow es como salir de vim, ya que la gui no ofrece ninguna ayuda.
Hay varias opciones, pero lo más sencillo es:

1. Ir a modo normal (se debe presionar la tecla "esc")
2. Presionar la tecla ":" para poder ingresar comandos
3. Presionar alguna de las siguientes opciones:
    1. "q" para salir
    2. "q!" para salir y descartar los cambios
    3. "qw" o "x" para guardar y salir

Una vez que solucionamos esto, viene la parte divertida que es aprender a usar los distintos modos de vim.
De ninguna manera el articulo pretende ser extensivo en el uso de vim porque es imposible,
pero al menos cubrir las bases para poder iniciar en el uso de la herramienta.

# Modos

Para utilizar vi/vim hay que entender que el mismo opera en modos:

* insertar
* visual
* normal

En modo insertar es cuando vamos a poder escribir y pegar contenido de la papelera del equipo.
El modo visual nos permite operar de una manera similar a si tuviésemos una gui, pero mejor.
Y en modo normal nos podemos desplazar por la pantalla utilizando los atajos de teclado de vim (movimientos).
A través del teclado, podemos ir cambiando entre los diversos modos de vim.
También es factible realizar operaciones como buscar y/o remplazar, consultar la ayuda de vim o ejecutar comandos.

## Normal

En este modo es donde no solo debemos aprender a movernos, si no también acostumbrarnos a volver.
Por volver me refiero a realizar una operación en algún otro modo, y luego presionar **esc** para salir de dicho modo.
Esto nos otorga una de las primeras ventajas de vim, que es la atomicidad.
Por ejemplo:

* Presiono **i** para entrar a modo
* Escribo una línea
* Presiono **esc** para volver a modo normal

Ahora la línea que escribí, puedo repetir, deshacer o rehacer.
Luego tenemos los movimientos, como por ejemplo:

* **hjkl** actúan como si fueran las flechas
* **wbe** nos permiten movernos entre palabras
* **fF** nos permite buscar para adelante o para atrás en la línea
* **gg** nos permite ir a la primer línea del archivo
* **g#** nos permite ir a la línea # donde # es un número de línea
* **G** nos permite ir a la última línea del archivo
* **0$** nos permite ir al principio o al final de la línea

A su vez, los movimientos pueden ser combinados con números para realizarlo x cantidad de veces.
Ejemplo:

* **5w** va a avanzar 5 palabras
* **5b** va a retroceder 5 palabras
* **5e** va a avanzar al final de la quinta palabra
* **5j** va a bajar 5 líneas
* **5k** va a subir 5 líneas
* **5h** va a ir 5 columnas a la izquierda
* **5l** va a ir 5 columnas a la derecha

Los movimientos luego pueden ser combinados con acciones:

* **d** para borrar
* **y** para copiar
* **p** para pegar
* **r** para reemplazar un carácter
* **c** para cambiar

En el caso de cambiar, la acción nos cambia al modo inserción, mientras que el resto nos mantienen en modo normal.
Estos son solo algunas de las acciones y movimientos que se pueden realizar.
Suena más complejo de lo que realmente es, pero podemos utilizar vimtutor para familiarizarnos con estas tareas.
Ya sea en la terminal u opciones en línea como [esta](https://www.openvim.com/)

### Búsqueda

En modo normal podemos realizar búsquedas, para esto presionaremos:

* **/** si queremos buscar hacia adelante
* **?** si queremos buscar hacia atrás
* ***** si queremos buscar la palabra sobre la que está el cursor para adelante
* **#** si queremos buscar la palabra sobre la que está el cursor para atrás
* **%** si queremos encontrar el par del parentesis

Adicionalmente podemos buscar y reemplazar, por línea o en todo el documento.
Para esto, en modo normal haremos :

```
:[rango]s/{patrón}/{remplazo}/[opciones] [número]
```

Los segmentos de busqueda son los siguientes:

* Rango es opcional, se puede usar **%** para indicar que la busqueda aplica a todo el archivo. Si no se indica rango aplica a la línea actual.
* Número, también opcional, indica el número de veces que vamos a repetir la acción
* Opciones, con ellas podremos determinar la conducta de la busqueda:
    * g (global) para buscar todas las coincidencias de la busqueda, no solo la primera
    * c (confirm) para que vim pida confirmación antes de reempazar
    * i (insensitive) para que no distinga mayúsculas y minúsculas en la busqueda

El patrón de busqueda puede aceptar expresiones regulares y subexpresiones, pero ese tópico no va a ser cubierto en este articulo.

## Insertar

En este modo vamos a escribir todo lo que querramos. Es importante determinar que todo lo que escribimos entre el momento que entramos en modo insertar y salimos es una sola acción.
Es decir, si luego queremos deshacer o repetir, va a aplicar para todo lo que se haya escrito entre el ingreso y la salida del modo insertar.
Es por esto, que lo más recomendable es utilizar acciones cortar en el modo insertar, y mantenerse la mayor cantidad de tiempo posible en el modo normal. Esto nos permitirá ser más veloces a la hora de tomar acciones en el editor.

Para ingresar al modo insertar tenemos varias opciones:

* **i** para ingresar al modo insertar donde el esta cursor
* **I** para ingresar al inicio de la línea donde esta el cursor
* **a** para ingresar en el caracter de adelante del cursor
* **A** para ingresar al final de la línea donde esta el cursor
* **c** para ingresar haciendo un cambio, esto debe combinarse con otros movimientos por ejemplo cw para cambiar una palabra
* **C** para ingresar cambiando desde donde esta el cursor al final de la línea
* **s** para eliminar el caracter sobre el que está el cursor y entrar al modo insertar
* **S** para eliminar toda la línea e ingresar al modo insertar
* **o** para ingresar una línea en blanco en la línea de abajo
* **O** para ingresar una línea en blanco en la línea de arriba
* **ea** para ingresar al modo inserción al final de la palabra bajo el cursor

## Visual

En este modo podemos realizar acciones similares al modo normal pero con indicadores visuales.
Este modo es similar a estar usando un editor con interfaz gráfica.
Su mayor ventaja es que si todavía no estás familiarizado con el uso de vim, te permite tener un feedback visual para validar las selecciones.

# Personalización

Vim puede ser utilizado de un monton de maneras, podemos optar por versiones adicionales como neovim, vim o vi.
Puede ser configurado a través de un archivo rc, y ofrece un montón de opciones para que se adapte a lo que uno quiere.
Adicionalmente, tiene su propio lenguaje de scripting, por lo que si vim no hace lo que queres, podes crear un script o plugin que si lo haga.
Usualmente no es necesario crear script a menos que sean casos muy puntuales, ya que hay una variedad enorme de plugins para vim ya creados.
Estoy pueden ser manejados nativamente por vim a partir de la versión 8, o utilizando un gestor de plugins como puede ser (pathogen de tpope)[https://github.com/tpope/vim-pathogen]
Tanto las opciones de configuración como los plugins tienen archivos de ayuda que podemos acceder para definir como configurarlos.
Para ver estas configuraciones solo debemos desde el modo normal presionar **:h** para acceder a la ayuda, y acepta palabras claves para ir a partes especificas de la ayuda.
Estas mismas opciones que podemos consultar las podemos configurar desde la terminal que activamos con **:** en modo normal, o podemos persistirlar en el archivo rc de vim.

# Conclusión

La versatilidad de vim lo hace una gran herramienta, sobretodo para quientes tienen que trabajar en una terminal.
Si les interesa ver ejemnplos de configuración de pueden consultar mi repositorio (acá)[https://github.com/Juanito87/.vim].
Con el pasar del tiempo, ire expandiendo este articulo con otros relcionados, como explicaciones de plugins y configuraciones.
O expandir sobre el uso de expresiones regulares, gestiones de ventanas y pestañas, buffers, macros, etc.
