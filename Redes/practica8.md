# Práctica 8 - Capa de Red - Ruteo

### 2. En las redes IP el ruteo puede configurarse en forma estática o en forma dinámica. Indique ventajas y desventajas de cada método.

#### IP Estáticas

** Ventajas **

* Ofrecen conexiones más fiables y estables.
* Suelen permitir mayores velocidades de carga y descarga.
* Te dan un control exclusivo de tu IP. Nadie más puede usarla. Así evitas bloqueos o problemas por malos usos que no sean culpa tuya.
* Son ideales por ejemplo para jugadores online, proveedores o usuarios de telefonía y vídeo por Internet (VoIP) como Skype. También en servidores de todo tipo y servicios de alojamiento web. O en sistemas de redes privadas virtuales (VPN).

** Desventajas **

* En principio son menos seguras. Los hackers u otros atacantes tienen más tiempo y oportunidades para atacar equipos con IPs que sean siempre iguales. Obligan a tomar medidas de seguridad extra y más rigurosas.
* En general hay que pagar un precio adicional para tener una IP fija en Internet. Además no las ofrecen todos los proveedores de acceso. Tendrías que consultar al tuyo.
* Lo normal es que haya que configurarlas a mano. Quien las usa debe tener más conocimientos de informática.

#### IP Dinámicas

** Ventajas **

* No tienes que pagar más por ellas. La mayoría de los proveedores de Internet asignan IPs dinámicas a sus clientes como parte de su plan de acceso normal.
* En principio son más seguras. Es más díficil para un atacante rastrear y buscar debilidades en un equipo con una IP variable.
* Ofrecen mayor privacidad en Internet. A los sitios web les cuesta más rastrear lo que haces si tu IP va cambiando.
* Su configuración en la red suele ser automática. No necesitas hacerla tú. Se encarga de ello lo que se llama un servidor DHCP.
* Optimizan el uso de recursos y abaratan costos. Cuando un equipo se desconecta de Internet u otra red ya "no necesita" su IP. Así puede reutilizarse asignándosela a otro equipo que se conecte.

** Desventajas **

* Es más fácil que la conexión falle o se interrumpa.

---

### 3. Una máquina conectada a una red pero no a Internet, ¿tiene tabla de ruteo?

Sí, porque aunque sea una red local se utiliza la tabla de enrutamiento para comunicarse con los demás dispositivos de la red. Internet es una red con las mismas características que la red privada, simplemente que es pública. Además vimos que en la tabla de enrutamiento hay que tener una entrada por defecto (0.0.0.0) que devuelve a la interface conectada y una entrada para la red donde estamos conectado con el default gateway local (0.0.0.0).

---

### 5. Con la máquina virtual con acceso a Internet realice las siguientes observaciones respecto de la autoconfiguración IP vía DHCP:

#### b. En una terminal de root, ejecute el comando sudo /sbin/dhclient eth0 y analice el intercambio de paquetes capturado.

El intercambio de mensajes que se realiza cuando un cliente desea obtener una dirección IP a traves del servicio de DHCP, es de 4 vías, las cuales se detallan a continuación:

** Descubrimiento del servidor DHCP **

La primera tarea de un host recién llegado es encontrar un servidor DHCP con el que interactuar. Esto se hace mediante un mensaje de descubrimiento DHCP, que envía un cliente dentro de un paquete UDP al puerto 67. El paquete UDP se encapsula en un datagrama IP. Pero, ¿a quién debería enviarse este datagrama? El host ni siquiera conoce la dirección IP de la red a la que se está conectando, y mucho menos la dirección de un servidor DHCP de esa red. En esta situación, el cliente DHCP crea un datagrama IP que contiene su mensaje de descubrimiento DHCP junto con la dirección IP de difusión 255.255.255.255 y una dirección IP de origen de “este host” igual a 0.0.0.0. El cliente DHCP pasa el datagrama IP a la capa de enlace, la cual difunde esta trama a todos los nodos conectados a la subred (en la Sección 5.4 estudiaremos en detalle el proceso de difusión de la capa de enlace).

** Oferta(s) del servidor DHCP **

Un servidor DHCP que recibe un mensaje de descubrimiento DHCP responde al cliente con un mensaje de oferta DHCP, que se difunde a todos los nodos de la subred utilizando de nuevo la dirección IP de difusión 255.255.  255.255 (es posible que se pregunte por qué la respuesta de este servidor también debe difundirse). Puesto que en la subred pueden existir varios servidores DHCP, el cliente puede encontrarse en la situación envidiable de poder elegir entre varias ofertas. Cada mensaje de oferta de servidor contiene el ID de transacción del mensaje de descubrimiento recibido, la dirección IP propuesta para el cliente, la máscara de red y el tiempo de arrendamiento de la dirección IP (el tiempo durante el que la dirección IP será válida). Es habitual que el servidor defina un tiempo de arrendamiento de varias horas o días.

** Solicitud DHCP **

El cliente recién llegado seleccionará de entre las ofertas de servidor y responderá a la oferta seleccionada con un mensaje de solicitud DHCP, devolviendo los parámetros de configuración.

