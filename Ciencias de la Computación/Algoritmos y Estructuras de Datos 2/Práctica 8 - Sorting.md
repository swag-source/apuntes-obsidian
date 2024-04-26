***

Ejercicio 2)
*Imagine secuencias de naturales de la forma s = Concatenar (s′, s′′), donde s′ es una secuencia ordenada de
naturales y s′′ es una secuencia de naturales elegidos al azar. ¿Qué método utilizaría para ordenar s? Justificar.*

-> supongamos que S = \[1,2,3,4,7,10,9,8] => S = concatenar(\[1,2,3,6\], \[5,10,9,8\]).

* Divido S a partir del índice i = |s'|, esto me garantiza que voy a separar el conjunto de números desordenados.
	=> \[1,2,3,6\] \[5,10,9,8\].

* Ordeno con algún algoritmo eficiente la segunda lista de elementos.
	=> \[1,2,3,6\] \[5,8,9,10]

* Vuelvo a concatenar las listas
	=> \[1,2,3,6,5,8,9,10]

* Reordeno esta lista una última vez.
	=> \[1,2,3,5,6,8,9,10]


Ejercicio 3)
*Escribir un algoritmo que encuentre los k elementos más chicos de un arreglo de dimensión, donde k ≤ n.
¿Cuál es su complejidad temporal? ¿A partir de qué valor de k es ventajoso ordenar el arreglo entero primero?*

**Algoritmo**
``` Pseudocódigo
int[] res;
int mínimo;
int i;
	
Function kMinimosArreglo(a: Array<Int>, k: Int) : Array<Int> {
	res = [];
	i = 0;
	while(i < k){
		res.insertar(obtenerMínimo(a));
		i++
	}

}

Function obtenerMínimo(a: Array<int>) : int {
	for(int i = 0, i < a.length, i++){
		mínimo = a[0];
		for(int j = i + 1, j < n, j++){ 
			if a[j] < mínimo {
				mínimo = a[j];
			}
		}	
	}
}
	
	-> busco el mínimo
	-> cada vez que lo encuentro, inserto en lista res
	-> repito k veces el ciclo
```



Ejercicio 4)
*Se tiene un conjunto de n secuencias {s1, s2, . . . , sn} en donde cada si (1 ≤ i ≤ n) es una secuencia ordenada de naturales. ¿Qué método utilizaría para obtener un arreglo que contenga todos los elementos de la unión de los si ordenados. Describirlo. Justificar.*





Ejercicio 5)
*Se tiene un arreglo de n números naturales que se quiere ordenar por frecuencia, y en caso de igual frecuencia, por su valor. Por ejemplo, a partir del arreglo \[1, 3, 1, 7, 2, 7, 1, 7, 3\] se quiere obtener \[1, 1, 1, 7, 7, 7, 3, 3, 2\].
Describa un algoritmo que realice el ordenamiento descrito, utilizando las estructuras de datos intermedias que
considere necesarias. Calcule el orden de complejidad temporal del algoritmo propuesto.*

**Idea**:
* Primero ordeno por *valor* los elementos del arreglo que me pasan como input
=> \[1, 1, 1, 2, 3, 3, 7, 7, 7]

* Teniendo todo ordenado, para cada elemento me guardo en una tupla el valor y su frecuencia.
=> \[(1,3),(2,1),(3,2),(7,3)]

* Ordeno nuevamente a partir de las frecuencias pero decrecientemente
=> \[(1,3),(7,3),(3,2),(2,1)\]

* Vuelvo a expandir
=> \[1, 1, 1, 7, 7, 7, 3, 3, 2]

**Algoritmo**
```
Pseudocodigo

Int[] arregloOrdenado;
Tupla[] elemPorFrecuencia;

O(nlog(n))
Function sortFrecuencia(a: Array<int>) {
    ArregloOrdenado = MergeSort(a);

	Compactar(ArregloOrdenado);

	// Ordenamos por frecuencia de forma descendiente
	res = MergeSort(Compactar) => O(nlog(n)) 

	Expandir(res);

}

O(n)
Function Compactar(a: Array<Int>) -> Array<int x int> {
    // Asignaciones => O(1)
    
    elemPorFrecuencia = [];
    valor = ArregloOrdenado[0];
    frecuencia = 1;
    tupla = (valor,freuencia)
    
    
    //Compactar Arreglo => O(n)
    for (int i = 1; i < ArregloOrdenado.Len; i++) { 
        if a[i] = valor Then
            frecuencia++;
        Else
            elemPorFrecuencia.insertarAtrás(tupla);
            valor = a[i];
            frecuencia = 1;
    }
    
    // tendré un arreglo del estilo [(1,3),(2,1),(3,2),(7,3)]
    
}

// O(n)
Function Expandir(a: Array<int x int>) -> Array<int> {
	res = [];
	for(int i = 0, i < a.lenth, i++) {
		for(int j = 0; j < a[i][1]) {
			res.insertar(a[i][0]);
		}
	}
}
```

