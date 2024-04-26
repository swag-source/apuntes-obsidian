***
**Ejercicio 1**
En cada uno de los siguientes escenarios, indique qué método de ordenamiento de los estudiados en clase utilizaría y porque. 
* Se tiene un arreglo de naturales A\[1..n\] y se desea ordernarlos (de mayor a menor) solamente sí la suma de los k elementos más grandes es menor que X. Se desea realizar ésta tarea de forma eficiente, dandola por terminada lo antes posible si la condición no se cumple. (la verificación forma parte de la tarea, no se debe verificar la condición antes de empezar a ordenar) 
* Se tiene un arreglo redimensionable de naturales A\[1..n\] y se desea ordenarlos. Sin embargo, durante el proceso de ordenamiento es posible que se agreguen nuevos elementos al final del arreglo.

**Ejercicio 2**
1. Dar la propiedad que hace que los ABB sean AVLs. O sea, el invariante. (en castellano) 
2. Dar un algoritmo lineal que verifique sí un ABB cumple con el invariante del AVL. Justificar la complejidad obtenida. 

**Ejercicio 3**
Explicar por que las colas de prioridad son ineficientes para realizar búsquedas pese a ser un árbol balanceado y por qué no se pueden modificar para que lo sean sin perder una de sus propiedades fundamentales.

**Ejercicio 4**
* ¿Cuál es la relación entre Weakest Precondition y la Tripla de Hoare?
* Enunciar la propiedad de monotonía de la Weakest Precondition y explicar la intuición detrás de la propiedad.
* Demostrar el corolario de la monotonía: 
		 Si $P ⇒ wp(S_{1}, Q)$ y $Q ⇒ wp(S_{2}, R)$ entonces $P ⇒ wp(S_{1}; S_{2}, R)$

#### Respuestas
**Ejercicio 1**

**Ejercicio 2**
Recordar que los ABB son una estructura de datos que permite la búsqueda de un elemento en tiempo logaritmico. El invariante de representación de esta estructura nos permite realizar esta operación en dicha complejidad ya que se basa en los tres principios:
* Todo elemento a la izquierda de un nodo raiz deberá ser menor a su padre
* Todo elemento a la derecha de un nodo raiz deberá ser mayor a su padre
* Todo subarbol izquierdo deberá ser un ABB
* Todo subarbol derecho deberá ser un ABB

El inconveniente que surge a partir de esta estructura es que, a partir del invariante de representación que planteamos, nunca garantizamos propiedades sobre la altura del árbol; por ende, podríamos tener un árbol de números ordenados donde buscar me costaría una complejidad lineal (como es el caso del ABB). 

Si pedimos a nuestro invariante, a su vez, que para cada árbol binario de búsqueda, la altura de sus respectivos árboles derechos e izquierdos tengan como diferencia a lo sumo 1 nivel de profundidad, introducimos la noción de AVL. Esto se llama: 
Factor de Balanceo => FactorBalanceArbol(nodo) = |longitud(subarbol_derecho) - longitud(subarbol_izquierdo)| $\leq$ 1.

b) 

Queremos proponer un algoritmo que verifique en tiempo lineal si un ABB es AVL. Para esto recibimos como entrada un ABB y queremos devolver un booleano que determina si la entrada representa un AVL o no. 
Mi implementación consiste en la noción del Factor de Balanceo calculando la longitud de los subarboles izquierdos y derechos de mi raiz y determinar si la suma de su longitud es menor o igual a 1; si lo fuese devolver True (ya que cumple con ser AVL) y False en el caso contrario.

```Pseudocodigo
Proc checkearAVL(a: ABB) -> Bool {
	int altura;

	altura = abbHeight(a.hijoDerecho) + abbHeight(a.hijoIzquierdo)

	if(altura <= 1)
		return True;
	else
		return False;
}

Proc abbHeight(a; ABB) -> int {
	if(a.root) == NULL
		return 0;
	alturaIzq = abbHeight(a.hijoIzquierdo)
	alturaDer = abbHeight(a.hijoDerecho)
	return max(alturaIzq, alturaDer) + 1
}
```

