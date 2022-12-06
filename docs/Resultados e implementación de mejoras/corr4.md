---
layout: default
title: Correción 4 - Sugerencias - Puntos de montaje, almacenamiento USB y paquetes
parent: Resultados e implementación de mejoras
nav_order: 4
---

## Corrección 4 - Sugerencias - Puntos de montaje, almacenamiento USB y paquetes

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/38.png"/>

Comenzamos creando dos discos duros de `VirtualBox` que vamos a agregar a nuestro entorno, [aquí](https://analisisyprogramacionoop.blogspot.com/2018/11/crear-nueva-unidad-disco-maquina-virtual.html) podemos encontrar como hacerlo. Una vez hecho esto listaremos los nuevos discos.

~~~
sudo lsblk
~~~

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/39.png"/>

Vamos a darle formato a dichos discos con `fdisk`, crearemos una nueva partición dejaremos todo por defecto:

~~~
sudo fdisk /dev/sdb

sudo fdisk /dev/sdc
~~~

Lo siguiente es darle formato a la partición, vamos a darle `ext4`.

~~~
sudo mkfs -t ext4 /dev/sdb1

sudo mkfs -t ext4 /dev/sdc1
~~~


<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/40.png"/>


Ahora vamos a hacer un par de carpetas donde vamos a montar temporalmente estos discos duros nuevos:

~~~
sudo mkdir /mnt/home_prueba /mnt/tmp_prueba

sudo mount /dev/sdb1 /mnt/home_prueba

sudo mount /dev/sdc1 /mnt/tmp_prueba
~~~


<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/41.png"/>

Apuntamos sus `UUID`, que son sus identificadores únicos y hacemos una copia de `/etc/fstab`

~~~
vagrant@mt3:~$ sudo blkid
/dev/sda1: UUID="ae0deea2-cc32-44c2-836d-e79494fe5910" TYPE="ext2" 
/dev/sda5: UUID="eRKv7b-Uh6n-ZZmC-gq9e-nruv-IiFY-23ryuE" TYPE="LVM2_member" 
/dev/sdb1: UUID="9dc561cd-7d94-4e4a-9870-9f8bcd5d393a" TYPE="ext4" 
/dev/sdc1: UUID="2cf2cfdc-eb40-4ae1-8aa4-41bdf0206fe2" TYPE="ext4" 
/dev/mapper/ubuntu--vg-root: UUID="e05f6a4d-16a4-45ed-999c-b5675500fc61" TYPE="ext4" 
/dev/mapper/ubuntu--vg-swap_1: UUID="cf7d347e-4bac-4a80-83f3-dda193585a2e" TYPE="swap" 


vagrant@mt3:~$ sudo cp /etc/fstab /etc/fstab.bk
vagrant@mt3:~$ 
~~~

Ahora, antes de tocar nada vamos a hacer una copia temporal de los datos a los nuevos puntos:

~~~
sudo cp -av /home/* /mnt/home_prueba

sudo cp -av /tmp/* /mnt/tmp_prueba
~~~

Editamos el archivo `fstab` para poner los nuevos puntos de montaje de `/home` y `/tmp` :

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/42.png"/>

Reiniciamos y si lo hemos hecho bien deberemos ver lo siguiente:

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/43.png"/>

Ya podríamos borrar las carpetas temporales con las copias que hicimos por si algo fallaba. Lo siguiente que vamos a hacer es deshabilitar el USB para los usuarios no privilegiados, para ello crearemos una lista negra con el controlador que carga el `Kernel` y permite leer estos dispositivos. Dentro de `/etc/modprobe.d/` creamos un fichero, si no está creado previamente, llamado `blacklist.conf`.

~~~
sudo nano /etc/modprobe.d/blacklist.conf

## Dentro añadimos lo siguiente y lo guardamos

blacklist usb-storage
~~~

Tras esto se debe volver a reiniciar el sistema para que funcione este cambio. Continuamos con la instalación de `apt-show-versions`, este analiza el fichero de estado de `dpkg` y las listas de `apt` en busca de las versiones de los paquetes instalados y disponibles así como de la distribución de estos, y muestra las opciones de actualización con la distribución específica del paquete seleccionado.

~~~
sudo apt install -y apt-show-versions
~~~

Y acabando este bloque, instalaremos una herramienta de auditoria de paquetes como mencionamos en el **Bloque 8** de la primera auditoria. Quería instalar `apt-listbugs`, pero éste fue removido de la paquetería de `Ubuntu` en 2008 por este [motivo](https://lists.ubuntu.com/archives/ubuntu-devel/2009-June/028332.html) y no he encontrado una alternativa para esta distribución.

