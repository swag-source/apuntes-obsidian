***

**Definiciones:**
-> Decimos que un **árbol** es un grafo conexo sin circuitos simples.

-> Una arista $e$ de $G$ es un **puente** si $G - e$ tiene más componentes conexas que $G$. 

![[Pasted image 20240429181650.png|500]]

-> Un vértice de $v$ de $G$ es un **punto de corte** o **punto de articulación** si $G - v$ tiene más componentes conexas que $G$.

**Teorema:** Dado un grafo $G = (V,E)$, es equivalente:

1. $G$ es un árbol.
2. $G$ es un grafo sin circutos simples, pero si se agrega una arista $e$ a $G$ resulta un grafo con exactamente un circuito simple, y ese circuito contiene a $e$.
3. Existe exactamente un camino entre todo par de nodos.
4. $G$ es conexo, pero si se quita cualquier arista a $G$ queda un grafo no conexto (toda arista es puente).

Entonces

**Lema 1:** Sea $G = (V,E)$ un grafo conexo y $e \in E$. $G - e$ es conexo si y solo si $e \in$ circuto simple de $G$.

-> Una **hoja** es un nodo de grado 1.

**Lema 2:** Todo grafo no trivial tiene al menos dos hojas.

**Lema 3:** Sea $G = (V,E)$ árbol. Entonces $m = n-1$ para todo $n \in \mathbb{N}$. => hoja de mile

* En general hacemos demostraciones de la siguiente manera: Sea $G(V,E)$ es árbol. Yo tengo probado que $G$ es árbol para $n-1$, quiero ver si para $G$ de $n$ si le resto una hoja, entonces vale que también es árbol.

Un *bosque* es un grafo sin circuitos simples.

**Corolario 1:** Sea $G = (V,E)$ un bosque con $c$ componentes conexas. Entonces $m = n - c$.



### Árboles enraizados
***

-> Un ***árbol enraizado*** es un árbol que tiene un vértice distinguido que llamamos ***raíz***.

-> Explícitamente queda definido un árbol dirigido. (Si bien no tiene flechas para hacerlo *dígrafo*, es claro hacia donde va).

-> El ***Nivel*** $n$ de un vértice es la distancia de la raíz a ese vértice.

-> La ***Altura*** $h$ de un árbol enraizado es el máximo nivel de sus vértices. 

-> Tenemos ***Vértices Internos*** de un árbol enraizado a los nodos que se encuentran entre medio del árbol, ni en las hojas ni la raíz.

-> Un árbol se dice (exáctamente) $k$-ario si todos sus vértices, salvo las
hojas y la raíz tienen grado (exáctamente) a lo sumo $m + 1$ y la raíz
(exáctamente) a lo sumo $m$.


![[Pasted image 20240429192911.png|500]]
![[Pasted image 20240429194224.png|500]]

*¿Cuántos nodos tiene en total un árbol exactamente k-ario que tiene i nodos internos?* 
=> $Q = k * i + (k+1)$

**Teorema:**

-> Un árbol $k$-ario de altura $h$ tiene a lo sumo $m^{h}$ hojas. Alcanza esta cota si es un árbol exactamente $k$-ario balanceado completo con $h \geq 1.$


### Árboles generadores
***
**Definición**
-> Un **árbol generador** (AG) de un grafo $G$ es un subgrafo generador (que tiene el mismo conjunto de vértices) de $G$ que es árbol.

> **Subgrafo generador:** Decimos que $T$ es un subgrafo generador de $G$ al grafo tal que $\forall v \in V(T) \implies V(T) \subseteq V(G)$ 

![[Pasted image 20240429200943.png|500]]

**Teorema**
-> Todo grafo conexo tiene (al menos) un árbol generador.

-> $G$ conexo. $G$ tiene un único árbol generador $\iff$ $G$ es árbol.

-> Sea $T = (V, E_{T})$ un árbol generador de $G = (V,E)$ y $e \in X - X_{T}$, entonces $T + e - f = (V, X_{T} \cup \{e\} - \{f\}$, con $f$ una arista del único circuito de $G + e$, es un árbol generador de $G$.

### Recorrido de árboles, grafos o digrafos
***
-> Muchas veces, en algoritmos o situaciones, nos interesa recorrer un grafo o digrafo exactamente una sola vez.
* Tenemos dos filosofías de recorrer **ordenada y sistemáticamente**:
	1. **BFS (Breadth-First search):** Comienza por el nivel 0 (la raíz) y visita cada vértice en un nivel $h' \leq h$ antes de pasar al siguiente nivel.
	2. **DFS (Depth-First search):** Comenzamos por el nivel 0 y exploramos cada rama lo más profundo que podemos ( ͡° ͜ʖ ͡°) antes de retroceder.

* Tenemos un esquema para recorrer gráfos.

![[Pasted image 20240429202425.png|500]]

-> Si queremos elegir entre **BFS** y **DFS**, tenemos que implementarlo de la siguiente manera.
* **BFS**: LISTA implementada como cola.
* **DFS:** LISTA implementada como pila.

-> Cuando recorremos los vértices de un grafo G, los valores de pred
implícitamente definen un $\text{AG}(G)$.

-> Utilizamos estos procedimientos para encontrar:
* Encontrar todas las componentes fuertemente conexas de un digrafo.
* Determinar si un grafo o digrafo tiene ciclos.
* El problema de flujo.
* Hallar camino mínimo entre dos puntos $A$ y $B$.
* Encontrar vértices a una distancia menor que $k$.

**BFS para calcular distancia**
-> Agregamos una matriz *distancia* de tamaño $n \times n$
-> Inicializamos *disntacia*$[i,j] \leftarrow \infty$ si $i \neq j$ sino  *disntacia*$[i,j] \leftarrow 0$ para $1 \leq i \leq n$.
-> Luego de $pred[j] \leftarrow i$ dentro de la función *recorrer(G)*, se debe agregar $distancia[r,j] \leftarrow distancia[i,j] + 1$.
-> Aplicar BFS para cada raíz *r* : $1 \leq r \leq n-1$ 


**DFS con más info.**
-> Si aplicamos DFS para enumerar todos los vértices de un **digrafo**, se pueden clasificar sus arcos en 4 tipos:
* **tree edges**: arcos que forman el bosque DFS. => **bosque =** conjunto de todos los nodos por los que pasé.
* **backward edges**: van hacia un ancestro.
* **forward edges:** van hacia un descendiente.
* **cross edges:** van hacia otro árbol (anterior) del bosque o a otra rama (anterior) del árbol.

-> Para **grafos**
* Solamente existen aristas *tree edges* y *back edges* ya que no tenés sentido de la orientación.

-> Incorporamos algunos elementos adicionales para brindar más info.
* Agregar una variable *timer* $\leftarrow$ 0.
* 2 arreglos: *desde* y *hasta*, ambos de tamaño $n$ 


*Algunas aplicaciones concretas:*

**Detección de ciclos**
**Teorema:** Dado un digrafo $D$.

$D$ tiene un ciclo (ciclo orientado) $\iff$ $\exists$ DFS en el digrafo que clasifica aristas como backward edge en $D$

*Demo: https://sharmaeklavya2.github.io/theoremdep/nodes/graph-theory/dfs/cycle-iff-back-edges-directed.html

**Sort topológico**

Ordenar los nodos de acuerdo a su valor en el arreglo *hasta* de mayor a menor.