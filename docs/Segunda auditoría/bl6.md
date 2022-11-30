---
layout: default
title: Bloque 6 - Paquetes
parent: Segunda auditoría
nav_order: 6
---

## Bloque 6 - Paquetes

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/62.png"/>

Nos muestra algo distinto que el apartado **Bloque 8** de **Primera auditoría**. Primero determina, probando distintas opciones, cuál gestor de paquetes usamos:

~~~
...

2022-11-29 09:22:55 ====
2022-11-29 09:22:55 Skipped test PKGS-7320 (Checking for arch-audit tooling)
2022-11-29 09:22:55 Reason to skip: Test only applies to Arch Linux and Garuda Linux
2022-11-29 09:22:55 ====
2022-11-29 09:22:55 Skipped test PKGS-7322 (Discover vulnerable packages with arch-audit)
2022-11-29 09:22:55 Reason to skip: arch-audit not found
2022-11-29 09:22:55 ====
2022-11-29 09:22:55 Skipped test PKGS-7328 (Querying Zypper for installed packages)
2022-11-29 09:22:55 Reason to skip: Prerequisites not met (ie missing tool, other type of Linux distribut
ion)
2022-11-29 09:22:55 ====
2022-11-29 09:22:55 Skipped test PKGS-7330 (Querying Zypper for vulnerable packages)
2022-11-29 09:22:55 Reason to skip: Prerequisites not met (ie missing tool, other type of Linux distribut
ion)
2022-11-29 09:22:55 ====
2022-11-29 09:22:55 Skipped test PKGS-7332 (Query macOS ports)
2022-11-29 09:22:55 Reason to skip: Incorrect guest OS (macOS only)
2022-11-29 09:22:55 ====
2022-11-29 09:22:55 Skipped test PKGS-7334 (Query port for port upgrades)
2022-11-29 09:22:55 Reason to skip: Incorrect guest OS (macOS only)
2022-11-29 09:22:55 ====
2022-11-29 09:22:55 Performing test ID PKGS-7345 (Querying dpkg)
2022-11-29 09:22:55 Result: Found dpkg binary
2022-11-29 09:22:55 Test: Querying dpkg -l to get package list
2022-11-29 09:22:55 Output:
2022-11-29 09:22:55 Found package: accountsservice (version: 0.6.35-0ubuntu7.3)
2022-11-29 09:22:55 Found package: adduser (version: 3.113+nmu3ubuntu3)

...
~~~

Recibe una lista de los paquetes encontrados en el sistema y encuentra paquetes que no han sido purgados y te recomienda hacer uso del comando `dpkg --purge [nombre_del_paquete]` para eliminar a estos de la lista:

~~~
...

2022-11-29 09:22:56 Found unpurged package: libvisual-0.4-0:amd64
2022-11-29 09:22:56 Found unpurged package: libvpx1:amd64
2022-11-29 09:22:56 Found unpurged package: libwavpack1:amd64
2022-11-29 09:22:56 Found unpurged package: libwebkitgtk-1.0-0:amd64
2022-11-29 09:22:56 Found unpurged package: libwebp5:amd64
2022-11-29 09:22:56 Found unpurged package: libwmf0.2-7:amd64
2022-11-29 09:22:56 Found unpurged package: libx11-xcb1:amd64
2022-11-29 09:22:56 Found unpurged package: libxcb-dri2-0:amd64
2022-11-29 09:22:56 Found unpurged package: libxcb-dri3-0:amd64
2022-11-29 09:22:56 Found unpurged package: libxcb-glx0:amd64
2022-11-29 09:22:56 Found unpurged package: libxcb-present0:amd64
2022-11-29 09:22:56 Found unpurged package: libxcb-sync1:amd64
2022-11-29 09:22:56 Found unpurged package: libxshmfence1:amd64
2022-11-29 09:22:56 Found unpurged package: libxv1:amd64
2022-11-29 09:22:56 Found unpurged package: libxxf86vm1:amd64
2022-11-29 09:22:56 Found unpurged package: policykit-1
2022-11-29 09:22:56 Found unpurged package: poppler-data
2022-11-29 09:22:56 Suggestion: Purge old/removed packages (111 found) with aptitude purge or dpkg --purge command. This will cleanup old configuration files, cron jobs and startup scripts. [test:PKGS-7346] [details:-] [solution:-]
2022-11-29 09:22:56 ====


...
~~~

En este caso da un error en el repositorio de paquetes `http://extras.ubuntu.com trusty`, esto es porque no tiene agregada una clave pública o un error al intentar exportar esta clave. Como son repositorios que no estamos usando, podemos borrarlos `/etc/sources.list` o si fuera otro haciendo uso del siguiente comando:

~~~
sudo gpg --keyserver keyserver.ubuntu.com --recv [clave_después_NO_PUBKEY]
~~~

Vemos un peligro al ejecutar el test `PKGS-7392` porque ha tardado mucho en ejecutarse, veamos qué nos muestra los registros:

~~~
2022-11-29 09:22:56 ====
2022-11-29 09:22:56 Performing test ID PKGS-7392 (Check for Debian/Ubuntu security updates)
2022-11-29 09:22:56 Action: updating package repository with apt-get
2022-11-29 09:23:08 Result: apt-get finished
2022-11-29 09:23:08 Test: Checking if /usr/lib/update-notifier/apt-check exists
2022-11-29 09:23:08 Result: apt-check (update-notifier-common) not found
2022-11-29 09:23:09 Result: test not fully executed (missing apt-check output)
2022-11-29 09:23:09 ====

...
~~~

Por lo que vemos nos muestra que no encuentra el archivo de notificaciones de `apt-check`, esto puede ser debido a que como se aplicó un `apt update` y `apt upgrade` manual antes de ejecutar esta auditoría, se borraron los registros de `apt-check`. 

Por último, vemos más abajo que ha detectado una herramienta de auditoría que instalamos en **Corrección 4** de la sección **Resultados e implementación de mejoras**:

~~~
2022-11-29 09:23:09 ====
2022-11-29 09:23:09 Performing test ID PKGS-7394 (Check for Ubuntu updates)
2022-11-29 09:23:09 Test: checking /usr/bin/apt-show-versions
2022-11-29 09:23:09 Result: found /usr/bin/apt-show-versions
2022-11-29 09:23:09 Test: Checking packages which can be upgraded via apt-show-versions
2022-11-29 09:23:09 Result: no packages found which can be upgraded
2022-11-29 09:23:09 Hardening: assigned maximum number of hardening points for this item (3). Currently having 119 points (out of 163)
2022-11-29 09:23:09 ====

...
~~~