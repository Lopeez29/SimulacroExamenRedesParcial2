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

### Pregunta 7: Establecimiento y Terminación de Conexión en TCP

Describe en detalle el proceso de establecimiento de conexión en TCP (Three-Way Handshake) y el proceso de terminación de la conexión (Four-Way Handshake). Explica la importancia de cada uno de los pasos involucrados.

### Pregunta 8: Multiplexación y Demultiplexación

Explica el concepto de **multiplexación descendente** y **multiplexación ascendente** en la capa de transporte, y proporciona un ejemplo práctico para cada caso.

### Pregunta 9: Cálculo del Tamaño de Ventana en TCP

Se tiene un enlace con los siguientes parámetros:

- **RTT** = 50 ms  
- **Ancho de banda** = 100 Mbps  
- **MSS (Tamaño máximo de segmento)** = 1 500 bytes  

Realiza lo siguiente:

1. Convierte las unidades necesarias (por ejemplo, RTT a segundos y ancho de banda a bps).  
2. Calcula el tamaño óptimo de la ventana en bits y en bytes usando la fórmula:  
   
   > Ventana óptima = Ancho de banda × RTT  

3. Determina aproximadamente el número de segmentos MSS que pueden estar en tránsito simultáneamente.  

Muestra todos los pasos de tu cálculo.  

### Pregunta 10: Control de Congestión en TCP

Explica brevemente cómo TCP implementa mecanismos de control de congestión, haciendo énfasis en:

- El **Algoritmo de Arranque Lento (Slow Start)**  
- El **Algoritmo de Nagle**  
- El **Algoritmo de Clark**  

Para cada uno, menciona cuál es su objetivo y cómo contribuye a optimizar el rendimiento de la red.

## Parte III: Capa de Aplicación y Aplicaciones Multimedia

### Pregunta 11: Funcionamiento de DNS

Describe el proceso completo de resolución de nombres en el **Sistema de Nombres de Dominio (DNS)**, desde que el usuario ingresa un dominio en el navegador hasta que se obtiene la dirección IP del servidor.

### Pregunta 12: Protocolos de Correo Electrónico

Compara las características de los protocolos **POP3**, **IMAP** y **SMTP** en términos de:

- Función principal  
- Tipo de uso y acceso  
- Modo de almacenamiento de correos  

Incluye ejemplos de situaciones en las que cada uno es más adecuado.

### Pregunta 13: Funcionamiento de HTTP y FTP

**a)** Explica el funcionamiento básico de **HTTP**, mencionando los métodos más utilizados (*GET*, *POST*, *PUT*, *DELETE*).  

**b)** Describe el funcionamiento de **FTP** y señala las diferencias esenciales con HTTP, en especial en lo que se refiere al uso de conexiones (control y datos).  

### Pregunta 14: Streaming y VoIP

**a)** Define y compara brevemente los siguientes tipos de streaming:  
- **UDP Streaming**  
- **HTTP Streaming**  
- **Adaptive HTTP Streaming (DASH)**  

Menciona un ejemplo de aplicación para cada uno.

**b)** Explica el proceso de funcionamiento de **VoIP** (Voz sobre IP) y enumera algunos problemas comunes (como retardo, pérdida de paquetes y eco) junto con posibles soluciones.  

### Pregunta 15: Control de Congestión en Redes Multimedia

Describe dos técnicas utilizadas para evitar la congestión en aplicaciones multimedia (por ejemplo, el uso de buffering en el cliente o el marcado de paquetes – DiffServ) y explica cómo contribuyen a mejorar la calidad del servicio.

### Pregunta 16: Best-Effort vs Servicios Multiclase

Compara el modelo **Best-Effort** con los **Servicios Multiclase**, enfatizando:

- La manera en que se maneja el tráfico  
- La garantía (o falta de ella) en la calidad de servicio  
- Ejemplos de aplicaciones en las que se utiliza cada enfoque  

## Parte IV: Seguridad en Redes

### Pregunta 17: Problemas de Seguridad en Redes

Identifica y explica brevemente las siguientes áreas críticas de seguridad en redes:

- **Confidencialidad**  
- **Autenticación**  
- **No repudio**  
- **Integridad**  

Para cada una, menciona una solución o técnica que se emplea para mitigar el riesgo (por ejemplo, cifrado, autenticación multifactor, firmas digitales, etc.).  

### Pregunta 18: Cifrado Simétrico vs Asimétrico

Realiza una comparación entre **cifrado simétrico** y **cifrado asimétrico**, abordando:

- **Número de claves requeridas**  
- **Velocidad y eficiencia**  
- **Ejemplos de algoritmos y aplicaciones típicas en cada caso**

### Pregunta 19: Funcionamiento del Algoritmo RSA

**a)** Describe el proceso de generación de claves en RSA, incluyendo la selección de dos números primos, el cálculo de _n_ y φ(_n_), y la determinación de _e_ y _d_.  

**b)** Realiza un ejemplo numérico sencillo (por ejemplo, usando _p_ = 3 (_p_ = 3), _q_ = 11 (_q_ = 11), _e_ = 7 (_e_ = 7)) para demostrar el cifrado y descifrado de un mensaje (puedes usar _M_ = 4 (_M_ = 4)).  

### Pregunta 20: Firewalls, VPN e IPSec

**a)** Explica el funcionamiento de un **firewall**, mencionando al menos dos tipos (por ejemplo, filtrado de paquetes y firewall de estado) y su importancia en la protección de la red.

**b)** Compara **VPN** e **IPSec** en términos de propósito, modo de operación y ejemplos de uso (por ejemplo, acceso remoto a recursos corporativos vs. cifrado entre routers).

### Pregunta 21: SSL/TLS y DNS Spoofing

**a)** Describe brevemente el funcionamiento del protocolo **SSL/TLS** y su importancia para la seguridad en la web (por ejemplo, en HTTPS).

**b)** Explica qué es el **DNS Spoofing** y cómo la extensión **DNSSEC** contribuye a proteger la integridad de las respuestas DNS.





