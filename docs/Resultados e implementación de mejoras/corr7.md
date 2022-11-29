---
layout: default
title: Correción 7 - Sugerecias -  Herramienta de integridad de archivos, endurecimiento del Kernel y escáner de programas maliciosos
parent: Resultados e implementación de mejoras
nav_order: 7
---

## Corrección 7 - Sugerencias - Herramienta de integridad de archivos, endurecimiento del Kernel y escáner de programas maliciosos

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/48.png"/>

Iniciamos este apartado instalando una herramienta para asegurar la integridad de los archivos, en este caso vamos a instalar `AIDE`.

~~~
sudo apt install -y aide
~~~

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/49.png"/>

El archivo de configuración está en `/etc/default/aide`, las reglas en `/etc/aide/` y la base de datos se localiza en `/var/lib/aide`. Antes de proceder a la configuración de `AIDE`, es necesario crear una nueva base de datos, esto se hace con un `script` que creará el archivo `aide.db.new`

~~~
sudo aideinit
~~~

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/50.png"/>


[Aquí](https://kifarunix.com/install-and-configure-aide-on-ubuntu-20-04/) podemos ver en mayor profundidad su funcionamiento, aunque sea en `Ubuntu 20.04` es válido para aumentar los conocimientos sobre esta herramienta.

Seguimos con el endurecimiento de nuestro `Kernel`, concretamente en los valores de `sysctl` que eran dispares a los sugeridos por `Lynis`, para ello vamos a ir modificando los parámetros:

~~~
sudo sysctl -w kernel.core_uses_pid=1

sudo sysctl -w kernel.sysrq=1

sudo sysctl -w net.ipv4.conf.all.forwarding=0

sudo sysctl -w net.ipv4.conf.all.log_martians=1

sudo sysctl -w net.ipv4.conf.all.send_redirects=0

sudo sysctl -w net.ipv4.conf.default.accept_redirects=0

sudo sysctl -w net.ipv4.conf.default.accept_source_route=0

sudo sysctl -w net.ipv4.conf.default.log_martians=1

sudo sysctl -w net.ipv4.tcp_timestamps=0

sudo sysctl -w net.ipv6.conf.all.accept_redirects=0

sudo sysctl -w net.ipv6.conf.default.accept_redirects=0
~~~

Recargamos `sysctl` y comprobamos:

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/51.png"/>


**Nota: Esto es algo temporal, si reiniciamos perderemos esta configuración, si queremos algo más definitivo tendremos que cambiar estos valores en `/etc/sysctl.conf` y añadir los valores que no estén de manera manual.**


El clásico compilador que se suele usar para lenguaje `C` y `C++` (la base de la mayoría de los sistemas operativos) es el que contiene el paquete de `build-essential`, para ello vamos a desinstalarlo.

~~~
sudo apt remove -y build-essential --purge

sudo apt-get autoremove -y
~~~


Y acabamos instalando un escáner de programas maliciosos, vamos a instalar `chkrootkit`:

~~~
sudo apt install -y chkrootkit
~~~

[Aquí](https://esgeeks.com/buscar-rootkits-linux-rkhunter-chkrootkit/) podemos encontrar información de uso de este programa, `RkHunter` y otras herramientas, para conocer mejor su funcionamiento.