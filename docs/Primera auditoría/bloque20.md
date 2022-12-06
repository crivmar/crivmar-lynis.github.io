---
layout: default
title: Bloque 20 - Endurecimiento del Kernel, endurecimiento del sistema y test personalizados
parent: Primera auditoría
nav_order: 20
---

## Bloque 20 - Endurecimiento del Kernel, endurecimiento del sistema y test personalizados


<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/23.png"/>


Aquí vemos como `Lynis` compara una lista de valores por defecto de las mejores prácticas para `sysctl`. Da una sugerencia relacionada cuando el valor real en el sistema es diferente al valor esperado; esto no significa expresamente que el sistema esté débilmente configurado. Depende del objetivo principal del sistema, y su enfoque. Por ejemplo, un sistema con la función de ser un router tendrá que ser ajustado más agresivamente cuando se trata de la pila de red.

Vamos a comprobar las partes que son "diferentes", según los test que realiza y veamos a qué hace referencia:

- `kernel.core_uses_pid`, comprueba si los volcados de núcleo añadirán el `PID` (*Process ID*) al nombre del archivo, útil para depurar aplicaciones multihilo. El valor esperado es `1` (habilitado), vemos qué indica nuestro sistema con el siguiente comando:

~~~
sudo /sbin/sysctl -a | grep [nombre_del_modulo]
~~~

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/23_01.png"/>


- `kernel.sysrq`, activa el uso de la tecla `SysRQ` (la misma que usamos para imprimir pantalla), algo que cada vez está más en desuso, pero el mejor uso que se le puede dar es  para guardar todos los datos a disco cuando no tenemos ya control del equipo. Se usa una regla mnemotécnica en inglés:

**R**eboot **E**ven **I**f **S**ystem **U**tterly **B**roken (Reiniciar incluso si el equipo está terriblemente roto).

Esta regla nos indica que podemos ejecutar SysRQ con las letras R, E, I, S, U, B para:

~~~
R – Recuperar el control del teclado

E – Terminar todos los procesos, enviando una señal SIGTERM que les permita finalizar de forma pacífica
    
I – Enviar una señal SIGKILL a aquellos procesos que no hayan parado con la señal SIGTERM anterior
    
S – Sincronizar los buffers a disco

U – Remontar los filesystems en modo solo lectura.

B – Reiniciar el equipo
~~~

El valor esperado por el test es `0` (deshabilitado) y vemos qué tenemos en nuestro sistema de pruebas:

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/23_02.png"/>

Devuelve un valor `176`, esto es la suma que varía en funcionalidad según la siguiente lista:

~~~
0 - deshabilitar sysrq completamente.

1 - habilitar todas las funciones de sysrq.

2 - habilitar el control del nivel de registro de la consola.

4 - habilita el control del teclado (SAK, unraw).

8 - habilitar los volcados de depuración de procesos, etc.

16 - habilitar el comando sync.

32 - habilitar el remontado de sistemas de archivos en modo sólo lectura.

64 - permitir la señalización de procesos (term, kill, oom-kill).

128 - permitir el reinicio/apagado.

256 - permite la señalización de todas las tareas en tiempo real.
~~~

Éste se obtiene de la suma de `128` (que permite el reinicio y el apagado), más `32` (capacidad de remontar sistemas de archivos en modo de sólo lectura)  y más `16` (habilita el comando `sync`). 


- `net.ipv4.conf.all.forwarding`, este es similar al que habilitamos en `/etc/sysctl.conf` para permitir que el tráfico pueda atravesar nuestra máquina. Este parámetro establecerá el reenvío de paquetes `IPv4` en todas las interfaces sin afectar a otras configuraciones y el que usamos habitualmente `net.ipv4.ip_forward` afecta a otras configuraciones de este archivo. El valor recomendado es `0` y vamos a cotejarlo con el valor de nuestro sistema; podemos comprobar también con el siguiente comando:

~~~
sysctl net.ipv4.conf.all.forwarding
~~~

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/23_03.png"/>


- `net.ipv4.conf.all.log_martians`, la activación de esta función y el registro de los *paquetes marcianos* permite a un administrador investigar la posibilidad de que un atacante esté enviando paquetes falsos a su servidor. El valor recomendado es `1`:

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/23_04.png"/>


