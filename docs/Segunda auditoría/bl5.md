---
layout: default
title: Bloque 5 - NFS y DNS
parent: Segunda auditoría
nav_order: 5
---

## Bloque 5 - NFS y DNS

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/60.png"/>

La parte de almacenamiento respecto a `FireWire` permanece igual que en la primera auditoría, así que pasamos a `ǸFS` directamente. En el apartado **Bloque 8** de la sección **Primera auditoría**, se limitaba a comprobar si había un **demonio** corriendo este servicio. Ahora observamos más detalles, como por ejemplo busca los programas registrados con el comando `rpcinfo`, es un programa que recupera una lista de todos los servicios de llamada de procedimiento remoto (RPC) actualmente en ejecución, sus nombres y descripciones, y los puertos que están utilizando.

~~~
2022-11-29 09:22:55 Action: Performing tests from category: NFS
2022-11-29 09:22:55 ====
2022-11-29 09:22:55 Performing test ID STRG-1902 (Check rpcinfo registered programs)
2022-11-29 09:22:55 Test: Checking rpcinfo registered programs
2022-11-29 09:22:55 rpcinfo: ,program,vers,proto,port,service
2022-11-29 09:22:55 rpcinfo: ,100000,4,tcp,111,portmapper
2022-11-29 09:22:55 rpcinfo: ,100000,3,tcp,111,portmapper
2022-11-29 09:22:55 rpcinfo: ,100000,2,tcp,111,portmapper
2022-11-29 09:22:55 rpcinfo: ,100000,4,udp,111,portmapper
2022-11-29 09:22:55 rpcinfo: ,100000,3,udp,111,portmapper
2022-11-29 09:22:55 rpcinfo: ,100000,2,udp,111,portmapper
2022-11-29 09:22:55 rpcinfo: ,100024,1,udp,45998,status
2022-11-29 09:22:55 rpcinfo: ,100024,1,tcp,56430,status

...
~~~

En los números de los programas podemos observar el 100000 que pertenece al servicio `portmappper`, este sirve para redireccionar las peticiones a un puerto a una dirección que configuremos. Este servicio siempre usa el `TCP` o `UDP` con el número `111`, lo que vemos son las diferentes versiones de este protocolo.  Hay que tener cuidado con esto porque hay ataques de negación de servicio que utilizan el puerto `111 UDP`. Si nosotros, hacemos una comprobación desde nuetra máquina, veremos si ignora al `DROP` de `Iptables`:

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/61.png"/>

No he encontrado información fiable al respecto sobre esto, pero sospecho que `Vagrant` y/o `VirtualBox` hacen uso de este protocolo para la conexión con la máquina virtual y puentearán la política por defecto, incluso parando el servicio de `rpcbind` (lo recomendable si no hacemos uso de este servicio) me lo sigue detectando.


Lo siguiente que comprueba es si detecta los protocolos de `NFS` y si está corriendo este servicio:

~~~
2022-11-29 09:22:55 ====
2022-11-29 09:22:55 Performing test ID STRG-1904 (Check nfs rpc)
2022-11-29 09:22:55 Test: Checking NFS registered versions
2022-11-29 09:22:55 ====
2022-11-29 09:22:55 Performing test ID STRG-1906 (Check nfs rpc)
2022-11-29 09:22:55 Test: Checking NFS registered protocols
2022-11-29 09:22:55 Output: no NFS protocols found
2022-11-29 09:22:55 Test: Checking NFS registered ports
2022-11-29 09:22:55 Output: no NFS port number found
2022-11-29 09:22:55 ====
2022-11-29 09:22:55 Performing test ID STRG-1920 (Checking NFS daemon)
2022-11-29 09:22:55 Test: Checking running NFS daemon
2022-11-29 09:22:55 Output: NFS daemon is not running
2022-11-29 09:22:55 ====

...
~~~


### DNS

En este caso muestra menos información que el anterior, pero si vemos el registro vemos que hace las mismas comprobaciones:

