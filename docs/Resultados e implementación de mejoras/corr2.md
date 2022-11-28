---
layout: default
title: Correción 2 - Sugerencias - Listar todos los servicios
parent: Resultados e implementación de mejoras
nav_order: 2
---

## Corrección 2 - Sugerencias - Listar todos los servicios

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/31.png"/>

Aunque al principio se nos muestra que actualicemos a la última versión estable, pensando que se refiere a la distribución, no lo vamos a hacer. Supongamos, que por algún motivo tenemos que usar esta versión de `Ubuntu` y vamos a trabajar sobre esta versión. Si quisiéramos actualizar habría que hacer lo siguiente:

~~~
sudo apt update && sudo apt upgrade && sudo apt dist-upgrade

sudo do-release-upgrade
~~~

Así que vamos a listar los servicios que están en nuestro sistema con:

~~~
## Servicios activos

sudo systemctl --full --type=service

## Servicios habilitados

sudo systemctl list-unit-files --type=service
~~~

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/32.png"/>


Vemos con el primer comando que hay dos unidades cargadas:

-  `dbus.service`, `D-Bus` (*Desktop Bus*) es un sistema de comunicación entre procesos (IPC) y una llamada a procedimiento remoto (RPC) , para aplicaciones de software con el fin de comunicarse entre sí. Se usa para la comunicación entre aplicaciones de escritorio en la misma sesión, facilitando la integración de aplicaciones dentro de un mismo entorno de escritorio y el tratamiento de asuntos relativos al ciclo de vida de procesos. Está `exited`, esto quiere decir que se ha ejecutado en algún momento, pero ahora  `systemd` no es capaz de detectar si está corriendo. Esto suele pasar cuando por ejemplo se carga el fichero de configuración del servicio y a posteriori el servicio pasa a ser controlado por el `Kernel` sin la necesidad que ningún *demonio* lo esté corriendo. Básicamente es para uso de una interfaz gráfica.

- `systemd-journald.service`, cuando se inicia el sistema y durante su operación se registra la actividad en varios archivos que reciben el nombre de `logs` del sistema. Bajo `systemd`, el encargado de recolectar y almacenar los mensajes del `Kernel` y otras fuentes es `Journald`. Este está `running`, quiere decir que está activo y ejecuntándose en estos momentos.

En principio son servicios que podemos dejar funcionando, así que pasamos a la información del otro comando:

- `dbus-org.freedesktop.Avahi.service`. `Avahi` es un sistema que permite a los programas publicar y descubrir servicios y equipos que se ejecutan en una red local. Por ejemplo, un usuario puede conectar un ordenador a una red y hacer que `Avahi` anuncie automáticamente los servicios de red que se ejecutan en su máquina, facilitando el acceso del usuario a esos servicios. Este servicio está `enabled`, quiere decir que está disponible para su uso o se está usando. En principio parece un servicio que no necesitamos y podríamos deshabilitar su demonio `avahi-daemon`.

~~~
sudo service avahi-daemon stop

## Dejamos el paquete instalado (por si se necesitara en un futuro), pero no
## queremos que se vuelva a iniciar cuando se reinicie el sistema.

sudo mv /etc/init/avahi-daemon.conf /etc/init/avahi-daemon.conf.disabled
~~~


- `sshd.service`, servicio para la conexión `SSH`(`OpenSSH`).

- `syslog.service`,  servicio de envío de mensajes de registro. Sirve para recolectar, registrar y analizar, entre otras cosas, accesos con contraseñas equivocadas, accesos correctos, alertas, etc.

- `systemd-fsck@.service`, responsable de la comprobación del sistema de archivos y para el sistema de archivos raíz en `initrd`. Este no conoce ningún detalle sobre sistemas de archivos y simplemente ejecuta verificadores de sistemas de archivos específicos para cada tipo de sistema de archivos (`/sbin/fsck.type`). Estos comprobadores decidirán si el sistema de archivos debe ser comprobado en base al tiempo transcurrido desde la última comprobación, número de montajes, desmontaje, etc. Vemos que está en `static`, quiere decir que únicamente se usarán en el caso que otro servicio o unidad lo precise. Estos servicios pueden estar activos o inactivos, pero siempre están disponibles para cuando se necesite usarlos.

- `systemd-halt.service`, `systemd-hibernate.service`, `systemd-poweroff.service `, `systemd-reboot.service `, `systemd-kexec.service` y `systemd-reboot.service`, como su propio nombre indica son los responsables de detener, reiniciar, apagar, montar y desmontar las unidades, entre otros, de nuestro sistema.

Como no se observa más cosas a las que debamos prestar atención, pasamos al siguiente bloque.