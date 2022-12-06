---
layout: default
title: Información adicional
nav_order: 7
has_children: false
has_toc: false
---

# Información adicional

Ahora que hemos hecho un par de auditorías en nuestro sistema, podemos hablar de funciones más específicas de este programa y que nos permiten hacer distintas cosas.


## Pasar test por grupo específico

Durante todas las entradas anteriores se han basado en una pasada completa de todos sus test por nuestro sistema. Pero existe la opción de sólo hacer un grupo de test, bien porque en ese momento nos interesa sólo comprobar la seguridad de ese apartado porque hemos realizado cambios, porque hemos implementado algo nuevo y queremos probarlo o porque no nos interese una auditoría completa. En cuyo caso, como vimos en la introducción de **Primera auditoría** podemos ver los grupos de test que hay, pero esa opción ya no es válida en la versión más moderna que hemos usado en **Segunda auditoría**, ahora se pueden ver por categorías (rendimiento, privacidad y seguridad) o por grupos (como vimos al principio):

~~~
sudo ./lynis show [categories|groups]
~~~

Como anteriormente no estaba la base de datos corriendo durante la segunda auditoría, he habilitado una base de datos `MongoDB` para correr individualmente las pruebas con este propósito:

~~~
sudo ./lynis --test-from-group "databases"
~~~ 

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/91.png"/>

Ha detectado la base de datos de `MongoDB` y ha comprobado las autorizaciones, deshabilitadas, para indicarnos como "peligro" que cualquier usuario puede acceder a la base de datos. veamos lo que nos dice el registro:

~~~
2022-12-05 10:28:25 ====
2022-12-05 10:28:25 Performing test ID DBS-1818 (Check status of MongoDB server)
2022-12-05 10:28:25 Performing pgrep scan without uid
2022-12-05 10:28:25 IsRunning: process 'mongod' found (3098 )
2022-12-05 10:28:25 ====
2022-12-05 10:28:25 Performing test ID DBS-1820 (Check for authorization in MongoDB)
2022-12-05 10:28:25 Result: found MongoDB configuration file (/etc/mongod.conf)
2022-12-05 10:28:25 Test: determine authorization setting in new style YAML format
2022-12-05 10:28:25 Result: did NOT find authorization option enabled in configuration file (with YAML format)
2022-12-05 10:28:25 Test: now searching for old style configuration (auth = true) in configuration file
2022-12-05 10:28:25 Result: did NOT find auth = true in configuration file
2022-12-05 10:28:25 Result: configuration file /etc/mongodb.conf not found
2022-12-05 10:28:25 Result: no authorization enabled via parameter or configuration file
2022-12-05 10:28:25 Warning: MongoDB instance allows any user to access databases [test:DBS-1820] [details:-] [solution:-]

...
~~~

Vemos que no detecta la línea de `auth` en el archivo `/etc/mongod.conf`, así que vamos a ir al archivo de configuración a habilitar dicha línea dentro de `security`:


<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/92.png"/>

Tras reiniciar el servicio, vamos a volver a pasar el test a ver si ha habido algún cambio:

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/93.png"/>


Veamos que dicen los registros:

~~~
2022-12-05 10:50:06 ====
2022-12-05 10:50:06 Performing test ID DBS-1818 (Check status of MongoDB server)
2022-12-05 10:50:06 Performing pgrep scan without uid
2022-12-05 10:50:06 IsRunning: process 'mongod' found (33375 )
2022-12-05 10:50:06 ====
2022-12-05 10:50:06 Performing test ID DBS-1820 (Check for authorization in MongoDB)
2022-12-05 10:50:06 Result: found MongoDB configuration file (/etc/mongod.conf)
2022-12-05 10:50:06 Test: determine authorization setting in new style YAML format
2022-12-05 10:50:06 Result: GOOD, found authorization option enabled in configuration file (YAML format with quotes)
2022-12-05 10:50:06 Result: configuration file /etc/mongodb.conf not found
2022-12-05 10:50:06 ====

...
~~~

Ahora la base de datos tiene restringido el acceso, pero no hace más comprobaciones sobre esta, quizás para ello haga falta crear un test personalizado (hablaremos un poco más adelante de ello) para comprobar todavía más la configuración de la base de datos que estemos usando.


## Perfiles

Los perfiles son un conjunto de opciones predefinidas por el administrador para su sistema operativo y sus preferencias de seguridad sobre este. Si no se proporciona un perfil (`--profile [nombre]`) se utiliza el por defecto que trae `Lynis` (`default.prf`) y que aplica todos los test que tiene. Pero esto te permite crear tus propios perfiles, para ajustar a tus necesidades, como por ejemplo ajustarlo por "sistemas operativos" y así eliminar entradas en los registros sobre pruebas que se saltan porque no están destinadas para el que estamos usando. Si descontentamos una línea del archivo por defecto, podemos saltar los test de `FreeBSD`:

