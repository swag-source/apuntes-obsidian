***
## Conceptos claves

* *Escalabilidad*
* *Flexibilidad*
* *Microprogramación*
* *Unidad de control (UC) - Director de orquesta*

## Apuntes de clase

*Breve repaso*

* Una arquitectura al final del día es una *especificación* respecto a las máquinas (nos dicen qué tenemos disponibles, qué recursos tiene y qué operaciones podemos realizar).
* Un problema que nos encontramos es la **Escalabilidad**:
	-> Ese problema lo podemos resolver agregando "grados" de flexibilidad.
* Introducimos la *Microprogramación*
	-> Otorga un grado de flexibilidad en el caso de que se quiera escalar un producto a los desarrolladores de hardware.

>[!Registros]
>Investigar sobre diferencia entre registros y memoria.


### Pasos necesarios

1. Analizar el conjunto de instrucciones para determinar los requiere del camino de datos.
2. Seleccionar componentes
3. Adaptar el camino de datos según los requiere.
4. Analizar las instrucciones y determinar las señales de control necesarias.
5. Construir el control.
### Etapas de ejecucion de una instrucción

1. Fetch de la instrucción: **IF** (fetch)
2. Decodificación (y lectura de registros: **ID**)(Decode)
3. Ejecución o cálculo de dirección de memoria: **EX** (Execution)
4. Acceso a datos en memoria: **MEM**
5. Escritura en registros: **WB** (Write Back)

### Register Transfer Language

Es un lenguaje utilizado para entender la transferencia de información entre registros.

+ Cada instrucción está formada por un conjunto de microopoeraciones.
+ El *Register Transfer Language* (RTL) se utiliza para describir la secuencia exacta de las microoperaciones.

![[Pasted image 20230919215429.png]]

#### Paso 1: Requerimientos de la ISA
* Memoria
	* Para instrucciones y datos (Von Neumann).
* Registros 32x32
	* Leer Rs, Leer Rt.
	* Escribir Rt ó Rd.
* PC, MDR (memory data register).
* A,B para datos intermedios, ALUout (retiene salida de la ALU).
* Extensor de signo de 16 a 32 bits.
* Sumar y restar registros y/o valores inmediatos.
* Operaciones lógicas (and, or) entre registros y o valores inmediatos.

#### Paso 2: Conectar los componentes de la ISA
* **Elementos de almacenamiento**: banco de registros con dos puertos de lectura.
	-> Nos permite leer dos tipos de registros a la vez.


>[!Registros]
>La idea detrás de la unidad de control es modularizar cada "operación" (cada bloque está separado), luego el control-flow se va encargando de pasar entre bloque y bloque. El conjunto de control-flow entre bloques hace al total de las operaciones. Sería el "control de orquesta".

![[Pasted image 20230919215541.png]]

