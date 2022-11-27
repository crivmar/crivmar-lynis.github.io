---
layout: default
title: Resultados e implementación de mejoras
nav_order: 5
has_children: true
has_toc: false
---

# Resultados e implementación de mejoras

Tras todo lo mostrado en la anterior sección, es momento de ver los resultados del test que hemos realizado y las sugerencias dadas por el programa. En primer lugar muestra los apartados de "*Peligro*":

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/24.png"/>

Como vemos y hemos mencionado varias veces, la versión de `Lynis` que hemos usado está anticuada y además nos muestra que hay reglas de `iptables` que hay que revisar; esto es lo más peligroso que detecta y más adelante iremos tratando estos problemas. Luego muestra las sugerencias que podemos implementar para mejorar esta máquina, no son obligatorias y dependerá de las necesidades de cada entorno; en nuestro caso vamos a intentar hacer todas las que podamos:

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/25.png"/>


Y ya por último, nos muestra donde se guardan los archivos generados para su revisión:

- `~/auditoria1.log`, es el lugar donde indicamos en el comando inicial de ejecución que debía guardarse los resultado de los test. Los resultados se muestran en pantalla durante el análisis del sistema, además, los detalles adicionales se guardan en el archivo mencionado. Esta información es útil para los usuarios avanzados, para ver lo que el programa hizo en el fondo, o donde aparecieron anomalías. Información que se muestra en el archivo de registro:

- Hora de una acción/evento
    
- Razón(es) por la que una prueba ha fallado o se ha omitido
    
- Resultados de las pruebas (internas)
    
- Sugerencias sobre las opciones de configuración o cómo arreglar/mejorar.

Podemos ver un fragmento de este archivo, concretamente cuando escanea la versión de del núcleo de la máquina, reconoce si es monolítico o modular y comprueba los módulos cargados:

~~~
 ===---------------------------------------------------------------===
[10:22:51] Performing test ID KRNL-5695 (Determine Linux kernel version and release number)
[10:22:51] Result: found kernel release 3.13.0-24-generic
[10:22:51] Result: found kernel version #46-Ubuntu SMP Thu Apr 10 19:11:08 UTC 2014
[10:22:51] ===---------------------------------------------------------------===
[10:22:51] Performing test ID KRNL-5723 (Determining if Linux kernel is monolithic)
[10:22:51] Test: checking if kernel is monolithic or modular
[10:22:51] Result: Found modular kernel
[10:22:51] ===---------------------------------------------------------------===
[10:22:51] Performing test ID KRNL-5726 (Checking Linux loaded kernel modules)
[10:22:51] Loaded modules according lsmod:
[10:22:51] Loaded module: ablk_helper
[10:22:51] Loaded module: aesni_intel
[10:22:51] Loaded module: aes_x86_64
[10:22:51] Loaded module: aufs
[10:22:51] Loaded module: auth_rpcgss
[10:22:51] Loaded module: autofs4
[10:22:51] Loaded module: bridge
[10:22:51] Loaded module: crc32_pclmul
[10:22:51] Loaded module: crct10dif_pclmul
[10:22:51] Loaded module: cryptd
[10:22:51] Loaded module: dm_crypt
[10:22:51] Loaded module: e1000
[10:22:51] Loaded module: fscache
[10:22:51] Loaded module: gf128mul
[10:22:51] Loaded module: ghash_clmulni_intel
[10:22:51] Loaded module: glue_helper
[10:22:51] Loaded module: i2c_piix4
[10:22:51] Loaded module: ip6table_filter
[10:22:51] Loaded module: ip6_tables
[10:22:51] Loaded module: iptable_filter
[10:22:51] Loaded module: iptable_nat
[10:22:51] Loaded module: ip_tables
[10:22:51] Loaded module: ipt_MASQUERADE

...

~~~

- `/var/log/lynis-report.dat`, en este archivo, por defecto, es recogido los hallazgos y otros datos dados durante la auditoría. El archivo de informe también se puede utilizar para comparar los resultados del análisis del pasado con un análisis actual. `Lynis Enterprise` tiene muchas más posibilidades de mostrar los datos, incluyendo informes extendidos en varios formatos. Este archivo de registro se purga en cada pasada, por lo que es importante hacer una copia de respaldo o programar una rotación de los registros antes de volver a escanear el sistema.
Podemos ver su estructura, por ejemplo, toma los usuarios que hay en el sistema y los guarda junto a su `UID` y , además, observamos que se guardan las sugerencias dadas por el programa:

~~~
real_user[]=root,0
real_user[]=vagrant,900
real_user[]=leia_organa,1111
real_user[]=luke_skywalker,1112
real_user[]=han_solo,1113
real_user[]=artoo_detoo,1114
real_user[]=c_three_pio,1115
real_user[]=ben_kenobi,1116
real_user[]=darth_vader,1117
real_user[]=anakin_skywalker,1118
real_user[]=jarjar_binks,1119
real_user[]=lando_calrissian,1120
real_user[]=boba_fett,1121
real_user[]=jabba_hutt,1122
real_user[]=greedo,1123
real_user[]=chewbacca,1124
real_user[]=kylo_ren,1125
suggestion[]=AUTH-9262|Install a PAM module for password strength testing like pam_cracklib or pam_passwdqc|
suggestion[]=AUTH-9286|Configure password aging limits to enforce password changing on a regular base|
exception_event[]=AUTH-9328:01

...
~~~

Y al final nos da una puntuación de "endurecimiento" de esta máquina, vemos que tenemos un `64 sobre 100`; no es mucho, pero es mejor de lo que me esperaba.

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/26.png"/>

En los siguientes bloques de esta sección iremos viendo la implementación de posibles mejoras, si las consideráramos necesarias, en nuestro sistema.


### Contenido

- [Correción 1 - Peligros - Versión de lynis e Iptables]({{ https://crivmar.github.io/crivmar-lynis.github.io }}{% link docs/Resultados e implementación de mejoras/corr1.md %})