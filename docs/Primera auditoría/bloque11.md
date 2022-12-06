---
layout: default
title: Bloque 11 - Servidor web
parent: Primera auditoría
nav_order: 11
---

## Bloque 11 - Servidor web

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/14.png"/>

Al igual que otros bloques comprueba las herramientas más utilizadas dentro de los servidores web, que no quiere decir que sean las únicas. `Apache2` es de la más utilizadas en este momento y además es el servidor web que tiene instalado la máquina. Las comprobaciones que hace es en `/etc/apache2/apache2.conf`y nos muestra como muchas variables que aparecen en ese archivo de configuración no están definidas en `/etc/apache2/envvars`, pero veamos que nos indica este documento:

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/14_01.png"/>

Además, encuentra un fallo de sintaxis en la línea 74 del archivo de configuración, pero más adelante nos comunica que no ha podido encontrar este archivo y por tanto salta otras comprobaciones. Todos estos errores pueden deberse a varias causas, entre ellas que el test tenga un error y no se ejecute correctamente. Nuevamente, más adelante averiguaremos si con actualizar `Lynis` se soluciona. Finaliza la comprobación de `Apache2` averiguando si tiene ciertos módulos de seguridad, ninguno de ellos están en el sistema. Finaliza comprobando si hay otro de los servidores web más conocidos, `nginx`, que no está instalado en este caso.


