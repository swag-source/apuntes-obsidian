***

## Memoria cache

### Line Replacement
***

*¿Qué criterio utilizo para reemplazar una linea?*

-> Generalmente en las caches del tipo Set-Asociativas, la discusión es ¿cuantas vías?

-> Hay que resolver en cual de las $n$ vías desalojar cuando se genera un **conflict miss**.

3 criterios posibles:
* **Random:** Reparte la asignación de manera uniforme, selecciona los candidatos a desalojar de manera aleatoria.
* **LRU:** *Least Recently Used*. Si nos ponemos de acuerdo con la localidad, si utilizamos una linea, la mejor linea a borrar es la que menos fue utilizada recientemente.
* **FIFO:** Simplificación de la *LRU*, la primera que entra es la primera que sale.

-> El random tiene la facilidad de su construcción en Hardware (no tiene tanta lógica).

-> LRU tiende a aumentar su costo a medida que más lineas tengas (termina siendo un *pseudo LRU*).

![[Pasted image 20240425174253.png]]

-> Tabla de Cache Misses cada 1000 instrucciones que comparan reemplazos LRU, Random y FIFO para varios tamaños y asociatividades. (*Computer Architecture a Quantitative approach”, Henessy y Patterson*).

-> Los resultados permiten ver que la diferencia entre LRU y Random tiende a bajar cuando aumenta el tamaño del cache.

* LRU se impone en los cache más pequeños.
* FIFO supera a Random en cachés más chicos.


### Write Policies - Politicas de escritura
***

-> La mayoría de los accesos a memoria son lecturas, no escrituras. Por ejemplo, constantemente *fetcheamos* instrucciones (**acceso de lectura**).

-> (Depende la arquitectura) el 25% a 30% ejecutan lecturas de datos (load) y entre 10%/15% escrituras de datos (Store).

-> En el mismo clock leemos el dato del cache y verificamos el tag para saber si es un hit. Si lo es, el dato está listo para ser procesado.

* No podemos escribir una posición de memoria sin antes saber si el acceso es un hit.
* Las escrituras deben modificar únicamente la parte de la lı́nea que corresponde. El procesador establece de manera precisa a partir de que dirección escribir, y el tamaño del dato a escribir, que puede variar entre 1 y 8 **B**.


-> Las políticas de escritura definen el diseño de nuestro cache.

Tenemos dos opciones diferentes de diseño:

**Write through:** El dato se escribe en la Cache y en el nivel inferior de la jerarquía (L1 a L2. L2 a L3. L3 a DRAM)

**Write Back:** El dato se escribe solo en el cache, y se actualiza en el resto de la jerarquía solo cuando es desalojado. (Lo escribo en el caché y el resto *que se cague*). => Más rápido.

* Para minimizar la cantidad de write back's, se utiliza un bit por cada linea que indique si la misma está dirty (modificada) o clean (no modificada).
* El write back se hace al momento de desalojar lineas dirty, ya que son las modificadas respecto a los niveles inferiores.

-> Tenemos una situación de ***write stall***, es decir, la espera del procesador por una escritura que debe replicarse a niveles inferiores de la memoria.

* Si queremos mejorar la performance producto del *write stall* es incluir un Write Buffer en el Controlador Cache.
* El Write Buffer puede recibir escrituras a mayor frecuencia que la que le t

-> Cuando se requiere escritura, puede que el dato no esté en la caché. Es decir, *Write Miss*. Tenemos dos formas de tratar el problema:

* **Write Allocate:** Actúa como un Read Miss. Se lee la línea en el cache y luego se escribe.
* **No-Write Allocate:** La operación no afecta al caché. El dato se escribe en el nivel inferior inmediato.

![[Pasted image 20240425181735.png]]

*Caso Práctico AMD Opteron Cache L1*

**Conclusiones del caso:**
* Lo que buscan los Sistemas Multiprocesador y los Subsistemas de E/S es lo mejor de los dos mundos:
	* El mı́nimo tráfico y consumo de energı́a transaccional, y la menor penalización de performance que proporciona Write Back, y al mismo tiempo la robustez en materia de coherencia que proporciona Write Through.


## Cache en Sistemas Multiprocesador

### Organización de Sistemas Multiprocesador
***
**Symetrical Multi Processing**
![[Pasted image 20240425185238.png]]

-> Todo el conjunto es administrado por un único Sistema Operativo (OS).
-> Cada par CPU - CacheSystem en un mismo Circuito Integrado (CI) se denomina **Core**.
-> Este diagrama en un único circuito integrado es un sistema **MultiCore**.



### Coherencia de un cache
***

-> Cada variable que está almacenada en un nivel de la caché también está alojada en los niveles inferiores hasta la DRAM.

