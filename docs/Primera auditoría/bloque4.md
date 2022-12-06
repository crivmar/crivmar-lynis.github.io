---
layout: default
title: Bloque 4 - Kernel 
parent: Primera auditoría
nav_order: 4
---

## Bloque 4 - Kernel

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/07.png"/>

Aquí nos muestra información específica del `Kernel` que tiene el equipo que está analizando, la información más destacada que podemos ver es:

- Lo que vimos en el anterior bloque respecto a los `runlevel`, que por defecto es el indicado.

- Si la CPU de la máquina soporta `NX`(*No Execute*) que indica si puede hacer partes de la memoria seguras, para evitar que programas ejecuten códigos en ese espacio. Y el `PAE` (*Physical Address Extension*), que es un administrador de la memoria para la arquitectura `x86` que permite usar hasta 64GB de memoria física.

- Los módulos que están activos en el núcleo, nos indican que son 53.

- Indica que hay una actualización del `Kernel` actual.