***

-> Al igual que *divide and conquer*, se divide en subproblemas de tamaños menores que se resuelven recursivamente. Una vez resueltos los subproblemas, se combinan las soluciones para generar la solución del problema original.

![[Pasted image 20240325181122.png]]

-> **Superposición de estados:** El árbol de llamadas recursivas resuelve el mismo problema varias veces.
* Alternativamente, podemos decir que se realizan muchas veces a la función recursiva con los mismos parámetros.

-> Un algoritmo de programación dinámica evita estas repeticiones con alguno de estos dos esquemas:
1. **Top-Down:** Se implementa recursivamente, pero se guarda el resultado de cada llamada recursiva en una estructura de datos (*memorización*). Si una llamada recursiva se repite, se toma el resultado de esta estructura.
2. **Bottom-Up:** Resolvemos primero los subproblemas más pequeños y guardamos (habitualmente en una tabla) todos los resultados.
3. **No todo problema puede resolverse mediante programación dinámica** -> *Principio de optimalidad de Bellman*

Tipicamente este tipo de problemas son aplicados a los de optimización combinatoria y aquellos problemas dificiles de resolver por D&C suelen ser aproximados a partir de la programación dinámica.

-> Evita repetir llamadas recursivas almacenando los resultados calculados.

### Principio de optimalidad de Bellman
***
*Un problema de optimización satisface este principio si en una solución óptima, cada subsolución es a su vez óptima del subproblema correspondiente.* => **Buscamos una Estructura de Suboptimalidad**.

Camino mínimo (sin distancias negativas) cumple este principio:
-> Si sabemos el camino más corto desde la ciudad A a la B pasa por una ciudad C, entonces el camino más corto de A a B tendremos que pasar por el camino más corto de A -> C y C -> B.




*Ejemplo*: El problema del cambio

>[!Enunciado]
> Supongamos que queremos dar el vuelto a un cliente usando el mínimo número de monedas posibles, utilizando monedas de 1, 5, 10 y 25 centavos. Por ejemplo, si el monto es $0.69, debemos entregar 8 monedas: 2 de 23 centavos, una de 10 centavos, una de 5 centavos y cuatro de un centavo.

**Problema:** Dadas las denominaciones $a_{1}, ..., a_{k} \in \mathbb{Z}_{+}$ de monedas (con $a_{i} > a_{i+1}$) y un objetivo $t \in \mathbb{Z}_{+}$, encontrar $(x_{1}, ..., x_{k}) \in \mathbb{Z}_{+}$ tales que:
$$
t = \sum_{i=1}^{k}a_i x_{i}
$$
que maximicen la suma.

```
BTMaxiSubconjunto(M: Matrix, n : {1,...,n}, k: N, I : {1,..,n})
	if(k = 0 & |I| = k){
		ret ($\max_{i,j \in I} (\sum_{i = 1, i \neq j}^{k} M_{ij})$)
	}
	else (|I| < k) {
		for i in {1,...,n}
			BTMaxiSubconjunto(M, {1,...,n} - i, k-1, I : {i})
	endif
	}
```


*Ejemplo: Subsecuencia común más larga.*

Dada una secuencia, una subsecuencia se obtiene eliminando 0 o más símbolos. Por ejemplo \[4,7,2,3\] y \[7,5\] son subsecuencias de \[4,7,8,2,5,3\]. \[7,5\] no lo es.
* Observemos que las subsecuencias posibles son de forma decreciente y para todo s\[i\] < s\[j\] con i < j.

-> **Fuerza bruta (mala complejidad)**: generamos todas las subsecuencias posibles entre A y B buscamos entro todas las secuencias que son subsecuencias de A y B, busco la intersección, y me quedo con la más larga.

-> **Programación dinámica:** Dadas las dos secuencias A = $[a1, ..., a_{r}]$ y B = $[b_{1}, ..., b_{s}]$, tenemos dos posibilidades => $a_{r} = b_{s}$ o $a_{r} \neq b_{s}$. Vemos los dos casos:
1. $a_{r} = b_{s}$Significa que la SCML entre A y B contiene a los últimos elementos de la secuencia A y B. Por ende, lo colocamos al final de la SCML.
2. $a_{r} \neq b_{s}$. Si son distintos, entonces tenemos dos casos distintos. 
	1. $a_{r} \notin$ SCML o $b_{s} \notin$ SCML, para el primero, calculamos entonces el mismo problema aplicado a $A - \{a_{r}\}$ y $B$; luego $A$ y $B - \{b_{s}\}$. Al resultado de ambas operaciones, tomamos la más larga de ambas.


$$
optiPago(B,c) =
\begin{cases}

(|B|, 0) & \text{si } c < 0 \ \land \ B \neq \emptyset \\

(\infty, \infty) & \text{si } B = \emptyset \ \land c > 0\\

\min\{(optiPago(B \ - \ \{b\}, c \ - b)_{0}, optiPago(B \ - \ \{b\}, c \ - b)_{0}), \\ (optiPago(B \ - \ \{b\}, c)_{0}, optiPago(B \ - \ \{b\}, c \ - b)_{1})\} & \text{sino}

\end{cases}
$$

