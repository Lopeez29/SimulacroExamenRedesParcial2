https://github.com/Lopeez29/SimulacroExamenRedesParcial2.git
# SimulacroExamenRedesParcial2


# Parte I: Capa de Red

## Pregunta 1: Cálculo de Ruta Más Corta ✓

**a)** Explica brevemente el funcionamiento del **Algoritmo de Dijkstra** para encontrar la ruta más corta entre dos nodos en un grafo ponderado.

1. **Inicialización de distancias**  
   - Asigna al nodo de origen `s` una distancia provisional de `0`.  
   - Para todos los demás nodos `N`, fija su distancia provisional en `∞`.  
   - Mantén dos conjuntos:  
     - **No visitados**: contiene todos los nodos.  
     - **Visitados**: inicialmente vacío.

2. **Selección del nodo activo**  
   - Dentro del conjunto de nodos **no visitados**, elijo aquel con la distancia provisional más baja; lo llamaremos `DisBaja`.  
   - Muevo `DisBaja` del conjunto **no visitados** al conjunto **visitados**.  

3. **Actualización de distancias (relajación)**  
   - Para cada nodo vecino `N` de `DisBaja` (es decir, para cada arista `(N,DisBaja)`):  
     1. Calcula la distancia alternativa:  
        ```
        dist_alt = dist[DisBaja] + peso(DisBaja, N)
        ```  
     2. Si `dist_alt < dist[N]`, reemplaza `dist[N]` por `dist_alt` y guarda `DisBaja

**b)** Describe el método de **Enrutamiento por Inundación (Flooding)** y discute sus ventajas y desventajas comparándolo con Dijkstra.

### b) Enrutamiento por Inundación (Flooding)

El enrutamiento por inundación funciona de modo que cuando un nodo recibe un paquete que no le corresponde, lo reenvía a todos sus vecinos salvo al que se lo envió.
Para ello, utiliza un identificador único para descartar duplicados y evitar bucles. Este no requiere de tablas ni cálculos de rutas, lo que simplifica su implementación y aporta gran tolerancia a errores, ya que el paquete puede llegar por cualquier camino disponible. Sin embargo, al generar réplicas masivas, satura el ancho de banda, incrementa la latencia y no resulta práctico en redes de tamaño medio o grande.  

## Pregunta 2: Cálculo de Direcciones de Broadcast y Subredes ✓

**a)** Para la subred `172.29.152.0` con máscara `255.255.248.0`, determina la dirección de broadcast. Explica el proceso de conversión de la máscara a binario y cómo se obtiene el resultado.

Para la subred **172.29.152.0/21** (máscara 255.255.248.0) se reservan 21 bits para identificar la red y los 11 restantes para hosts. Al avanzar en saltos de 8 en el tercer octeto (256−248) el último bloque que incluye 172.29.152.0 termina en **172.29.159.255**, que es la dirección de broadcast.  

- La máscara en binario es  
  `11111111.11111111.11111000.00000000`  
  — 21 bits en “1” para la red y 11 bits en “0” para hosts.

- El incremento en el tercer octeto es  
  `256 − 248 = 8`,  
  de modo que los bloques son 172.29.152.0, 172.29.160.0, y sucesivamente.

- El broadcast de la primera red se obtiene poniendo a “1” todos los bits de host:  
  172.29.152.0 → 172.29.159.255.

Por tanto, la dirección de broadcast es **172.29.159.255**.

**b)** Dado el bloque `172.18.26.0/23`, calcula la dirección de broadcast y justifica el proceso.

Para la red **172.18.26.0/23** (máscara 255.255.254.0) reservo 23 bits para la parte de red y quedan 9 para los hosts, por lo que el rango va de 172.18.26.0 a 172.18.27.255. En cuanto a la última dirección, **172.18.27.255**, es el broadcast, ya que equivale a poner todos los bits de host a 1.

## Pregunta 3: Última Dirección Válida y Rango de Hosts ✓

**a)** Con la subred `172.30.67.192` y máscara `255.255.255.192`, determina cuál es la última dirección de host válida (excluyendo la dirección de broadcast).

Para esta subred `172.30.67.192/26` (máscara `255.255.255.192`) tengo 6 bits de host, el bloque va de `172.30.67.192` a `172.30.67.255`. Como la última IP que nos excluyen es el "broadcast", la última dirección de host válida en esta caso sería **172.30.67.254**.

**b)** Para el host `172.22.53.199` con máscara `255.255.252.0`, determina el rango de direcciones válidas (primera y última dirección de host) de la subred.

En `172.22.53.199/22` (máscara `255.255.252.0`), el salto es de 4 en el tercer octeto, así que la subred cubre de `172.22.52.0` a `172.22.55.255`. Queda:
- Primera IP de host: **172.22.52.1**  
- Última IP de host: **172.22.55.254**  

## Pregunta 4: Capacidad y Segmentación de Subredes ✓

**a)** Calcula el número de equipos (hosts) que pueden conectarse en la red `172.26.0.0` con máscara `255.255.255.192`.

Para la red `172.26.0.0/26` (máscara `255.255.255.192`) quedan **6 bits** para identificadores de host, por lo que caben `2^6 − 2 = 62` dispositivos.

**b)** Dado el host `172.18.171.190/23`, identifica a qué subred pertenece, explicando cómo se determina el bloque correspondiente.

Con el host `172.18.171.190/23` (máscara `255.255.254.0`) presto 23 bits a la red y dejo 9 para hosts. El salto en el tercer octeto es de 2 (`256 − 254`), así que el bloque en el que cae el valor 171 es el que empieza en `170` (`171 − 1`). Entonces, la subred es **172.18.170.0/23**.

## Pregunta 5: Número de Subredes Necesarias ✓

Explica cómo se determina el número de subredes disponibles utilizando la fórmula:

**Nº de subredes = 2^s**

donde **s** es el número de bits prestados al identificador de subred. Aplica este concepto a un escenario en el que se requieren al menos 4 subredes para segmentar una red.

Para calcular el número de subredes disponibles se usa la fórmula **Nº de subredes = 2^s**, donde *s* es el número de bits prestados. Si necesitamos al menos 4 subredes, buscamos el menor *s* tal que 2^s ≥ 4; con *s* = 2 tenemos 2^2 = 4 subredes. Por tanto basta con tomar 2 bits prestados (por ejemplo, pasar de /24 a /26) para obtener exactamente 4 subredes.  

## Parte II: Capa de Transporte

### Pregunta 6: Comparación entre TCP y UDP ✓

**a)** Compara TCP y UDP en términos de:  
- Necesidad de establecer conexión  
- Fiabilidad y control de errores  
- Control de flujo y congestión  
- Velocidad de transmisión

En TCP se establece primero un three-way handshake antes de enviar datos; se garantiza la entrega y el orden gracias a ACKs y retransmisiones, además de incorporar control de flujo (ventana deslizante) y gestión de congestión (slow start), aunque esto añade latencia. En UDP no hay conexión previa, no hay confirmaciones ni reenvíos, no controla flujo ni congestión y, por tanto, transmite más rápido pero sin asegurar que todo llegue.

**b)** Menciona dos ejemplos de aplicaciones en las que se prefiera usar UDP y justifica la elección.  

**Streaming**: se prioriza la fluidez, se tolera alguna pérdida antes que cortes. 
**Juegos online**: se busca latencia mínima; un retraso por confirmaciones dañaría la experiencia.

### Pregunta 7: Establecimiento y Terminación de Conexión en TCP ✓

Describe en detalle el proceso de establecimiento de conexión en TCP (Three-Way Handshake) y el proceso de terminación de la conexión (Four-Way Handshake). Explica la importancia de cada uno de los pasos involucrados.

Para abrir la conexión en TCP primero yo envío un paquete **SYN** con mi número de secuencia, el servidor me responde con **SYN+ACK** aceptando mi número y anunciando el suyo, y luego yo remito un **ACK** final para confirmar que todo está sincronizado. Para cerrar la conexión, cualquiera de los dos lados puede mandar un **FIN**, el otro contesta con un **ACK** y cuando ya no tiene más datos que enviar envía su propio **FIN**, al que yo remito un último **ACK**. Así me aseguro de que todos los datos se han entregado antes de liberar la conexión.  

### Pregunta 8: Multiplexación y Demultiplexación ✓

Explica el concepto de **multiplexación descendente** y **multiplexación ascendente** en la capa de transporte, y proporciona un ejemplo práctico para cada caso.

La **multiplexación descendente** es el proceso por el cual la capa de transporte agrupa los datos de varias aplicaciones (cada una identificada por su puerto de origen) en un único flujo de segmentos, añadiendo el encabezado TCP/UDP correspondiente. En cambio, la **multiplexación ascendente** ocurre en el receptor: al llegar un segmento, la capa de transporte lee el puerto de destino y entrega el cuerpo de datos a la aplicación correcta, separando así el flujo único en sus correspondientes sockets.
los ejemplos son un cliente de correo y una aplicación de videoconferencia enviando datos simultáneos por UDP hacia el mismo servidor. En la **multiplexación ascendente**, cuando llegan los segmentos la capa lee el puerto de destino y reenvía los datos al proceso adecuado; los ejemplos son un servidor que atiende peticiones HTTP en el puerto 80 mientras gestiona conexiones FTP en el 21 y, según el puerto, entrega los datos al módulo web o al servicio de archivos.

### Pregunta 9: Cálculo del Tamaño de Ventana en TCP ✓

Se tiene un enlace con los siguientes parámetros:

- **RTT** = 50 ms  
- **Ancho de banda** = 100 Mbps  
- **MSS (Tamaño máximo de segmento)** = 1 500 bytes  

Realiza lo siguiente:

1. Convierte las unidades necesarias (por ejemplo, RTT a segundos y ancho de banda a bps).
   Para el enlace con RTT = 50 ms, ancho de banda = 100 Mbps y MSS = 1 500 B:

   **Convertir unidades**  
   - RTT: 50 ms = 0,05 s  
   - Ancho de banda: 100 Mbps = 100 × 10⁶ bps

2. Calcula el tamaño óptimo de la ventana en bits y en bytes usando la fórmula:  
   
   > Ventana óptima = Ancho de banda × RTT
   
**Calcular ventana óptima en bits**  

ventana_bits = ancho_banda × RTT
= 100×10⁶ bps × 0,05 s
= 5 000 000 bits

**Pasar a bytes**

ventana_bytes = 5 000 000 bits ÷ 8
= 625 000 bytes

3. Determina aproximadamente el número de segmentos MSS que pueden estar en tránsito simultáneamente.
   
   **Número de segmentos MSS en vuelo**
n_segmentos ≈ 625 000 bytes ÷ 1 500 bytes/segmento
≈ 416,67

Caben **≈ 417** segmentos simultáneos (416 completos y uno a la mitad aproxidamente).
 
- **Ventana óptima** = 5 000 000 bits (625 000 bytes)  
- **Segmentos simultáneos** ≈ 417  

### Pregunta 10: Control de Congestión en TCP ✓

Explica brevemente cómo TCP implementa mecanismos de control de congestión, haciendo énfasis en:

- El **Algoritmo de Arranque Lento (Slow Start)**

**Arranque Lento (Slow Start):** al iniciar una conexión pongo la ventana en 1 MSS y, por cada ACK recibido, la incremento en 1 MSS (duplicando la ventana cada RTT). Así pruebo rápida y gradualmente la capacidad de la red sin saturarla de golpe.
  
- El **Algoritmo de Nagle**

**Algoritmo de Nagle:** si tengo varios datos pequeños pendientes, no envío cada uno de inmediato, sino que agrupo hasta formar un MSS o hasta recibir un ACK que me permita vaciar el buffer. Esto evita inundar la red con paquetes diminutos.
  
- El **Algoritmo de Clark**

**Algoritmo de Clark:** cuando detecto pérdida de un paquete, en lugar de reiniciar la ventana al mínimo, reduzco la `ssthresh` a la mitad de la `cwnd` actual, paso a Slow Start sólo hasta alcanzar esa nueva `ssthresh` y luego sigo con congestión aditiva. Con ello no pierdo todo el progreso al recuperarme de la congestión.

## Parte III: Capa de Aplicación y Aplicaciones Multimedia

### Pregunta 11: Funcionamiento de DNS ✓

Describe el proceso completo de resolución de nombres en el **Sistema de Nombres de Dominio (DNS)**, desde que el usuario ingresa un dominio en el navegador hasta que se obtiene la dirección IP del servidor.

Cuando escribo un dominio en el navegador, mi equipo pregunta primero a su servidor DNS configurado (resolver), que suele tener en caché la IP; si no la tiene, hace una consulta recursiva al root, recibe la referencia a los servidores TLD (por ejemplo “.com”), luego pregunta a esos servidores TLD para que le digan quién es el autoritativo de “midominio.com” y, finalmente, consulta a ese autoritativo para obtener la IP concreta. Cada respuesta va almacenándose en caché (root, TLD y autoritativo) según su tiempo de vida (TTL), de modo que las siguientes búsquedas sean más rápidas hasta que expire la información.

### Pregunta 12: Protocolos de Correo Electrónico ✓

Compara las características de los protocolos **POP3**, **IMAP** y **SMTP** en términos de:

- Función principal  
- Tipo de uso y acceso  
- Modo de almacenamiento de correos  

Incluye ejemplos de situaciones en las que cada uno es más adecuado.

**POP3** se encarga de descargar los correos del servidor al cliente y, por defecto, los elimina del servidor tras la descarga. Es muy sencillo y funciona bien si siempre accedo al mismo equipo (p. ej. mi PC de sobremesa), pero no es ideal si quiero leer el mismo buzón desde varios dispositivos.

**IMAP** mantiene los mensajes en el servidor y permite acceder a carpetas remotas sin descargarlos completamente. Es el más recomendable cuando uso varios dispositivos (móvil, tablet), porque cualquier cambio se refleja en todos ellos al quedar todo sincronizado en el servidor.

**SMTP** es el protocolo de envío de correo: gestiona la comunicación entre el cliente y el servidor de salida. No almacena mensajes a largo plazo, solo se ocupa de entregarlos al servidor de destino. Lo uso siempre que envío un correo, ya sea desde el cliente de escritorio o la app web, y luego queda disponible para que el receptor lo reciba vía POP3 o IMAP según su configuración.

### Pregunta 13: Funcionamiento de HTTP y FTP ✓

**a)** Explica el funcionamiento básico de **HTTP**, mencionando los métodos más utilizados (*GET*, *POST*, *PUT*, *DELETE*).  

En HTTP el cliente inicia una conexión TCP al servidor (puerto 80 o 443), envía una petición que incluye un método (GET para leer, POST para enviar datos, PUT para actualizar y DELETE para eliminar) y espera la respuesta con sus cabeceras.

**b)** Describe el funcionamiento de **FTP** y señala las diferencias esenciales con HTTP, en especial en lo que se refiere al uso de conexiones (control y datos).  

FTP usa dos conexiones TCP: una de control (en el puerto 21) para enviar comandos al servidor y otra de datos (en el puerto 20 o un puerto acordado en modo pasivo) para transferir ficheros. A diferencia de HTTP, que abre y cierra la misma sesión para cada petición y respuesta y es sin estado, FTP mantiene un canal permanente de control separado del de datos, lo que puede dificultar su uso a través de firewalls y NAT.

### Pregunta 14: Streaming y VoIP ✓

**a)** Define y compara brevemente los siguientes tipos de streaming:  
- **UDP Streaming**  
- **HTTP Streaming**  
- **Adaptive HTTP Streaming (DASH)**  

Menciona un ejemplo de aplicación para cada uno.

- **UDP Streaming:** envía paquetes sin confirmar recepción ni orden, lo que minimiza retrasos. Los ejemplos son las transmisiones en directo de radio por Internet donde se tolera alguna pérdida de audio.  
- **HTTP Streaming:** usa conexiones HTTP/TCP para descargar segmentos secuenciales de un archivo multimedia. Los ejemplos son los vídeos de plataformas como YouTube.  
- **Adaptive HTTP Streaming (DASH):** divide el contenido en fragmentos de distintas calidades y ajusta automáticamente el nivel según el ancho de banda disponible. Los ejemplos son los servicios de vídeo en streaming como Netflix

**b)** Explica el proceso de funcionamiento de **VoIP** (Voz sobre IP) y enumera algunos problemas comunes (como retardo, pérdida de paquetes y eco) junto con posibles soluciones.  

En **VoIP** la voz se fragmenta en paquetes RTP sobre UDP y se envía sin establecer conexión previa. Los problemas más comunes son:  
- **Retardo:** se soluciona con mecanismos de calidad de servicio (QoS) y priorización de paquetes.  
- **Pérdida de paquetes:** se mitiga usando buffers de jitter y técnicas de corrección FEC.  
- **Eco:** se combate con algoritmos de cancelación de eco integrados en el terminal o en el gateway.

### Pregunta 15: Control de Congestión en Redes Multimedia ✓

Describe dos técnicas utilizadas para evitar la congestión en aplicaciones multimedia (por ejemplo, el uso de buffering en el cliente o el marcado de paquetes – DiffServ) y explica cómo contribuyen a mejorar la calidad del servicio.

En aplicaciones multimedia se emplea el **buffering en el cliente**, que consiste en acumular unos milisegundos de vídeo o audio antes de reproducirlos para absorber variaciones de retardo y evitar interrupciones. También se utiliza el **marcado de paquetes**, donde los flujos multimedia reciben prioridad en los routers mediante clases de servicio, garantizando que estos paquetes avancen antes que el resto y reduciendo pérdidas y jitter. Ambas técnicas ayudan a mantener una reproducción continua y de calidad incluso ante congestión de la red.

### Pregunta 16: Best-Effort vs Servicios Multiclase ✓

Compara el modelo **Best-Effort** con los **Servicios Multiclase**, enfatizando:

- La manera en que se maneja el tráfico  
- La garantía (o falta de ella) en la calidad de servicio  
- Ejemplos de aplicaciones en las que se utiliza cada enfoque

Yo veo que en **Best-Effort** los paquetes se tratan todos igual: los routers los envían según van llegando y, si hay congestión, simplemente empiezan a descartarlos sin aviso ni prioridad. No hay garantía ni de entrega, ni de retardo ni de ancho de banda, así que encaja bien con tareas como navegación web, correo o descargas de ficheros, donde una pequeña pérdida no rompe la experiencia.

En cambio, con **Servicios Multiclase** se clasifican los flujos y se marcan para darles distintas colas en los routers, de modo que el tráfico crítico recibe prioridad y se cumple un mínimo de retardo, jitter o ancho de banda. Esto es imprescindible en escenarios como llamadas VoIP corporativas, videoconferencias o streaming en directo, donde la calidad debe mantenerse constante.

## Parte IV: Seguridad en Redes

### Pregunta 17: Problemas de Seguridad en Redes ✓

Identifica y explica brevemente las siguientes áreas críticas de seguridad en redes:

- **Confidencialidad**  
- **Autenticación**  
- **No repudio**  
- **Integridad**  

Para cada una, menciona una solución o técnica que se emplea para mitigar el riesgo (por ejemplo, cifrado, autenticación multifactor, firmas digitales, etc.).  

**Confidencialidad:** asegurar que sólo las partes autorizadas puedan leer los datos. Para ello uso cifrado.

**Autenticación:** verificar que quien se conecta es quien dice ser. Para ello empleo métodos como contraseñas robustas, autenticación multifactor o certificados digitales.

**No repudio:** impedir que una parte niegue haber enviado un mensaje. Se logra con firmas digitales basadas en PKI, ya que sólo el poseedor de la clave privada puede generar una firma válida.

**Integridad:** garantizar que los datos no se alteren durante la transmisión o almacenamiento. Para ello utilizo mecanismos de verificación como HMAC o sumas de comprobación dentro de protocolos como IPsec o SSH.

### Pregunta 18: Cifrado Simétrico vs Asimétrico ✓

Realiza una comparación entre **cifrado simétrico** y **cifrado asimétrico**, abordando:

- **Número de claves requeridas**  
- **Velocidad y eficiencia**  
- **Ejemplos de algoritmos y aplicaciones típicas en cada caso**

En **cifrado simétrico** solo hace falta una única clave secreta que comparten emisor y receptor, lo que lo hace muy rápido y eficiente para cifrar grandes volúmenes de datos. En cambio, el **cifrado asimétrico** emplea un par de claves (pública y privada) para encriptar y desencriptar,el problema es que es más lento y caro pero permite intercambiar claves sin canal seguro y sirve para firmar mensajes y certificados en HTTPS o correo seguro.

### Pregunta 19: Funcionamiento del Algoritmo RSA ✓

**a)** Describe el proceso de generación de claves en RSA, incluyendo la selección de dos números primos, el cálculo de _n_ y φ(_n_), y la determinación de _e_ y _d_.  

Elijo dos números primos `p` y `q`.  
2. Calculo `n = p * q` y `φ = (p - 1) * (q - 1)`.  
3. Selecciono un entero `e` tal que `1 < e < φ` y `gcd(e, φ) = 1`. La clave pública es el par `(n, e)`.  
4. Encuentro `d`, el inverso modular de `e` módulo `φ`, de modo que `e * d ≡ 1 (mod φ)`. Ese `d` será la clave privada.

**b)** Realiza un ejemplo numérico sencillo (por ejemplo, usando _p_ = 3 (_p_ = 3), _q_ = 11 (_q_ = 11), _e_ = 7 (_e_ = 7)) para demostrar el cifrado y descifrado de un mensaje (puedes usar _M_ = 4 (_M_ = 4)).  

Ejemplo con `p = 3`, `q = 11`, `e = 7` y `M = 4`:

- Calculo de parámetros:  
  - `n = p * q = 3 * 11 = 33`  
  - `φ = (p - 1) * (q - 1) = 2 * 10 = 20`  
  - Verifico `gcd(e, φ) = gcd(7, 20) = 1`  
  - Busco `d` tal que `e * d ≡ 1 (mod φ)` → `7 * d ≡ 1 (mod 20)` → `d = 3`

- Cifrado de `M = 4`:
  C = M^e mod n = 4^7 mod 33 = 16384 mod 33 = 16

  Descifrado de `C = 16`:
  M' = C^d mod n = 16^3 mod 33 = 4096 mod 33 = 4

### Pregunta 20: Firewalls, VPN e IPSec ✓

**a)** Explica el funcionamiento de un **firewall**, mencionando al menos dos tipos (por ejemplo, filtrado de paquetes y firewall de estado) y su importancia en la protección de la red.

Un firewall es un filtro que examina y cada paquete que entra o sale de la red; un **firewall de filtrado de paquetes** bloquea o permite tráfico según reglas sobre IP y puertos, mientras que uno **stateful** también vigila el estado de cada conexión (establecida, nueva, cerrada) para evitar ataques de falsificación. Ambos son vitales para proteger la red de accesos no autorizados y tráfico malicioso.

**b)** Compara **VPN** e **IPSec** en términos de propósito, modo de operación y ejemplos de uso (por ejemplo, acceso remoto a recursos corporativos vs. cifrado entre routers).

Una **VPN** crea un túnel cifrado entre mi dispositivo y un servidor remoto, mientras que **IPSec** ofrece cifrado y autenticación a nivel de red, a menudo configurado entre routers o gateways para proteger todo el tráfico entre sedes. La VPN me protege usuario a servidor; IPSec asegura enlace a enlace.  

### Pregunta 21: SSL/TLS y DNS Spoofing ✓

**a)** Describe brevemente el funcionamiento del protocolo **SSL/TLS** y su importancia para la seguridad en la web (por ejemplo, en HTTPS).

SSL/TLS establece primero un canal seguro antes de intercambiar datos: el cliente envía un “paquete” con versiones y cifrados soportados, el servidor responde con su certificado y su “Paquete”, con la clave pública necesaria para cifrar un secreto compartido. A partir de ahí ambos generan claves simétricas y todo el tráfico va cifrado, garantizando confidencialidad e integridad (HTTPS usa esto para proteger las páginas web).

**b)** Explica qué es el **DNS Spoofing** y cómo la extensión **DNSSEC** contribuye a proteger la integridad de las respuestas DNS.

El DNS Spoofing consiste en inyectar respuestas falsas en las consultas DNS, redirigiendo al usuario a un servidor malicioso. DNSSEC evita esto firmando digitalmente cada registro con claves asimétricas: el resolver comprueba la firma de cada respuesta contra la clave pública del dominio, así solo acepta datos auténticos y no modificados.





