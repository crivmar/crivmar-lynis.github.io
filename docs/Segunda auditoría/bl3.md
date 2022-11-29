---
layout: default
title: Bloque 3 -  Usuarios, grupos y autenticación
parent: Segunda auditoría
nav_order: 3
---

## Bloque 3 - Usuarios, grupos y autenticación

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/56.png"/>

Este apartado está reliacionado con el **Bloque 6** de la sección **Primera auditoría**. Aquí empieza de la misma forma, hasta que llega a a **métodos de hasing de contraseñas**, dónde aparece una sugerencia. Veamos que dice `auditoria2.log`:

~~~
2022-11-29 09:22:54 ====
2022-11-29 09:22:54 Performing test ID AUTH-9229 (Check password hashing methods)
2022-11-29 09:22:54 Test: Checking password hashing methods
2022-11-29 09:22:54 Result: poor password hashing methods found: md5crypt sha256crypt/sha512crypt(default=5000rounds) 
2022-11-29 09:22:54 Suggestion: Check PAM configuration, add rounds if applicable and expire passwords to encrypt with new values [test:AUTH-9229] [details:-] [solution:-]
2022-11-29 09:22:54 Hardening: assigned partial number of hardening points (0 of 2). Currently having 15 points (out of 23)

...
~~~

Si vamos al archivo `/etc/login.def` veremos cuál es el algoritmo de cifrado de las contraseñas:

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/57.png"/>

Vemos que es `SHA512`, que es por defecto el que se usa para cifrar las contraseñas con `passwd`, pero en este caso parece que se refiere al número de rondas y no al algoritmo en sí. Las rondas ayudan a fortaleces una clave, por ejemplo, el valor que tenemos es de 5000, esto quiere decir que un atacante tiene que calcular 5000 `hashes` para cada contraseña que pruebe contra el `hash` de `/etc/shadow`; también significa que cada vez que te conectas el ordenador debe hacer ese mismo cálculo para validar tu contraseña. Incluso si pusieramos un número de rondas mayor a 65000 un ordenador suele tardar menos de un segundo, por lo que a mayor número de rondas pongas, más difícil será calcular tu contraseña para un atacante y más recursos requerirá tu ordenador para validarla.

Como tenemos el módulo `PAM` esto se administra ahí e inmediatamente debajo nos dice que las rondas están desahabilitadas (también se puede configurar en el otro archivo). Por lo que habría que ir a `/etc/pam.d/passwd` y añadir algo así:

~~~
password	required	pam_unix.so sha512 shadow nullok rounds=XXXXX
~~~


Continuamos hasta llegar al "peligro" para los permisos del archivo `/etc/sudoers.d`, este directorio está pensado para que las aplicaciones alteren los privilegios de `sudo` después de instalarse. Vamos la información que nos aporta:

~~~
2022-11-29 09:22:54 ====
2022-11-29 09:22:54 Performing test ID AUTH-9252 (Check ownership and permissions for sudo configuration files)
2022-11-29 09:22:54 Test: checking drop-in directory (/etc/sudoers.d)
2022-11-29 09:22:54 Result: Found directory permissions: rwxr-xr-x and owner UID GID: 00
2022-11-29 09:22:54 Result: directory /etc/sudoers.d has possibly unsafe permissions
2022-11-29 09:22:54 Result: directory /etc/sudoers.d ownership OK
2022-11-29 09:22:54 Test: checking file (/etc/sudoers)
2022-11-29 09:22:54 Result: Found file permissions: r--r----- and owner UID GID: 00
2022-11-29 09:22:54 Result: file /etc/sudoers permissions OK
2022-11-29 09:22:54 Result: file /etc/sudoers ownership OK
2022-11-29 09:22:54 Test: checking file (/etc/sudoers.d/README)
2022-11-29 09:22:54 Result: Found file permissions: r--r----- and owner UID GID: 00
2022-11-29 09:22:54 Result: file /etc/sudoers.d/README permissions OK
2022-11-29 09:22:54 Result: file /etc/sudoers.d/README ownership OK

