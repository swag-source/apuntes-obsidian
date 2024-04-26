***
**Resumen tipo de registros en x86**
![[Pasted image 20240416112331.png]]

### Contrato con funciones
***
Hemos visto que para mantener una estructura ordenada en memoria, es necesario realizar una alineación de estos datos en memoria. => Garantía en la ubicación de los datos en memoria.

Cuando declaramos una función en C:

```
int32_t producto(int32_t *arr, uint32_t length);
```

Estamos especificando las siguientes reglas:
* Existe una función en memoria llamada producto.
* Tenemos una función *producto* que devuelve un elemento de tipo *int32_t*.
* La función *producto* recibirá dos parametros, el primero de tipo puntero y el segundo de tipo uint32_t.

Si nosotros garantizamos que la información es pasada hacia la función de la forma en la que nos pide dicha implementación
* El compilador se encargará de respetar este contrato automáticamente (GCC, Clang)
* Podemos querer llamar a una función desde nuestro código de Assembler la cual fue implementada en C o viceversa.

Para llamar a una función en C desde ASM o ASM desde C, tenemos que definir el *alcance de nuestro contrato* en términos de la plataforma particular.

=> Introducimos la noción de **ABI** (Application Binary Interface).

### ABI
***
* Cuando queremos exponer una interfaz parecida a quien desarrolla código en bajo nivel vamos a tener que definir contratos especı́ficos para la plataforma.

* Estos contratos especı́ficos se llamarán Interfaces Binarias de Aplicación (ABIs) y definen la forma en que las funciones serán llamadas, cómo se pasan los parámetros y que invariantes estructurales deben hacerse valer.

=> **Contratos importantes de la ABI:**
* Instruction set,
* Alineación y tamaño de los tipos de datos primitivos.
* Enviar y recibir información usando funciones del sistema (Convención para **SYSCALLS**)
* Enviar y recibir información usando funciones del usuario (**Convención de llamada**).

*Pregunta:* Cuando hacemos una llamada a función ¿Qué pasa con los registros? ¿Se conservan sus valores o se pierden al regresar del llamado?

**Depende**:
* **Registros volátiles:** La función llamada no tiene que conservarlos.
* **Registros no volátiles:** Si la función llamada los cambia, debe restaurarlos antes del return.

Si nuestra arquitectura es de 64 bits => nuestra **ABI** define que *utiliza registros de propósito general y la pila* <=> **System V AMD64 ABI**

Si nuestra arquitectura es de 32 bits => *sólo utiliza la pila.* <=> **System V i386 ABI**

### Uso de la pila
***
* La pila es una estructura **EN LA MEMORIA**.
* Sirve para guardar información **local** a una función.
* Contiene información de **contexto**: parámetros, dirección de retorno.

![[Pasted image 20240416114713.png]]

En una arquitectura de 64 bits:
* La pila está alineada a 8 bytes.
* Para el llamado de función se alinea a 16 bytes.

En una arquitectura de 32 bits:
* La pila está alineada a 4 bytes.
* Para el llamado a función tambien se alinea a 16 bytes.

### Interacción con la pila
***
Cuando hablabamos de tener datos temporales, los mismos se encontrarán en la pila.

Nos interesa saber de qué manera podemos acceder a esos datos temporales y a los registros pasados por la pila.
* Estos registros dentro del stack se encuentran en posiciónes de memoria **relativas al RBP**.

Dentro del código de nuestra función de ASM, definimos dos secciones particulares:
* **Prólogo**: es donde se reserva espacio en la pila para **datos temporales**, se agrega **padding** para mantenerla alineada a 16 bytes y se preserva los valores de los **registros no volátiles**.
* **Epı́logo**: es donde restauramos los valores de los **registros no volátiles** y devolvemos la pila a su estado inicial. => Sección donde liberamos la pila.


### System V 64 bits
***
Cuando trabajamos en 64 bits System V, se cumplen las siguientes especificaciones:
* **Registros no volátiles**: RBX, RBP, R12, R13, R14 y R15.
* El valor de retorno de una función será almacenado en RAX para enteros (punteros para la ABI) y XMM0 para flotantes.
* Al salir de una función, la pila debe volver al estado en el cual ingresó (#PUSH = \#POP).
* Se debe alinear la pila a 16 bytes antes de realizar una llamada a una función.
* Los parámetros enteros se pasan de izquierda a derecha en: *RDI, RSI, RDX, RCX, R8, R9* respectivamente.
* Los parámetros flotantes se pasan de izquierda a derecha en XMM0, XMM1, XMM2, XMM3, XMM4, XMM5, XMM6, XMM7 respectivamente.

![[Pasted image 20240416120417.png]]

***Pregunta importante:*** *¿Por qué hace falta alinear la pila a 16 bytes si*
*hacemos una llamada a otra biblioteca?*
* Las funciones pueden hacer uso de operaciones con registros largos (trabajan con información larga) y deberán guardarlos en registros tipo XMM, YMM. Estos registros requieren estar alineados a 16 bytes.