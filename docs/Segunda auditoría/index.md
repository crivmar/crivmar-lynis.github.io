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

En los siguientes bloques nos centraremos en información más detallada de los test  y en muchos casos se aportará alguna solución o explicación que pueda ser necesaria, siempre teniendo en cuenta que todo es opcional y depende la necesidades que tengamos para cada sistema.

### Contenido

- [Bloque 1 - Herramientas, plugins y arranque]({{ https://crivmar.github.io/crivmar-lynis.github.io }}{% link docs/Segunda auditoría/bl1.md %})

- [Bloque 2 - Kernel y memoria]({{ https://crivmar.github.io/crivmar-lynis.github.io }}{% link docs/Segunda auditoría/bl2.md %})

- [Bloque 3 - Usuarios, grupos y autenticación]({{ https://crivmar.github.io/crivmar-lynis.github.io }}{% link docs/Segunda auditoría/bl3.md %})

- [Bloque 4 - Shells, sistema de archivos y USB]({{ https://crivmar.github.io/crivmar-lynis.github.io }}{% link docs/Segunda auditoría/bl4.md %})

- [Bloque 5 - NFS y DNS]({{ https://crivmar.github.io/crivmar-lynis.github.io }}{% link docs/Segunda auditoría/bl5.md %})

- [Bloque 6 - Paquetes]({{ https://crivmar.github.io/crivmar-lynis.github.io }}{% link docs/Segunda auditoría/bl6.md %})

- [Bloque 7 - Conexiones, impresoras y correo electrónico]({{ https://crivmar.github.io/crivmar-lynis.github.io }}{% link docs/Segunda auditoría/bl7.md %})

- [Bloque 8 - Cortafuegos y servidor web]({{ https://crivmar.github.io/crivmar-lynis.github.io }}{% link docs/Segunda auditoría/bl8.md %})

- [Bloque 9 - SSH, SNMP y base de datos]({{ https://crivmar.github.io/crivmar-lynis.github.io }}{% link docs/Segunda auditoría/bl9.md %})

- [Bloque 10 - LDAP, PHP, Squid y registros]({{ https://crivmar.github.io/crivmar-lynis.github.io }}{% link docs/Segunda auditoría/bl10.md %})

- [Bloque 11 - Servicios inseguros, mensajes, tareas programadas y gestión de procesos]({{ https://crivmar.github.io/crivmar-lynis.github.io }}{% link docs/Segunda auditoría/bl11.md %})

- [Bloque 12 - Tiempo y sincronización, criptografía, virtualización y esquemas de seguridad]({{ https://crivmar.github.io/crivmar-lynis.github.io }}{% link docs/Segunda auditoría/bl12.md %})

- [Bloque 13 - Integridad de archivos, otras herramientas y escáneres de seguridad]({{ https://crivmar.github.io/crivmar-lynis.github.io }}{% link docs/Segunda auditoría/bl13.md %})

- [Bloque 14 - Permisos de archivos y directorios "home"]({{ https://crivmar.github.io/crivmar-lynis.github.io }}{% link docs/Segunda auditoría/bl14.md %})

- [Bloque 15 - Endurecimiento del Kernel]({{ https://crivmar.github.io/crivmar-lynis.github.io }}{% link docs/Segunda auditoría/bl15.md %})

- [Bloque 16 - Endurecimiento del sistema, fase 2 de los añadidos, test personalizados y final]({{ https://crivmar.github.io/crivmar-lynis.github.io }}{% link docs/Segunda auditoría/bl16.md %})

