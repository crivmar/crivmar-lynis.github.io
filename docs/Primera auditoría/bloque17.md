---
layout: default
title: Bloque 17 - Criptografía, virtualización y esquemas de seguridad
parent: Primera auditoría
nav_order: 17
---

## Bloque 17 - Criptografía, virtualización y esquemas de seguridad

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/20.png"/>

El inicio es con criptografía, concretamente el certificado `SSL`. El `SSL` (*Secure Socket Layer*) es una tecnología de seguridad estandarizada para establecer una conexión cifrada entre un servidor y un cliente (generalmente servidor web y navegador, pero también pueden ser otros servicios). Los certificados contienen un par de claves: una pública y otra privada, además, también contiene lo que se denomina "sujeto", que es la identidad del propietario del mismo.
La comprobación del certificado la da como buena y este sistema no tiene un certificado `SSL`, esto puede deberse un error o a que el hecho de no encontrarlo lo da como válido.

En cuanto a la virtualización, no muestra nada. Esto es una máquina virtualizada y si ejecutamos el siguiente comando, no aparecerá nada:

~~~
cat /proc/cpuinfo | egrep '(vmx|svm)'


vmx -> para procesadores Intel.
svm -> para procesadores AMD.
~~~

`Virtualbox` añade esta función desde la versión 6.1 (que es la que nosotros tenemos) pero no tenemos activada esta función, por lo cuál no va a simular que nuestro sistema pueda virtualizar y por tanto no lo detecta. Si quieres saber más acerca de esta función, dejo este [enlace](https://redessy.com/como-habilitar-la-virtualizacion-anidada-en-virtualbox-en-linux/).

Para terminar este bloque, vamos a los esquemas de seguridad:

- **AppArmor**, habilitado, es un sistema de control obligatorio de acceso (*Mandatory Access Control* o *MAC*) basado en la interfaz `LSM` (*Linux Security Modules*). En la práctica, el núcleo pregunta a `AppArmor` antes de cada llamada del sistema para saber si un proceso está autorizado a realizar dicha operación. A través de este mecanismo, `Apparmor` confina un programa a un conjunto limitado de recursos. `AppArmor` aplica un conjunto de reglas (*perfil*) a cada programa, estas no dependen del usuario y estas se ejecutan para un determinado programa (aunque pueden dar comportamientos distintos, dependiendo de los privilegios del usuario). Los perfiles se guardan en `/etc/apparmor.d`, cada perfil se puede cargar bien en modo estrícto (`enforcing`) o bien en modo relajado (`complaining`). El modo **estricto** aplica las reglas y registra las tentativas de violación, mientras que en el modo **relajado** sólo se registran las llamadas al sistema que hubieran sido bloqueadas, pero no se bloquean realmente. 

- **SELinux**, no encontrado, al igual que el anterior es un sistema de control obligatorio de accesos, basado en la misma interfaz. Utiliza una serie de reglas (*policy*) para autorizar o denegar operaciones; es complejo de configurar. Se basa en el concepto de un "contexto" de seguridad, que se puede asignar a cualquier cosa dentro de un sistema Linux, incluidos puertos, directorios, archivos y cuentas de usuario; además es posible asignar diferentes contextos de seguridad a diferentes tipos de objetos. Solamente impondrá las políticas de seguridad cuando se habilita, es decir, `SELinux` también puede arrancarse en modo **permisivo**, pero de esta manera no se aplica ninguna restricción. Este es muy útil para depurar los permisos que requieren las aplicaciones antes de activar el servicio, pues los errores de acceso quedarán almacenados en el registro del sistema.

- **Grsecurity**, no encontrado, son un conjunto de parches que hasta hace poco se ofrecían de forma gratuita y que están destinados a endurecer la seguridad del kernel, a través del control de acceso inteligente, la prevención de vulnerabilidades basadas en la corrupción de la memoria, y una serie de otros endurecimientos del sistema que generalmente no requieren configuración.

- **MAC**, implementado y mencionado en los dos primeros gestores de acceso de este apartado. 

### MAC vs DAC, dos enfoques de control de acceso

La diferencia entre `MAC` y `DAC` es que difieren en el nivel de restricciones y reglas que se establecen para limitar el acceso a los datos. `MAC` permite establecer diferentes niveles de acceso y restricciones individualmente para todos y cada uno de los usuarios. El usuario obtiene acceso a todos los datos que se encuentran bajo su nivel de acceso, pero proporciona mayor nivel de seguridad ya que es el administrador el que controla estos permisos.

`DAC` es fácil de usar ya que permite un acceso rápido a datos y archivos de otros usuarios. Es bastante flexible, ya que todos los usuarios tienen el privilegio de acceder y modificar los datos y las políticas de restricción de otros, esto va en detrimento de la seguridad.


