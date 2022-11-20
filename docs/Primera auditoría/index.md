---
layout: default
title: Primera auditoría
nav_order: 4
has_children: true
has_toc: false
---

# Primera auditoría

Es el momento de auditar este sistema y comprobar el poder bruto de la herramienta, pero antes vamos a comprobar los test que puede realizar la versión de la comunidad a este sistema de pruebas

~~~
sudo lynis --view-categories
~~~

![](/img/03.png)

Como vemos hace una gran cantidad de test, ya iremos entrando en estas categorías con la primera pasada de este programa. Vamos a hacerlo usando algunas opciones:

- `--auditor [nombre]` , en el registro de la auditoría pone el nombre que le indiquemos.

- `-c`, indicamos que haga un escaneo completo con todos los test disponibles.

- `--logfile [ruta]`, cambiamos la ruta donde se va a guardar el registro de la auditoría. Por defecto es `/var/log/lynis.log`

~~~
sudo lynis --auditor CR -c --logfile /home/vagrant/auditoria1.log
~~~

Como el resultado que nos arroja es bastante extenso, vamos a ir bloque a bloque intentando explicar lo máximo posible.


### Contenido

- [Bloque 1 - Inicio del programa]({{ https://crivmar.github.io/crivmar-lynis.github.io/ }}{% link docs/Primera auditoría/bloque1.md %})

