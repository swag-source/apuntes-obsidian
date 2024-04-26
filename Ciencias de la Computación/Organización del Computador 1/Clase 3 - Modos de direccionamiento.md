***
## Apuntes de clase

 + Definimos que la arquitectura es el módelo en el que almacenamos la información, la estructura.
 + Esta clase vemos conceptos vinculados a la arquitectura.
 + El modelo de Von Neumann es el modelo más conocido de arquitectura del computador.

### Modelo de Von Neumann

* Es una arquitectura que se utiliza en la mayoría de los procesadores actuales
 -> CPU (central processing unit)
 -> Input/Output
 -> Main memory

¿Cómo funciona una CPU?

Podemos definirla según su ciclo de ejecución.

1. UC (*Control Unit*) obtiene la **próxima** instrucción de memoria. Usa el registro PC (*Program Counter*) -> director de orquesta/marca el ritmo de ejecución de todos los comandos.
* Se lee la instrucción del Program Counter (registro especial que no forma parte de la memoria, pertenece a la CPU)

2. Se **incrementa** el Program Counter (ej. 2000 -> 2001)
3. La instrucción es **decodificada** a un lenguaje que entiende la ALU (si tiene que sumar/restar etc.).
4. Va a memoria para obtener los **operandos** requeridos por la operación.
5. La ALU **ejecuta** y deja los resultados en registros o en memoria.
6. **Repite el paso 1 (actúa como un while)**.

Constantemente la computadora está ejecutando instrucciones (a no ser que esté en modo *ahorro*).

### Arquitectura ISA (Instruction Set Architecture)

![[Pasted image 20230912171036.png]]

* Nivel de Lenguaje de Máquina.
* Representa el límite entre Hardware-Software.
* Define:
	-> Cómo se representan los datos, cómo se almacenan, cómo se acceden.
	-> Qué operaciones se pueden realizar.
	-> Cómo se codifican estas operaciones.

* En el desarrollo de software sirve la compatibilidad para utilizar programas con distintos componentes.

#### Características de una ISA
* Cantidad de memoria que un programa requiere.
* Complejidad del conjunto de instrucciones (RISC (conjunto reducido de instrucciones) vs. CISC)

*¿Qué soporte hay para distintos tipos de datos?*
+ Enteros (8,16,32... bits ¿complemento a 2?)
+ Punto Flotante.
+ ASCII - Unicode.

*¿Dónde y cómo se almacenan?*
* Stack


*¿Qué **operaciones** se puede hacer?*
+ **Movimiento de datos** (Move, Load, Store).
+ **Aritméticas**(Add (suma), Sub (resta)).
+ **Lógicas**(And, Xor,...).
+ **I/O**: Acceso a dispositivos de Entrada/Salida.
+ **Transferencia de control** (Jump, Skip, Call,...).

#### Jerarquía de niveles

![[Pasted image 20230912171216.png]]

#### Modos de Direccionamiento

Instrucción: **OpCode + Operandos**
¿Qué tipos de cosas pueden tener los operandos?

-> Constantes
-> Registros
-> Arrays
-> Estructuras de Datos

Existen **tipos** de direccionamiento:
+ Inmediato
+ Directo (o absoluto)
+ Indirecto
+ Registro
+ Indirecto con registro
+ Desplazamiento (indexado)


##### Inmediato
![[Pasted image 20230912171308.png]]
+ El operando es parte de la instrucción
+ Ej en Marie(Linda Null): ADD 5
	-> Efecto: AC = AC + 5
	-> 5 es un operando.
+ Ej2: Jump 110 (saltar a la posición de memoria 110)

##### Directo
[|imagen del word]
+ El operando está en la dirección referenciada por A
+ Operando = $[A]$
+ Ej: ADD $[941](A = A+[941])$

##### Indirecto
![[Pasted image 20230912171436.png]]
* A es un Puntero
* Operando = $[[A]]$
* Usos: 
	* Acceso a Arrays, Listas u otras estructuras.
	* Aumenta el espacio de direccionamiento.
* Existe acceso múltiple a la memoria para encontrar el operando.
* Ventaja
##### Registro

##### Desplazamiento



#### Ejemplos assembler



#### Resumen modos de direccionamiento



### Diseñando una ISA

<u>Algunas características:</u>

+ Memoria principal ocupada por el programa.
+ Tamaño de la instrucción (en bits)
+ Complejidad de Instrucción
+ Número de instrucciones disponibles.
+ Tamaño de la instrucción
+ Cuántos operandos
+ Cuántos registros.
(...)

Tenemos que ver *Las Reglas del Juego* para ver cómo armamos una ISA, qué cosas tenemos disponibles y qué no.