** ACK DHCP **

El servidor contesta al mensaje de solicitud DHCP con un mensaje ACK DHCP, que confirma los parámetros solicitados.

#### c. Analice la información registrada en el archivo /var/lib/dhcp/dhclient.leases, ¿cuál parece su función?

Contiene información cómo:

* Interfaz de red
* Dirección IP
* Máscara de subred
* Tiempo de alocamiento de la dirección IP
* Default gateway
* Identificador del servidor DHCP
* Dirección del servidor de DNS
* Time Stamp de actualización y expiración de la información obtenida

#### e. En una terminal de root, vuelva a ejecutar el comando sudo /sbin/dhclient eth0 y analice el intercambio de paquetes capturado nuevamente ¿a que se debió la diferencia con lo observado en el punto “b”?

Al borrar el archivo dhclient.leases, el nuevo request no solicitará la renovación de una dirección IP anteriormente obtenida.

---

### 6. ¿Qué es NAT y para qué sirve? De un ejemplo de su uso y analice cómo funcionaría en ese entorno.  Ayuda: analizar el servicio de Internet hogareño en el cual varios dispositivos usan Internet simultáneamente.

Todo dispositivo IP necesita una dirección IP. Con la proliferación de las subredes domésticas y de oficina pequeña (SOHO, Small Office, Home Office), podría parecer que esto implica asignar un bloque de direcciones enorme, por lo tanto, el ISP debería asignar un rango de direcciones para cubrir todas las máquinas de la red SOHO. Pero ¿Qué ocurre si el ISP ya ha asignado las porciones adyacentes al rango de direcciones actualmente en uso de la red SOHO?.  Afortunadamente, existe una forma más simple de asignar direcciones que ha encontrado un uso cada vez más amplio en escenarios de este tipo: la traducción de direcciones de red (NAT Network Address Translation).

Un router NAT no parece un router a ojos del mundo exterior. En su lugar, el router NAT se comporta de cara al exterior como un único dispositivo con una dirección IP única. Todo el tráfico que sale del router doméstico hacia Internet tiene una dirección IP de origen igual, y todo el tráfico que entra en él tienen que tener la misma dirección de destino única. En resumen, el router NAT oculta los detalles de la red doméstica al mundo exterior. (Como nota al margen, posiblemente se esté preguntando dónde obtienen las computadoras de la red doméstica sus direcciones y dónde obtiene el router su dirección IP única, A menudo, la respuesta a ambas preguntas es la misma: ¡DHCP! El router obtiene su dirección del servidor DHCP del ISP y el router ejecuta un servidor DHCP para proporcionar direcciones a las computadoras, dentro del espacio de direcciones de la red doméstica controlada por el router NAT-DHCP).

Si todos los datagramas que llegan al router NAT procedentes de la WAN tienen la misma dirección IP de destino (específicamente, la de interfaz WAN del router NAT), entonces ¿Cómo sabe el router a qué host interno debería reenviar un datagrama dado? El truco consiste en utilizar una tabla de traducciones NAT almacenada en el router NAT, e incluir los números de puerto, así como las direcciones IP en las entradas de la tabla.

Suponga que un usuario de la red doméstica que utiliza el host con la dirección 10.0.0.1 solicita una página web almacenada en un servidor web (puerto 80) con la dirección IP 128.119.40.186. El host 10.0.0.1, asigna el número de puerto de origen (arbitrario) 3345 y envía el datagrama a la LAN. El router NAT recibe el datagrama, genera un nuevo número de puerto de origen, 5001, para el datagrama, sustituye la dirección IP de origen por su dirección IP de la red WAN 138.76.29.7, y sustituye el número de puerto de origen original 3345 por el nuevo número de puerto de origen 5001. Al generar un nuevo número de puerto de origen, el router NAT puede seleccionar cualquier número de puerto de origen que actualmente no se encuentre en la tabla de traducciones NAT. (Observe que, puesto que la longitud del campo número de puerto es de 16 bits, el protocolo NAT puede dar soporte a 60.000 conexiones simultáneas utilizando la única dirección IP WAN del router.) En el router, NAT también añade una entrada a su tabla de traducciones. El servidor web, que afortunadamente no es consciente de que el datagrama entrante que contiene la solicitud HTTP ha sido manipulado por el router NAT, responde con un datagrama cuya dirección de destino es la dirección IP del router NAT y cuto número de puerto de destino es 5001. Cuando este datagrama llega al router NAT, éste indexa la tabla de traducciones NAT utilizando la dirección IP de destino (138.76.29.7) y el número de puerto de destino (5001) para obtener la dirección IP (10.0.0.1) y el número de puerto de destino (3345) apropiados para el navegador de la red doméstica. A continuación, el router reescribe la dirección de destino y el número de puerto de destino del datagrama y lo reenvía a la red doméstica.

---

### 7. ¿Qué especifica la RFC 1918 y cómo se relaciona con NAT?

Este documento especifica unas Mejores Prácticas Actuales, Best Current Practices (BCP), para la comunidad Internet, y solicita su discusión y sugerencias para mejorarlas.

