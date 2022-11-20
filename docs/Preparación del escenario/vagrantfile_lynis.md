---
layout: default
title: Vagrantfile e instalación de Lynis
parent: Preparación del escenario
nav_order: 3
---

## Vagrantfile para máquina de pruebas

Vamos a hacer uso de un `Vagrantfile` para levantar la máquina y así empezar con el escenario. Para que tenga comunicación con la red, voy a indicar que haga un puente con mi tarjeta de red "WiFi". Averiguamos con `ip a` el nombre y lo añadimos a este fichero. 

~~~
Vagrant.configure("2") do |config|

# Configuración de la caja a usar y eliminación de fichero sincronizado

  config.vm.box = "rapid7/metasploitable3-ub1404"
  config.vm.synced_folder ".", "/vagrant", disabled: true
  
# Configuración específica de la máquina virtual

  config.vm.define "mt3" do |mt3|
    mt3.vm.hostname = "mt3"
    mt3.ssh.username = "vagrant"
    mt3.ssh.password = "vagrant"
    mt3.vm.network :public_network, bridge: "wlp4s0"
    mt3.vm.provider "virtualbox" do |v|
      v.cpus = 2
      v.memory = 2048
    end
  end
end 
~~~

**Hay que especificarle el usuario y contraseña `ssh` para que cuando cree la máquina inyecte la clave de `Vagrant`**


Una vez creado este fichero con esos parámetros, vamos a levantarlo con el siguiente comando, dentro del directorio donde lo hemos creado:

~~~
vagrant up
~~~


## Instalar Lynis en la máquina

Una vez creada la máquina, el proceso de instalar este programa es sencillo. Tal y como dice la [documentación oficial](https://cisofy.com/documentation/lynis/get-started/#installation) hay varios métodos:

- Haciendo uso de los repositorios de paquetes de las distintas distribuciones (el que vamos a usar nosotros).

- Clonando el repositorio de Github, para tener la versión más moderna.

- Descarga directa.

- Homebrew (para los que tengan `macOS`)


En nuestro caso es tan simple como primero ejecutar un `update` y luego:

~~~
sudo apt install lynis -y
~~~


Tras esto comprobamos su versión, recordando que para usar el programa hacen falta **privilegios de administrador**:

![](/assets/images/02.png)

Si vamos a su manual, nos mostrará las opciones que podemos hacer con unas pequeñas descripciones:

~~~
Lynis(8)                                                               Unix System Administrator's Manual                                                               Lynis(8)



NAME
        Lynis - Run an system and security audit on the system

SYNOPSIS
       lynis --check-all(-c) [other options]

DESCRIPTION
       Lynis  is  an auditing tool for Unix (specialists). It checks the system and software configuration and logs all the found information into a log file for debugging pur‐
       poses, and in a report file suitable to create fancy looking auditing reports.  Lynis can be run as a cronjob, or from the command line. It needs to have full access  to
       the system, so running it as root (or with sudo rights) is required.

       The following system areas may be checked:

              - Boot loader files

              - Configuration files

              - Common files by software packages

              - Directories and files related to logging and auditing

OPTIONS
       --auditor <full name>
              Define the name of the auditor/pen-tester. When a full name is used, add double quotes, like "Michael Boelen".


       --checkall (or -c)
              Lynis  performs  a  full  check  of  the system, printing out the results of each test to stdout. Additional information will be saved into a log file (default is
              /.

              In case the outcome of a scan needs to be automated, use the report file.

       --check-update (or --info)
              Show program, database and update information

       --cronjob
              Perform automatic scan with cron safe options (no colors, no questions, no breaks).

       --logfile </path/to/logfile>
              Defines location and name of log file, instead of default /var/log/lynis.log.

       --no-colors
              Do not use colors for messages, warnings and sections.

       --no-log
              Redirect all logging information to /dev/null, prevent sensitive information to be written to disk.

       --quick (-Q)
              Do a quick scan (don't wait for user input)

       --quiet (-q)
              Try to run as silent as possible, showing only warnings. This option activates --quick as well.

       --reverse-colors
              Optimize screen output for light backgrounds.

       --tests TEST-IDs
              Only run the specific test(s). When using multiple tests, add quotes around the line.

       --view-categories
              Display all available test categories

       Multiple parameters are allowed, though some parameters can only be used together with others. When running Lynis without any parameters, help will be shown and the pro‐
       gram will exit.

BUGS
       There are no known bugs. Bugs can be reported directly to author.

LICENSING
       Lynis is licensed under the GPL v3 license and under development by Michael Boelen.

CONTACT INFORMATION
       Project related questions and comments should be asked via http://www.rootkit.nl/contact/.



1.10                                                                            24 December 2013                                                                        Lynis(8)

~~~
