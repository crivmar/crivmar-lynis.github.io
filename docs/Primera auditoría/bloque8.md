---
layout: default
title: Bloque 8 - NFS, DNS y paquetes
parent: Primera auditoría
nav_order: 8
---

## Bloque 8 - NFS, DNS y paquetes

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/11.png"/>

El primer apartado de este bloque comprueba si está funcionando el servicio `NFS`(*Network File System*), esto es un sistema de ficheros distribuido en la red local. Permite configurar el acceso remoto de varios dispositivos a un servidor y montarlos en un punto local y así trabajar con ellos como si estuviera en nuestro propio sistema. Esto está pensado para que la capacidad de almacenamiento recaiga sobre el servidor y en las otras máquinas tener un ahorro de espacio de almacenamiento, así como la facilidad de compartir recursos.

El siguiente apartado va relacionado con servicios de "nombres", busca en la máquina si tiene dominios y si usa algunos de los programas más típicos de `DNS` (*BIND, PowerDNS, etc*). En este caso encuentra el nombre de dominio que se le asigna al crearse (`mt3`), pero no puede resolverlo porque realmente el servidor DNS al que apunta la máquina no tiene un registro para ello. Luego comprueba el archivo `/etc/hosts` para ver si ha registros duplicados o extraños.

Este bloque finaliza con los gestores de paquetes, encuentra `dpkg` como manejador de paquetes del sistema y hace una consulta para paquetes no purgados, dando un resultado negativo. Luego comprueba si está el repositorio de seguridad del archivo `/etc/apt/sources.list`, estos son los repositorios que cada distribución tiene asociados a las actualizaciones de seguridad del sistema para parchear vulnerabilidades detectadas.
Comprueba que la base de datos del comando `apt` están actualizadas, si los paquetes instalados por este comando tienen alguna vulnerabilidad y si hay paquetes de versiones más recientes; este caso lo omite debido a la antigüedad de la versión de `Ubuntu` que tiene la máquina; la versión `14.04` ya no tiene unos repositorios que funcionen debido a que su ciclo de soporte acabó en 2019. Aunque `Canonical` anunció una ampliación hasta 2024 si pagas por ello, no es el caso. 

Finaliza comprobando si dispone de alguna herramienta de auditoria de paquetes, tales como `apt-listbugs` o `pkg_admin`(*OpenBSD*).

