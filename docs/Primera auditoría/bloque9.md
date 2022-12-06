---
layout: default
title: Bloque 9 - Conexión a la red e impresoras
parent: Primera auditoría
nav_order: 9
---

## Bloque 9 - Conexión a la red e impresoras

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/12.png"/>

Inicia comprobando las direcciones de nombres en `etc/resolv.conf`y luego comprueba su validez probando si responden; en este caso detecta la suministrada por `Vagrant` (`10.0.2.3`, su red de mantenimiento) y las que le suministra el servidor `DHCP` de mi máquina local a la que está conectada. Comprueba la puerta de enlace por defecto, esta podemos verla con el comando `ip r`.

Prosigue con la obtención de los puertos que tenemos en escucha (`TCP/UDP`), aunque en la imagen observamos que tiene una errata y pone `TCP/TCP`, encuentra 32 puertos escuchando, vamos a comprobarlo:

~~~
ss -tul | wc -l


-t -> puertos TCP
-u -> puertos UDP
-l -> puertos que estén escuchando

el comando wc para contar la salida del comando anterior y con la opción -l cuenta el número de líneas
~~~

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/12_01.png"/>

Como vemos nos muestra 55 puertos escuchando (serían menos porque está teniendo en cuenta los puerto IPv6), pero el programa nos muestra algo distinto. Vamos a comprobarlo yendo al archivo `/var/log/lynis-report.dat`

~~~
sudo /var/log/lynis-report.dat | grep "network_listen_port"
~~~

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/12_02.png"/>

Nos fijamos que los puertos dados son todos `UDP` tanto de `IPv4` como `IPv6`, por lo cuál el test debe estar fallando, posiblemente este error se haya corregido en una versión actualizada de este programa (**hay que tener en cuenta que al principio nos saltó un "peligro" al no tener la versión más moderna**), más adelante comprobaremos si actualizando se corrige.

Seguimos con este apartado, el paso siguiente es comprobar si hay interfaces en modo promiscuo. Esto quiere decir que si tuviéramos activado este modo, todos el tráfico de una red se envía a un `sniffer` que recolecta esta información; si estuviera activo y no lo estuviéramos monitorizando nosotros, podría tratarse de alguien ajeno que está intentado capturar nuestro tráfico.
Después, pasa a comprobar si hay conexiones esperando, en un servidor web es normal tener miles de peticiones TCP en espera; nuestro servidor `Apache` no tiene ninguna porque está en una red privada. También comprueba el estado del cliente `DHCP`, en nuestro caso está activo porque usa IP dinámica.


La última parte está centrada en las impresoras, comprueba si el *demonio* `cups` (*Common Unix Printing System*) está corriendo y si tiene archivo de configuración en `/etc/cups/cupsd.conf`, como no tenemos ninguna impresora y tampoco hay ninguna que esté en nuestra red, no se detecta ninguna conexión final (`socket`).



---

Información adicional
{: .label .label-purple}

\* `sniffer`-> es una herramienta de software o hardware que permite al usuario supervisar su tráfico en Internet en tiempo real y capturar todo el tráfico de datos que entran y salen de su equipo. Algunos ejemplos serían `Wireshark`, `tcpdump`, `NetworkMiner`, `Fiddler`,etc.

