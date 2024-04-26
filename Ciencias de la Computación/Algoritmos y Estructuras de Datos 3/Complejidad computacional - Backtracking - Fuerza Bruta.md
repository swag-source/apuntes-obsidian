***
Dentro de la teoría de complejidad computacional, tenemos distintos conceptos para entender las reglas del juego.
+ **Problema:** Descripción de los datos de entrada y respuesta a proporcionar para cada valor de entrada posible.
+ **Instancia:** Dado un problema determinado, una *instancia* de un conjunto válido de datos de entrada.

-> Suponemos una ***Máquina RAM***
1. La memoria está dada por una sucesión de celdas numeradas. Cada celda puede almacenar un valor de *n* bits.
2. Se tiene un *programa imperativo* no almacenado en memoria, compuesto por asignaciones y las estructuras de control habituales.
3. Las asignaciones pueden acceder a celdas de memoria y realizar las operaciones estándar sobre los *tipos de datos*.

-> Sabemos cuanto cuesta cada operación (es decir, su ***tiempo de ejecución***)
1. Lectura y escritura $\longrightarrow$  $O(1)$
2. Asignaciones $\longrightarrow$ $O(1)$
3. Operaciones entre valores lógicos (if, elseif, else) $\longrightarrow$ $O(1)$
4. Sumas y restas $\longrightarrow$ $O(n)$
5. Multiplicaciones y divisiones $\longrightarrow$ $O(n \ log(n))$

-> **Complejidad de un algoritmo** A:
$$f_{A}(n) \ = \ max_{I:|I|=n} \ T_{A}(I)$$

* Lo que nos dice la expresión es que, de todos los tiempos de ejecución, la complejidad toma el valor de *costo máximo de ejecución de un algoritmo*.

[[Complejidad computacional]] -> Repaso de la Notación $O, \Theta, \Omega$

### Optimización
***
**Problemas de optimización**: La idea de estos problemas consiste en encontra la mejor solución *posible* dentro de un conjunto:

$$z^* \ \ = \ \ \max_{x \in S} \ \ f(x) \ \ \ \ \ \text{o bien} \ \ \ \ \ z^* \ \ = \ \min_{x \in S} \ \ f(x)$$

* La función $f : \ S \  \longrightarrow \ \mathbb{R}$ se denomina ***función objetivo*** del problema.
* El conjunto *S* representa la ***región factible*** y los elementos $x \ \in \ S$ se llaman **soluciones factibles**.
* Ese valor $z^* \ \in \mathbb{R}$ es el **valor óptimo** del problema. Cualquier solución $x^* \in S$ tal que $f(x^*) = z^*$ se llama un **óptimo** del problema

-> En resumen, dado un problema, tenemos un conjunto de soluciones posibles. Si encontramos una solución copada que pertenezca a $S$ entonces esa será una solución óptima al problema.


Algunos problemas dentro del área de la optimización actualmente son:
=> ***optimización combinatoria**:* dado un problema donde la región factible $S$ está dada por combinación/permutación de un conjunto/subconjunto finito de elementos. Un ejemplo de esto son los caminos de $A \longrightarrow B$ en un grafo $G$.


### Algoritmos de Fuerza Bruta
***
**Algoritmos de fuerza bruta:** Para un problema de optimización combinatoria, consiste en generar **TODAS** las soluciones factibles (implica bastante costo computacional) y quedarse con la mejor solución. => *vamos a ir probando hasta encontrar la mejor solución*.

1. Se los suele llamar algoritmos de *búsqueda exhaustiva* o *generate and test*.
2. Se trata de una técnica fácil de implementar y existe una regla que => si existe alguna solución para mi problema, la misma siempre se encontrará con esta técnica.
3. Es una técnica muy trivial pero útil en muchos casos.

Habitualmente los problemas de tipo *brute force* tiene una **complejidad exponencial**.

