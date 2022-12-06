---
layout: default
title: Bloque 12 - SSH y SNMP
parent: Primera auditoría
nav_order: 12
---

## Bloque 12 - SSH y SNMP

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/15.png"/>

Aquí se comprueba si tenemos un servidor `SSH` funcionando, en este caso de manera afirmativa. Busca el fichero de configuración en la ruta `/etc/ssh/sshd_config` (`Openssh`) y comprueba los valores que tiene con una serie de valores que tiene definido el test y en función de eso nos muestra lo siguiente:

- **Opción "PermitRootLogin"**, está puesto por defecto, esto quiere decir que en muchas distribuciones de Linux no se permite el registro de un usuario como `root`vía SSH de forma predeterminada.

- **Protocol**, dado como válido. En el archivo de configuración vemos un "2", esto significa que sólo admite el protocolo `SSH-2`. Este se adoptó como estándar en 2006 y añade nuevas características y mejoras de seguridad; como la verificación de la integridad mediante códigos de autenticación de mensajes.

- **StrictModes**, requiere que `sshd` realice verificaciones del anfitrión y permisos en el directorio de inicio del usuario, además de los archivos de configuración relacionados antes de aceptar la solicitud de conexión.

- **AllowUsers**, esta opción (no especificada en el archivo) puede ir seguida de una lista de usuarios, separados por espacios. Si se especifica, sólo se permite realizar el registro a los usuarios cuyo nombre concuerde con el patrón. Se pueden usar '\*' y '?' para la construcción de patrones. Sólo se admiten nombres de usuario; nada de identificadores numéricos. 

- **AllowGroups**, al igual que el anterior pero siendo sólo permitido por usuarios que tienen como grupo principal o suplementarios alguno de los de la lista o que coincidan con el patrón.

Hay muchas más opciones en este archivo de configuración, pero la aplicación muestra en esta lista esos valores que tiene importancia a la hora de entrar en el sistema.

Otro apartado que mira en este bloque es el soporte de `SNMP`(*Simple Network Management Protocol*), este sirve para facilitar el intercambio de información de administración entre los dispositivos de red. Estos estarán gestionados por uno central (servidor), que almacenará información de estos en una base de datos denominada `MIB` (*Management Information Base*); en esta podemos encontrar usuarios, programas instalados, puertos abiertos y más, de todos los dispositivos clientes. No está instalado este servicio, por lo cuál pasa al siguiente bloque.