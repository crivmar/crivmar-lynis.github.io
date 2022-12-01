---
layout: default
title: Correción 6 - Sugerencias - Eliminar archivos borrados en uso, añadir mensaje y habilitar Auditd
parent: Resultados e implementación de mejoras
nav_order: 6
---

## Corrección 6 - Sugerencias - Eliminar archivos borrados en uso, añadir mensaje y habilitar Auditd

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/45.png"/>

Los archivos borrados en uso se pueden eliminar de una manera muy simple, siempre y cuando el servidor pueda permitirse reiniciar. Si no fuera el caso, listaríamos estos con `lsof`:

~~~
sudo lsof | grep deleted
~~~

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/46.png"/>

Iriamos uno a uno comprobando los procesos por su `PID`, por ejemplo el `31269`

~~~
sudo ps aux | grep 31269
~~~

Si este proceso no nos vale, lo podríamos eliminar con:

~~~
sudo kill PID
~~~

Continuamos añadiendo mensajes a mensajes a `/etc/issue` o `/etc/issue.net` y lo siguiente es entrar en `/etc/ssh/sshd_config` y descomentar su línea correspondiente

~~~
Banner /etc/issue.net
~~~

Tras reiniciar el servicio, debe aparecer lo siguiente cada vez que nos conectemos por `ssh`:

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/47.png"/>

Por último, vamos a instalar `auditd` en nuestro sistema, este programa sirve para registrar la mayor cantidad posible de información sobre los eventos en el sistema. [Aquí](https://www.busindre.com/auditando_el_sistema_con_auditd) podemos ver una guía sobre este programa, en este caso se dejará por defecto.

~~~
sudo apt install -y auditd
~~~

El fichero donde se guarda esta información está en `/var/log/audit/audit.log`, pero es necesario configurar reglas con el comando `auditctl` para tomar una determinada información.
