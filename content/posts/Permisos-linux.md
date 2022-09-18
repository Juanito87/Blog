---
title: "Sistema de permisos en Linux"
date: 2022-06-18T20:12:09-03:00
draft: true
---

# Introducción al sistema de permisos de linux

A diferencia de los sistemas operativos windows, en linux los permisos se pueden otorgar tres permisos básicos:

* Lectura   (r)
* Escritura (w)
* Ejecución (x)

Estos a su vez pueden ser dados a tres grupos:

* Dueño (o)
* Grupo (g)
* Mundo (w)

Hay algunos permisos especiales, pero están por fuera del alcance de este articulo.
Una manera adicional de gestionar los permisos en linux es utilizando números.
Esto utiliza 3 pares de números, que describen la totalidad de los permisos del grupo.
Por ejemplo:

```
Lectura = 4
Escritura = 2
Ejecución = 1

Con esos valores podemos calcular lo siguiente:

Todos los permisos (7) = Lectura (4) + Escritura (2) + Ejecución (1)
Lectura y escritura (6) = Lectura (4) + Escritura (2)
Lectura y ejecución (5) = Lectura (4) + Ejecución (1)
```

De seta manera podemos definir un número para asignar los permisos a cada grupo como de usuarios que puede operar sobre el archivo.
El dueño del archivo, es inicialmente quien lo haya creado, pero esto puede modificarse.

```
-rwxrwxr-x  1 user group        1325 mar  9 18:05  script.sh
||||||||||    |    |> grupo al que pertenece el archivo
||||||||||    |> usuario dueño del archivo
||||||||||> permiso de ejecución para los usuarios que no son el dueño y no pertencen al grupo
|||||||||> permiso de escritura para los usuarios que no son el dueño y no pertencen al grupo
||||||||> permiso de lectura para los usuarios que no son el dueño y no pertencen al grupo
|||||||>
||||||>
|||||>
||||>
||||>
|||>
||>
|>

```

La línea muestra como se ven los permisos en una terminal de linux
