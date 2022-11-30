---
layout: default
title: Bloque 9 - SSH, SNMP y base de datos
parent: Segunda auditoría
nav_order: 9
---

## Bloque 9 - SSH, SNMP y base de datos

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/69.png"/>

En el apartado `SSH` nos encontramos muchas sugerencias, vamos una a una:

- `AllowTcpForwarding`, esta opción normalmente se utiliza para hacer que tu servidor sea un bastión frente a internet y de este dirija como "saltos" las conexiones a las máquinas de la red privada. En nuestro caso no hay red privada, por lo cuál deberíamos ponerlo en `/etc/ssh/sshd_config` como `NO`. Aque hay una herramienta llamada [SaSSHimi](https://www.tarlogic.com/es/blog/sasshimi-bypass-allowtcpforwarding/), que permite evadir esta configuración:

~~~
2022-11-29 09:23:11 ====
2022-11-29 09:23:11 Performing test ID SSH-7408 (Check SSH specific defined options)
2022-11-29 09:23:11 Test: Checking specific defined options in /tmp/lynis.bA5xSb129I
2022-11-29 09:23:11 Result: added additional options for OpenSSH 6.x and lower
2022-11-29 09:23:11 Test: Checking AllowTcpForwarding in /tmp/lynis.bA5xSb129I
2022-11-29 09:23:11 Result: Option AllowTcpForwarding found
2022-11-29 09:23:11 Result: Option AllowTcpForwarding value is YES
2022-11-29 09:23:11 Result: OpenSSH option AllowTcpForwarding is in a weak configuration state and should be fixed
2022-11-29 09:23:11 Suggestion: Consider hardening SSH configuration [test:SSH-7408] [details:AllowTcpForwarding (set YES to NO)] [solution:-]
2022-11-29 09:23:11 Hardening: assigned partial number of hardening points (0 of 3). Currently having 145 points (out of 192)

~~~

- `ClientAliveCountMax`, le indica cuantos mensajes deben de ser enviados antes de cerrar la sesión, este está relacionado con `ClientAliveInterval` que envía cada X segundos mensajes al cliente para ver si "está vivo". Nos aconseja pasar de 3 a 2 mensajes:

~~~
2022-11-29 09:23:11 Test: Checking ClientAliveCountMax in /tmp/lynis.bA5xSb129I
2022-11-29 09:23:11 Result: Option ClientAliveCountMax found
2022-11-29 09:23:11 Result: Option ClientAliveCountMax value is 3
2022-11-29 09:23:11 Result: OpenSSH option ClientAliveCountMax is configured reasonably
2022-11-29 09:23:11 Suggestion: Consider hardening SSH configuration [test:SSH-7408] [details:ClientAliveCountMax (set 3 to 2)] [solution:-]
2022-11-29 09:23:11 Hardening: assigned partial number of hardening points (1 of 3). Currently having 146 points (out of 195)

...
~~~

- `FingerprintHash`, Especifica el algoritmo utilizado al mostrar las huellas digitales de las claves (sólo opción `MD5` y `SHA256`). Esto sirve para cuando nos conectamos a un servidor SSH por primera vez obtenemos un mensaje que nos advierte de la identidad con su huella digital, si esta coincide con la del servidor al que nos vamos a conectar podemos descartar que sea un `Man in the Middle`. Habría que añadir esta opción al archivo de configuración:

~~~
2022-11-29 09:23:11 Test: Checking FingerprintHash in /tmp/lynis.bA5xSb129I
2022-11-29 09:23:11 Result: Option FingerprintHash not found in output

...
~~~

- `LogLevel`, opción que permite el registro de quienes acceden a esta máquina. Está en modo `INFO` que indica poco más que la Ip de la conexión, si lo cambiamos a `VERBOSE`, mostraría mucha más información:

~~~
2022-11-29 09:23:11 Test: Checking LogLevel in /tmp/lynis.bA5xSb129I
2022-11-29 09:23:11 Result: Option LogLevel found
2022-11-29 09:23:11 Result: Option LogLevel value is INFO
2022-11-29 09:23:11 Result: OpenSSH option LogLevel is configured reasonably
2022-11-29 09:23:11 Suggestion: Consider hardening SSH configuration [test:SSH-7408] [details:LogLevel (set INFO to VERBOSE)] [solution:-]

...
~~~

- `MaxAuthTries`, indica el número de veces que podemos equivocarnos al ingresar usuario y/o contraseña. El valor actual es 6, sugiere ponerlo a 3:

~~~
2022-11-29 09:23:11 Test: Checking MaxAuthTries in /tmp/lynis.bA5xSb129I
2022-11-29 09:23:11 Result: Option MaxAuthTries found
2022-11-29 09:23:11 Result: Option MaxAuthTries value is 6
2022-11-29 09:23:11 Result: OpenSSH option MaxAuthTries is configured reasonably
2022-11-29 09:23:11 Suggestion: Consider hardening SSH configuration [test:SSH-7408] [details:MaxAuthTries (set 6 to 3)] [solution:-]

...
~~~


- `MaxSessions`, define el número máximo de sesiones abiertas por conexión. Está en el valor por defecto que es 10, sugieren pasarla a 2:

~~~
2022-11-29 09:23:11 Test: Checking MaxSessions in /tmp/lynis.bA5xSb129I
2022-11-29 09:23:11 Result: Option MaxSessions found
2022-11-29 09:23:11 Result: Option MaxSessions value is 10
2022-11-29 09:23:11 Result: OpenSSH option MaxSessions is in a weak configuration state and should be fixed
2022-11-29 09:23:11 Suggestion: Consider hardening SSH configuration [test:SSH-7408] [details:MaxSessions (set 10 to 2)] [solution:-]

...
~~~


- `Port`, puerto por defecto por el cuál el servidor va a esperar las conexiones `SSH`, por defecto es el `TCP 22`. Aquí no nos da un valor óptimo porque es algo que queda al criterio del administrador y de su política de seguridad:

~~~
2022-11-29 09:23:11 Test: Checking Port in /tmp/lynis.bA5xSb129I
2022-11-29 09:23:11 Result: Option Port found
2022-11-29 09:23:11 Result: Option Port value is 22
2022-11-29 09:23:11 Result: OpenSSH option Port is in a weak configuration state and should be fixed
2022-11-29 09:23:11 Suggestion: Consider hardening SSH configuration [test:SSH-7408] [details:Port (set 22 to )] [solution:-]

...
~~~


- `TCPKeepAlive`, está opción no tiene sentido ya que si usas las de `ClientAlive*` estas no deben estar activas, además los mensajes generados por esta opción no está cifrados, por lo cuál se corre el riesgo de poder interceptarlos y/o falsificarlos. Como nos indican, el valor más óptimo es `NO`:

~~~
2022-11-29 09:23:11 Test: Checking TCPKeepAlive in /tmp/lynis.bA5xSb129I
2022-11-29 09:23:11 Result: Option TCPKeepAlive found
2022-11-29 09:23:11 Result: Option TCPKeepAlive value is YES
2022-11-29 09:23:11 Result: OpenSSH option TCPKeepAlive is in a weak configuration state and should be fixed
2022-11-29 09:23:11 Suggestion: Consider hardening SSH configuration [test:SSH-7408] [details:TCPKeepAlive (set YES to NO)] [solution:-]

...
~~~


- `UseDNS`, esta opción actualmente está en desuso ya que era para compatibilizar con instalaciones antiguas que usaban `rsh` (permite ejecutar comandos en ordenadores remotos, sin cifrar). El valor óptimo es `NO`:

~~~
2022-11-29 09:23:11 Test: Checking UseDNS in /tmp/lynis.bA5xSb129I
2022-11-29 09:23:11 Result: Option UseDNS found
2022-11-29 09:23:11 Result: Option UseDNS value is YES
2022-11-29 09:23:11 Result: OpenSSH option UseDNS is in a weak configuration state and should be fixed
2022-11-29 09:23:11 Suggestion: Consider hardening SSH configuration [test:SSH-7408] [details:UseDNS (set YES to NO)] [solution:-]

...
~~~


- `X11Forwarding`, permite ejecutar aplicaciones gráficas de una máquina remota exportando su "pantalla" a nuestro escritorio. Si tu máquina tiene una aplicación gráfica te hará falta esta opción, por lo cuál dependerá del caso. Evidentemente, si no lo usas, lo mejor es poner esta opción a `NO`:

~~~
2022-11-29 09:23:11 Test: Checking X11Forwarding in /tmp/lynis.bA5xSb129I
2022-11-29 09:23:11 Result: Option X11Forwarding found
2022-11-29 09:23:11 Result: Option X11Forwarding value is YES
2022-11-29 09:23:11 Result: OpenSSH option X11Forwarding is in a weak configuration state and should be fixed
2022-11-29 09:23:11 Suggestion: Consider hardening SSH configuration [test:SSH-7408] [details:X11Forwarding (set YES to NO)] [solution:-]

...
~~~

- `AllowAgentForwarding`, esto sirve para permitir el reenvío del agente `SSH` para que lea, por ejemplo, las llaves en tu máquina local en vez de dejarlas en el servidor. Esta opción también depende del administrador, porque si tienes una aplicación que se despliega en tu servidor desde un repositorio de `Github`, como ejemplo, te hace falta esta opción:

~~~
2022-11-29 09:23:11 Test: Checking AllowAgentForwarding in /tmp/lynis.bA5xSb129I
2022-11-29 09:23:11 Result: Option AllowAgentForwarding not found in output

...
~~~


- `Compression`, `SSH` puede utilizar algoritmos `gzip` para comprimir datos. Si existen vulnerabilidades en el software de compresión, podría resultar en un compromiso del sistema. La compresión se usa normalmente para quienes tienen conexiones lentas previas al `ADSL`, por lo cuál hoy en día no tiene mucho sentido. Si por algún motivo tiene que usarla, utiliza la opción `DELAYED` para asegurarse de que el usuario se autentifica primero antes de que comience la compresión:

~~~
2022-11-29 09:23:11 Test: Checking Compression in /tmp/lynis.bA5xSb129I
2022-11-29 09:23:11 Result: Option Compression found
2022-11-29 09:23:11 Result: Option Compression value is YES
2022-11-29 09:23:11 Result: OpenSSH option Compression is in a weak configuration state and should be fixed
2022-11-29 09:23:11 Suggestion: Consider hardening SSH configuration [test:SSH-7408] [details:Compression (set YES to (DELAYED|NO))] [solution:-]
...
~~~


- `UsePrivilegeSeparation`, separa los privilegios creando un proceso hijo sin privilegios para tratar el tráfico de red entrante. Después de la autenticación exitosa, se creará otro proceso que tiene el privilegio del usuario autenticado. Esto ayuda a prevenir la escalada de privilegios ya que puede contener a un atacante dentro del proceso sin privilegios. Por defecto, esto está configurado en sí, establecerlo en `sandbox` pone en marcha restricciones adicionales:

~~~
2022-11-29 09:23:11 Test: Checking UsePrivilegeSeparation in /tmp/lynis.bA5xSb129I
2022-11-29 09:23:11 Result: Option UsePrivilegeSeparation found
2022-11-29 09:23:11 Result: Option UsePrivilegeSeparation value is YES
2022-11-29 09:23:11 Result: OpenSSH option UsePrivilegeSeparation is configured reasonably
2022-11-29 09:23:11 Suggestion: Consider hardening SSH configuration [test:SSH-7408] [details:UsePrivileg
eSeparation (set YES to SANDBOX)] [solution:-]

...
~~~


### SNMP

No ha habido ningún cambio respecto a la primera auditoría:

~~~
2022-11-29 09:23:11 ====
2022-11-29 09:23:11 Action: Performing tests from category: SNMP Support
2022-11-29 09:23:11 ====
2022-11-29 09:23:11 Performing test ID SNMP-3302 (Check for running SNMP daemon)
2022-11-29 09:23:11 Test: Searching for a SNMP daemon
2022-11-29 09:23:11 Performing pgrep scan without uid
2022-11-29 09:23:11 IsRunning: process 'snmpd' not found
2022-11-29 09:23:11 Result: No running SNMP daemon found
2022-11-29 09:23:11 ====
2022-11-29 09:23:11 Skipped test SNMP-3304 (Check SNMP daemon file location)
2022-11-29 09:23:11 Reason to skip: Prerequisites not met (ie missing tool, other type of Linux distribution)
2022-11-29 09:23:11 ====

...
~~~

### Base de datos

Cuando ejecuté la auditoría no tenía el servicio de `MySQL` funcionando y por tanto no encuentra nada e ignora el resto de test. Más adelante rescataremos información de esto al hablar de información adicional sobre el uso de `Lynis`:

~~~
2022-11-29 09:23:11 ====
2022-11-29 09:23:11 Action: Performing tests from category: Databases
2022-11-29 09:23:11 ====
2022-11-29 09:23:11 Performing test ID DBS-1804 (Checking active MySQL process)
2022-11-29 09:23:11 Result: MySQL process not active
2022-11-29 09:23:11 ====
2022-11-29 09:23:11 Skipped test DBS-1816 (Checking MySQL root password)
2022-11-29 09:23:11 Reason to skip: MySQL not installed, or not running
2022-11-29 09:23:11 Test skipped, MySQL daemon not running or no MySQL client available
2022-11-29 09:23:11 ====
2022-11-29 09:23:11 Performing test ID DBS-1818 (Check status of MongoDB server)
2022-11-29 09:23:11 Performing pgrep scan without uid
2022-11-29 09:23:11 IsRunning: process 'mongod' not found
2022-11-29 09:23:11 ====
2022-11-29 09:23:11 Performing test ID DBS-1820 (Check for authorization in MongoDB)
2022-11-29 09:23:11 ====
2022-11-29 09:23:11 Performing test ID DBS-1826 (Checking active PostgreSQL processes)
2022-11-29 09:23:11 Performing pgrep scan without uid
2022-11-29 09:23:11 IsRunning: process 'postgres' not found
2022-11-29 09:23:11 Result: PostgreSQL process not active
2022-11-29 09:23:11 ====

...
~~~

