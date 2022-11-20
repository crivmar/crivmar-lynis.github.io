---
layout: default
title: Auditoria de seguridad
parent: Uso de Lynis
grant_parent: Introducción
nav_order: 1
---

Proceso donde se evalúa el estado de seguridad de un sistema informático, detectando fallos o debilidades que terceros podrían aprovechar para infectar el sistema, impedir su funcionamiento, robar datos o causar algún tipo de daño. Esta herramienta lleva a cabo una auditoría que incluye al menos 9 pasos, que empiezan con su inicio y pasan por las comprobaciones básicas para luego determinar el sistema operativo y continuar con la búsqueda de componentes de software disponibles para utilizar en la auditoría.

Los siguientes pasos se enfocan en comprobar la última versión de Lynis, ejecutar los complementos habilitados y las pruebas de seguridad de cada categoría (se pueden incluir también pruebas personalizadas por el usuario), derivando, finalmente, en un informe que indica los resultados del análisis de seguridad.

Este informe, así como los detalles técnicos del escaneo, se almacenan en un archivo de registro (`lynis.log`), mientras que la información relacionada con las advertencias y sugerencias de cambios se guarda en un archivo de informe separado (`lynis-report.dat`).