---
layout: default
title: Correción 3 - Sugerencias - Módulo PAM, contraseñas y umask
parent: Resultados e implementación de mejoras
nav_order: 3
---

## Correción 3 - Sugerencias - Módulo PAM, contraseñas y umask

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/33.png"/>

`Linux-PAM` (*Pluggable Authentication Modules*) es un poderoso conjunto de bibliotecas compartidas que se utilizan para autenticar dinámicamente a un usuario en aplicaciones (o servicios) en un sistema Linux. `PAM` tiene el potencial de alterar seriamente la seguridad de su sistema Linux; una configuración incorrecta puede deshabilitar el acceso a su sistema parcial o completamente. Por ejemplo, una eliminación accidental de un archivo de configuración en `/etc/pam.d/*` y/o `/etc/pam.conf` puede bloquearlo de su propio sistema. 
El archivo de configuración principal para `PAM` es `/etc/pam.conf` y el directorio `/etc/pam.d/` contiene los archivos de configuración de PAM para cada aplicación/servicio compatible con PAM. PAM ignorará el archivo si el directorio existe.

En la sugerencia nos dicen que usemos la librería `pam_cracklib` ó `pam_passwdqc`. Vamos a configurar la primera, comenzando con la instalación de `libpam-cracklib`:

~~~
sudo apt install -y libpam-cracklib
~~~

Lo siguiente es hacer una copia al fichero `/etc/pam.d/common-password`, como medida de seguridad:

~~~
sudo cp /etc/pam.d/common-password /root/
~~~

Vamos a editar el archivo con `nano`:

~~~
sudo nano /etc/pam.d/common-password
~~~

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/34.png"/>

Estás son las opciones que tenemos definidas:

- `retry=3`, es el número de intentos antes de que el sistema devuelva un error.

- `minlen=8`, longitud mínima de la contraseña.

- `difok=3`, número de cambio de caracteres que debe tener una nueva contraseña, respecto a la antigua.


Vamos a cambiar algunos parámetros y dejarla tal que así:

~~~
password requisite pam_cracklib.so retry=2 minlen=10 difok=3 ucredit=-2 lcredit=-2 dcredit=-2 ocredit=-2
~~~

- `ucredit=-2`, mínimo de caracteres en mayúscula.

- `lcredit=-2`, mínimo de caracteres en minúscula.

- `dcredit=-2`, mínimo de dígitos que debe tener la nueva contraseña.

- `ocredit=-2`, mínimo de digitos que debe tener la contraseña.


Ahora lo comprobamos cambiando la contraseña con `passwd`:

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/35.png"/>

Ahora es el momento de añadir una fecha de cadudicad a las contraseñas de los usuarios, para ello vamos a usar el comando `chage` que está instalado por defecto:

~~~
## Le indicamos con -E la fecha de cuándo expira la cuenta

sudo chage -E 2023-05-01 vagrant

## Le indicamos -M el número máximo de días para cambiar la contraseña, desde
## la fecha actual y con -m el mínimo de días

sudo chage -M 46 -m 46 vagrant
~~~

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/36.png"/>

Vemos que el último cambio de contraseña fue el `28/11/22`, la contraseña expira el `13/01/23`, la cuenta caduca el `01/05/23` y sietes días antes de que caduque la contraseña le hará una advertencia al usuario `vagrant`. Esto es algo que habría que configurar para todos los usuarios del sistema.


La última sugerencia de este bloque es cambiar el permiso base dentro de los archivos:

- `/etc/login.defs`, aquí están definidas las variables que contolan los aspectos de la creación de usuarios y de los campos de `shadow` usados por defecto. 

- `/etc/init.d/rc`, en `init.d` están definidos los scripts de inicio, concretamente el `rc` se ecnarga que los scripts correspondientes sean ejecutados en el orden correcto.

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/37.png"/>

Vemos que los valores por defecto son `022` y nos recomiendan `027`, por lo cuál es modificar esas líneas para adaptarlos. `022` significa que el permiso de escritura está enmascarado para otros, así que por defecto los archivos pueden ser leídos pero no escritos o modificados por otros, sin embargo, pueden acceder a elementos con directorios. Si cambiamos a`027`, la lectura y la ejecución también estarán enmascaradas; así que los archivos y directorios recién creados se mantienen privados de los demás. Además, los elementos dentro de los directorios recién creados siempre serán inaccesibles para los demás.

