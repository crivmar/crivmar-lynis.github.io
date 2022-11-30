---
layout: default
title: Bloque 4 - Shells, sistema de archivos y USB
parent: Segunda auditoría
nav_order: 4
---

## Bloque 4 -  Shells, sistema de archivos y USB

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/59.png"/>

Aquí se nos muestra más información que en el programa anterior. En el apartado de las `shells` podemos ver que aparece `session timeout` y este aparece como "ninguno". Vamos a ver a qué se refiere:

~~~
2022-11-29 09:22:54 ====
2022-11-29 09:22:54 Performing test ID SHLL-6220 (Idle session killing tools or settings)
2022-11-29 09:22:54 Test: Search for session timeout tools or settings in shell
2022-11-29 09:22:54 Performing pgrep scan without uid
2022-11-29 09:22:54 IsRunning: process 'timeoutd' not found
2022-11-29 09:22:54 Performing pgrep scan without uid
2022-11-29 09:22:54 IsRunning: process 'autolog' not found
2022-11-29 09:22:54 Result: could not find TMOUT setting in /etc/profile
2022-11-29 09:22:54 Result: could not find export, readonly or typeset -r in /etc/profile
2022-11-29 09:22:54 Result: could not find TMOUT setting in /etc/profile.d/*.sh
2022-11-29 09:22:54 Result: could not find export, readonly or typeset -r in /etc/profile
2022-11-29 09:22:54 Hardening: assigned partial number of hardening points (1 of 3). Currently having 36 points (out of 50)
2022-11-29 09:22:54 ====

...
~~~ 

Vemos que no encuentra `TMOUT`, esto es una variable de `bash` para que, transcurrido un tiempo de inactividad, cierre la sesión al usuario que la haya abierto. Para ello habría que añadir la siguiente línea al archivo `/etc/profile`:

~~~
# Política para tiempo de espera antes de cerrar la sesión (5 min)

TMOUT=300
readonly TMOUT
export TMOUT
~~~

Respecto al valor de `umask` del archivo `/etc/profile` viene definido por un módulo de `PAM` como vimos en el apartado anterior y en `/etc/bash.bashrc` podemos modificarlo manualmente añadiendo la siguiente línea a este archivo:

~~~
umask 027
~~~


### Sistema de archivos

Como en el apartado **Corrección 4** de la sección **Resultados e implementación de mejoras** cambiamos los puntos de montaje de `home` y `tmp`, ahora nos lo da por correctos. Ahora, añade como sugerencia a `/var` que se haría con el mismo proceso.

Vemos que ahora mira también la `SWAP` (Zona de intercambio) dentro del archivo `fstab`, cosa que antes no hacía. Nos muestra una sugerencia sobre `/proc`:

~~~
2022-11-29 09:22:54 ====
2022-11-29 09:22:54 Performing test ID FILE-6344 (Checking proc mount options)
2022-11-29 09:22:54 Test: check proc mount with incorrect mount options
2022-11-29 09:22:54 Hardening: assigned partial number of hardening points (0 of 3). Currently having 65 points (out of 83)
2022-11-29 09:22:54 Result: /proc filesystem is not mounted with option hidepid=1 or hidepid=2
2022-11-29 09:22:54 ====

...
~~~

Como vemos también sería interesante cambiar el punto de montaje de `/proc` y modificar el `/etc/fstab` con la siguiente línea:

~~~
proc    /proc        proc        defaults,hidepid=2    0 0
~~~


Por defecto, la opción `hidepid` tiene el valor 0. Esto significa que todos los usuarios pueden ver todos los datos. Si se establece en 1, las entradas de los directorios en `/proc` seguirán siendo visibles, pero no accesibles. Con el valor 2 se ocultan por completo. 

Abajo nos salta un peligro relacionado con el `sticky bit` de `/tmp`:

~~~
2022-11-29 09:22:54 ====
2022-11-29 09:22:54 Performing test ID FILE-6362 (Checking /tmp sticky bit)
2022-11-29 09:22:54 Suggestion: Set the sticky bit on /tmp, to prevent users deleting (by other owned) files in the /tmp directory. [test:FILE-6362] [details:/tmp] [solution:text:Set sticky bit]
2022-11-29 09:22:54 Hardening: assigned partial number of hardening points (0 of 3). Currently having 65 points (out of 86)
2022-11-29 09:22:54 ====

...
~~~

Como nos dice, al activarlo, previene que usuarios puedan borrar archivos dentro de este directorio de los cuales no sean dueños, lo podemos resolver con el siguiente comando:

~~~
sudo chmod +t /tmp
~~~

Lo siguiente que nos muestra son los valores que hay añadidos a los puntos de montaje dentro de `/etc/fstab` y compara con los valores que el programa cree que deben tener, estos podemos verlo en los registros y sería cuestión de adaptarlo:

~~~
2022-11-29 09:22:54 File system:    /dev
2022-11-29 09:22:54 Expected flags: noexec nosuid
2022-11-29 09:22:54 Found flags:    (rw mode=0755) 
2022-11-29 09:22:54 Result: Could not find mount option noexec on file system /dev
2022-11-29 09:22:54 Result: Could not find mount option nosuid on file system /dev
2022-11-29 09:22:54 Result: marked /dev options as non-default (unclear about hardening)
2022-11-29 09:22:54 Hardening: assigned partial number of hardening points (4 of 5). Currently having 78 points (out of 102)
2022-11-29 09:22:54 Result: file system /dev/shm not found in /etc/fstab
2022-11-29 09:22:54 File system:    /home
2022-11-29 09:22:54 Expected flags: nodev nosuid
2022-11-29 09:22:54 Found flags:    errors=remount-ro 
2022-11-29 09:22:54 Result: Could not find mount option nodev on file system /home
2022-11-29 09:22:54 Result: Could not find mount option nosuid on file system /home
2022-11-29 09:22:54 Result: marked /home options as non-default (unclear about hardening)
2022-11-29 09:22:54 Hardening: assigned partial number of hardening points (4 of 5). Currently having 82 points (out of 107)

...
~~~


### USB

En el apartado **Correción 4** de la sección anterior, deshabilitamos el uso de USB y ahora nos muestra como tal. Lo que añade a esta categoría es si tenemos instalado `USBGuard`, un programa que ayuda a proteger el sistema contra USB engañosos mediante la implementación de listas negras y blancas basadas en los atributos de estos. Como no permitimos el uso de estos, directamente no nos haría falta instalarlo, pero [aquí](https://usbguard.github.io/) dejo su web por si se cambia de política respecto a los dispositivos USB.


~~~
2022-11-29 09:22:55 ====
2022-11-29 09:22:55 Action: Performing tests from category: USB Devices
2022-11-29 09:22:55 ====
2022-11-29 09:22:55 Performing test ID USB-1000 (Check if USB storage is disabled)
2022-11-29 09:22:55 Test: Checking USB storage driver in directory /etc/modprobe.d and configuration file /etc/modprobe.conf
2022-11-29 09:22:55 Result: found usb-storage driver in disabled state (blacklisted)
2022-11-29 09:22:55 Result: usb-storage driver is disabled
2022-11-29 09:22:55 Hardening: assigned maximum number of hardening points for this item (3). Currently having 107 points (out of 141)
2022-11-29 09:22:55 ====
2022-11-29 09:22:55 Performing test ID USB-2000 (Check USB authorizations)
2022-11-29 09:22:55 Test: checking presence of USB devices path (/sys/bus/usb/devices)
2022-11-29 09:22:55 Test: Checking USB devices authorization to connect to the system
2022-11-29 09:22:55 Result: None USB devices are authorized by default (or temporary) to connect to the system
2022-11-29 09:22:55 Hardening: assigned maximum number of hardening points for this item (3). Currently having 110 points (out of 144)
2022-11-29 09:22:55 ====
2022-11-29 09:22:55 Performing test ID USB-3000 (Check for presence of USBGuard)
2022-11-29 09:22:55 Result: USBGuard not found
2022-11-29 09:22:55 Hardening: assigned partial number of hardening points (0 of 8). Currently having 110 points (out of 152)
2022-11-29 09:22:55 Security check: file is normal
2022-11-29 09:22:55 Checking permissions of /usr/local/lynis/include/tests_storage
2022-11-29 09:22:55 File permissions are OK
2022-11-29 09:22:55 ====

...
~~~
