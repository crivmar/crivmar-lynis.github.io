---
layout: default
title: Instalación de Oracle Virtualbox y Vagrant
parent: Preparación del escenario
nav_order: 1
---

## Instalación de Oracle VirtualBox

**Oracle VM VirtualBox** (conocido generalmente como **VirtualBox**) es un software de virtualización para arquitecturas `x86/amd64`. Actualmente es desarrollado por `Oracle Corporation` como parte de su familia de productos de virtualización. 
Por medio de esta aplicación es posible instalar sistemas operativos adicionales, conocidos como "sistemas invitados", dentro de otro sistema operativo "anfitrión", cada uno con su propio ambiente virtual. Entre los sistemas operativos soportados para instalar este programa se encuentran `GNU/Linux`, `Mac OS X`, `OS/2 Warp`, `Genode`, `Windows` y `Solaris/OpenSolaris`, y es posible virtualizar una gran cantidad de sistemas operativos distintos.

Podemos instalar en nuestro sistema `Virtualbox` y sus paquetes de complementos con el siguiente comando:

~~~
sudo apt install -y virtualbox virtualbox-guest-additions-iso virtualbox-guest-utils
~~~


## Instalación de Vagrant

**Vagrant** es una herramienta para la creación y configuración de entornos de desarrollo virtualizados. Originalmente se desarrolló para `VirtualBox` y sistemas de configuración tales como `Chef`, `Salt` y `Puppet`. Sin embargo desde la versión `1.1` **Vagrant** es capaz de trabajar con múltiples proveedores, como `VMware`, `Amazon EC2`, `LXC`, `DigitalOcean`,`KVM` etc. 
Aunque Vagrant se ha desarrollado en Ruby se puede usar en multitud de proyectos escritos en otros lenguajes, tales como `PHP`, `Python`, `Java`, `C#` y `JavaScript`.

Para crear el entorno donde vamos a trabajar, instalamos el paquete homónimo:

~~~
sudo apt install -y vagrant 
~~~

`Vagrant`, como mencionamos antes, usa por defecto `VirtualBox` así que no necesitamos hacer nada adicional, con todo esto vamos a bajar la caja que más adelante usaremos. Esta es una imagen basada en `Ubuntu` de `Metasploitable3`.

~~~
vagrant box add rapid7/metasploitable3-ub1404 --provider=virtualbox
~~~