Para entender un poco más esta filosofía para resolver problemas, introducimos el ejercicio de la mochila o *knutsack problem* el cual consiste en:
* Dada una mochila con capacidad $C$, una cantidad de objetos $n$ que queremos poner en la mochila. Todo elemento en la mesa tiene un peso $P_{i}$ y un beneficio $B_{i}$. Nosotros queremos encontrar la mejor forma de llevar elementos que **maximizan** el beneficio de los elementos sin exceder la capacidad que la mochila puede cargar.

**Datos de entrada**
* Capacidad $C \in \mathbb{N}$ de la mochila (peso máximo)
* Cantidad de objetos $n \in \mathbb{N}$
* Peso $p_{i} \in \mathbb{N}$  tal que $i \in \{1, ..., n\}$ 
* Beneficio $b_{i} \in \mathbb{N}$ tal que $i \in \{1, ..., n\}$ 

![[Pasted image 20240320114003.png]]

A partir de esto se nos arma un árbol recursivo donde podemos observar todas las posibles combinaciones para elementos en la mochila y decidir cuál es la combinación que maximiza el problema. => **árboles recursivos, técnica de complejidad con divide & conquer.**

* Este pequeño agregado de $peso(S) \ \leq \ C$ hace que nuestra solución comienza a tener una pinta de **backtracking**. 
* Empezamos con un caso base y construimos la solución que queremos a partir de la exploración recursiva. En este caso, agregamos el $k$-esimo elemento a nuestra **región factible** y seguimos explorando las soluciones.

### Backtracking
***
**Filosofía de backtracking:** la idea detrás de backtracking consiste en recorrer todas las posibles configuraciones de soluciones al problema qu queremos resolver, eliminando las *configuraciones parciales* (es decir, no llevan a nada).

-> Generalmente se suele representar como un *vector* $a = (a_{1}, ..., a_{n})$ a la solución candidata para nuestro problema tal que  $a_{i} \in A_{i}$.
-> El espacio de soluciones $S$ está dado por el producto cartesiano $A_{1} \ \times \ ... \ \times A_{n}$.

Como vimos en el ejercicio de la mochila, con nuestro problema *extendido*, lo que nos interesa es ir construyendo durante cada recursión soluciones que agreguen elementos posibles. 

-> $a = (a_{1}, ..., a_{n})$, $k < n$, agregamos un elemento más tal que $a_{k+1} \in S_{k+1} \subseteq A_{k+1}$ al final del vector. => basicamente meter un nuevo elemento a la solución.

-> Si $S_{k+1}$ es vacío, retrocedemos a la solución anterior.

Este arbol podemos optimizarlo para no tener que visitar todas las soluciones, este proceso se denomina *podado*. => **cuando una solución parcial no nos llevará a una solución válida.**

-> La raíz del árbol se corresponde con el vector vacío y los vértices del primer nivel serán soluciones parciales definidas por el $i$-ésimo elemento (con $i$ el nivel del árbol).

-> Las soluciones completas se corresponden con las hojas del árbol, es decir, con el último nivel del árbol.

#### Tipos de podas
* **Factibilidad:** ninguna extensión de la solución parcial derivará en una solución válida del problema.
* **Optimalidad:** Ninguna extensión de la solución parcial derivará en una solución óptima.
* **Propiedad dominó:** Si encontramos una solución que ya no cumple una propiedad principal, entonces todos sus descendientes tampoco lo harán.

![[Pasted image 20240331123025.png]]
![[Pasted image 20240331123102.png]]

Para este problema con las podas de optimalidad que pedimos, nuestro árbol eliminará las soluciones repetidas: *ej:* (c1, c3, c2) $\equiv$ (c3, c1, c2.)

![[Pasted image 20240331123204.png]]

Ahora, una vez que eliminamos todos los elementos repetidos, simplemente consistirá en verificar las sumas de cada solución parcial en función de k y decidir:
* Vamos guardando la suma de los elementos de la solución.
* Si encontramos una solución tal que $\sum \\ (c_{1}, ..., c_{i}) = k$ entonces es válida y **dejo de extendrla.**
* Si $\sum \\ (c_{1}, ..., c_{i}) > k$ => **propiedad dominó** y todos sus sucesores serán *soluciones inválidas* y no se arregalan al extenderlas.

