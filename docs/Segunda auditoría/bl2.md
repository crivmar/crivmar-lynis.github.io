---
layout: default
title: Bloque 2 - Kernel y memoria
parent: Segunda auditoría
nav_order: 2
---

## Bloque 2 - Kernel y memoria

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/55.png"/>

La parte de `Kernel` es similar al del **Bloque 4** de la sección **Primera auditoría**. Por ejemplo cómo comprueba el `runlevel` o si la CPU puede usar `PAE` o `NX`:

~~~
2022-11-29 09:22:53 Action: Performing tests from category: Kernel
2022-11-29 09:22:53 ====
2022-11-29 09:22:53 Performing test ID KRNL-5622 (Determine Linux default run level)
2022-11-29 09:22:53 Test: Checking for systemd default.target
2022-11-29 09:22:53 Result: no systemd found, so trying inittab
2022-11-29 09:22:53 Test: Checking /etc/inittab
2022-11-29 09:22:53 Result: file /etc/inittab not found
2022-11-29 09:22:53 Test: Checking run level with who -r, for Debian based systems
2022-11-29 09:22:53 Result: Found default run level '2'
2022-11-29 09:22:53 ====
2022-11-29 09:22:53 Performing test ID KRNL-5677 (Check CPU options and support)
2022-11-29 09:22:53 Test: Checking /proc/cpuinfo
2022-11-29 09:22:53 Result: found /proc/cpuinfo
2022-11-29 09:22:53 Test: Checking CPU options (XD/NX/PAE)
2022-11-29 09:22:53 PAE: Yes
2022-11-29 09:22:53 NX: Yes
2022-11-29 09:22:54 Result: PAE or No eXecute option(s) both found
2022-11-29 09:22:54 ====

...
~~~ 



Las difrencias radican en los módulos activos encontrados (44 en esta auditoría respecto a los 53 de la otra) haciendo uso del comando `lsmod`:

~~~
2022-11-29 09:22:54 Performing test ID KRNL-5723 (Determining if Linux kernel is monolithic)
2022-11-29 09:22:54 Test: checking if kernel is monolithic or modular
2022-11-29 09:22:54 Result: Found modular kernel
2022-11-29 09:22:54 ====
2022-11-29 09:22:54 Performing test ID KRNL-5726 (Checking Linux loaded kernel modules)
2022-11-29 09:22:54 Loaded modules according lsmod:
2022-11-29 09:22:54 Loaded module: ablk_helper
2022-11-29 09:22:54 Loaded module: aes_x86_64
2022-11-29 09:22:54 Loaded module: aesni_intel
2022-11-29 09:22:54 Loaded module: auth_rpcgss
2022-11-29 09:22:54 Loaded module: autofs4
2022-11-29 09:22:54 Loaded module: crc32_pclmul
2022-11-29 09:22:54 Loaded module: crct10dif_pclmul
2022-11-29 09:22:54 Loaded module: cryptd
2022-11-29 09:22:54 Loaded module: dm_crypt
2022-11-29 09:22:54 Loaded module: e1000
2022-11-29 09:22:54 Loaded module: fscache
2022-11-29 09:22:54 Loaded module: gf128mul
2022-11-29 09:22:54 Loaded module: ghash_clmulni_intel
2022-11-29 09:22:54 Loaded module: glue_helper
2022-11-29 09:22:54 Loaded module: i2c_piix4
2022-11-29 09:22:54 Loaded module: ip6_tables
2022-11-29 09:22:54 Loaded module: ip6table_filter
2022-11-29 09:22:54 Loaded module: ip_tables

...
~~~

Y en la comprobación de la configuración de los `Core dumps` muestra más información que en la versión anterior. Comprueba como están los archivos de configuración (este caso por defecto) en `/etc/systemd/coredump.conf`, `/etc/profile/`, `/etc/security/`, etc. En el archivo de registro de la auditoría vemos cómo lo comprueba y el resultado que ha obtenido:

