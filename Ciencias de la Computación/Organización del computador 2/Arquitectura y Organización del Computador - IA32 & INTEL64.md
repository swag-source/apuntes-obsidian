
### Arquitectura
***
***Definición:*** Es el conjunto de recursos accesibles para el programador. Por lo general, se mantienen a lo largo de los diferentes modelos de procesadores dentro de la arquitectura.

* *Registros* (forman parte de la arquitectura)
* *Instruction set*
* *Estructuras de memoria* (descriptores de segmentos, de página, etc.)

En general podemos decir que todo el modelo de programación se basa en lo que la arquitectura nos permite realizar. 

### Micro Arquitectura
***
***Definición:*** Es la implementación en silicio de la arquitectura. Podríamos decir que es la capa física de la arquitectura que buscamos representar. Esta relacionado con la electrónica, materiales, simplicidad o complejidad. Una misma arquitectura puede tener distintas implementaciones dependiendo quien fabrique el procesador.

* Es una de las tareas más arduas para un diseñador de procesadores proveer una solución que sea:
	-> **Económico**.
	-> **Eficiente** (en términos energéticos y distribución de recursos).
	-> **Accesible**.

* También la definición de una arquitectura comprende el diseño del set de instrucciones, el manejo de memoria y sus modos de direccionamiento (de operandos, si es directo o indirecto, etc.), el diseño lógico, su refrigeración, encapsulamiento entre otros detalles.

*¿Qué es lo necesario para diseñar tecnologías de procesador?*
* Diseño lógico.
* Tecnología de encapsulado.
* Funcionamiento y diseño de compiladores y Sistemas Operativos. => Buscamos que lo diseñado optimice los recursos de nuestro sistema de la mejor manera.

### Instruction Set Architecture
***
*Definicion:* Nos referimos al conjunto de instrucciones visibles por el programador. Es además el límite entre el software y hardware.

Dentro de la ISA, podemos categorizarlos por:
-> **Clases de ISA:** ISAs con Registros de Propósito general vs. Registros dedicados.
-> **Direccionamiento de Memoria:** Alineación obligatoria de datos vs. administración de a bytes.
-> **Modo de Direccionamiento:** Como se especifican los operandos (ej. Cuentas entre registros o entre operandos directamente.)
-> **Tipos y tamaños de operandos:** Enteros, Punto Flotante, Punto Fijo, Tamaños y precisiones.
-> **Operaciones:** RISC (Reduced) o CISC (Complex).
-> **Instrucciones de control de flujo:** Saltos condicionales, calls, jumps etc.
-> **Longitud del código:** Instrucciones de tamaño fijo vs. variable.

$$
\text{Microarquitectura = Organización + Hardware}
$$

### Organización
***
Se refiere a los detalles de la implementación de la ISA. Es decir:
* Organización e interconexión de la memoria.
* Diseño de los bloques de la CPU y para implementar el Instruction Set
* Implementación de paralelismo a nivel de instrucciones, y/o de datos.
* De esta forma, podemos encontrar procesadores que poseen un mismo Instrucction Set Architecture pero con organización muy diferente.

*Ej:* AMD FX y Intel Core i7 tienen la misma ISA pero organizan su caché y motor de ejecución de maneras diferentes.

### Hardware
***
Se refiere a los detalles de diseño lógico y tecnología de fabricación.
* Existiran, por lo tanto, distintos procesadores con la misma ISA y la misma organización, pero distintos a nivel de hardware y diseño lógico detallado.

Actualmente, la preocupación de los fabricantes de microprocesadores es el consumo de energía eléctrica. Constantemente se trata de tener procesadores que minimicen el consumo de energía pero maximicen la efectividad del mismo.

### Modelo de Von Neumann
***
Recordamos que una de las primeras arquitecturas que inspira las computadoras actuales es el *Modelo Von Neumann - Turing*. 

1. Modelo de programa almacenado. (Cargamos en memoria el programa que queremos ejecutar).
2. Datos y programas son cargados en el único banco de memoria disponible (memoria principal).
3. Maquina de estados perpetua.

![[Pasted image 20240319180210.png]]

El problema con el modelo de Von Neumann es "una única Unidad de Memoria para datos y código" resulta obsoleto para las necesidades que tenemos. => **Von Neumann Bottleneck**.


# Procesadores IA-32 e Intel64
### Modos de Operación
***
Los procesadores IA-32 tienen tres modos de operaciones (principales) y una extensión de uno de los anteriores.
1. Modo Real
2. Modo Protegido
3. Modo Mantenimiento del Sistema
4. Modo extendido a 64 bits (denominado IA-32e) => e = extended.
	1. Submodo Compatibilidad.
	2. Submodo 64 bits.

