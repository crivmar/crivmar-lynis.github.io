---
layout: default
title: Bloque 7 - Shells, sistema de archivos y almacenamiento
parent: Primera auditoría
nav_order: 7
---

## Bloque 7 - Shells, sistema de archivos y almacenamiento

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/10.png"/>

La primera parte analiza las `shells` (Intérprete de línea de comandos) disponibles en el archivo `/etc/shells` y cuántas de ellas son válidas. Esto es importante porque la `shell` gestiona la interacción entre el usuario y el sistema operativo solicitándole la entrada, interpretando dicha entrada para el sistema operativo y gestionando cualquier resultado de salida procedente del sistema operativo; por tanto es recomendable comprobar la integridad de estas para evitar que se puedan ejecutar códigos máliciosos en ellas.

Por ejemplo, existe una vulnerabilidad llamada "Shellshock", descubierta por **Sthéphane Chazelas** en 2014, y que guarda relación en cómo `Bash` (Shell usada por muchos sitemas Unix) procesa variables de entorno dictadas por el sistema operativo o algún script. Configurado por defecto, puede ser usado por atacantes a través de `SSH`, `telnet` o cualquier otro programa que ejecute scripts en esta shell. Afecta a versiones de la `1.14` hasta la `4.3`. Hay varias formas de comprobar si tu version de bash es vulnerable, una de ellas es:

~~~
env x='() { :;}; echo vulnerable' bash -c "echo this is a test"
~~~

Si es vulnerable mostrará en una línea "vulnerable" y en otra "this is a test", si no lo fuera, sólo se mostraría la última línea.

Como vemos en la imagen anterior, parece que las shells son válidas, vamos a hacer la prueba

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/10_01.png"/>

Vemos que nuestro sistema es vulnerable porque posee una versión afectada, por lo que podemos suponer que el test no ha detectado esta vulnerabilidad, parcheada en la mayoría de sistemas operativos actuales, o más adelante nos mostrará, dentro de la correciones, cómo parchear esta vulnerabilidad. En cualquier caso, nos sirve para recordar que el programa no es perfecto y que nos bueno confiarse en la seguridad de los sistemas sólo en base a una auditoría genérica.

Continuacon el sistema de archivos y su primer punto es revisar los puntos de montaje; donde se sugire que hay dos posibles mejoras en `home` y `tmp`. Comprueba si hay volúmenes lógicos (LVM), con el comando `lvscan` podemos ver estos volúmenes y si están activos

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/10_02.png"/>

Lo siguiente es comprobar si hay archivos viejos en `/tmp` y si este punto tiene el `sticky bit`; es uno de los permisos especiales de acceso utilizado en ficheros y directorios del entorno Unix y derivados como GNU/Linux. Su objetivo es que solo el usuario creador pueda eliminar o renombrar un archivo en sistemas donde todos los usuarios tienen permisos de lectura y escritura. Además, su activación está representada con la letra T mayúscula en el listado de permisos de un directorio. Originalmente, este permiso solía utilizarse en dispositivos con poca RAM sobre ficheros ejecutables para indicarle al sistema operativo que debía mantener un determinado programa en SWAP para las siguientes ejecuciones, incluso si estas las realizaban otros usuarios. Sin embargo, debido a la actualización y mejora de los sistemas de almacenamiento, esta labor dejó de ser necesaria, por lo que, en la actualidad, las funciones de Sticky Bit están enfocadas en los directorios y es comúnmente ejecutado sobre  `/tmp` y `/var/tmp`.

Finalizando, dentro del sistema de archivos, comprueba si están habilitadas las `ACL` (Listas te control de Acceso) en el sistema de archivos raíz y si `Locate` tiene una base de datos; es una utilidad de Unix que sirve para encontrar archivos en sistemas de archivos. Busca a través de una base de datos preconstruida de archivos generada por el comando `updatedb` o por un demonio y comprimida mediante codificación incremental. Funciona mucho más rápido que find, pero requiere la actualización periódica de la base de datos.  

Acabando con esta sección, se recae en el almacenamiento, concretamente en si están los módulos de "USB" y "Firewire" activados:

- **Almacenamiento USB**: El programa `modprobe` utilizado para la carga automática de módulos del kernel está configurado para que cargue el controlador de almacenamiento USB. Si estuviera quitado, evitaría que el programa cargue el módulo `usb-storage`, pero no evitará que `root` (u otro programa con privilegios) lo utilice para cargar el módulo manualmente. No es raro que las memorias USB se utilicen para transportar y transmitir malware y virus destructivos a los ordenadores; dependerá de tus estrategias de seguridad si te conviene o no tener activo este módulo.

- **Controlador FireWire**: El puerto `FireWire` es un tipo de conexión para conectar en tiempo real diferentes tipos de dispositivo digital, desde ordenadores hasta discos duros o cámaras digitales; fue un competidor del `USB` y actualmente se encuentra en desuso. En este caso está deshabilitado.




