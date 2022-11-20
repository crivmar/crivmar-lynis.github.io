---
layout: default
title: Comparación con otras herramientas
nav_order: 2
parent: Introducción
has_toc: false
---

# Comparación con otras herramientas

Esta consideración es muy subjetiva y depende de las necesidades de tu sistema, ya que hay entornos que no requieren que sea necesario una protección tan fuerte y otros que necesitan ser entornos más controlados. A continuación se van a mencionar algunos ejemplos y posibles ventajas que puede obtener la herramienta que estamos estudiando.


### Bastille

Es una herramienta de endurecimiento (`hardening`) para sistemas operativos Linux, durante mucho tiempo fue muy conocida y trabaja de manera automática cuando se ejecuta. Esta herramienta ejecuta unos ajustes prefijados y esto puede llegar a dar una falsa sensación de seguridad, ya que no audita más allá de los parámetros fijados.

Ventajas de `Lynis` frente a `Bastille`:

* Soporta más sistemas operativos
* Menor riesgo de romper el sistema por acciones automáticas que ejecuta `Bastille`.
* Auditoría de más profundidad.


### OpenVAS y Nessus

Estos programas se centran principalmente en la búsqueda de vulnerabilidades. Lo hacen a través de la red buscando los servicios detectables y de manera opcional, registran un sistema y recopila datos.

**Lynis** se ejecuta en la propia máquina, por tanto puede realizar análisis más profundos en comparación con los escaneos que llegan desde la red. Esto significa menor riesgo en tus servicios de producción y los archivos de registro de estos permanecen limpios de intentos de conexión y solicitudes incorrectas.

Si bien **Lynis** es una herramienta de auditoría, también añade la funcionalidad de descubrir vulnerabilidades, por lo cual nos dará mayor funcionalidad frente al resto. Además, **Nessus** es un programa propietario y tiene muchas limitaciones hasta que no te haces con una licencia de pago.

Ventajas de `Lynis` frente a `OpenVAS` y `Nessus` :

* Más rápido, ya que se ejecuta en el propio sistema.
* No hay contaminación de los archivos de registro.
* Los escaneos dentro de la propia máquina realiza auditorías más profundas.
* Menor riesgo de interrumpir tus servicios en producción.



### Tiger

`Tiger` es una colección gratuita y de código abierto de `scripts shell` para auditorías de seguridad y detección de intrusiones en la máquina, para sistemas tipo `Unix` como `Linux`. Es un comprobador de seguridad escrito enteramente en lenguaje `shell` y emplea varias herramientas `POSIX`. Su propósito principal es comprobar la configuración y el estado del sistema.

Es muy extensible que las otras herramientas de seguridad, y tiene un buen archivo de configuración. Analiza los archivos de configuración del sistema, los sistemas de archivos y los archivos de configuración del usuario en busca de posibles problemas de seguridad e informa sobre lo detectado.

Es similar a `Lynis`, como vemos, pero la mayor diferencia es que la herramienta propuesta sigue en mantenimiento y soporta nuevas tecnologías, `Tiger` no.