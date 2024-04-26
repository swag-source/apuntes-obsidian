***

*Ejercicio 1:* Sean $P$ y $Q$ dos caminos disjuntos de un grafo $G$ que unen un vértice $v$ y otro $w$ . Probar que $G$ tiene un ciclo cuyas aristas pertenecen a $P$ y $Q$.

-> Vamos a realizar un approach de esta demostración por *construcción*.

*Demostración*:

Supongamos que tenemos dos caminos distintos $P$ y $Q$ que unen a los vértices $v$ y $w$. Recordamos que un camino se nota $P = v_{0},...,v_{p}$.
* $P = v_{0},...,v_{p}$
* $Q = w_{0},...,w_{q}$

*Obs:* Tenemos dos opciones posibles:
* $P$ y $Q$ son dos caminos disjuntos.
* $P$ y $Q$ son dos caminos que comparten nodos => no son disjuntos. (ejemplo de figura del infinito, sin tener en cuenta los extremos de $v$ y $w$)


**Para el primer caso**: Queremos mostrar que ambos caminos $P$ y $Q$ van a crear un ciclo a partir del último vértice por donde pasan (es decir, $w_{q}$).

Supongamos que $\forall i,j$ $0 < i,j <P,Q$ y $v_{i} \neq w_{j}$. Queremos ver que G tiene un ciclo cuyas aristas pertenecen a $P$ o $Q$ (inclusive).

![[Pasted image 20240426102718.png|500]]

Se puede ver que el ciclo está formado por:
$$
C =\{v_0 = v, ..., v_p = w_q, w_{q-1}, ..., w_0 = v_0\}
$$
Esto quiere decir que cuando el camino $P$ llega al último punto, necesariamente ese último punto tiene que compartirse con el último punto del camino $Q$. 

**Para el segundo caso**: Queremos mostrar que ambos caminos $P$ y $Q$ tendrán que compartir nodos pero son caminos distintos.

O sea, que tenemos dos nodos $u_0, u_1 \in P \land u_0, u_1 \in Q$ tales que no hayan nodos en el medio que pertenezcan a ambos caminos. Si existieran, estos caminos serían iguales y sería **ABS!**.

Esto quiere decir que existirá un vértice en el grafo tal que pertenezca a un camino y no al otro.

![[Pasted image 20240426103204.png]]

Más formalmente:

$$
u_1 \in P \implies \exists \ i, 0\leq i \leq P \  : \ v_i = u_1
$$
$$
u_2 \in P \implies \exists \ i', 0\leq i' \leq P \  : \ v_i^{'} = u_2
$$
$$
u_2 \in Q \implies \exists \ j, 0\leq j' \leq Q \  : \ w_j = u_1
$$
$$
u_2 \in Q \implies \exists \ j', 0\leq j' \leq P \  : \ w_j^{'} = u_2
$$
Teniendo entonces el ciclo:
$$
\{v_i = u_1, ..., v_i^{'} = u_2, ..., w_j = u1\} \ \text{con} \ i\neq i' \ \land \ j \neq j'
$$


*Ejercicio 2:* Probar que todo digrafo $D$ satisface:

$$
\sum_{v \in V(D)}D_{in}(v) = \sum_{v \in V(D)}D_{out}(v) = |E(D)|
$$


* La idea es encarar el ejercicio a partir de inducción. Las inducciones se suelen hacer sobre lo que queremos probar. En este caso, queremos probar algo sobre la cantidad de aristas del grafo.
* Armo mi estructura de inducción => Caso Base, Hipotesis Inductiva, Paso Inductivo.

Sea $n = |E(D)|$ para facilitarnos la notación de la inducción.

**Caso Base:** ¿Vale P(0)?
* $P(0): n = 0$ => vale ya que el grafo *vacío* contiene 0 aristas.

**Hipotesis Inductiva:** ¿Si P(n) => P(n+1)? En castellano: si se cumple mi propiedad de aristas, ¿se cumple para un grafo $D'$ tal que $|E(D')|$ = $|E(D)| + 1$?

* $P(n) =$ $$\sum_{v \in V(D)}D_{in}(v) = \sum_{v \in V(D)}D_{out}(v) = n$$

* $P(n) \implies P(n+1)$ $$\sum_{v \in V(D)}D_{in}(v) = \sum_{v \in V(D)}D_{out}(v) = n \implies \sum_{v \in V(D')}D'_{in}(v) = \sum_{v \in V(D')}D'_{out}(v) = (n +1)$$
Pensemos en términos matemáticos que significa $D'$.
$D' = (V(D),E(D) \cup \{(s,t)\})$. O sea, es exactamente el mismo grafo que $D$ solamente que metiendole una nodo $(s,t)$.

Entonces:

$$
|E(D')| = |E(D) \ \cup \ \{(s,t)\}| = |E(D)| \ + \ |\{(s,t)\}| = \ ...
$$

Este paso de separar vale ya que $D$ es un digrafo que previamente es "disconexo" de ese par $(s,t)$ que metemos. Entonces es simplemente sumarle 1 elemento a $D$.

$$
...\ = \ |E(D)| \ + 1 = (n+1)
$$

Por otro lado, nos queda probar las sumatorias:

$$
\sum_{v \in V(D')}D'_{in}(v) = D'_{in}(S_{D'}) \ + \sum_{v \in V(D)-\{S_{D}\}}D_{in}(v)
$$

basicamente, es agarrar la sumatoria hasta el $(n-1)$  y sumarle el último término que metimos nosotros (se ve en D'in). Como haciamos en álgebra, reemplazabamos el (n+1) por algo que ya conocíamos y veíamos la diferencia.

$$
= 1+ D_{in}(S_{D}) \ + \sum_{v \in V(D)}D_{in}(v)
$$
$$
= 1\ + \sum_{v \in V(D)}D_{in}(v)
$$

*Idem con el* $D_{out}$.

Entonces, por H.I:

$$
(\sum_{v \in V(D)}D_{in}(v) = \sum_{v \in V(D)}D_{out}(v) = n) \implies 1\ + \sum_{v \in V(D)}D_{out}(v) = 1\ + \sum_{v \in V(D)}D_{in}(v) = n  + 1
$$

Como se quería probar.