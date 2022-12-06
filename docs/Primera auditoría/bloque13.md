---
layout: default
title: Bloque 13 - Base de datos, LDAP, PHP y Squid
parent: Primera auditoría
nav_order: 13
---

## Bloque 13 - Base de datos, LDAP, PHP y Squid

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/16.png"/>

Detecta una base de datos `MySQL` que coincide con la documentación de esta máquina y hace una comprobación de la contraseña del usuario `root` de la base de datos. Puede que al ser una versión antigua del S.O. o del propio `Lynis` no realiza más comprobaciones o salte algunas pruebas.

No detecta un servicio de `LDAP` (*Lightweight Directory Access Protocol*), que es un directorio donde se almacenan objetos con atributos organizados jerárquicamente (como si fueran unas "*páginas amarillas*"); tampoco detecta que tenga instalado el paquete `php`, aunque es extraño porque tiene instalado `phpMyAdmin` según la documentación de `Metasploitable 3`. Lo comprobamos:

~~~
apt-cache policy [nombre_del_paquete]
~~~

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/16_01.png"/>

Efectivamente no está instalado en el equipo, puede que esto sea debido a la configuración que hayan hecho para crear esta imagen de `Metasploitable3`, pero no he encontrado información al respecto.

Por último mira el servicio de `Squid`, que es un servidor `proxy` que soporta peticiones `HTTP/S` y `FTP` a equipos que acceden a internet a través de él y, además, almacena de manera local, caché, las páginas consultadas recientemente por los usuarios y así mejorar la rapidez de acceso a la información. Otro servicio que no está en nuestro sistema.