...
~~~

Vemos que sus permisos son `u=rwx,g=r-x,o=r-x`, habría que cambiarlos con `chmod` para que se ajustaran al de los demás. Más abajo encontramos que hay cuentas que no tienen tiempo de expiración, esto tiene sentido ya que nosotros sólo configuramos la del usuario `vagrant`:

~~~
2022-11-29 09:22:54 ====
2022-11-29 09:22:54 Performing test ID AUTH-9282 (Checking password protected account without expire date)
2022-11-29 09:22:54 Test: Checking Linux version and password expire date status
2022-11-29 09:22:54 Result: found one or more accounts without expire date set
2022-11-29 09:22:54 Account without expire date: leia_organa
2022-11-29 09:22:54 Account without expire date: luke_skywalker
2022-11-29 09:22:54 Account without expire date: han_solo
2022-11-29 09:22:54 Account without expire date: artoo_detoo
2022-11-29 09:22:54 Account without expire date: c_three_pio
2022-11-29 09:22:54 Account without expire date: ben_kenobi
2022-11-29 09:22:54 Account without expire date: darth_vader
2022-11-29 09:22:54 Account without expire date: anakin_skywalker
2022-11-29 09:22:54 Account without expire date: jarjar_binks
2022-11-29 09:22:54 Account without expire date: lando_calrissian
2022-11-29 09:22:54 Account without expire date: boba_fett
2022-11-29 09:22:54 Account without expire date: jabba_hutt
2022-11-29 09:22:54 Account without expire date: greedo
2022-11-29 09:22:54 Account without expire date: chewbacca
2022-11-29 09:22:54 Account without expire date: kylo_ren
2022-11-29 09:22:54 Suggestion: When possible set expire dates for all password protected accounts [test:AUTH-9282] [details:-] [solution:-]
2022-11-29 09:22:54 ====

...

~~~

Vemos que no hay una política por defecto para que las contraseñas expiren, en el apartado **Correción 3** de la sección **Resultados e implementación de mejoras** vimos como modificar esto para una cuenta como un comando. Esto también podemos hacerlo, especificando los valores deseados en `/etc/login.defs`:

<img src="https://raw.githubusercontent.com/crivmar/crivmar-lynis.github.io/main/assets/images/58.png"/>

Más abajo nos dice que el valor de `umask` en el archivo `/etc/profile` no se encuentra, pero este archivo nos indica lo siguiente, por lo que no tenemos que preocuparnos:

~~~
...

# The default umask is now handled by pam_umask.
# See pam_umask(8) and /etc/login.defs.

if [ -d /etc/profile.d ]; then
  for i in /etc/profile.d/*.sh; do
    if [ -r $i ]; then
      . $i
    fi
  done
  unset i
fi
~~~

Ya por último, que no salía en la anterior versión del programa es que está habilitado el registro de los fallos de acceso:

~~~
...

2022-11-29 09:22:54 Test: Checking FAILLOG_ENAB option in /etc/login.defs 
2022-11-29 09:22:54 Result: FAILLOG_ENAB is set to 'yes'
2022-11-29 09:22:54 Outcome: failed login attempts are logged in /var/log/faillog
2022-11-29 09:22:54 Hardening: assigned maximum number of hardening points for this item (3). Currently having 35 points (out of 47)
2022-11-29 09:22:54 ====
~~~


---

- **Hash -> Cifrado de las contraseñas tras tomar estas y aplicarles un determinado algoritmo y esta resulta en un `hash` de longitud fija o variable (dependiendo del alogritmo usado) y cuya función es aumentar la seguridad a la hora de guardar las contraseñas.**

- **comando sudo -> permite a los usuarios que no son root ejecutar otros comandos que normalmente requerirían privilegios de superusuario. El archivo `/etc/sudoers` le indica al sistema cómo manejar este comando.