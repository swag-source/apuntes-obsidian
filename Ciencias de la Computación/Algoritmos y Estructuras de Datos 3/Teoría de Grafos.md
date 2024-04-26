***
Los grafos son modelos *matemáticos* que explican relaciones entre objetos.

* Esos "objetos" dentro de un gráfo se denomina **vértice** o **nodo**.
* Cada vértice o nodo se conecta a partir de **ejes**.  
* Son una forba conveniente y flexible de representar problemas de la vida real qu consideran una red como estructura subyacente. 
* Pueden ser redes físicas o abstractas (relaciones sociales, bases de datos, ecosistemas).
* Desarrollamos algoritmos para resolver problemas que se plantean a partir de una estructura sobre grafos.
* Cuando hacemos un estudio de un grafo, no me interesan las variables "intermedias"; solamente la relación entre los nodos.

![[Pasted image 20240422182222.png]]

### Puentes de Königsberg (Euler)
***
![[Pasted image 20240422183752.png]]
* Problema introducido por Euler en 1736.
* La ciudad planeó una estructura donde para llegar 
* Euler mostró que el problema *no tiene solución* y dio una condición necesaria para el caso general.
* *Hierholzer* mostró en 1871 que la condición es suficiente.
* Dado un grafo en particular, queremos ver si existe un camino tal que pasemos por todos los ejes de manera única para volver al punto de partida.
## El problema del caballo de Ajedrez
***
* En el *problema del caballo de ajedrez* nos interesa saber de qué manera podemos hacer que un caballo recorra el tablero entero de ajedrez sin dejar una posición no visitada. 
* Queremos encontrar un camino circular de un caballo de ajedrez pasando **EXACTAMENTE una vez** por cada nodo.
* *Camino Hamiltoniano/Ciclo Hamiltoniano:* Decimos que es un ciclo Hamiltoniano que cada vértice es visitado una sola vez.

![[Pasted image 20240422184833.png|500]]


### Grafos
***
-> Definimos un grafo $G = (V, E)$ es un par de conjuntos donde $V$ es un conjunto de *nodos* y $E$ un subconjunto del conjunto de pares no ordenados de elementos, distintos de $V$.

* Los elementos de $E$ son *nodos* del grafo $V$.

-> Dados $v$ y $w$ $\in V$, si $e = (v,w) \in E$, se dice que $v, w$ son **adyacentes** y que $e$ es **incidente** a $v$ y $w$.

-> La vecindad de un vértice $v$, $N(v)$ es el conjunto de todos los vértices adyacentes a $v$. Es decir:

$$
N(v) = \{w \in V : (v,w) \in E\}.
$$

* $n_{G} = |V|$ es la cantidad de elementos que tiene $V$.  $m_{G} = |X|$ es la cantidad de elementos que tiene $X$.

![[Pasted image 20240422190646.png | 500]]

### Multigrafos y Pseudografos
***
En algunas aplicaciones, por ejemplo para modelar un vuelo entre dos ciudades, la definición anterior de grafo no es la apropidada.

-> Un **multigrafo** es un grafo en el que puede haber varias aristas entre el mismo par de vértices distintos.
-> Un **pseudografo** es un grafo en el que puede haber varias aristas entre cada par de vértices y también pueden haber (loops) que unan vértices con sí mismo (relación de reflexividad).

![[Pasted image 20240422190754.png | 500]]

**Grado de un grafo:**
* El grado de un vértice $V$ en el grafo $G$, notamos $d_{G}(V)$, es la cantidad de aristas incidentes de $v$ en $G$.
* Llamamos $\Delta(G)$ al máximo grado de vértices de $G$, $\delta(G)$ al mínimo.

***Teorema***: La suma de los grados de todos los vértices de un grafo es igual a 2 veces el número de aristas. -> Se demuestra por inducción.

$$
\sum_{v \in V(G)}d(v_{i}) = 2|E(G)|
$$

***Corolario:*** La cantidad de vértices de un grafo de grado impar es par. 

### Complemento
***

-> Dado un grafo $G = (V,E)$, su grafo *complemento*, que notaremos $G^{c} = (V, E^{c})$, tienen el mismo conjunto de vértices y un par de vértices son adyacentes en  $G^{c}$ si, y solo si, no son adyacentes en $G$.

![[Pasted image 20240422194321.png|500]]
* Si en $G$ un vértice se conectaba con unos nodos, en $G^{c}$ se conectará con todos aquellos que no se conectó en $G$.

$G = ({v1, \ v2, \ v3, \ v4, \ v5}, \{(v1, \ v2), \ (v1, \ v3), (v2, \ v3), \ ... \})$ 
### Recorridos, caminos, circuitos y ciclos
***

-> Un **recorrido** de un grafo es una secuencia alternada de vértices y aristas $P = v_{0}e_{1},...,v_{k-1}e_{k}v_{k}$ tal que un extremo de la arista $e_{i}$ es $v_{i-1}$ y el otro es $v_{i}$ para $i = 1, ..., k$. Decimos que $P$ es un recorrido entre $v_{0}$ y $v_{k}$.

-> Un **camino** es un recorrido que no pasa dos veces por el mismo vértice. Se define como $P = v_{0}v_{1}...v_{k-1}v_{k}$.

-> Una **sección** ...

-> Un **circuito** es un recorrido que empieza y termina en el mismo vértice.

-> Un **ciclo o circuito simple** es un circuito de 3 o más vértices que no pasa dos veces por el mismo vértice.

