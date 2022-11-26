---
layout: default
title: Bloque 15 - Banderas e identificaciones y tareas programadas
parent: Primera auditoría
nav_order: 15
---

## Bloque 15 - Banderas e identificaciones y tareas programadas

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/18.png"/>

El bloque que hace referencia a las "banderas" e identificaciones, a priori confuso para mí, hace referencia a los mensajes de bienvenida de `Linux` que aparecen en la terminal cuando te registras en la máquina. Vemos:

- `/etc/motd`, no encontrado, hace referencia al "mensaje del día". Este aparece una vez el usuario se ha conectado desde una consola (local o remota) y por defecto está vacío, pero el administrador puede modificar el archivo mencionado para informar a los usuarios que se conecten, por ejemplo, de un reinicio.

- `/etc/issue` y `etc/issue.net`, encontrados. El primero hace referencia al mensaje que aparece antes de la inserción de los datos de registro (usuario y contraseña) cuando un usuario se conecta por consola. El archivo `issue.net`, es cuando un usuario se conecta desde una consola remota (`telnet`, `ssh`, etc.); en ambos casos, por defecto, suelen indicar el nombre de la distribución y la versión de la misma. Nos indican que son débiles, más adelante veremos a qué se refiere.


La otra parte de este apartado es sobre las tareas programadas, buscando si `crontab` está corriendo y si tiene algún `cronjob` (*tarea cíclica*) ejecutando. En este caso crontab está corriendo por defecto, pero si queremos ver si hay alguna tarea creada debemos usar los comandos:

~~~
# Para el usuario actual

crontab -e

# Para un usuario concreto, necesario privilegios de administrador

sudo crontab -eu [nombre_usuario]
~~~

Comprobamos el del usuario `root` y observamos que está por defecto y no hay ninguna tarea configurada:

~~~
# Edit this file to introduce tasks to be run by cron.
#
# Each task to run has to be defined through a single line
# indicating with different fields when the task will be run
# and what command to run for the task
#
# To define the time you can provide concrete values for
# minute (m), hour (h), day of month (dom), month (mon),
# and day of week (dow) or use '*' in these fields (for 'any').#
# Notice that tasks will be started based on the cron's system
# daemon's notion of time and timezones.
#
# Output of the crontab jobs (including errors) is sent through
# email to the user the crontab file belongs to (unless redirected).
#
# For example, you can run a backup of all your user accounts
# at 5 a.m every week with:
# 0 5 * * 1 tar -zcf /var/backups/home.tgz /home/
#
# For more information see the manual pages of crontab(5) and cron(8)
#
# m h  dom mon dow   command

~~~

Por último comprueba si el el demonio `atd` está corriendo, este se utiliza para tareas que se ejecutarán una sola vez, pero en un momento específico en el futuro. No está en uso por esta máquina.