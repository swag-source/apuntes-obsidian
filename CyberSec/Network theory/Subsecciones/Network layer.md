****

![[Network-layer.png]]

**Network layer (Layer 3)**
* Provee la forma de transmisión de paquetes desde una fuente (cliente) hasta el destino (otro cliente) dentro de una misma red o redes diferentes.

Se utilizan recursos tales como:

**Direccionamiento lógico:** Si bien en la 2° capa se utiliza direccionamiento físico (MAC address), en esta red se utilizan protocolos lógicos para direccionar la información. El protocolo más usado en esta capa es el de IP (internet protocol).

**Switching:** Toma las decisiones de cómo deberían ser enviados los paquetes. Existe switching de diferentes cosas.
+ **Switching de paquetes:** equivalente a _routing_ se divide el flujo de data en paquetes (más pequeños).
+ **Switching de circuitos:** genera el vínculo entre dos partes para poder comunicarse entre sí. Una vez terminado el intercambio, desaparece la conexión (analogía llamada de teléfono).

**Descubrimiento y selección de rutas:** Como la capa 3 se encarga de las decisiones lógicas de una red (el cómo), también se tiene que encargar del envío (a quien) le llega el paquete.


