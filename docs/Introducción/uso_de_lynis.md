---
layout: default
title: Uso de Lynis
nav_order: 1
parent: Introducción
has_toc: false
---

# Uso de Lynis

**Lynis** es modular y oportunista, esto quiere decir que sólo utilizará y probará los componentes que pueda encontrar en el sistema, como servicios habilitados, bibliotecas de controladores, etc. Tiene la ventaja de que no necesita instalar otras herramienta, manteniendo el sistema más limpio.

Al realizar con esta herramienta el escaneo, puede funcionar casi sin dependencias adicionales; añadido a que cuanto más componentes descubra, más extensa será la auditoría.

A continuación se da una breve explicación en terminología y en procesos que realiza esta herramienta y a qué va enfocada.


## Auditoria de seguridad

Proceso donde se evalúa el estado de seguridad de un sistema informático, detectando fallos o debilidades que terceros podrían aprovechar para infectar el sistema, impedir su funcionamiento, robar datos o causar algún tipo de daño. Esta herramienta lleva a cabo una auditoría que incluye al menos 9 pasos, que empiezan con su inicio y pasan por las comprobaciones básicas para luego determinar el sistema operativo y continuar con la búsqueda de componentes de software disponibles para utilizar en la auditoría.

Los siguientes pasos se enfocan en comprobar la última versión de Lynis, ejecutar los complementos habilitados y las pruebas de seguridad de cada categoría (se pueden incluir también pruebas personalizadas por el usuario), derivando, finalmente, en un informe que indica los resultados del análisis de seguridad.

Este informe, así como los detalles técnicos del escaneo, se almacenan en un archivo de registro (`lynis.log`), mientras que la información relacionada con las advertencias y sugerencias de cambios se guarda en un archivo de informe separado (`lynis-report.dat`).

## Pruebas de cumplimiento

Su función es recopilar información que evidencia el cumplimiento de un sistema en lo que respecta a los procedimientos de control. Los controles de seguridad de `Lynis` llevan a cabo un gran número de pruebas individuales que contribuyen a determinar el estado de seguridad. Incluye categorías como correo electrónico, almacenamiento, malware, inicio de sesión, base de datos, servidores web, sistemas de archivos o cortafuegos, entre otros. 

## Pruebas de penetración y endurecimiento

Es necesario conocer un poco estas palabras para ver el enfoque que tiene esta herramienta y la utilidad que tiene:

- El `pentesting`, también conocido como pruebas de penetración, se encarga del diseño e impulso de ataques cibernéticos contra el sistema de una organización, con el objetivo de detectar cuáles son sus vulnerabilidades a nivel de seguridad.

- El `hardening` o endurecimiento del sistema consiste en asegurar un determinado sistema y reducir los agujeros de seguridad, con una serie de acciones por parte del administrador que mejoren la seguridad . Aquí entra esta herramienta, como una respuesta a las pruebas de penetración. Sus análisis van enfocados a mejorar los puntos de vulenrabilidad del sistema auditado; por supuesto, la herramienta no es perfecta y esta sujeta a las actualizaciones de la comunidad (en su versión gratuita) y la propia empresa (versión de pago), por lo que no significa que esto evite que se exploten otras vulnerabilidades no conocidas por el programa.