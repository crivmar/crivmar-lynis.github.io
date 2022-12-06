---
layout: default
title: Conclusiones
nav_order: 8
has_children: false
has_toc: false
---

# Conclusiones

Desde el principio abordé el proyecto con la idea, siempre en mente, de que me adentraba en un mundo que desconocía totalmente. La ciberseguridad es una rama muy vasta y compleja dentro del mundo de la informática, cosa que para un mero principiante de la administración de sistema tiene el paralelismo de "David contra Goliat".

Entonces, al comenzar a trabajar sobre el proyecto me di cuenta de que la dificultad no radicaba en el manejo del programa en sí, si no en lo que te mostraba y la información que te daba. Decidí darle un enfoque de entender los datos que nos revelaba la auditoría y porque era importante revisar esos puntos en los cuáles nos hacía hincapié.

A medida que se ha desarrollado este proyecto, hemos visto hay mucha información y en algunas he podido profundizar más que en otras, siempre con un criterio lo más objetivo posible de lo que era más relevante para una persona que no tiene conocimientos suficientes para abordar una auditoría de un sistema. Esto ha planteado mucha búsqueda de información, recordar conceptos que durante la formación han sido inculcados y otros que por falta de conocimientos previos no conocía. Todo esto ha resultado en este trabajo, que más que una auditoría y manejo de una herramienta, ha resultado en un "*entiende a tu sistema*"; cosa que evidentemente se aleja del objetivo principal fijado por mí mismo.

A nivel subjetivo creo que se ha cumplido con el objetivo de aprender sobre el contenido que te arroja esta herramienta y como con pinceladas sobre este, abre la posibilidad al estudio de conceptos más avanzados de configuraciones de tu sistema y los servicios que pueden ofrecer. Si bien, a criterio propio, el trabajo podría ser más detallado y profuso, había que luchar con el factor tiempo y la inexperiencia de lo que se estaba realizando. 


La consecuencia más directa de esto nos lleva a la misma idea inicial de hacer el proyecto, eligiendo una herramienta, poco conocida para mí, como es `Metasploitable3`. Es una imagen basada en una versión de `Ubuntu` anticuada y sin soporte, aunque pudiera darse la posibilidad que un escenario real fuera así de "anticuado". A medida que lo iba desarrollando, esperaba que las evidencias de las vulnerabilidades fueran patentes y no ha sido un caso tan claro; esta herramienta está pensada para que lo explote otra llamada `Metasploit`y aunque, muchas cosas las ha detectado `Lynis`, hay otros aspectos que no tiene en cuenta porque son específicos para la otra herramienta.

A medida que iba viendo los resultados y trabajando en las correcciones, me iba dando cuenta que no era necesario tener una máquina en sí plagada de vulnerabilidades. Hubiera bastado, simplemente, con una configuración básica de una distribución como `Debian` y de algunos servicios para aprender en igual o mayor medida sobre los resultados obtenidos. También ha servido para tener la consideración de que `Lynis` no es perfecto, que hay cosas que ignora, cosas que no profundiza tanto y que su mayor fortaleza recae en la personalización de sus pruebas realizadas por una administrador de sistemas experiencia y altos conocimientos en `shell script`. También ha servido para introducir un poco más mis conocimientos en una materia tan necesaria, muy especializada y crítica como es la seguridad. Hay muchas vulnerabilidades, cada una de ellas ataca o afecta a distintos vectores de una sistema en el cual influyen todo tipo de variables como la versión del mismo, modularidad de su núcleo, servicios habilitados, configuraciones de los mismos, permisos, etc.

Por otro lado, la documentación sobre esta herramienta no es precisamente muy profusa o explicativa y esto puede deberse a intereses comerciales (`CISOfy`, no deja de ser una empresa) y, creo, también porque se da por supuesto mucho conocimiento previo. Esto ha dado dificultad a la hora de comprender y a dar muchas vueltas sobre los conceptos que, para mí, no estaban claros. Pese a ello, he podido llegar a la conclusión de que `Lynis` sirve para auditar un sistema, pero que debes poner mucho de tu criterio y conocimiento propio para ajustar y afinar la herramienta con tus intereses y no caer simplemente en la búsqueda en un número alto de "endurecimiento" de tu sistema, porque quizás el tiempo y el esfuerzo que requiere no merece la pena para el uso que va a tener.