Este documento describe la asignación de direcciones en las redes privadas. La asignación permite la completa conectividad de nivel de red entre todas las máquinas de la empresa, así como entre todas las máquinas públicas de diferentes empresas. El coste de usar un espacio privado de direcciones de Internet es el coste potencial del esfuerzo de reasignar las direcciones de las máquinas y redes de públicos a privados.

---

## Ejercicio de parcial 1

### 8. Asigne las redes que faltan utilizando los siguientes bloques y las consideraciones debajo:

* Red C y la Red D deben ser públicas.
* Los enlaces entre routers deben utilizar redes privadas.
* Se debe desperdiciar la menor cantidad de IP posibles.
* Si va a utilizar un bloque para dividir en subredes, asignar primero la red con más cantidad de hosts y luego las que tienen menos.
* Las redes elegidas deben ser válidas.

**Asignaciones:**

* Red D --> 200.30.55.64/27
* Red C --> 200.30.55.96/28
* Libre --> 200.30.55.112/28

* Red A --> 192.68.10.0/25
* Red B --> 192.68.10.128/26
* Libre --> 192.68.10.192/26

* Enlace izquierdo --> 192.168.10.192/29
* Enlace derecho --> 192.168.10.200/30
* Libre --> 192.168.10.204/30
* Libre --> 192.168.10.208/29
* Libre --> 192.168.10.216/29
* Libre --> 192.168.10.224/29
* Libre --> 192.168.10.232/29
* Libre --> 192.168.10.240/29
* Libre --> 192.168.10.248/29

### 9. Asigne IP a todas las interfaces de las redes listadas a continuación. Nota: Los routers deben tener asignadas las primeras IP de la red. Para enlaces entre routers, asignar en el siguiente orden: RouterA, RouterB, RouterC, RouterD y RouterE

* Red A, Red B, Red C y Red D.
* Red entre RouterA-RouterB-RouterE.
* Red entre RouterC-RouterD

![Punto 9](https://i.imgur.com/p41XOfQ.png)

### 10. Realice las tablas de rutas de RouterE y BORDER considerando:

* Siempre se deberá tomar la ruta más corta.
* Sumarizar siempre que sea posible.
* El tráfico de Internet a la Red D y viceversa debe atravesar el RouterC.
* Todos los hosts deben poder conectarse entre sí y a Internet.

**RouterE:**

* default via 10.10.10.6/30
* 192.68.10.128/26 via 192.168.10.124/29
* 192.68.10.0/25 via 192.168.10.123/29
* 200.30.55.64/27 via 10.10.10.2/30
* 200.30.55.96/28 via 10.10.10.14/30

**BORDER:**

* default via 172.16.0.1/24
* 192.68.10.128/26 via 10.10.10.5/30
* 192.68.10.0/25 via 10.10.10.5/30
* 200.30.55.64/27 via 10.10.10.9/30
* 200.30.55.96/28 via 10.10.10.9/30

---

## Ejercicio de parcial 2

### 11. Dado el siguiente bloque: 160.22.0.0/23. Asigne redes IP a las redes A, B, C y DMZ. Considere desperdiciar la menor cantidad de direcciones IP posibles.

* Red B --> 160.22.0.0/24
* Red DMZ --> 160.22.1.0/25
* Red A --> 160.22.1.128/26
* Red C --> 160.22.1.192/27

### 12. Liste las redes IP que quedaron libres luego de la asignación realizada, sumarizando si es posible.

* 160.22.1.224/27

### 13. Asigne redes IP al resto de las redes de la topología no consideradas. Utilice direccionamiento privado y desperdicie la menor cantidad de direcciones posibles.

* R1 - R3 --> 10.0.0.0/30 
* R1 - R2 --> 10.0.0.4/30
* R2 - R3 --> 10.0.0.8/30
* R2 - R4 --> 10.0.0.12/30
* R3 - R4 --> 10.0.0.16/30

### 14. Asigne direcciones IP a todas las interfaces de todos los dispositivos que corresponda. La primera dirección IP del rango debe utilizarse para la interfaz del router.

![Putno 14](https://i.imgur.com/LKNi8Iq.png)

### 15. Realice las tablas de ruteo de los routers R3 y R4, de manera tal que se cumplan las siguientes condiciones:

* Cada dispositivo pueda conectarse entre sí.
* Sólo los hosts de la DMZ pueden salir a Internet.
* Siempre tomar la ruta más corta.

**R3:**

* 160.22.1.192/27 via
* 160.22.1.0/25 via 10.0.0.18/30
* 160.22.0.0/24 via 10.0.0.9/30
* 160.22.1.128/26 via 10.0.0.1/30

**R4:**

* default via ISP eth0
* 160.22.0.0/24 via 10.0.0.13/30
* 160.22.1.128/26 via 10.0.0.13/30
* 160.22.1.192/27 via 10.0.0.17/30

### 16. Considerando las condiciones establecidas en el punto anterior, realice las tablas de ruteo del servidor Mail y del host PC-A.

**Mail:**

* default via 160.22.1.1


**PC-a:**

* default via 160.22.1.129

---