### Distancia
***
*Recordemos las propiedades de la función de distancia:*
* $d(v,v) = 0$
* $d(u, \ v) \geq 0$ y $d(u,v) = 0 \iff u = v$
* $d(u,v) = d(v,u)$
* $d(u,w) \leq d(u,v) + d(v,w) \ \text{(Desigualdad Triangular)}$ 
* $d(v,w) = \infty \iff$ no existe camino entre $v$ y $w$. 

-> Notamos como $l(P)$ a la longitud de un camino $P$, es decir, la cantidad de aristas que tiene.

### Subgrafos
***
* Decimos que, un grafo $H = (V_{H}, E_{H})$ es ***subgrafo*** de $G$ con $G = (V_{G}, E_{G})$ si $V_{H} \subseteq V_{G}$ (los vértices de $H$ tienen que estar contenidos en $G$) y $E_{H} \subseteq E_{G} \cup (V_{H} \times V_{H})$.
* Es más fácil de ver si tenemos un grafo **completo** (es decir, que para todo nodo del grafo existe una forma de llegar a cada otro nodo del grafo).

![[Pasted image 20240424151835.png|500]]

-> Podemos agarrar un **subgrafo** $H$ el cual alguno de sus vértices $v$ sea inconexo con algún otro nodo del mismo subrafo. Si estamos en la situación en que el subgrafo tenga todos sus nodos conexos, decimos que tenemos un **subgrafo inducido** de $G$.

![[Pasted image 20240424153821.png|500]]



### Conexo
***

-> Un grafo se dice **conexo** si para todo par de vértices $v$ y $w$ existe un camino entre ambos.

-> Una **componente conexa** de un grafo $G$ es un subgrafo conexo maximal de $G$.

![[Pasted image 20240424144559.png]]


### Grafos bipartitos
***
-> Un grafo $G = (V,E)$ se dice *bipartito* si existen dos subconjuntos $V_{1}, V_{2}$ del conjunto de vértices $V$ tales que:

$$
V = V_{1} \ \cup \ V_{2}, \ \ \ \ \ \ \ V_{1} \ \cap \ V_{2} = \emptyset
$$
y tal que todas las aristas de $G$ tienen un extremo en $V_{1}$ y otro en $V_{2}$. 

-> Un grafo bipartito con subconjuntos $V_{1}, V_{2}$ es *bipartito completo* si todo vértice $V_{1}$ es adyacente a todo vértice en $V_{2}$.


**Teorema:**
Un grafo $G$ es bipartito si, y solo si, no tiene ciclos de longitud impar.


### Isomorfismos
***

-> Existen grafos que "visualmente" parecen tener apariencias distintas pero que en el fondo terminan siendo lo mismo. No podemos guiarnos visualmente en la representación de un grafo y deducir que son diferentes a partir de ello.

-> Podemos tener un grafo $G$ que "visualmente" sea distinto que $H$ pero en el fondo, uno es un **isomorfismo** del otro. Notamos:

$$
G \overset{\phi}{\cong} H
$$

![[Pasted image 20240424155508.png]]

-> Decimos que dos grafos $G$ y $H$ son **isomórficos** si existe una función  $\phi : V(G) \rightarrow V(H)$, biyectiva con $v_{1}, v_{2} \in E(G) \iff \phi(v_{1}) \phi(v_{2}) \in E(H)$.  En tal caso notamos $G \overset{\phi}{\cong} H$.


$$\phi: \begin{bmatrix}
a & b & c \\
x & y & z 
\end{bmatrix}
$$

* Observemos que si tenemos un isomorfismo entre grafos, tendríamos que poder preservar la *adyacencia* entre nodos de ambos grafos.




Pregunta: *¿Cómo representamos estructuras de grafos en la computadora?*

### Representación de grafos en la computadora
***

-> Para desarrollar algoritmos sobre grafos, debemos representarlos en la computadora.

-> Una buena representación es la matriz:
* La representación de grafos mediante matrices es útil conceptual y teóricamente.

-> Listas - Estructuras más eficientes en espacio.

**Matriz de Adyacencia:** es una forma de representar una conexión entre vértices mediante valores binarios.

$$
A \in \{0,1\}^{n \times n}, \ \text{donde los elementos} \ a_{ij} \ \text{se definen como:}
$$
$$
a_{ij}=
\begin{cases}

1 & \text{si G tiene una arista en los vertices vi, vj} \\

0 & \text{si no}
\end{cases}
$$

![[Pasted image 20240422210341.png|500]]

**Proposición**
Si A es la matriz de adyacencia del grafo $G$, entonces:
* La suma de los elementos de la columna (o fila) $i$ de A es igual a $d(v_{i})$.
* Los elementos de la diagonal de $A^{2}$ indican los grados de los vértices: $a_{ii}² = d(v_{i})$.


### Dígrafos (Grafos dirigidos)
***

-> Un *dígrafo* o *grafo dirigido* $G = (V,E)$ es un par de conjuntos $V$ y $E$ tal que $E$ es un subconjunto de pares **ordenados** (existe un sentido de dirección) entre los distintos elementos de $V$.

-> Cada *nodo* recibe el nombre de **arcos**.

-> Dado un arco $e = (u, w)$ llamamos al primer elemento $u$ (salida) cola de $e$ y al segundo elemento $w$ cabeza de $e$.

-> El **grado de entrada** de un vertice $v$ son la cantidad de arvos que llegan a $v$ (es decir, las flechas que caen en $v$). Es decir, la cantidad de arcos que tienen a $v$ como cabeza.

-> El **grado de salida** de un vértice $v$ es la cantidad de arcos que salen de $v$. Es decir, la cantidad de arcos que tienen a $v$ como cola.

-> El **grafo subyacente** de un digrafo $G$ es el grafo $G^{s}$ resultante de quitar las direcciones en G.

![[Pasted image 20240424145657.png]]

