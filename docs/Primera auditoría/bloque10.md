---
layout: default
title: Bloque 10 - Correo electrónico y cortafuegos
parent: Primera auditoría
nav_order: 10
---

## Bloque 10 - Correo electrónico y cortafuegos

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/13.png"/>

Se comprueba si hay servidores de correo funcionando en el sistema, tales como `Postfix`, `Qmail` y `Exim`, hay muchos más tipos de servidores de correo pero se centra en los más típicos, esto quiere decir que si está fuera de la lista predeterminada este paso no te sería valido (habría que hacer un test personalizado). 

Continua con la comprobación de cortafuegos en este sistema, detectando el módulo de `iptables` y con ello comprueba si hay grupos de reglas vacíos en `/etc/iptables/`. Lo siguiente que nos muestra es otro indicativo de peligro en estas reglas, que veremos más adelante. Sigue con la comprobación de `pf`(*Packet Filter*), que es un filtrado basado en una configuración dinámica por reglas de estado para `OpenBSD` y sería el equivalente al que tenemos en uso en esta máquina. Finalizando con la comprobación de si está activo el cortafuego detectado.

