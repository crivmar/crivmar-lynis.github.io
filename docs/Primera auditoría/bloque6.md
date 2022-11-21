---
layout: default
title: Bloque 6 - Usuario, grupos y autenticación
parent: Primera auditoría
nav_order: 6
---

# Bloque 6 - Usuario, grupos y autenticación

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/09.png"/>

En este bloque podemos sacar información de comprobaciones que hace este programa, a destacar:

- **Comprobación de la consistencia de los archivos de grupos**, esto lo hace con el comando `grpck` que verifica la integridad de todas las entradas en `/etc/group` y `/etc/gshadow` tengan el formato adecuado y contengan datos válidos.

- **Autenticación NIS/+ (Sistema de Información de Red)**, es un protocolo (cliente-servidor) para el envío de datos de configuración de un sistema distribuido (clústeres). Este protocolo proporciona prestaciones de acceso a bases de datos genéricas que pueden utilizarse para distribuir, por ejemplo, la información contenida en los ficheros `passwd` y `groups` a todos los nodos de su red. En este caso no tenemos ese servicio habilitado.

- **Comprobación del archivo `sudoers`**, es importante comprobar este archivo porque es la base de uso del comando `sudo` (ubicado en `/etc/`) y los permisos sobre este deben ser los adecuados ya que puede suponer una vulnerabilidad crítica para un sistema si se modifica este archivo, ya que puede dar al usuario que se encuentre dentro privilegios de administrador.  

- **Comprobación de herramientas PAM (Módulo de Autenticación Conectable)**, es una colección de módulos que ponen barreras entre un servicio del sistema y el usuario de ese servicio. Esto permite tener un control centralizado y personalizado para autenticar a los usuarios en uno o varios servicios. Los archivos de configuración están alojados en `/etc/pam.conf`, de forma general, y si se quiere hacer de manera individual, se alojan en `/etc/pam.d/` (si este existe, no se tiene en cuenta el anterior). Estos módulos dividen la tarea en 4 grupos:
	
	+  **Account**, realiza diversas verificaciones en las cuentas. Por ejemplo: ¿tiene la contraseña en vigor? ¿tiene permiso para acceder a un determinado servicio?.

    + **Auth**, autentifica a los usuarios y define sus credenciales. Esto suele realizarse mediante algún tipo de desafío-respuesta que el usuario debe cumplir del tipo:introduzca su contraseña. No todas las autentificaciones se realizan de este modo, existen dispositivos físicos tales como tarjetas inteligentes e incluso dispositivos biométricos, para realizar esta tarea.
    
    + **Password**, este es responsable de actualizar los mecanismos de identificación ligados al grupo `Auth`.
    
    + **Session**, se encarga de las tareas que se llevan a cabo antes de iniciar un determinado servicio y después de finalizarlo. Incluye tareas de gestión de archivos de registro, montaje del directorio `home` del usuario, entre otras. 

Aquí tenemos una sugerencia que trataremos en un apartado más adelante.

- **Comprobaciones de cuentas de usuario**, aquí comprueba si existen cuentas caducadas, sin contraseña y tiempo de uso de la misma contraseña (en este caso deshabilitado, a tener en cuenta).

- **Determinación de la Umask (Máscara de Usuario)**, esto establece los valores que tiene un archivo o directorio cuando son creados. Actualmente los sistemas `Unix` permiten dos formas de especificarlos:

	+ **Máscara simbólica** , como ejemplo `u=rw-,g=rwx,o=`. Esto quiere decir que el usuario (`u`), tendrá permisos de lectura y escritura; el grupo (`g`), tiene lectura, escritura y ejecución; y otros (`o`) no tiene permisos.
	
	+ **Máscara octal**, asignando un valor numérico comprendido entre el 0 y el 7.
	

		Número Octal | Binario | Tipo de Permiso
		------------ | ------------ | ------------
		0 | 000 | sin permisos
		1 | 001 | solo ejecución
		2 | 010 | solo escritura 
		3 | 011 | escritura y ejecución
		4 | 100 | solo lectura
		5 | 101 | lectura y ejecución
		6 | 110 | lectura y escritura
		7 | 111 | todos los permisos


El valor de `umask` de todo el sistema se puede establecer en `/etc/profile` o en los archivos de configuración predeterminados del shell, por ejemplo `/etc/bash.bashrc`. La mayoría de las distribuciones Linux establecen un valor predeterminado 022 de umask. En este caso no parece que haya establecido una directriz principal, como vemos en la imagen, pero nos apunta a posibles sugerencias de estos valores en otros archivos de `/etc/`.
