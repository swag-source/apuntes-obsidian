***
## Conceptos clave
1. *Correctitud*
2. *Verificación*
3. *Memoria dinámica*
4. *Conjuntos Acotados*

### Repaso
* TAD define el qué. Tiene estado y operaciones descriptas mediante pre y post condición.
* Una implementación define el cómo. Tiene un estado, invariante de representación, función de abstracción y algoritmos para las operaciones.
* Un TAD puede tener muchas implementaciones, según los requerimientos y el contexto de uso (por eejemplo de eficiencia).
* No necesariamente la estructura que elegimos para representar un concepto/dominio cumple con las propiedades del conjunto que queremos representar.
	* **Ej:** representamos un conjunto {} a partir de un arreglo \[\]

### Verificación
***
* Nos interesa demostrar si la implementación de un TAD es correcta respecto a la especificación que nosotros hicimos.
	* Utilizamos diagramas conmutativos para verificar la correctitud

![[Pasted image 20230926175314.png]]

* Para cada operación, hay que demostrar que:
-> Conserva el invariante.
***-> El algoritmo respeta la pre y postcondición del TAD.***

*Ej: Conservación del invariante y Correctitud del algoritmo - Ejemplo*

-- Insertar ejemplo --

* Como siempre, tenemos que probar que **InvRep(p') ==> *wp*(código, InvRep(p'))**
* Para probar la correctitud del algoritmo, queremos ver que:
	* Partir del requiere TAD ==> requiere ImplementaciónPunto
	* requiere ImplementaciónPunto ==> asegura ImplementaciónPunto
	* Asegura ImplementaciónPunto ==> Asegura TAD

Es decir.

PreTAD ==> (PreImple && PreImple ==> PostImple && PostImple) ==> PostTad entonces
**PreTAD ==> PostTAD**

* Todo el camino de (PreImple && PreImple ==> PostImple && PostImple) es exactamente igual a la parte de Especificación y Correctitud de una implementación cuando se veía la especificación.
* Solo metemos en las "puntas" la precondición del TAD y la postcondición del TAD.


### Conjuntos Acotados
***
* Charlamos formas distintas para representar conjuntos acotados.
* Mencionamos representaciones como Array con un invariante que no permita repetidos.
* Mencionamos cuestiones de complejidad (crear, mover, borrar, buscar, agregar, etc.)
* **Función carácterística** <=> **Función indicadora (proba)**
	* Me devuelve 1 si la operación/valor pertenece al conjunto, 0 si no.
* *Ej: tengo un conjunto acotado por un número (ejemplo tipo Counting sort)*.

A\[i\] = T <==> i pertenece C
\[0,0,0,1,0,0,1,0,0,0,1\] = \[3,6,11\]

* Esta forma es muy eficiente para buscar, borrar, agregar, pertenece, etc.
* Esta idea es muy costosa a nivel "alocación de memoria" en función del número más grande y "tiempo de operación" es muy alto.
* Podemos introducir la idea de **Complejidad Espacial**. Este algoritmo funciona muy bien si tenemos números chicos.
* Es decir, cuanto más grande es el número, más complejo es espaciar memoria para inicializar un conjunto.

*Ej:* \[1,2,3,4\] = \[0,1,1,1,1\] pero \[3,4,9139837\] = \[0,0,0,1,1,0,.......\ -> (i = 9139837) = 1]

* Introducimos el TAD BoundedSet (Conjunto Acotado por N) 

-- Insertar TAD BoundedSet -- 

* Resto de la clase consistió en demostrar la correctitud de este conjunto.


### Conjuntos acotados (Memoria Dinámica)
***
* *Problema recurrente:* planificar la utilización de la memoria.
* Podemos planificar la memoria **CUANDO SABEMOS** cuanta memoria necesitamos.
* Pero ¿**si no sé cuanta memoria necesito**? -> *Memoria Dinámica (va cambiando).*
* Puedo ir pidiendo memoria cuando necesitamos.

*Memoria dinámica:* es la capacidad de un algoritmo de utilizar memoria si y solo si el programa necesita utilizar memoria. Nos permite la gestión de memoria, no con límites establecidos a priori.

* La Memoria Dinámica es el mecanismo a través del cual los lenguajes nos proveen de primitivas para almacenar información cuando no sabemos de antemano cuánto espacio necesitamos o cuando la cota superior es muy grande.
* La organización de la memoria se divide en:
	--> **Stack**(pila): memoria estática.
	--> **Heap**(montón): memoria dinámica.

### Heap
* El "Heap" es un espacio de memoria para almacenar objetos (estructuras y arreglos) a los que se accede por medio de punteros o referencias.
* Estos elementos pueden contener referencia a otros elementos del heap.
* La memoria del Heap se puede pedir de distintas maneras.