Ejercicio 6)

**Idea:**
* Recibo mi array A = \[5,6,8,9,10,7,8,9,20,15\], fijo un puntero al primer elemento y voy iterando hasta que el *i = j+1* elemento no tenga diferencia de uno.
* Lo voy guardando en un array de arrays donde me guarde "esa diferencia".

=> \[\[5, 6\], \[8, 9, 10\], \[7, 8, 9\], \[20\], \[15\]\]
=> \[2, 3, 3, 1, 1\],

* Teniendo este arreglo, ordeno nuevamente por *frecuencia* o cantidad de elementos en los subarray.

=> \[\[8, 9, 10\], \[7, 8, 9\], \[5, 6\], \[20\], \[15\]\]
=> \[3, 3, 2, 1, 1\],

* Dentro de cada frecuencia miro los primeros elementos e intercambio de ser necesario.

=>\[\[7, 8, 9\], \[8, 9, 10\], \[5, 6\], \[15\], \[20\]\]

* Ordenado ahora por frecuencia y por tamaño de cada elemento, *expando* mi array.

=> \[7, 8, 9, 8, 9, 10, 5, 6, 15, 20\]


Ejercicio 7)
*Suponga que su objetivo es ordenar arreglos de naturales (sobre los que no se conoce nada en especial), y que cuenta con una implementación de árboles AVL. ¿Se le ocurre alguna forma de aprovecharla? Conciba un algoritmo que llamaremos AVL Sort. ¿Cuál es el mejor orden de complejidad que puede lograr?
Ayuda: debería hallar un algoritmo que requiera tiempo O(n log d) en peor caso, donde n es la cantidad de elementos a ordenar y d es la cantidad de elementos distintos. Para inspirarse, piense en Heap Sort (no en los detalles puntuales, sino en la idea general de este último).
Justifique por qué su algoritmo cumple con lo pedido.*

**Idea**

* Preguntar

```
Pseudocódigo

\\ Asumimos que tenemos una estructura de AVL para utilizar.

Func AVLSort(a: array) -> array<int> { 

	Compactar(a); \\ 

	
	
}
```


Ejercicio 9)
*Se necesita ordenar un arreglo(alumno) de forma tal que todas las mujeres aparezcan al inicio de la tabla según un orden creciente de notas y todos los varones aparezcan al final de la tabla también ordenados de manera creciente respecto de su puntaje.*

*Ejemplo*:  

\[(Ana, 1, 10), (Juan, 0, 6), (Rita, 1, 6),(Paula, 1, 7), (Jose, 0, 7), (Pedro, 0, 8)\]
=> \[(Rita, 1, 6), (Paula, 1, 7), (Ana, 1, 10), (Juan, 0, 6), (Jose, 0, 7), (Pedro, 0, 8)\]


*Idea*
* Como estoy trabajando con dos valores numéricos acotados (género y nota), ordeno primero por género decrecientemente. *Counting Sort*
* Teniendo ordenado por género, nuevamente ordeno por nota dentro del género.

```
Pseudocódigo

// Asumo que tengo una función Counting Sort tal que 


Func ordenarPlanilla(alu: array<Alumnos>) : array<Alumnos> {
	countingSort(alu); -> clave de ordenamiento: genero de forma decreciente
	countingSort(alu); -> clave de ordenamiento: notas de alumnos.

	return alu;
 }

=> O(n)
```


Ejercicio 12)
Se desea ordenar los datos generados por un sensor industrial que monitorea la presencia de una sustancia en un proceso químico. Cada una de estas mediciones es un número entero positivo. Dada la naturaleza del proceso se sabe que, dada una secuencia de $n$ mediciones, a lo sumo ⌊$\sqrt{n}$⌋ valores están fuera del rango \[20, 40\]. Proponer un algoritmo 
$\mathcal{O}$(n) que permita ordenar ascendentemente una secuencia de mediciones y justificar la complejidad del algoritmo propuesto.

```
Pseudocódigo

Function ordenarMuestra(a: array<int>) :array<int> {
	máximo = obtenerMáximo(a) -> O(n)
	res = [a.length];
	cuenta = [máximo]; -> inicializo mi array con tamaño del máximo elemento
	

	for(int i = 0; i < máximo, i++){
		cuenta[i] = 0;
	}

	// cuenta = [0, 0, 0, ... , máximo]

	for(int i = 0; i < a.length; i++){
		cuenta[a[i]]++;
	} -> O(n)
	
	for(int i = 0; i < cuenta.length - 1; i++){
		cuenta[i+1] += cuenta[i] + 1;
	} -> O(m)

} 

``` 