~~~
2022-11-29 09:22:54 ====
2022-11-29 09:22:54 Performing test ID KRNL-5820 (Checking core dumps configuration)
2022-11-29 09:22:54 Test: Checking presence of systemd
2022-11-29 09:22:54 Result: systemd is present on this system
2022-11-29 09:22:54 Test: Checking if core dumps are disabled in /etc/systemd/coredump.conf and /etc/systemd/coredump.conf.d/*.conf
2022-11-29 09:22:54 Result: core dumps are not disabled in systemd configuration. Didn't find settings 'ProcessSizeMax=0' and 'Storage=none'
2022-11-29 09:22:54 Hardening: assigned partial number of hardening points (0 of 1). Currently having 3 points (out of 6)
2022-11-29 09:22:54 Test: Checking presence /etc/profile
2022-11-29 09:22:54 Test: Checking if 'ulimit -c 0' exists in /etc/profile or /etc/profile.d/*.sh
2022-11-29 09:22:54 Result: core dumps are not disabled in /etc/profile or /etc/profile.d/*.sh config files. Didn't find setting 'ulimit -c 0'
2022-11-29 09:22:54 Hardening: assigned partial number of hardening points (0 of 1). Currently having 3 points (out of 7)

...
~~~ 

Por último comprueba si el sistema necesita un reinicio por algún paquete o servicio que se haya actualizado.

### Memoria y procesos

Sólo añade una cosa respecto a lo visto en el **Bloque 5** de la sección **Primera auditoría**. La presencia del paquete `prelink`, que es un programa que mejora la velocidad del sistema reduciendo el tiempo de carga de las aplicaciones, pero este pude interferir con la herramienta `AIDE` (Integridad de archivos) ya que cambia los binarios. Este puede aumentar la vulnerabilidad del sistema si un usuario es capaz de comprometer una librería común como `libc`. Por eso nos indica con el color verde que no se ha encontrado.

~~~
2022-11-29 09:22:54 Action: Performing tests from category: Memory and Processes
2022-11-29 09:22:54 ====
2022-11-29 09:22:54 Performing test ID PROC-3602 (Checking /proc/meminfo for memory details)
2022-11-29 09:22:54 Result: found /proc/meminfo
2022-11-29 09:22:54 Result: Found 2049720 kB memory
2022-11-29 09:22:54 ====
2022-11-29 09:22:54 Skipped test PROC-3604 (Query prtconf for memory details)
2022-11-29 09:22:54 Reason to skip: Incorrect guest OS (Solaris only)
2022-11-29 09:22:54 ====
2022-11-29 09:22:54 Performing test ID PROC-3612 (Check dead or zombie processes)
2022-11-29 09:22:54 Result: no zombie processes found
2022-11-29 09:22:54 ====
2022-11-29 09:22:54 Performing test ID PROC-3614 (Check heavy IO waiting based processes)
2022-11-29 09:22:54 Result: No processes were waiting for IO requests to be handled first
2022-11-29 09:22:54 ====
2022-11-29 09:22:54 Performing test ID PROC-3802 (Check presence of prelink tooling)
2022-11-29 09:22:54 Result: prelink package is NOT installed
2022-11-29 09:22:54 Hardening: assigned maximum number of hardening points for this item (3). Currently having 13 points (
out of 19)

...
~~~



**NOTA: Como se ha mencionado anteriormente, se puede ver en más información sobre estos procesos en `/var/log/lynis-log.dat` .**



---

- **Core dump (Volcado de núcleo) -> Es un archivo que contiene el espacio de direcciones (memoria) de un proceso cuando éste termina inesperadamente. Los volcados del núcleo pueden ser producidos bajo demanda (por un depurador), o automáticamente al terminar el proceso.Son activados por el núcleo en respuesta a la caída del programa, y pueden ser pasados a un programa de ayuda (como `systemd-coredump`) para su posterior procesamiento.**