**Modo real:** El procesador puede implementar el entorno del 8086 con ciertas extensiones:
* Utilizar registros de 32 bits
* Reconfigurar la ubicación del vector de atención de interrupciones (8086 no lo tenía, ahora si)
* Podemos pasar luego a *Modo Protegido* o *Modo Mantenimiento del Sistema*.

**Modo protegido:** Se implementa el multitasking y es el modo de excelencia de los procesadores luego del 8086. Desde el 80386 se pasa a 32 bits con espacio de direccionamiento de 4Gb a 64Gb.
* Introduce el Virtual-8086 lo cual permite la rertocompatibilidad entre programas diseñados para 8086 con procesasdores más modernos.

**Modo Mantenimiento del Sistema:** Es el modo que se ingresa cuando la computadora se encuentra "inactiva". Este modo fue diseñado inicialmente para Notebooks cuya capacidad de batería era una limitación. Para entrar en este modo, se puede realizar de dos manera distintas:
* Activación de la señal de interrupción \#SMI (Signal Managment Interruption) -> Similar a encender un flag.
* Activación de la señal \#SMM.

Cuando ingresamos a este modo el procesador se encarga de guardar el *contexto completo de la(s) tarea(s)*  y pasa a ejecutar otra porción de memoria separada. Una vez que, luego de haber entrado en modo mantenimiento, el procesador recibe I/O se interrumpe el modo y resume la tarea previa.

**Modo extendido a 64 bits**:  En el modo extendido a 64 bits, el procesador es capaz de manejar direcciones de memoria de hasta 64 bits de longitud, lo que permite acceder a una cantidad significativamente mayor de memoria RAM (hasta 16 exabytes en teoría). Además, este modo proporciona registros de propósito general de 64 bits (RAX, RBX, RCX, RDX, etc.), lo que permite realizar operaciones aritméticas y manipular datos de manera más eficiente que en el modo de 32 bits.

**Modo Compatibilidad:** Para mejorar la transición entre 16 bits y 32 bits hacia arquitecturas de 64 bits sin la necesidad de recompilar.
* Las tareas de 32 bits se ejecutan bajo la arquitectura IA-32.
* Incluye los mecanismos de protección del modo 64 bits.

En el modo compatibilidad, las tareas de 32 bits son ejecutadas por una arquitectura IA-32 pura, utilizando direcciones de 16 y 32 bits con espacio de direccionamiento.

### Modos de Direccionamiento
***
*¿Cómo podemos recibir los operandos con los cuales trabajaremos en nuestro procesador?*
* Las instrucciones (operandos implicitos dentro de las instrucciones).
* Registros.
* Posición de memoria.
* Puertos de I/O.

A la hora de manipular estos operandos, tenemos que decidir de qué forma queremos direccionarlos de Memoria a Procesador. Tenemos 3 modos de direccionamiento:
* Modo Implícito
* Modo Inmediato
* Modo Registro

**Modo Implícito:** Instrucciones donde el código de operación es suficiente para especificar que queremos realizar y sobre que registro. Ej. Operaciones sobre flags
* *CLC*: Clear Carry => Setear el flag de Carry en 0.
* *CLD*: Clear Direction Flag
* *CLI* y *STI*: Limpiar o setear el flag de Interrupción.
* *HLT*: salta al halt hasta recibir una nueva instrucción.

**Modo Inmediato:** El operando fuente viene dentro del código, no hace falta buscarlo en memoria luego del decode de una instrucción.

**Modo Registro:** Todos los operandos involucrados son Registros del procesador. La instrucción trabajará directamente con el registro en sí o con el contenido dentro del registro.
* Registros de Proposito General tanto de 64, 32, 16 u 8 bits. 
* Registros de segmentos.
* EFlags (o RFlags).
* Registros de la FPU.
* MM0 a MM7.
* XMM0 a XMM7 o XMM15 segun sea IA-32 o Intel 64.
* Registros de Control.
* Registros de Debug.
* MSR’s (Model Specific Registers).

![[Pasted image 20240320162817.png]]


### Tipos de datos y Almacenamiento en memoria
***
Dentro de la arquitectura x86 contamos con distintos Tipos de datos fundamentales.

![[Pasted image 20240322111606.png]]

Ahora, ¿de qué forma la familia de 8086 se encarga de almacenar la información? => Bit más significativo a la derecha (Big Endian) o a la izquierda (Little Endian).

**Little Endian:** una variable de varios bytes almacena su byte menos significativo en la dirección de memoria donde se hace referencia a la variable. El byte más significativo se ubica en una dirección de memoria más alta.
![[Pasted image 20240322112820.png | 500]]

