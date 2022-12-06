---
layout: default
title: Bloque 8 - Cortafuegos y servidor web
parent: Segunda auditoría
nav_order: 8
---

## Bloque 8 - Cortafuegos y servidor web

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/66.png"/>

En nuestro cortafuegos podemos ver que las reglas están por defecto a `DROP` como indicamos, pero veamos lo que nos dice sobre las reglas sin uso:

~~~
2022-11-29 09:23:10 ====
2022-11-29 09:23:10 Performing test ID FIRE-4512 (Check iptables for empty ruleset)
2022-11-29 09:23:10 Result: one or more rules are available (8 rules)
2022-11-29 09:23:10 ====
2022-11-29 09:23:10 Performing test ID FIRE-4513 (Check iptables for unused rules)
2022-11-29 09:23:10 Result: Found one or more possible unused rules
2022-11-29 09:23:10 Description: Unused rules can be a sign that the firewall rules aren't optimized or up-to-date
2022-11-29 09:23:10 Note: Sometimes rules aren't triggered but still in use. Keep this in mind before cleaning up rules.
2022-11-29 09:23:10 Output: iptables rule numbers: 4 4
2022-11-29 09:23:10 Suggestion: Check iptables rules to see which rules are currently not used [test:FIRE-4513] [details:-] [solution:-]
2022-11-29 09:23:10 Tip: iptables --list --numeric --line-numbers --verbose
2022-11-29 09:23:10 ====

...
~~~

Como bien nos indica hay veces que las reglas no se activan pero siguen en uso, vamos a listarlas y veamos a las reglas que se refieren:

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/67.png"/>

Se refiere a las reglas para permitir el tráfico `HTTP` de la máquina hacia internet y que efectivamente no hemos usado, por lo cuál nos salta este aviso. Respecto a la configuración de `Iptables`, es ajustar las políticas y las reglas a nuestras necesidades, esto es un ejemplo de una configuración simple para un cortafuegos personal.

### Servidor web

En esta auditoría, nos muestra más o menos lo mismo que en la primera, con algunas excepciones. Vemos que hay cargados 107 módulos cargables los cuales habría que revisar si son necesarios para nuestro entorno. Para ello podemos listarlos de la siguiente manera:

~~~
sudo apachectl -M
~~~

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/68.png"/>

Según nuestras necesidades, podemos deshabilitarlos con el siguiente comando:

~~~
sudo a2dismod [nombre_módulo]
~~~

Podemos ver que ha encontrado módulos que nos sugería en su momento para mejorar la seguridad de nuestro servidor web:

~~~
2022-11-29 09:23:11 ====
2022-11-29 09:23:11 Performing test ID HTTP-6640 (Determining existence of specific Apache modules)
2022-11-29 09:23:11 Test: search string /mod_evasive([0-9][0-9])?.so in earlier discovered results
2022-11-29 09:23:11 Result: found search string (result: apache_module[]=/usr/lib/apache2/modules/mod_evasive20.so)
2022-11-29 09:23:11 Hardening: assigned maximum number of hardening points for this item (3). Currently having 139 points (out of 183)
2022-11-29 09:23:11 ====
2022-11-29 09:23:11 Performing test ID HTTP-6641 (Determining existence of specific Apache modules)
2022-11-29 09:23:11 Test: search string /mod_(reqtimeout|qos).so in earlier discovered results
2022-11-29 09:23:11 Result: found search string (result: apache_module[]=/usr/lib/apache2/modules/mod_qos.so
apache_module[]=/usr/lib/apache2/modules/mod_reqtimeout.so)
2022-11-29 09:23:11 Hardening: assigned maximum number of hardening points for this item (3). Currently having 142 points (out of 186)
2022-11-29 09:23:11 ====
2022-11-29 09:23:11 Performing test ID HTTP-6643 (Determining existence of specific Apache modules)
2022-11-29 09:23:11 Test: search string /mod_security(2|3).so in earlier discovered results
2022-11-29 09:23:11 Result: found search string (result: apache_module[]=/usr/lib/apache2/modules/mod_security2.so)
2022-11-29 09:23:11 Hardening: assigned maximum number of hardening points for this item (3). Currently having 145 points (out of 189)
2022-11-29 09:23:11 ====

...
~~~