~~~
#################################################################################
#
# Operating system specific
# -------------------------
#
#################################################################################

# Skip the FreeBSD portaudit test
freebsd-skip-portaudit=yes

# Skip security repository check for Debian based systems
#debian-skip-security-repository=yes

...
~~~

## Otras opciones

`Lynis` puede también ejecutarse en modo "no privilegiado", pero saltará todos aquellos test que requieran privilegios de administrador. Además, tiene estos modos:

- `--pentest`, de manera no privilegiada, muestra los puntos de interés donde hay posibles puntos de ruptura para la penetración del sistema. Al igual que el modo completo que hemos usado, muestra sugerencias y peligros de todas aquellas cosas que se pueden mejorar.

- `--forensics`, permite hacer una auditoría en un sistema montado. Si tenemos un disco duro que montamos temporalmente en `/mnt/`, especificando su ruta podemos hacer una auditoría a este y ver las posibles vulnerabilidades que pueda tener el sistema operativo que él se disponga.


También vamos a repasar otras opciones que pueden ser interesantes, según el caso:

- `--cronjob`, puedes configurar como una tarea que se haga regularmente para mantener una auditoría constante de tu sistema.

- `--developer`, muestra información detalla y útil para los desarrolladores que vayan a crear test personalizados en su archivo de registros:

~~~
2022-12-05 11:37:31 ====
2022-12-05 11:37:31 Performing test ID BOOT-5180 (Check for Linux boot services (Debian style))
2022-12-05 11:37:31 Result: found runlevel 5
2022-12-05 11:37:31 Result: skipping further actions
2022-12-05 11:37:31 ====
2022-12-05 11:37:31 Performing test ID BOOT-5184 (Check permissions for boot files/scripts)
2022-12-05 11:37:31 Result: checking /etc/init.d scripts for writable bit
2022-12-05 11:37:31 Test: checking if directory /etc/init.d exists
2022-12-05 11:37:31 Result: directory /etc/init.d found
2022-12-05 11:37:31 Test: checking for available files in directory
2022-12-05 11:37:31 Result: found files in directory, checking permissions now
2022-12-05 11:37:31 Test: checking permissions of file /etc/init.d/apparmor
2022-12-05 11:37:31 Result: good, file /etc/init.d/apparmor not world writable
2022-12-05 11:37:31 Test: checking permissions of file /etc/init.d/chrony
2022-12-05 11:37:31 Result: good, file /etc/init.d/chrony not world writable
2022-12-05 11:37:31 Test: checking permissions of file /etc/init.d/cron
2022-12-05 11:37:31 Result: good, file /etc/init.d/cron not world writable
2022-12-05 11:37:31 Test: checking permissions of file /etc/init.d/dbus
2022-12-05 11:37:31 Result: good, file /etc/init.d/dbus not world writable

...
~~~

- `--no-log`, envía todos los registros a `/dev/null` por si hubiera interés en prevenir que información sensible se escriba en el disco duro.

- `--quiet`, no muestra nada por la pantalla y sólo pueden verse las acciones y resultados en los registros.

- `--verbose`, muestra más detalles por la pantalla, como servicios o componentes que no encuentra y están ocultos por defecto. Esto le veíamos en los registros durante la segunda auditoría; los registros mostraban más información de la que salía en pantalla.

- `--warnings-only`, con esta opción sólo nos mostraría los apartados que encontrara "peligrosos" en nuestro sistema.


## Pruebas personalizadas

Hablamos en anteriores apartados que los `plugins` tiene la función de recolectar datos y estos pueden ser los que necesitemos creando pruebas personalizadas.
Cada "*plugin*" consta de una o varias pruebas individuales. Están marcados con un ID único, para permitir el registro y el almacenamiento adecuado de los resultados de la prueba. Por esta misma razón, todas las pruebas que se creen deben comenzar con "CUST-" (*Custom*), seguido de cuatro números (por ejemplo, CUST-0001).

Estas pruebas se crean con lenguaje `shell script` y `Lynis` facilita la creación añadiendo unas funciones predefinidas en `include/functions`, por ejemplo:

- `Display`, muestra algo en pantalla.

- `LogText`, almacenar los detalles de la prueba en el archivo de registros.

- `Register`, pone ne la cola la prueba para su ejecución.

- `Report`, almacena el resultado de la prueba en el informe.


Algunos ejemplos de pruebas y funciones pueden verse en el archivo `include/tests_custom.template`, además en la documentación oficial hay una pequeña guía para [desarrolladores](https://cisofy.com/documentation/lynis/plugins/development/). Otro gran recurso son las pruebas normales dentro de `Lynis`, que también se pueden encontrar en el directorio `include`. Podemos ver esta plantilla en [Pastebin](https://pastebin.com/0x83n2vy) o en el [repositorio](https://github.com/CISOfy/lynis/blob/master/include/tests_custom.template) de `Lynis`.


