---
title: "Como instalar Wsl"
date: 2021-02-18T22:35:14-03:00
draft: false
tags: ['wsl', 'linux', 'windows', 'git']
---

# Qué es WSL

WSL significa windows subsystem para linux. Esto nos permite ejecutar ambientes linux en equipos windows, de una manera mucho más sencilla y con menos carga para el sistema, que si usaramos una maquina virtual.
Actualmente existen dos versiones, wsl2 incluye optimizaciones en la arquitectura de wsl, pero solo esta disponible para windows 10 a partir de la versión 2004 (mayo 2020) o compilación 19041.
Una vez instalado wsl, podemos instalar algunas distros (ubuntu, kali, etc) desde la tienda de microsoft.
Para más información, respecto de las distintas versiones de wsl, pueden revisar el siguiente [link](https://docs.microsoft.com/es-es/windows/wsl/compare-versions).

## Cómo instalarlo

Este proceso va a depender de la versión de windows 10 que se este utilizando. Por lo que para instalar wsl2 tenemos que contar con la versión 2004 de windows 10, si no, solo podemos  instalar la versión normal de wsl.
Se debe corroborar antes de iniciar el proceso:

* Que la versión de windows sea compatible
* Que en el BIOS esta habilitada la opción de visualización
* Que exista un punto de restauración antes de iniciar

La instalación se realiza en dos etapas. Primero tenemos que habilitar el subsistema de linux en nuestro windows, lo podemos hacer por cli o gui.

**cli**

`dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart`

*Nota:* para instalar por cli el comando debe ejecutarse en powershell como administrador.

**gui**

Control panel > Programs and features > Turn windows features on or off

Una vez seleccionada la opción, esperamos a que active los componentes, y nos solicitará reiniciar el equipo.
A partir de ahora podemos instalar distribuciones de linux desde la tienda de microsoft.
Pero hay que considerar que si se va a utilizar wsl2, todavía quedan pasos por realizar antes de instalar la distro de linux.
Ahora se puede actualizar a wsl 2. Este proceso lo debemos hacer por cli.

```
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart

wsl --set-default-version 2
```

El primer comando, actualiza los componentes y el segundo setea wsl2 como la opción por defecto.
El último paso es instalar el kernel para wsl2 de manera manual, lo podemos descargar [aquí](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)
El proceso es sencillo solo hay que ejecutar el instalador y darle siguiente, terminada la instalación, reiniciamos.
Ahora podemos instalar nuestra distribución de linux desde la tienda (yo instale ubuntu 20.04).

*Nota:* la tienda de microsoft puede solicitar el ingreso de una cuenta microsoft, esto es para asociar la instalación con la cuenta, pero no es necesario tenerla para poder avanzar con la instalación. Se puede cerrar la ventana y la instalación avanza igual.

Cuando iniciemos la imagen por primera vez, nos solicitará ingresar un usuario y contraseña, y con esto ya tenemos instalada una distribución en wsl.

## Siguientes pasos

Ya con wsl instalado y funcionando en la maquina, ya podemos buscar las aplicación que nos interesen en linux.
Como ejemplo podemos usar algo así:

```
sudo apt update && sudo apt upgrade -y
sudo apt install git vim powerline shellcheck php php-pear npm ripgrep
sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
      $(lsb_release -cs) \
         stable"
sudo apt install docker-ce docker-ce-cli containerd.io docker-compose
```

El primer comando va a actualizar los datos del repositorio y todos los paquetes instalados.
El segundo va a instalar git (para control de versiones), vim (ide) y el resto de los paquetes son dependencias de mi configuración personal de vim (que pueden revisar [acá](https://github.com/Juanito87/dotfiles).
A partir del paso tres instalamos las dependencias de docker, la llave pgp del repositorio, agregamos el repositorio, e instalamos docker y docker-compose.
Con esto ya podemos andar creando contenedores con plex, pihole o lo que quieran.
