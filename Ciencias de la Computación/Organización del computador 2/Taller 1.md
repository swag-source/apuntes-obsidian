***

### Ejercicio 1
**1.a:** ¿Cuál es el tamaño en bits de una dirección de memoria en la arquitectura IA-32 y cuál es la unidad más pequeña que podemos direccionar?

**Rta:** En la arquitectura IA-32, el tamaño en bits de una dirección de memoria es de 32 bits, mientras que la unidad más pequeña que podemos direccionar es a byte (8 bits).

**1.b:** ¿Cuántos registros de propósito general hay en IA-32 y qué tamaño tienen?

**Rta:** En IA-32 tenemos 8 registros de propósito general (EAX, EBX, ECX, EDX, ESI, EDI, EBP, ESP) con tamaño de 32 bits. 

**1.c:** Busquen en el manual qué guarda el registro EIP (Instruction Pointer) e indiquen su tamaño en bits. ¿Por qué motivo creen que el EIP tiene ese tamaño en bits?

**Rta:** El Instruction Pointer (EIP) es un registro dedicado para apuntar durante la ejecución de un programa a la siguiente instrucción a fetchear. El tamaño de este registro es de 32 bits ya que, las direcciones almacenadas en memoria tendrán también un largo de 32 bits.

### Ejercicio 2
**2.a:** Busquen en la sección del manual qué guarda el registro EFLAGS e indiquen su tamaño en bits.
**Rta:** El registro EFLAGS guarda los flags de estado del procesador. Su tamaño en bits es de 32.

**2.b:** En el formato del registro, busquen los siguiente bits e indiquen para qué son y en qué posición del registro están almacenados:
* Flag de Overflow
* Flag de Signo
* Flag de Interrupciones
**Rta:** El Flag de Overflow se encuentra en el bit 11. El mismo se encarga de encender el flag en el caso que una operación produzca overflow. Por otro lado, el Flag de Signo se encuentra en el bit 7 y se encarga de setear si un número con el que trabajamos es positivo o negativo. Por último, el Flag de Interrupciones, ubicado en el bit 9, se utiliza para permitir una interrupción o no dentro del flujo de ejecución del programa.

**2.c:** Indiquen si en la arquitectura Intel 64 se usa el mismo registro. En caso que sea otro, indiquen su tamaño y la relación que tendría con el de IA-32.
**Rta:** Si, en Intel 64 se utiliza el mismo registro que IA-32 con la extensión en la parte high del registro por otro nuevo de 32 bits, extendiendo así nuestro registro a 64 bits. Este registro recibe el nombre de RFLAGS. 


### Ejercicio 3
**3.a:** ¿Para qué es necesaria la pila? ¿Donde está ubicada?
**Rta:** La pila es necesaria para sopoRtar las llamadas a subrutinas y procedimientos. Es decir, nos permite relacionar los parametros necesarios para su ejecución. La pila está ubicada en memoria principal. 

**3.b:** ¿Para qué sirven los registros ESP y EBP?¿Qué consideraciones debemos tener al trabajar con cada uno ellos?
**Rta:** El registro ESP (Extended Stack Pointer)  se utiliza para tener cuenta de la ubicación donde se encontrará la proxima posición libre para inseRtar instrucciones mientras que el  EBP (Extended Base Pointer) es un registro que se utiliza en la implementacion de funciones escritas en ASM para tener una noción de iteración en la pila.

**3.c:** ¿Qué registro se pushea en la pila al hacer un CALL?
**Rta:** Al realizar un CALL a una función lo que hacemos es guardarnos el puntero de la instrucción en el stack. Luego de finalizada la funcion (RET) se fetchea el valor del stack y se continua con la ejecucion del programa.

**3.d**: ¿Qué debe asegurarse el programador antes de llamar a un RET cuando esta escribiendo una subrutina? ¿Cómo lo asegura?
**Rta:** Cuando ejecutamos una subrutina y guardamos en el stack el valor del EIP tenemos que asegurarnos de saber exactamente donde encontrar el valor del return instruction pointer para volver a la posición correcta. Lo aseguramos con el EBP por ejemplo. El sp va a apuntar al ip despues


