---
layout: default
title: Bloque 1 - Herramientas, plugins y arranque
parent: Segunda auditoría
nav_order: 1
---

## Bloque 1 - Herramientas, plugins y arranque

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/53.png"/>

En el apartado **Bloque 2** de la sección **Primera auditoría**, podemos ver que hacía referencia a muchas rutas dentro `/usr/`, `/bin`, etc. Aquí sólo aparece que escanea esos sitios, pero no da más información. Tenemos que irnos a registro de esta auditoría (`auditoria2.log`) para ver que hace lo mismo:

~~~
2022-11-29 09:22:49 Action: Performing tests from category: System tools
2022-11-29 09:22:49 Start scanning for available audit binaries and tools...
2022-11-29 09:22:49 ====
2022-11-29 09:22:49 Performing test ID CORE-1000 (Check all system binaries)
2022-11-29 09:22:49 Status: Starting binary scan...
2022-11-29 09:22:49 Test: Checking binaries in directory /snap/bin
2022-11-29 09:22:49 Result: Directory /snap/bin does NOT exist
2022-11-29 09:22:49 Test: Checking binaries in directory /bin
2022-11-29 09:22:49 Directory /bin exists. Starting directory scanning...
2022-11-29 09:22:49   Found known binary: cat (generic file handling) - /bin/cat
2022-11-29 09:22:49   Found known binary: dnsdomainname (DNS domain) - /bin/dnsdomainname
2022-11-29 09:22:49   Found known binary: domainname (NIS domain) - /bin/domainname
2022-11-29 09:22:49   Found known binary: egrep (text search) - /bin/egrep
2022-11-29 09:22:49   Found known binary: grep (text search) - /bin/grep
2022-11-29 09:22:49   Found known binary: gzip (compressing utility) - /bin/gzip
2022-11-29 09:22:49   Found known binary: ip (IP configuration) - /bin/ip
2022-11-29 09:22:49   Found known binary: journalctl (systemd journal) - /bin/journalctl
2022-11-29 09:22:49   Found known binary: ls (file listing) - /bin/ls
2022-11-29 09:22:49   Found known binary: lsblk (block devices) - /bin/lsblk
2022-11-29 09:22:49   Found known binary: lsmod (kernel modules) - /bin/lsmod
2022-11-29 09:22:49   Found known binary: mount (disk utility) - /bin/mount
2022-11-29 09:22:49   Found known binary: netstat (network statistics) - /bin/netstat
2022-11-29 09:22:49   Found known binary: ps (process listing) - /bin/ps
2022-11-29 09:22:49   Found known binary: readlink (follows symlinks) - /bin/readlink
2022-11-29 09:22:49   Found known binary: sed (text stream editor) - /bin/sed
2022-11-29 09:22:49   Found known binary: ss (show sockets) - /bin/ss
2022-11-29 09:22:49   Found known binary: systemctl (client to systemd) - /bin/systemctl
2022-11-29 09:22:49   Found known binary: uname (operating system details) - /bin/uname
2022-11-29 09:22:49   Found known binary: zgrep (text search for compressed files) - /bin/zgrep
2022-11-29 09:22:49 Test: Checking binaries in directory /sbin
2022-11-29 09:22:49 Directory /sbin exists. Starting directory scanning...

...

~~~

