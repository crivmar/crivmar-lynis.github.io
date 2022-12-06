---
layout: default
title: Bloque 3 - Arranque del sistema y servicios 
parent: Primera auditoría
nav_order: 3
---

## Bloque 3 - Arranque del sistema y servicios

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/06.png"/>

Detecta los sistemas de arranque, en este caso detecta `GRUB2` que es el que usa `Ubuntu` y comprueba si usas otros como [Lilo](https://es.wikipedia.org/wiki/Lilo_(Linux)), [Silo](https://es.wikipedia.org/wiki/SILO_(boot_loader)) y [Yaboot](https://en.wikipedia.org/wiki/Yaboot). Lo siguiente es comprobar los servicios que están corriendo con `systemd` y cuántos están habilitados.

Vemos que hay 14 servicios que se inician en el arranque, indican que son `rc2.d`. Esto tiene que ver con los `runlevel` que determinan los programas que se ejecutan al iniciar el sistema; el "2" indica que es "modo multiusuario" (por defecto) y si fuéramos a `/etc/rc2.d/`, veríamos una lista de ficheros que son enlaces simbólicos a sus respectivos `script` en `/etc/init.d`, que controlan la detención o inicio de estos servicios.

Los archivos en `rc2.d` tienen la siguiente nomenclatura:

~~~
[S | K]nn[nombre del servicio]

- S o K , indican si se inicia (S) al entrar en este nivel o se mata (K).

- nn , es el número que va del 01 al 99 que indica la prioridad ascendente de iniciar el programa. Si tuvieran la misma, se inician por orden alfabético.
~~~ 