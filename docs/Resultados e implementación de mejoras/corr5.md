---
layout: default
title: Correción 5 - Sugerencias - CUPS y módulos Apache2
parent: Resultados e implementación de mejoras
nav_order: 5
---

## Corrección 5 - Sugerencias - CUPS y módulos Apache2

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/44.png"/>

Vamos a desinstalar `CUPS` de nuestro sistema, ya que no necesitamos hacer uso de impresoras

~~~
sudo apt remove -y cups --purge

sudo apt-get -y autoremove
~~~

Como ya en el primer apartado de esta sección nos ocupamos de `Iptables`, vamos a ir directamente a instalar los `mods` sugeridos por el programa para `Apache2`y los dejaremos por defecto:

~~~
## mod_evasive para prevenir contra ataques DdS/fuerza bruta

sudo apt install -y libapache2-mod-evasive

## mod_qos para prevenir ataque Slowloris

sudo apt install -y libapache2-mod-qos

## mod_spamhaus contra "spammers"

sudo apt install -y libapache2-mod-spamhaus

## modsecurity para prevenir ataques a las aplicaciones web

sudo apt install -y libapache2-modsecurity

~~~

Tras todo esto, reiniciamos el servidor `Apache2` y estarán disponibles.

~~~
sudo service apache2 restart
~~~


---

- **Configuración de mod_evasive -> [Aquí](https://juantrucupei.wordpress.com/2016/09/07/instalacion-y-configuracion-de-modulo-mod_evasive-servidor-web-apache/)**

- **Ataque Slowloris -> ataque de denegación de servicio que permite a una sola máquina haciendo uso de un ancho de banda mínimo. Este intenta mantener abiertas muchas conexiones al servidor web objetivo y mantenerlas abiertas el mayor tiempo posible. Lo consigue abriendo conexiones al servidor web de destino y enviando una petición parcial.**

- **Configuracion de mod_qos -> [Aquí](https://mod-qos.sourceforge.net/#configuration)**

- **Configuración de mod_spamhaus -> [Aquí](http://www.softwero.com/2014/11/inyeccion-dns-guia-asegurar-apache-de-ataques.html)**

- **Configuración de modsecurity -> [Aquí](https://phoenixnap.com/kb/setup-configure-modsecurity-on-apache)**