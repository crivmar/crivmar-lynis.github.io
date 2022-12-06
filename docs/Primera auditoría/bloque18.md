---
layout: default
title: Bloque 18 - Integridad de archivos y escáneres de programas maliciosos
parent: Primera auditoría
nav_order: 18
---

## Bloque 18 - Integridad de archivos y escáneres de programas maliciosos

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/21.png"/>

Comprueba si hay alguna herramienta que sirva para verificar la integridad de los archivos, tales como:

- **AFICK**, (*Another File Integrity Checker*), permite monitorear los cambios en los sistemas de archivos y por tanto, detectar intrusos, está diseñado para ser un clon portátil de `AIDE`.

- **AIDE**, (*Advance Intrusion Detection Enviroment*), es una utilidad que crea una base de datos (como una *instantánea*) de archivos en el sistema, y luego utiliza esa base de datos para asegurar la integridad de los archivos y detectar intrusiones en el sistema. 

- **Samhain**, aplicación multiplataforma que ha sido diseñada para monitorizar múltiples máquinas con sistemas operativos potencialmente diferentes, proporcionando un registro y mantenimiento centralizados, aunque también puede ser utilizado como aplicación independiente en una única máquina. 

Después pasa a los **escáneres de programas maliciosos**, tales como:

- **chkrootkit**, permite detectar `rootkits` conocidos y realiza varias pruebas en busca de binarios que hayan podido ser modificados por estos.

- **ClamAV**, es un conjunto de herramientas de antivirus que identifican y bloquean a programas potencialmente dañinos provenientes, principalmente, del correo electrónico. Uno de los puntos fundamentales en este tipo de programas son la rápida localización e inclusión en la herramienta de los nuevos virus encontrados y escaneados. Esto se consigue gracias a la colaboración de los miles de usuarios que lo usan y a un gran soporte de desarrolladores.


---

Información adicional
{: .label .label-blue}

**rootkits -> es un conjunto de programas que permiten al usuario un acceso con privilegios a un ordenador, pero mantiene su presencia inicialmente oculta al control de los administradores al descomponer el funcionamiento normal del sistema operativo.**