**3.e:** ¿Cuál es el ancho de la pila en modo 32 bits y en 64 bits? (tamaño del dato de PUSH y POP)
**Rta:** En la arquitectura 64 bits, las instrucciones push y pop decrementan o incrementan el stack utilizando un largo de 64 bits. Al igual en la arquitectura de 32 bits, las mismas alinean con 32 bits el ancho de la pila.


**3.f:** Luego de responder las preguntan anteriores, discutan en grupo si el EBP podría ser usado para guardar datos que no sean la base de la pila. ¿Qué opinan?
**Rta:** Sí, porque hace referencia a direcciones de memoria.

## Set de Instrucciones IA-32 e Intel 64
**4.a:** Observen el formato de la instrucción y respondan, ¿cuántos operandos recibe, de qué tipo son y qué tamaño tienen?
**Rta:** 
**ADD:** la operación add toma dos operandos y guarda la suma de ambos en el operando fuente.
*Ej.*
ADD rax, 0x01

**MOV:** 2 Operando copia el op destino a source  puede ser registro-memoria o registro-registro 8 a 32 bits.
*Ej.*
MOV [rbx], 0x00

**DEC:** 1 Operando destino de tipo registro o memoria. Decrementa en 1 unidad. Preserva el flag CF.
*Ej.*
DEC rax 


**JZ:** es un salto condicional que interrumpe el flujo del programa cuando el flag Z está prendido RECIBE UN OPERANDO DE REL8 DE 8 BITS
*Ej.*
JZ rax

**JE:** el salto condicional jump equal me garantiza un salto cuando el flag E está prendido RECIBE UN OPERANDO DE REL8 DE 8 BITS
*Ej.*
JE rax

**4.d:** ¿Qué diferencia existe entre JZ y JE?
**Rta:** La condición de salto.

**Ejercicio 2: Programa en ASM**

```
section .data
    msg db "Ramiro Seltzer + 715/22 + 'Dividir y Conquistar'.", 0ah
    ; 0ah es un carácter de nueva línea

section .text
    global _start
    ; el punto de entrada principal del programa y se hace global, 
    ; lo que significa que puede ser referenciado desde fuera del archivo actual.

_start:
    ; Preparando para invocar sys_write para imprimir "Hello world!"
    
    mov rax, 1
    ; el registro rax generalmente se usa para almacenar el número de la syscall que se desea invocar. 
    ; En este caso, 1 corresponde al número de la syscall sys_write, 
    ; que se utiliza para escribir datos en un archivo o en una salida estándar.
    
    mov rdi, 1
    ; En el contexto de la syscall sys_write, 
    ; rdi generalmente se utiliza para especificar el descriptor de archivo. 
    ; Un descriptor de archivo de 1 generalmente corresponde a la salida estándar, la pantalla.
    
    mov rsi, msg
    ; rsi generalmente se utiliza para especificar la dirección de memoria del búfer que contiene los datos 
    ; que se van a escribir. En este caso, msg es una etiqueta que representa 
    ; el comienzo de una cadena de caracteres en memoria que contiene el mensaje que deseamos escribir.
    
    mov rdx, 13
    ; En sys_write, rdx se usa para especificar la cantidad de bytes que se desean escribir
    ; desde el búfer indicado por rsi
    
    syscall
    ; La CPU cambia al modo kernel, 
    ; y el sistema operativo identifica la syscall según el valor almacenado en rax. 
    ; En este caso, como rax contiene 1, el sistema operativo ejecutará la syscall sys_write, 
    ; que escribirá los datos del búfer msg en la salida estándar.

    ; Preparando para invocar sys_exit para terminar el programa
    
    mov rax, 60
    ; Cargar el número de la syscall para la función sys_exit en rax
    
    mov rdi, 0
    ; Establecer el código de salida en rdi (0 para indicar éxito)
    
    syscall
```




Dada una matriz D de n x n naturales, queremos encontrar una permutación $\pi^{1}$ de {1,...,n} que minimice $D\pi_{(n)}\pi_{(1)} + \sum_{i = 1}^{n-1} D\pi_{(i)}\pi_{(i+1)}$. Por ejemplo, si:

D = (
0 1 10 10
10 0 3 15
21 17 0 2
3 22 30 0
)

entonces $\pi(i) = i$ es una solución opatima.

Ayudame por favor a entender el propósito del ejercicio ya que non entiendo que es lo que nos esta pidiendo. No queremos una solución.