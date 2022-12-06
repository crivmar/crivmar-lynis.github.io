---
layout: default
title: Bloque 5 - Memoria y procesos 
parent: Primera auditoría
nav_order: 5
---

## Bloque 5 - Memoria y procesos

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/08.png"/>

Este bloque es pequeño y se centra en comprobar un archivo ubicado en `/proc` (contiene información de procesos y aplicaciones que se están ejecutando) e información de la memoria y la CPU. Busca si hay:

- **Procesos zombis**, es todo proceso (hijo) que ha completado su ejecución pero aún tiene una entrada en la tabla de procesos, permitiendo al proceso que lo ha creado (padre) leer el estado de su salida.

- **Procesos de Entrada/Salida en espera**, procesos que están pendientes de hacer una lectura o escritura en el disco o en memoria. La medida de tiempo de una CPU es el `IOWait` y si hay mucho, indican un cuello de botella en el rendimiento de la máquina.