- `net.ipv4.conf.all.send_redirects`, deshabilitado evita que se mande paquetes `ICMP` con redirecciones. En nuestro sistema está, presumiblemente, habilitado:

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/23_05.png"/>


- `net.ipv4.conf.default.accept_redirects`, estando su valor a `0` como nos sugiere, evita que se acepten redirecciones `ICMP` para prevenir ataques *Man on the middle*:

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/23_06.png"/>


- `net.ipv4.conf.default.accept_source_route`, el enrutamiento en origen es un mecanismo del protocolo de internet que permite que un paquete `IP` lleve información, una lista de direcciones, que indica a un enrutador la ruta que debe seguir el paquete. También existe la opción de registrar los saltos a medida que se recorre la ruta. La lista de saltos realizados, el "registro de ruta", proporciona al destino una ruta de retorno al origen. Esto permite a la fuente (remitente) especificar la ruta, de manera flexible o estricta, ignorando las tablas de enrutamiento de algunos o todos los routers. Habilitado puede permitir a un usuario redirigir el tráfico de la red con fines maliciosos. En este sistema está habilitado.

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/23_07.png"/>


- `net.ipv4.conf.default.log_martians`, al igual que el anterior pero especificado como valor por defecto para todas las configuraciones.

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/23_08.png"/>


- `net.ipv4.tcp_timestamps`, recomendado desactivar las marcas de tiempo para reducir los picos de rendimiento relacionados con la generación de las mismas. Además cuanta menos información esté disponible para un atacante, mejor para nuestra seguridad.

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/23_09.png"/>


- `net.ipv6.conf.all.accept_redirects`, los mensajes de redirección `ICMP` son paquetes que transmiten información de enrutamiento y le indican a su anfitrión (que actúa como un enrutador) que envíe los paquetes a través de una ruta alternativa. Así se permite que las tablas de enrutamiento se actualicen. El valor `0` no aceptará ningún mensaje y por tanto no permitirá que un dispositivo externo actualice estas tablas.

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/23_10.png"/>


- `net.ipv6.conf.default.accept_redirects`, igual que el anterior.

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/23_11.png"/>


Vamos a la siguiente parte el endurecimiento del sistema, empezando con el hallazgo de un compilador de código. Los compiladores se utilizan para traducir el código fuente a lenguaje de máquina. Este proceso se realiza normalmente durante el ciclo de desarrollo, o por los mantenedores de paquetes. Aunque muchos sistemas tienen un compilador instalado, a menudo no se utiliza. Esto supone un riesgo innecesario. Los atacantes podrían abusar de la presencia de un compilador para hacer funcionar sus ataques maliciosos. Debido a las protecciones del `kernel` de `Linux`, la asignación de memoria y la ejecución de procesos, está limitada; para eludir estas protecciones, se necesita un compilador en el sistema, para que el atacante pueda determinar ubicaciones de memoria específicas y aprovechar una supuesta vulnerabilidad. 
Tampoco tenemos escáner de programas maliciosos, como vimos en un apartado anterior y eso deja nuestro equipo más expuesto.

La última parte sería la dedicada a test personalizados que puedes crear, ya que está escrito en `shell script`. Podemos encontrar en su [Github](https://github.com/CISOfy/lynis/blob/master/include/tests_custom.template) una plantilla para que implementes las pruebas personalizadas que quieras en tu sistema.

---

Información adicional
{: .label .label-blue}

**`sysctl` -> es usado para visualizar, configurar y automatizar configuraciones del kernel en el directorio `/proc/sys/`.** 

**PID -> es el identificador de un proceso, un numero que identifica de manera inequivoca a un proceso en el sistema.**

**Paquetes marcianos -> es un paquete con una dirección de origen que es obviamente incorrecta, nada podría ser enviado de vuelta a esa dirección. Un ejemplo sería que se descubriera un paquete en Internet con una dirección de origen privada `192.168.0.1`.**

**Man in the middle -> es un tipo de ciberataque en el que los criminales interceptan una conversación o una transferencia de datos existente, ya sea escuchando o haciéndose pasar por un participante.**