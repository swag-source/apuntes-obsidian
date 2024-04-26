***

## Preguntas a responder

* ¿Cuál es el propósito de una red?
* ¿Que ejemplos existen de componentes en la red?
* ¿Cómo se definen las redes por GEOGRAFÍA?
* ¿Cómo se definen las redes por TOPOLOGÍA?
* ¿Cómo se definen las redes por ALOCACIÓN DE RECURSOS? 

*** 

## Propósito de una red y componentes

En esencia, nos interesa crear una red para establecer conexiones entre clientes (pueden ser PC's, impresoras, entre otros). El verdadero valor de la red no reside en su estructura, sino en la información que circula en esa estructura y sus formas.

Dentro de una red podemos encontrar los siguientes componentes de red

* **Cliente**: Dispositivo por el cual el usuario final se conecta a la red.
* **Servidor**: Se encarga de la administración de recursos, archivos o la información que circula sobre la red.
* **Hub**: Herramienta de interconexión entre cliente y servidor.
* **Switch**: Similar a un Hub, se encarga de la interconexión entre componentes de la red (clientes, servidores) con la diferencia que el switch analiza el tráfico de los puertos y aprende cómo administrarla de forma más eficiente. Se considera un dispositivo de 2° capa
* **Router:** Envía información a partir de decisiones basadas en direcciones de redes lógicas (por ejemplo: dirección IP). Se considera un dispositivo de 3° capa.
* **Media:** Todos los dispositivos en la red deben tener algún medio de interconexión (cable de fibra óptica o cobre) para poder comunicarse entre ellos.
* **WAN link:** Corto para [[Wide Area Networks (WANs)]], se utiliza para interconectar una o más redes. Si nuestra compañía tiene varias ubicaciones, la compañía se conecta entre ellas mediante el protocolo MPLS (Multiprotocol Label Switching)


### Ejemplo típico de red

![[Topología-red-básica.png]]

***

## Redes definidas por GEOGRAFÍA

Observamos que no todas las redes son iguales y dependiendo la necesidad, algunas redes son más eficientes que otras.

1. **Local area network (LAN)
2. **Wide area network (WAN)
3. **Wireless local area network (WLAN)
4. **Storage area network (SAN)
5. **Campus area network (CAN)
6. **Metropolitan area network (MAN)
7. **Personal area network (PAN)

## Redes definidas por TOPOLOGÍA

#### Topologías por cable
La topología de red está definida a partir de dos componentes principales:

* **Topología lógica:** El flujo de "operaciones" dentro de la red (como no lo podemos ver, en teoría) define la topología lógica de la red.
* **Topología física:** La estructura física, la forma en que se entre-conectan dispositivos dentro de una red determina su topología física.

El flujo de tránsito define la estructura lógica de una red.

Tenemos las siguientes topologías de red:

1. **Ring Topology
2. **Bus Topology
3. **Star Topology 
4. **Tree Topology
5. **Full Mesh Topology
6. **Partial Mesh Topology

![[Clases-de-topologías.png]]


### Topologías inalámbricas

Las tecnologías inalámbricas juegan actualmente un rol fundamental dentro de las estructuras de redes. Tenemos 3 principales: 

* **Ad Hoc networks (wireless P2P)
* **Client/Server networks
* **P2P (peer-to-peer) networks

**Ad Hoc**
	Se definen como aquellas redes que dos nodos se conectan entre sí sin la necesidad de un dispositivo intermediario como un switch o access point. No posee "infraestructura". Un ejemplo es la red AirDrop de Apple.

**Client/Server networks**
	Son aquellas infraestructuras donde, dado un servidor, se le autorizan permisos a los usuarios para acceder a archivos, información etc. dependiendo de los privilegios del usuario. Usualmente tienen un switch que controla el trafico en la red.

![[Client-server-network.png]]

>[!Nota de pie:]
>Un servidor puede ser cualquier computadora basada en servidores de Linux o Windows. También puede ser un ordenador dedicado a asignar privilegios y acceso a archivos pedidos por clientes de la red.


**P2P networks (peer to peer)**
	Definimos P2P como una arquitectura de red donde la locación de recursos y distribución de archivos es manejada directamente por los clientes de la red sin la intervención de un servidor único y fijo.
	Un ejemplo típico de una red P2P es uTorrent o redes de descarga e intercambio de archivos; también la compra y venta de cryptomonedas entre usuarios es un proceso P2P.

![[Red-p2p.png]]

###### Conceptos adicionales

**Network Redundancy (redundancia de red):** Proceso por el cual se proveen múltiples caminos/paths para la forma en la cual la información pueda circular en la red. El propósito de la redundancia es, en caso de alguna falla en algún cliente, que otro cliente pueda encargarse automáticamente del tráfico de la información hacia el cliente. 

