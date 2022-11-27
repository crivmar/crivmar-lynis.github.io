---
layout: default
title: Correción 1 - Peligros - Versión de Lynis e Iptables
parent: Resultados e implementación de mejoras
nav_order: 1
---

## Correción 1 - Peligros - Versión de Lynis e Iptables

Lo primero que vamos a hacer es usar la versión más reciente de `Lynis`, como lo instalamos usando el comando `apt`, vamos a desintalarlo primero; antes haremos una copia del archivo `/var/log/lynis-report.dat` para mantenerlo para futura referencia:

~~~
mkdir auditoria1_datos && cd auditoria1_datos

sudo cp /var/log/lynis-report.dat ./

# Movemos el archivo de auditoria1.log a esta carpeta

mv ../auditoria1.log ./
~~~

Ya teniendo estos archivos en una carpeta aparte, procedemos a desinstalar con `apt` el programa:

~~~
sudo apt remove -y lynis --purge
~~~

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/27.png"/>

Como vamos a instalar desde el repositorio de `Github`, vamos a descargar el paquete de la herramienta `git`

~~~
sudo apt install -y git
~~~

Ahora nos vamos a la carpeta `/usr/local` y clonamos el repositorio por [HTTPS](https://github.com/CISOfy/lynis.git)

~~~
sudo git clone https://github.com/CISOfy/lynis.git
~~~

Tras movernos a la carpeta del repositorio, vemos que está el ejecutrable `lynis`, el cuál usaremos para ejecutar el programa de la siguiente manera:

~~~
./lynis [comandos]
~~~

Vamos a ver si tenemos una versión más reciente:

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/28.png"/>


Cuando instalamos por `apt` la versión de `Lynis` era la `1.3.9` y ahora tenemos la `3.0.8`, por lo que ya hemos hecho un gran cambio de los que nos resaltaban como "críticos". Ahora es el momento de `Iptables`, nos mostraba varios números de reglas que pudieran ser innecesarias; veremos el número de estas junto a la regla de la siguiente manera:

~~~
sudo iptables -L --line-numbers
~~~

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/29.png"/>

Como vemos hay un gran número de reglas afectadas, pero lo más indicativo es que la política por defecto es `ACCEPT` y eso puede ser peligroso de cara a la seguiridad este sistema en internet. Comenzaremos cambiando las políticas por defecto a que sean todas "DROP", es decir, que rechazen todo tráfico que no esté autorizado y vamos a crear unas a medida de nuestras necesidades:

~~~
## Cambiamos su ruta por defecto para que siempre vaya por la interfaz eth1

sudo ip r del default
sudo ip r add default via X.X.X.X


## Limpiar las reglas anteriores


~~~