El siguiente apartado, [`plugins`](https://cisofy.com/lynis/plugins/) es nuevo respecto a la otra versión que hemos usado, además hay muchos de estos que son sólo para la versión `Enterprise`. Estos están escritos en `shell script`, como los test, por lo cuál permite que puedas desarrollar estos añadidos personalizados. Dentro de `Lynis` tienen la función de recoger más datos; estos pueden ser utilizados por cualquier prueba que haga el programa o combinarla con tus test personalizados; o simplemente se almacenen en los archivos de registro y que herramientas de terceros puedan leerlos.
Los añadidos tienen dos fases y se despliegan dentro del funcionamiento del programa de la siguiente manera:

- **Inicio del programa.**

- **Detección del SO.**

- **Detección de binarios (Primera parte de este apartado).**

- **Fase 1 de los "plugins". (Recoger información)**

- **Ejecutar test incorporados.**

- **Ejecutar test personalizados (si los hubiera).**

- **Fase 2 de los "plugins".**

- **Mostrar informes finales.**

- **Detención del programa.**

Se puede averiguar dónde están estos añadidos, usando este comando:

~~~
sudo ./lynis show plugindir
~~~

### Plugin de PAM

Recoge la información de los módulos de autenticación (**PAM**) y en el archivo de registro de la auditoría tiene este aspecto:

~~~
2022-11-29 09:22:50 Action: Performing plugin tests
2022-11-29 09:22:50 Searching plugins...
2022-11-29 09:22:50 Found plugin file: /usr/local/lynis/plugins/plugin_pam_phase1
2022-11-29 09:22:50 Action: checking plugin status in profile: /usr/local/lynis/default.prf
2022-11-29 09:22:50 Result: plugin enabled in profile (/usr/local/lynis/default.prf)
2022-11-29 09:22:50 Result: plugin pam is enabled
2022-11-29 09:22:50 Checking permissions of /usr/local/lynis/plugins/plugin_pam_phase1
2022-11-29 09:22:50 File permissions are OK
2022-11-29 09:22:50 Including plugin file: /usr/local/lynis/plugins/plugin_pam_phase1 (version: 1.0.5)
2022-11-29 09:22:50 ====
2022-11-29 09:22:50 Skipped test PLGN-0008 (Check PAM configuration (pwquality.conf))
2022-11-29 09:22:50 Reason to skip: Prerequisites not met (ie missing tool, other type of Linux distribution)
2022-11-29 09:22:50 ====
2022-11-29 09:22:50 Performing test ID PLGN-0010 (Check PAM configuration)
2022-11-29 09:22:50 Result: /etc/pam.d exists
2022-11-29 09:22:50 Now checking PAM file /etc/pam.d/common-auth
2022-11-29 09:22:50 Result: Found brackets in line, indicating multiple options for control flags: success=1 default=ignore
2022-11-29 09:22:50 Result: brackets used, ignoring control flags
2022-11-29 09:22:50 Result: using module pam_unix.so (other) with options nullok_secure
2022-11-29 09:22:50 Result: found pam_unix.so module (generic)

...
~~~

Como vemos busca los módulos y va descendiendo para comprobar los archivos de estos módulos, si están por defecto, si hay algún fallo o qué configuraciones tienen.


### Plugin systemd

Recoge detalles de `systemd`. Aprovecha herramientas como `systemctl` y `journalctl` para determinar el estado del sistema:

~~~
2022-11-29 09:22:52 --
2022-11-29 09:22:52 Found plugin file: /usr/local/lynis/plugins/plugin_systemd_phase1
2022-11-29 09:22:52 Action: checking plugin status in profile: /usr/local/lynis/default.prf
2022-11-29 09:22:52 Result: plugin enabled in profile (/usr/local/lynis/default.prf)
2022-11-29 09:22:52 Result: plugin systemd is enabled
2022-11-29 09:22:52 Checking permissions of /usr/local/lynis/plugins/plugin_systemd_phase1
2022-11-29 09:22:52 File permissions are OK
2022-11-29 09:22:52 Including plugin file: /usr/local/lynis/plugins/plugin_systemd_phase1 (version: 1.0.4)
2022-11-29 09:22:52 ====
2022-11-29 09:22:52 Performing test ID PLGN-3800 (Gather systemctl exit code)
2022-11-29 09:22:52 ====
2022-11-29 09:22:52 Performing test ID PLGN-3802 (Query systemd version and options)
2022-11-29 09:22:52 Result: found systemd version 204
2022-11-29 09:22:52 Result: found builtin components list
2022-11-29 09:22:52 ====
2022-11-29 09:22:52 Performing test ID PLGN-3804 (Gather systemd unit files and their status)
2022-11-29 09:22:52 Result: found systemd unit files via systemctl list-unit-files
2022-11-29 09:22:52 Output: cups-browsed.service|masked|
2022-11-29 09:22:52 Output: dbus-org.freedesktop.Avahi.service|enabled|
2022-11-29 09:22:52 Output: dbus.service|static|
2022-11-29 09:22:52 Output: sshd.service|enabled|
2022-11-29 09:22:52 Output: syslog.service|enabled|
2022-11-29 09:22:52 Output: systemd-fsck@.service|static|
2022-11-29 09:22:52 Output: systemd-halt.service|static|
2022-11-29 09:22:52 Output: systemd-hibernate.service|static|

...
~~~

Comprueba las unidades con `systemctl` y saca el estado, es como la sugerencia que nos hacía la anterior versión de listar todos los servicios, en la sección de **Resultados e implementación de mejoras**, apartado **Correción 2**.


### Arranque y servicios

Tras la recopilación de información anterior, aquí comienza determinando el administrador de servicios:

~~~
2022-11-29 09:22:52 Action: Performing tests from category: Boot and services
2022-11-29 09:22:52 ====
2022-11-29 09:22:52 Skipped test BOOT-5102 (Check for AIX boot device)
2022-11-29 09:22:52 Reason to skip: Incorrect guest OS (AIX only)
2022-11-29 09:22:52 ====
2022-11-29 09:22:52 Performing test ID BOOT-5104 (Determine service manager)
2022-11-29 09:22:52 Result: cmdline found = /sbin/init
2022-11-29 09:22:52 Result: file on disk = /sbin/init
2022-11-29 09:22:52 Found: init
2022-11-29 09:22:52 Result: service manager found = upstart
2022-11-29 09:22:52 ====

...
~~~ 

El servicio que encuentra es `Upstart`, es un reemplazo basado en eventos para el *demonio* `init`, el método utilizado por varios sistemas operativos tipo `Unix` para realizar tareas durante el arranque del sistema. `Init` tradicional es estrictamente síncrono, bloqueando futuras tareas hasta que la actual se haya completado. Estas deben ser definidas por adelantado, y solo pueden ser ejecutadas cuando el *demonio* cambia de estado (**cuando la máquina arranca o se apaga**). Esto hace que no sea capaz de manejar de forma elegante varias tareas en computadoras de escritorio modernas. El modelo basado en `Upstart` supervisa tareas mientras el sistema está funcionando y responde a eventos tales como la conexión o desconexión de dispositivos extraíbles, además de contar con la gestión de servicios durante el arranque o apagado. Este sistema está siendo apartado por el de `systemd`, como esta versión es antigua todavía lo tiene en uso.


Comprueba si el el arranque `UEFI` está habilitado, no es el caso, ya que no encuentra el directorio y esto nos indica que está usando `Legacy Bios` :

~~~
2022-11-29 09:22:52 ====
2022-11-29 09:22:52 Performing test ID BOOT-5116 (Check if system is booted in UEFI mode)
2022-11-29 09:22:52 Test: checking if UEFI is used
2022-11-29 09:22:52 Result: UEFI not used, can't find /sys/firmware/efi directory
2022-11-29 09:22:52 Test: determine if Secure Boot is used
2022-11-29 09:22:52 Result: system not booted with Secure Boot (no SecureBoot file found)
2022-11-29 09:22:52 ====
2022-11-29 09:22:52 Skipped test BOOT-5117 (Check for systemd-boot bootloader presence)
2022-11-29 09:22:52 Reason to skip: Prerequisites not met (ie missing tool, other type of Linux distribution)
2022-11-29 09:22:52 ====

...
~~~

Detecta la presencia de `GRUB2` (Gestor de arranque) y si este está protegido con contraseña para evitar que alguien lo modifique ajeno al administrador principal del sistema. Luego comprueba con `systemctl` los servicios que están habilitados y corriendo, pero en este sistema este comando no funciona, para este sistema habría que hacer uso de `service` (anterior a `systemd`):

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/54.png"/>

~~~
[+] -> El servicio está corriendo

[-] -> El servicio no está funcionando

[?] -> No puede determina la línea de estado en /etc/init.d/
~~~

Por último comprueba los servicios que se inician al arrancar con los `script` dentro de `/etc/rcN.d/` y si los permisos de estos son los correctos:

~~~
2022-11-29 09:22:53 Action: checking symlink for file /etc/rc2.d/S70pppd-dns
2022-11-29 09:22:53 Note: Using real readlink binary to determine symlink on /etc/rc2.d/S70pppd-dns
2022-11-29 09:22:53 Result: readlink shows /etc/init.d/pppd-dns as output
2022-11-29 09:22:53 Result: symlink found, pointing to file /etc/init.d/pppd-dns
2022-11-29 09:22:53 Result: good, file /etc/rc2.d/S70pppd-dns not world writable
2022-11-29 09:22:53 Action: checking symlink for file /etc/rc2.d/S80continuum
2022-11-29 09:22:53 Note: Using real readlink binary to determine symlink on /etc/rc2.d/S80continuum
2022-11-29 09:22:53 Result: readlink shows /opt/apache_continuum/apache-continuum-1.4.2/bin/continuum as output
2022-11-29 09:22:53 Result: symlink found, pointing to file /opt/apache_continuum/apache-continuum-1.4.2/bin/continuum
2022-11-29 09:22:53 Result: good, file /etc/rc2.d/S80continuum not world writable
2022-11-29 09:22:53 Action: checking symlink for file /etc/rc2.d/S91apache2
2022-11-29 09:22:53 Note: Using real readlink binary to determine symlink on /etc/rc2.d/S91apache2
2022-11-29 09:22:53 Result: readlink shows /etc/init.d/apache2 as output
2022-11-29 09:22:53 Result: symlink found, pointing to file /etc/init.d/apache2
2022-11-29 09:22:53 Result: good, file /etc/rc2.d/S91apache2 not world writable

...
~~~


---

Información adicional
{: .label .label-blue}

- **UEFI y Legacy-> Ambos hacen ,en esencia, más o menos lo mismo. Inician los componentes de hardware con comprobaciones y lanzan el sistema operativo. La diferencia radica en compatibilidades, rápidez del manejo de las comprobaciones, interfaces (en UEFI es gráfica), mejoras de seguridad,etc.**