~~~
2022-11-29 09:22:55 ====
2022-11-29 09:22:55 Action: Performing tests from category: Name services
2022-11-29 09:22:55 ====
2022-11-29 09:22:55 Performing test ID NAME-4016 (Check /etc/resolv.conf default domain)
2022-11-29 09:22:55 Test: check /etc/resolv.conf for default domain
2022-11-29 09:22:55 Result: /etc/resolv.conf found
2022-11-29 09:22:55 Result: no default domain found
2022-11-29 09:22:55 ====
2022-11-29 09:22:55 Performing test ID NAME-4018 (Check /etc/resolv.conf search domains)
2022-11-29 09:22:55 Test: check /etc/resolv.conf for search domains
2022-11-29 09:22:55 Result: /etc/resolv.conf found
2022-11-29 09:22:55 Found search domain: home
2022-11-29 09:22:55 Result: Found 1 search domains
2022-11-29 09:22:55 Result: found 1 line(s) with a search statement (expecting less than 2 lines)
2022-11-29 09:22:55 ====

...

2022-11-29 09:22:55 ====
2022-11-29 09:22:55 Performing test ID NAME-4032 (Check nscd status)
2022-11-29 09:22:55 Test: checking nscd status
2022-11-29 09:22:55 Performing pgrep scan without uid
2022-11-29 09:22:55 IsRunning: process 'nscd' not found
2022-11-29 09:22:55 Result: nscd is not running
2022-11-29 09:22:55 ====
2022-11-29 09:22:55 Performing test ID NAME-4034 (Check Unbound status)
2022-11-29 09:22:55 Test: checking Unbound (unbound) status
2022-11-29 09:22:55 Performing pgrep scan without uid
2022-11-29 09:22:55 IsRunning: process 'unbound' not found
2022-11-29 09:22:55 Result: Unbound daemon is not running
2022-11-29 09:22:55 ====


...

2022-11-29 09:22:55 ====
2022-11-29 09:22:55 Performing test ID NAME-4202 (Check BIND status)
2022-11-29 09:22:55 Test: Checking for running BIND instance
2022-11-29 09:22:55 Performing pgrep scan without uid
2022-11-29 09:22:55 IsRunning: process 'named' not found
2022-11-29 09:22:55 Result: BIND not running
2022-11-29 09:22:55 ====
2022-11-29 09:22:55 Skipped test NAME-4204 (Search BIND configuration file)
2022-11-29 09:22:55 Reason to skip: Prerequisites not met (ie missing tool, other type of Linux distribution)
2022-11-29 09:22:55 ====

...

2022-11-29 09:22:55 ====
2022-11-29 09:22:55 Performing test ID NAME-4402 (Check duplicate line in /etc/hosts)
2022-11-29 09:22:55 Test: check duplicate line in /etc/hosts
2022-11-29 09:22:55 Result: OK, no duplicate lines found
2022-11-29 09:22:55 ====
2022-11-29 09:22:55 Performing test ID NAME-4404 (Check /etc/hosts contains an entry for this server name)
2022-11-29 09:22:55 Test: Check /etc/hosts contains an entry for this server name
2022-11-29 09:22:55 Result: Found entry for mt3 in /etc/hosts
2022-11-29 09:22:55 ====
2022-11-29 09:22:55 Performing test ID NAME-4406 (Check server hostname mapping)
2022-11-29 09:22:55 Test: Check server hostname not locally mapped in /etc/hosts
2022-11-29 09:22:55 Result: this server hostname is not mapped to a local address
2022-11-29 09:22:55 ====
2022-11-29 09:22:55 Performing test ID NAME-4408 (Check localhost entry)
2022-11-29 09:22:55 Test: Check server hostname not locally mapped in /etc/hosts
2022-11-29 09:22:55 Result: localhost mapped to ::1
2022-11-29 09:22:55 Security check: file is normal
2022-11-29 09:22:55 Checking permissions of /usr/local/lynis/include/tests_dns
2022-11-29 09:22:55 File permissions are OK
2022-11-29 09:22:55 Security check: file is normal
2022-11-29 09:22:55 Checking permissions of /usr/local/lynis/include/tests_ports_packages
2022-11-29 09:22:55 File permissions are OK
2022-11-29 09:22:55 ====

...
~~~


Vemos que hace las mismas comprobaciones sólo que no te muestra la información porque estos servicios no están configurados.

