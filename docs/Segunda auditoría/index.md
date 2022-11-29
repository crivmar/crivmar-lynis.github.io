---
layout: default
title: Segunda auditoría
nav_order: 6
has_children: true
has_toc: false
---

# Segunda auditoría

Es el momento de realizar la segunda pasada a nuestro sistema, tras los cambios realizados. Como es una nueva versión del programa puede diferir de la primera auditoría y mostrarnos cosas que aunque hayamos cambiado, para los estándares más actualizados de esta versión pueden no ser válidos o insuficientes. Vamos a `/usr/local/lynis/` y allí ejecutamos el comando de la siguiente forma:

~~~
sudo ./lynis audit system --auditor "CR" --log-file ~/auditoria2.log
~~~

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/52.png"/>

Nada más ejecutar el programa ya nos dice en el apartado `End-of-life` que esta versión de `Ubuntu` está fuera de soporte y además, añade el concepto de `plugins` que antes no aparecían en el primer reporte. Iremos viendo por bloques lo que nos muestra esta versión del programa.



### Contenido

- [Bloque 1 - Herramientas, plugins y arranque]({{ https://crivmar.github.io/crivmar-lynis.github.io }}{% link docs/Segunda auditoría/bl1.md %})

