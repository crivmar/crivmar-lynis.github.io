---
layout: default
title: Bloque 16 - Gestión de procesos, tiempo y sincronización
parent: Primera auditoría
nav_order: 16
---

## Bloque 16 - Gestión de procesos, tiempo y sincronización

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/19.png"/>

La gestión de procesos es un método de seguimiento de los recursos del sistema. Incluye una manera de monitorear los recursos del sistema y cómo estos recursos son utilizados por los usuarios en el sistema. En los sistemas Linux, la contabilidad de procesos puede ser activada para rastrear estos recursos, no activada en nuestro caso. 
Lo siguiente que es ver si tenemos un *demonio* llamado `auditd` en nuestra máquina; este es permite auditar eventos realizados en el sistema, tales como:

- **Auditar el acceso y modificación de ficheros**:

    + Ver quién ha modificado un fichero concreto.
    
    + Detectar cambios no autorizados.
    
- **Monitorizar funciones y llamadas al sistema**.

- **Detectar anomalías como fallos de procesos**.

- **Establecer trampas para la detección de intrusos**.

- **Grabar los comandos ejecutados por un usuario concreto**.


Al no estar instalado pasa a la siguiente parte, *tiempo y sincronización*. Aquí se hace referencia a `NTP` (*Network Time Protocol*), se utiliza para sincronizar la hora de diferentes equipos informáticos a través de una red. Todos los dispositivos pueden sincronizar su hora con un servidor central llamado *Servidor NTP*. Este servidor puede ser un único equipo, varios equipos ó un conjunto de servidores `NTP`. Un servidor `NTP`, a su vez, puede sincronizar su hora con otro servidor `NTP` creando un sistema en cascada entre ellos. Este protocolo es capaz de sincronizar la hora de los equipos con una precisión de milisegundos.
Se nos indica que no se ha encontrado ningun servidor propio instalado y que no hay tarea específica en los archivos de `crontab`. Luego comprueba si hay algún evento basado en `ntpdate`; en este caso es afirmativo, podemos comprobarlo en `/etc/network/if-up.d/ntpdate`. Esto indica que cada vez que se inicien las interfaces de red hará una comprobación de la hora del sistema, como cliente, con el servidor por defecto.


