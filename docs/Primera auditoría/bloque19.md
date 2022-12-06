---
layout: default
title: Bloque 19 - Otras herramientas y directorios "home"
parent: Primera auditoría
nav_order: 19
---

## Bloque 19 - Otras herramientas y directorios "home"

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/22.png"/>

Al principio hace referencia a la comprobación de permisos sobre los siguientes ficheros:

- `/etc/lilo.conf`, `LILO` (*Linux Loader*) es un gestor de arranque, predecesor de `GRUB`, durante una época fue el sistema usado para arrancar `Linux` en sistemas `x86`; actualmente se encuentra más relegado a ciertos entornos muy concretos. 

- `/root/.ssh`, busca en la raíz si hay una carpeta oculta para guardar pares de claves del usuario `root`.


Esta última parte, sobre los directorios "*/home*", es también corta. Ya que se limita a comprobar si existen un registro histórico de la `shell` en los directorios personales de los usuarios, este archivo normalmente está en la ruta `/home/nombre_usuario/.bash_history`. Este archivo recoge los comandos usados por el usuario y puede ser un buen indicativo de la actividad de este en el sistema.