**Ejercicio 3**
Explicar por que las colas de prioridad son ineficientes para realizar búsquedas pese a ser un árbol balanceado y por qué no se pueden modificar para que lo sean sin perder una de sus propiedades fundamentales.

Supongamos que tenemos la siguiente Cola de Prioridad implementada a partir de un Heap. Sin importar si la prioridad de nuestra estructura está puesta en el máximo o en el mínimo elemento de la lista, el invariante de representación nos garantiza que, para que sea un Heap Binario, se deben cumplir los siguientes requisitos:
* Sea un árbol binario balanceado.
* El valor de cada nodo (raíz) deberá ser mayor/menor que el valor de sus hijos izquierdos y derechos.
* Todo subárbol de la raíz deberá ser un Heap.
* Sea izquierdista para garantizar el balanceo del árbol.

Podemos observar como, a partir de esta descripción, en ningún momento se imponen condiciones sobre la posición exacta que deberán tomar los elementos del árbol que son menores a la raíz. Por esta razón, si quisieramos ubicar un elemento que sabemos, en definitiva, que es menor/mayor que la raíz, la complejidad temporal de peor caso para la búsqueda será siempre lineal con respecto a la entrada; tendríamos que iterar sobre toda la rama izquierda de la raíz y luego sobre toda la rama derecha.

Si quisieramos mejorar el tiempo de búsqueda para obtener una complejidad logarítmica, estaríamos sacrificando una propiedad fundamental del Heap que es la posibilidad de obtener máximo o mínimo en tiempo constante => O(1).



**Ejercicio 4**

En el paradigma de la verificación de programas, se hace empleo de diversos mecanismos para verificar que un programa dispuesto a resolver un problema (el cual previamente tuvo que ser especificado) sea correcto respecto a la especificación. 

Una de las herramientas utilizadas es la Weakest Precondition de un programa S, notada como wp(S,Q); la precondición más debil para un programa S que, al finalizar su ejecución, garantiza que el estado final cumple con la postcondición Q. Por otra parte, se dice que un programa es correcto respecto a su especificación si la Tripla de Hoare $\{P\} \ S \ \{Q\}$ se cumple.

Observamos que, en este contexto, el predicado P puede ser cualquier precondición posible que satisfaga el programa S; en nuestro caso y dentro de lparadigma de la verificación de programas, nos interesa encontrar un predicado P tal que:

$$P \Longrightarrow_{L} wp(\textbf{S}, Q)$$
es decir, mi predicado P se encuentre contenido "dentro" del conjunto de la WP para dicho programa S:

Este requisito de encontrar la WP de un programa nos sirve para verificar que estemos siendo lo suficientemente restrictivos para admitir valores de entrada erróneos pero también lo suficientemente abiertos a que, para todo valor del dominio de la WS, exista un elemento en el conjunto de llegada.

En conclusión, al probar que $P \Longrightarrow_{L} wp(\textbf{S}, Q)$ estaríamos probando que *P*  también cumple con la Tripla de Hoare, implicando así que nuestro programa con su precondición sea válida.

b) La propiedad de monotonía de la Weakest Precondition establece que
$$P \Longrightarrow_{L} Q \ \therefore \ wp(\textbf{S}, P) \ \Longrightarrow_{L} wp(\textbf{S}, Q) \ $$
aquí observamos que este corolario relaciona los predicados P y Q con sus precondiciones más débiles. La noción detrás de este corolario es demostrar que si P es un predicado que se encuentra contenido dentro del predicado Q (por la implicancia), entonces sus precondiciones deberán "automáticamente" cumplir la misma propiedad.

**Demostración**
Sabemos que $P \Longrightarrow_{L} wp(\textbf{S}, P)$ por Axioma 1 y $Q \Longrightarrow_{L} wp(\textbf{S}, Q)$. Por la premisa sabemos que $$P \Longrightarrow_{L} Q$$por ende $$(P \Longrightarrow_{L} Q) \Longrightarrow_{L} (wp(\textbf{S}, P) \ \Longrightarrow_{L} wp(\textbf{S}, Q))$$por transitividad, como se quería probar.
