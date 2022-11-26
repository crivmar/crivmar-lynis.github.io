---
layout: default
title: Bloque 14 - Registros, servicios y inseguros
parent: Primera auditoría
nav_order: 14
---

## Bloque 14 - Registros y servicios inseguros

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/17.png"/>


La primera parte recala sobre la comprobación de los *demonios* (*programas no interactivos y ejecutados en segundo plano*), dentro de estos se comprueba distintos sistemas de registros, en este caso `Rsyslog` es el que se detecta. 
Este programa se encarga de recolectar los mensajes de servicios que provienen de aplicaciones y el núcleo para luego distribuirlos en archivos de registros (normalmente almacenados en el directorio `/var/log`). Dicho *demonio* obedece los parámetros de configuración del fichero llamado `rsyslog.conf`, que se encuentra en el directorio `/etc/`. 

La siguiente comprobación es de si detecta la presencia de `logrotate`, esta utilidad en `Linux` nos permite rotar, comprimir y renovar los ficheros de registro de forma automatizada y, habitualmente, se encuentra su archivo de configuración en `/etc/logrotate.conf`. Por defecto, ya se ocupa de aplicar políticas de rotación y compresión, pero podemos personalizarlas.

Comprueba los directorios estáticos habituales de los registros `/var/log/` y mira si hay archivos abiertos; para, al final, comprobar si hay archivos borrados  que todavía están en uso. Esto quiere decir que cuando borras archivos, realmente no lo estás haciendo; lo que haces es borrar la referencia de este en el disco (*inodos*), pero sigue existiendo esta referencia para una aplicación que lo esté usando. 
Esto se utiliza incluso para los archivos temporales: algunas implementaciones crean un archivo e inmediatamente lo desvinculan, por lo que no es visible en el sistema de archivos, pero el proceso que lo creó lo está utilizando normalmente. Si usamos el comando `lsof` (*List open files*) y lo filtramos por `deleted`, nos mostrará los archivos abiertos y los procesos que lo tienen abiertos

~~~
lsof | grep deleted
~~~

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/17_01.png"/>


En el apartado "*Servicios inseguros*" hace referencia a `inetd`, se le conoce como "*Super servidor de Internet*" ya que atiende las solicitudes de conexión que llegan al equipo y este lanza el proceso correspondiente a la petición. Por ejemplo, si le llega una petición por el puerto `443` (*HTTPS*), y este lo tiene configurado así, ejecutará el servidor web que tiene asignado y así reduce la carga del sistema. 
En nuestro caso vemos que que no está activado, quiere decir que este servicio no está habilitado en el archivo `/etc/rc.conf` (*ejecutado durante el arranque del sistema*) o directamente no existe; deberíamos encontrar en `/etc/` el archivo de configuración `inetd.conf` si estuviera instalado.
Pero, ¿por qué lo llama servicio inseguro?, no es debido a su uso si no más bien al contenido habilitado en su archivo de configuración, ya que una mala configuración puede poner en riesgo nuestro equipo. Por ejemplo, no se aconseja ejecutar algunos *demonios* determinados (un caso típico es `fingerd`, programa de gestión de usuarios) porque pueden proporcionar información valiosa para un atacante. Algunos *demonios* no presentan ninguna característica de seguridad y poseen grandes o incluso no poseen tiempos de expiración para los intentos de conexión. Esto permite que un atacante sature los recursos de nuestra máquina realizando intentos de conexión a una tasa relativamente baja contra uno de estos procesos. Puede ser una buena idea protegerse de esto utilizando las opciones `max-connections-per-ip-per-minute` y `max-child` para mitigarlos.


