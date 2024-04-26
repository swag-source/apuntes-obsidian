***
## Conceptos clave
* *Programación orientada a Datos*
* *Tipos de datos*
* *Punteros*
* *Arreglos*
* *Alineación de datos en Memoria*


### Memoria Principal
***
En nuestro esquema de memoria, la misma puede ser pensada como un *arreglo contiguo y ordenado de bits o bytes* que podemos determinar posiciones unívocas en la memoria (direcciones).

![[Pasted image 20240328113640.png]]


### Tipos de datos
***
Nuestra memoria principal tiene la capacidad de almacenar y representar tipos de datos para resolver nuestros problemas.
1. Tipos atómicos (int, char, float, bool)
2. Estructuras (structs)
3. Arreglos (arrays)
4. Punteros (pointers)

**Algunas medidas según su tipo**
-> ***Floats***: 2 bytes.
-> ***Char***: 1 byte.
![[Pasted image 20240328114118.png|500]]

-> ***Structs***: $\sum \text{tamaño por tipo de dato}$ =>  struct s{uint16 t i;char c;} = 2 bytes + 1 byte = 3 bytes

![[Pasted image 20240328114104.png|500]]

-> ***Arrays***: $$\text{Tamaño} = N \times \text{sizeof(type\_t)}$$ donde type_t es el tipo de dato que "utiliza" mi arreglo de elementos.
* A la hora de trabajar con punteros, nos referimos a ellos de dos maneras 
	* A partir de su posición en la región de memoria -> puntero a un elemento.
	* La región de memoria donde se encuentran los datos -> estructura.

![[Pasted image 20240328115012.png | 500]]

-> ***Punteros***: Los punteros son una referencia a una posición en memoria. Estas "flechitas" tienen que tener el tamaño de las direcciones de memoria de nuestra arquitectura.
* Si el tamaño del puntero fuera menor a la dirección de memoria, perderíamos "información" a la hora de traer el dato de ese puntero.

![[Pasted image 20240328115705.png | 500]]


## Punteros
***
Como dijimos antes, los punteros son datos numéricos del mismo tamaño que la dirección de memoria. 
* Declaraciones del tipo (val_type \*ptr) nos está indicando que queremos guardar un puntero a un elemento de tipo *val_type* llamado puntero.
* Ese puntero *ptr* = 0x1000 (por ejemplo) apunta a una dirección de memoria donde tenemos guardado el valor de un dato.
```
Dos ejemplos:
	int x = 4; -> Determinamos una posición de memoria para guardar la variable que llamamos X y le seteamos el valor 4.

	int *ptrX = &x; -> En otra posición de memoria, guardamos la posición de memoria donde se almacena el valor de X cuyo valor es 4.
```

![[Pasted image 20240328120831.png|500]]

-> **Operador de referencia** (&val) conseguimos la posición de memoria del valor *val*.
-> **Operador de desreferencia** (\*ptr) accedemos al dato apuntado por ptr.

```
Ejemplo: 
	uint16_t i = 3;
	struct s obj = {'a',4};
	float arr[3] = {123.43f, 2f, 3.5f};
	uint16_t *int_ptr = &i;
```

![[Pasted image 20240328121214.png | 500]]


*Ejemplo sencillo de desreferencia: Swap de dos letras*

```
char a = 'm';char b = 'n';
charSwap(&a,&b); -> Recibe como parámetros dos punteros

void charSwap(char* left, char* right){
	tmp = *left;
	*left = *right
	*right = tmp; // si hubiera sido right = tmp, estaría pisando el valor de right por la posición de memoria de tmp (right = 0x...)
}
```

![[Pasted image 20240328122516.png | 500]]

### Arreglos
***
* Los arreglos son simplemente una concatenación en memoria de elementos de un mismo tipo.
* Identificamos los arreglos a partir de la posición en memoria donde comienza.
* Podemos acceder a esa posición de memoria a partir de a\[i\] $\equiv$ \*(a+i) -> puntero a la posición donde empieza *a* y sumamos *i* para iterar sobre el arreglo.

*¿Podemos reproducir el funcionamiento de la indexación a partir de punteros?*

![[Pasted image 20240328125842.png]]
*Ejemplo de iteración a partir de punteros*.

La idea es guardarnos en una posición de memoria el puntero al principio del arreglo $A$ e iterar, sumando a esa posición de memoria \*ptr += i. Cuando accedemos a la posición que queremos, guardamos el contenido de esa posición guardado por \*ptr en una variable $d$.
 
![[Pasted image 20240328130017.png]]

### Alineación de datos en memoria
***
Muchas arquitecturas suponen un tamaño de lectura de memoria determinado (es decir, cada vez que leen información, traen siempre la misma porción de datos para leer). 
Supongamos que queremos leer el valor de $i$ y escribirlo en un registro $AX$. 

![[Pasted image 20240328131045.png]]

Observemos que si solamente podemos leer de a 4 bytes, tendremos que realizar *dos* lecturas de memoria:
-> **1° lectura**: \[0x0, 0x4\] => Ya agarramos una porción de i (parte baja)
-> **2° lectura:** \[0x4, 0x8] => Traermos el resto del dato (y también traeríamos una porción de C).

Claramente esto nos resulta más costoso que encontrar una forma de *alinear la información* en posiciones múltiplo de $n$ así cada dato que querramos leer nos cuesta 1 llamada. 

Este problema encuentra la siguiente solución:

![[Pasted image 20240328132017.png]]

* Sabiendo que el tamaño de la estructura está alineado al tamaño del atributo más grande, también tenemos que tener en cuenta la forma en la que alocamos dentro de una estructura los atributos. 
* A los datos de menor tamaño, lo que se hace es agregarle *padding* para rellenar los bytes faltantes tal que el dato ocupe siempre $n$ bytes de memoria.