-> Estas copias *idealmente* deben mantener el mismo valor. Cuando se la quiera escribir, ésta condición deja de cumplirse.

-> Si el sistema es SMP, y se ejecuta un programa paralelizado vía threads, éstos pueden ejecutarse en varias CPU al mismo tiempo.
* Eventualmente puede que haya más de una copia de una misma variable en uno o más caches restantes.

-> Como se volvieron obsoletas estas variables, los demás cores necesitan enterarse lo antes de este cambio, para invalidar ese valor previo.

**Migración y Replicado**
* *Migración*: Sucede cuando un ítem se mueve a un Cache a otro, demandando ancho de banda al Bus del sistema.
* *Replicado*: Cuando el dato se mueve a un Caché en un sistema SMP, se evitan contenciones de Bus para acceso al dato.

-> Los sistemas SMP implementan estos atributos por hardware, a partir del **Protocolo de Coherencia.**


**Protocolo de coherencia**
-> Mecanismos para llevar tracking de líneas compartidas entre cores.

-> Hay dos clases de protocolos:
* **Directory Based**: Se centraliza el estado de las líneas a todos los cores. Es más complejo. Surge a partir de la necesidad de *Sistemas Distributed Shared Memory*.
* **Snoopy:** Cada core mantiene el estado de sus líneas y monitorea lo que hace el resto de los cores. El Snoop Bus es entonces un conjunto de lı́neas entrantes a cada Controlador Cache con el cual éste “espı́a” lo que hacen los demás procesadores en particular con la memoria.
![[Pasted image 20240425195235.png]]

-> Para implementar esos protocolos, tenemos dos alternativas posibles:
* **Write invalidate:** Cada uno procesa en un bus local lo que necesite procesar. Cuando yo lo necesite, doy una señal al resto de los cores para yo poder usar el bus de datos.
* **Write update/broadcast:** Asegura menos Miss Rate. Cuando tengo que actualizar el valor, no necesito invalidar al resto. 

*¿Cómo hago para que la actualización le llegue a todos los cores?* -> Se ve luego.

**Arbitración del Bus**
* Si dos procesadores intentan escribir diferentes líneas compartidas al mismo timepo, el acceso al bus se invalidará mediante la arbitración del bus de cada sistema.
* Todos los esquemas de coherencia requieren un método de serialización de acceso a la línea de caché. Sea serializando el acceso al Bus o Cache Directory, por ejemplo.

**Implementación de Protocolos de coherencia**
-> Se implementa con una máquina de estados finita en el Controlador Caché de cada Core.
-> Un controlador soporta múltiples operaciones entrelazadas a líneas diferentes.
-> Una operación puede inicializarse antes de que se complete otra, aunque solo se permite el acceso a la caché o un acceso al bus a la vez.

* Venimos con una implementación inicial (bastante mala) pero que luego fue mejorada.

![[Pasted image 20240425201339.png]]

**MESI**
-> Agrega el estado Exclusive para disminuir la actividad del bus. Una linea *exclusive* en estado **E** puede escribirse sin invalidar el bus.

-> Aplica a cache L1 de datos y L2/L3. (para cache L1 de código solo Shared e Invalid).

**M - Modified:** Línea presente solamente en éste cache que varió respecto de su valor en memoria del sistema (dirty). Necesita write back hacia la memoria del sistema antes que otro procesador lea el dato desde ahí (que ya no es válido).

**E - Exclusive:** Línea presente solo en esta cache, coincide con la copia en memoria principal (clean).

**S - Shared:** Línea del cache presente y *puede* estar almacenada en caches de otros procesadores.

**I - Invalid:** Línea de caché no válida.

* Muy eficiente para mantener coherencia en un sistema multiprocesador con diferentes niveles jerárquicos de memoria.
* Optimiza el uso del Bus del sistema, habilitando escrituras *write back*.
* Invalida las copias cada vez que se escribe un dato y que esta escritura 

*Ejercicio de final:* Tengo una linea en estado M en el caché. Por el bus viene un read miss de esa línea. ¿Qué tengo que hacer? Describir la secuencia precisamente.
1. Prendo RFO, el lector tiene que tirar Hi-Z.
2. Deteca por Snoop Bus el Read Miss.
3. Se pone en alta impedancia, el Owner hace un Write-Back en memoria. $\cong$ 30 clocks.
4. Pongo mi linea en exclusive.
5. Bajo RFO.

-> 30 de ciclos de clock son un montón (años geológicos). Cuando baje el RFO, tengo el problema de que se pueden matar a palos por la línea del bus. Es mucho tiempo también ya que las instrucciones que otros cores necesitaban en esos 30 ciclos de clock ya no sean válidas. 

**MESIF**


**MOESI**
