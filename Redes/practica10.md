# Práctica 10 - Capa de Enlace - Parte I

### 1. ¿Qué función cumple la capa de enlace? Indique qué servicios presta esta capa.

Para transmitir un datagrama a través de un enlace individual se utiliza un protocolo de la capa de enlace. El protocolo de la capa de enlace define el formato de los paquetes intercambiados por los nodos situados en los extremos del enlace, así como las acciones que estos nodos llevan a cabo cuando se envían y reciben los paquetes.

Las unidades de datos intercambiadas por un protocolo de la capa de enlace se denominan tramas (frames), y cada trama de la capa de enlace suele encapsular un datagrama de la capa de red.

Mientras que la capa de red tiene asignada la tarea de mover los segmentos de la capa de transporte terminal a terminal desde el host de origen hasta el host destino, el protocolo de la capa de enlace tiene la tarea nodo a nodo, algo más simple, de mover los datagramas de la capa de red a través de un único enlace dentro de una ruta. Un mismo datagrama de la capa de enlace puede ser transportado por diferentes protocolos de la capa de enlace en los distintos enlaces que forman la ruta. Es importante observar que los servicios proporcionados por los diferentes protocolos de la capa de enlace a lo largo de la ruta terminal a terminal pueden ser distintos.

Servicios:

* Entramado o encapsulación: Casi todos los protocolos de la capa de enlace encapsulan cada datagrama de la capa de red dentro de una trama de la capa de enlace antes de transmitirla a través del enlace. Una trama consta de un campo de datos, en el que se inserta el datagrama de la capa de red, y de una serie de campos de cabecera (una trama también puede incluir campos de cola).

* Acceso al enlace: Un protocolo de control de acceso al medio (MAC, Medium Access Control) especifica las reglas que se utilizan para transmitir una trama a través de un enlace.
  * Para los enlaces punto a punto que tengan un único emisor en un extremo del enlace y un único receptor en el otro extremo, el protocolo MAC es muy simple (o no existe).
  * Cuando hay varios nodos compartiendo un mismo enlace de difusión, en cuyo caso se presenta el denominado problema del acceso múltiple. En este caso, el protocolo MAC sirve para coordinar la transmisión de las tramas de los múltiples nodos.

* Entrega fiable: Cuando se implementa, garantiza que va a transportar cada datagrama de la capa de red a través del enlace sin que se produzcan errores.
  * Para los enlaces inalámbricos es muy útil ya que tienen una alta tasa de errores.
  * Para los cableados puede considerarse una sobrecarga innecesaria ya que tienen una baja tasa de errores.

* Control de flujo: Los nodos situados en cada extremo de un enlace tienen una capacidad limitada de almacenamiento en buffer de tramas. Esto puede ser un problema cuando el nodo receptor puede recibir las tramas a más velocidad de la que puede procesarlas. Sin control de flujo, el buffer del receptor puede desbordarse con lo que las tramas se perderían. La idea es que el nodo emisor no abrume al nodo receptor.

* Detección de errores: Los errores de bit se introducen debido a la atenuación de las señales y al ruido electromagnético. Muchos protocolos de la capa de enlace proporcionan un mecanismo para detectar dichos errores de bit. Esto se lleva a cabo haciendo que el nodo transmisor incluya bits de detección de errores en la trama y que el nodo receptor realice una comprobación de errores. En general es más sofisticada que la de la capa de transporte y se implementa en hardware.

* Correción de errores: Es similar a la detección de errores, pero además, el receptor determina exactamente en qué puntos de la trama se han producido errores y los corrige. Algunos solo corrigen la cabecera y la cola (no el paquete completo).

* Semiduplex y full-duplex: Con la transmisión full-duplex, los nodos de ambos extremos de un enlace pueden transmitir paquetes al mismo tiempo. Sin embargo, con la transmisión semiduplex, un mismo nodo no puede transmitir y recibir al mismo tiempo.

---

### 2. Compare los servicios de la capa de enlace con los de la capa de transporte.

Muchos de los servicios proporcionados por la capa de enlace presentan notables paralelismos con los servicios proporcionados en la capa de transporte. Por ejemplo, el servicio de entrega fiable. Aunque los mecanismos utilizados para esto son similares en las dos capas, no son idénticos. Un protocolo de transporte proporciona una entrega fiable de segmentos entre dos procesos, en modo terminal a terminal; un protocolo de la capa de enlace fiable proporciona una entrega fiable de tramas entre dos nodos conectados por un único enlace. De forma similar, los protocolos de la capa de transporte como los de la capa de enlace pueden proporcionar servicios de control de flujo y detección de errores, pero de nuevo el control de flujo en un protocolo de la capa de transporte se proporciona en modo terminal a terminal, mientras que en un protocolo de la capa de enlace se proporciona entre dos nodos adyacentes.

---

### 3. Direccionamiento Ethernet:

* ¿Cómo se identifican dos máquinas en una red Ethernet?
* ¿Cómo se llaman y qué características poseen estas direcciones?
* ¿Cuál es la dirección de broadcast en capa de enlace? ¿Qué función cumple?

Se identifican mediante una dirección MAC (media access control). Esta dirección consta de 6 bytes de longitud, lo que da 2^48 posibles direcciones de MAC. Suelen expresarse en notación hexadecimal, indicándose cada byte de la dirección mediante una pareja de números hexadecimales. Una propiedad de las direcciones MAC es que nunca puede haber dos adaptadores con la misma dirección, para esto la IEEE se encarga de gestionar el espacio de direcciones MAC.

Broadcast  es una forma de transmisión de información donde un nodo emisor envía información a una multitud de nodos receptores de manera simultánea, sin necesidad de reproducir la misma transmisión nodo por nodo. Es posible la difusión de cualquier trama de datos a todas las paradas que se encuentren en el mismo segmento de la red. Para ello, se utiliza una dirección MAC especial. En el caso de Ethernet  realiza la difusión recibiendo tramas con dirección MAC de destino FF.FF.FF.FF.FF.FF.

---

