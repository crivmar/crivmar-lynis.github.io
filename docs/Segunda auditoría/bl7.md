---
layout: default
title: Bloque 7 - Conexiones, impresoras y correo electrónico
parent: Segunda auditoría
nav_order: 7
---

## Bloque 7 - Conexiones, impresoras y correo electrónico

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/63.png"/>


En este bloque se muestran algunas diferencias respecto a la anterior versión del programa, primero centrado en `IPv6`:

~~~
2022-11-29 09:23:09 ====
2022-11-29 09:23:09 Performing test ID NETW-2600 (Checking IPv6 configuration)
2022-11-29 09:23:10 Result: IPV6 mode is auto
2022-11-29 09:23:10 Result: IPv6 only configuration: NO
2022-11-29 09:23:10 ====

...
~~~

Ya hemos visto en anteriores apartados que este protocolo está habilitado y sus valores son dados por el servidor `DHCP` al que está conectada la máquina virtual, si no quisiéramos tenerlo, nos vamos al archivo `/etc/sysctl.conf` y cambiamos (ó añadimos) estas líneas para deshabilitarlo:

~~~
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1
net.ipv6.conf.lo.disable_ipv6 = 1
~~~

También vemos el apartado `ARP monitoring software`, que no se mostraba con el anterior versión de `Lynis`:

~~~
2022-11-29 09:23:10 ====
2022-11-29 09:23:10 Performing test ID NETW-3032 (Checking for ARP monitoring software)
2022-11-29 09:23:10 Performing pgrep scan without uid
2022-11-29 09:23:10 IsRunning: process 'addrwatch' not found
2022-11-29 09:23:10 Performing pgrep scan without uid
2022-11-29 09:23:10 IsRunning: process 'arpwatch' not found
2022-11-29 09:23:10 Performing pgrep scan without uid
2022-11-29 09:23:10 IsRunning: process 'arpon' not found
2022-11-29 09:23:10 ====

...
~~~

Vemos herramientas como `arpwatch`, esta produce un registro de emparejamientos anotados de información de direcciones IP y MAC junto con marcas de tiempo, para que pueda observar con atención cuándo apareció la actividad de emparejamiento en la red. Podemos ver como se instala en este [enlace](https://es.linux-console.net/?p=548#gsc.tab=0).

Finaliza buscando protocolos de red poco comunes:

~~~
2022-11-29 09:23:10 ====
2022-11-29 09:23:10 Performing test ID NETW-3200 (Determine available network protocols)
2022-11-29 09:23:10 Test: checking the status of some network protocols that typically are not used
2022-11-29 09:23:10 Test: now checking module 'dccp'
2022-11-29 09:23:10 Suggestion: Determine if protocol 'dccp' is really needed on this system [test:NETW-3200] [details:-] [solution:-]
2022-11-29 09:23:10 Test: now checking module 'sctp'
2022-11-29 09:23:10 Suggestion: Determine if protocol 'sctp' is really needed on this system [test:NETW-3200] [details:-] [solution:-]
2022-11-29 09:23:10 Test: now checking module 'rds'
2022-11-29 09:23:10 Suggestion: Determine if protocol 'rds' is really needed on this system [test:NETW-3200] [details:-] [solution:-]
2022-11-29 09:23:10 Test: now checking module 'tipc'
2022-11-29 09:23:10 Suggestion: Determine if protocol 'tipc' is really needed on this system [test:NETW-3200] [details:-] [solution:-]

...
~~~

Parece que no los encuentra y estos potencialmente tienen algunas vulnerabilidades conocidas, vamos a comprobar si están los módulos cargados en el `Kernel`:


<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/64.png"/>


### Impresoras

El controlador `CUPS` lo eliminamos durante las correcciones y por eso no aparece. Como algo novedoso, hace referencia al comando `lp` que es para la "impresora en línea", con él los usuarios pueden enviar documentos para que se impriman.

~~~
2022-11-29 09:23:10 ====
2022-11-29 09:23:10 Performing test ID PRNT-2314 (Check lpd status)
2022-11-29 09:23:10 Test: Checking lpd status
2022-11-29 09:23:10 Performing pgrep scan without uid
2022-11-29 09:23:10 IsRunning: process 'lpd' not found
2022-11-29 09:23:10 Result: lp daemon not running
2022-11-29 09:23:10 Hardening: assigned maximum number of hardening points for this item (4). Currently h
aving 131 points (out of 175)
2022-11-29 09:23:10 ====

...
~~~


### Correo electrónico

Este apartado a diferencia de la primera vez, encontramos un servidor de correo electrónico, `Postfix` en concreto. Este aparece porque se instaló con `AIDE` y es para que juntos puedan comunicarse y enviar correos a dónde se configure mostrando información sobre la integridad de tus archivos. A destacar el mensaje de "peligro" que aparece:

~~~
2022-11-29 09:23:10 ====
2022-11-29 09:23:10 Performing test ID MAIL-8818 (Check Postfix configuration: banner)
2022-11-29 09:23:10 Test: Checking Postfix banner
2022-11-29 09:23:10 Result: found OS, or mail_name in SMTP banner, and/or mail_name contains 'Postfix'.
2022-11-29 09:23:10 Warning: Found some information disclosure in SMTP banner (OS or software name) [test:MAIL-8818] [details:-] [solution:-]
2022-11-29 09:23:10 Suggestion: You are advised to hide the mail_name (option: smtpd_banner) from your postfix configuration. Use postconf -e or change your main.cf file (/etc/postfix/main.cf) [test:MAIL-8818] [details:-] [solution:-]

...
~~~

Si miramos dentro del archivo `/etc/postfix/main.cf` y buscamos por el apartado que nos advierte, veremos lo siguiente:

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/64.png"/>

Esto se puede solucionar bien desinstalando `postfix`, si no queremos que nos lleguen correos para la herramienta `AIDE` ó bien configurando totalmente o parcialmente este servidor, incluyendo la sugerencia de no mostrar información sobre nuestro sistema operativo.

---

Información adicional
{: .label .label-blue}

- **ARP (Address Resolution Protocol) -> responsable de encontrar la dirección de hardware (Ethernet MAC) que corresponde a una determinada dirección IP. Para ello se envía un paquete (ARP request) a la dirección de difusión de la red (broadcast, MAC = FF FF FF FF FF FF) que contiene la dirección IP por la que se pregunta, y se espera a que esa máquina (u otra) responda (ARP reply) con la dirección Ethernet que le corresponde.**


- **DCCP (Protocolo de Control de Congestión de Datagramas)-> [Información](https://es.wikipedia.org/wiki/Protocolo_de_Control_de_Congesti%C3%B3n_de_Datagramas) [Vulnerabilidad](https://www.incibe-cert.es/alerta-temprana/vulnerabilidades/cve-2020-16119)**

- **SCTP (Protocolo de Control de Transmisiones de Corrientes) -> [Información](https://es.wikipedia.org/wiki/Stream_Control_Transmission_Protocol) [Vulnerabilidad](https://www.incibe-cert.es/alerta-temprana/vulnerabilidades/cve-2008-2826)**

- **RDS -> [Información](https://docs.kernel.org/networking/rds.html) [Vulnerabilidad antigua](https://www.cvedetails.com/cve/CVE-2010-3865/)**

- **TIPC (Protocolo de Comunicación Transparente Entre Procesos) -> [Información](https://docs.kernel.org/networking/tipc.html) [Vulnerabilidad](https://www.incibe-cert.es/content/boletin-vulnerabilidades-7618)**