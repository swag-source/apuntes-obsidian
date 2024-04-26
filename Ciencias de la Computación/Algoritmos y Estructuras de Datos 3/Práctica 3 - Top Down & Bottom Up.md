***

*Recordemos:*
* **TOP-DOWN:** Ir de lo *más general* a lo *más específico*
* **BOTTOM-UP:** Ir de lo *menos específico* a lo *más general*.

**Problema Fibonacci Backtracking:** Dado un entero no negativo, calcular el n-ésimo número de la secuencia de fibonacci.

![[Pasted image 20240403174630.png]]

*¿Cuál sería la complejidad temporal de nuestro algoritmo?*

```
def fib(n: int) -> int:
	if n = 0:
		return 0
	elif n = 1:
		return 1:
	else
		return fib(n-1) + fib(n-2)
```

-> Observemos que si hacemos el árbol de llamadas recursivas. Tenemos que, por cada nivel, en el peor caso se abren $2^n$ posibles nodos por nivel => $O(2^n)$ y también tenemos que $\Omega(2^{n/2})$.

*¿Cuál sería la complejidad espacial de nuestro algoritmo?*

-> Si asumimos enteros de n bits, entonces la complejidad espacial sería => $O(n)$ ya que al realizar cada llamado recursivo, tendríamos que "guardar" esas llamadas en algún lugar de la memoria. Por esta razón, siempre tendrás (aunque sean casos base) llamadas recursivas para los n elementos de la función.


*Ejemplo del árbol recursivo*
```
							| 4 | -> fib(4)
						/	         \
		   fib(3)<- | 3 |     +     | 2 | -> fib(2)
		          /      \         /     \
				| 2 | + | 1 |   | 1 | + | 0 |
				/   \
			| 1 | + | 0 |
```

Ahora, esta implementación podemos bajarle la complejidad a partir del manejo de la memoria.

**Approach Top-Down:**
```
def fib(n: int, memoria: list = []) -> int:
	if memoria = []:
		memoria = [-1] * (n + 1) 
		// [-1, -1, ... , -1 = memoria[n] , -1 = memoria[n+1]]

	if memoria[n] == -1:
		if n <= 1:
			memoria[n] = n
		else
			memoria[n] = fib(n-1, memoria) + fib(n-2, memoria)

	return memoria[n]
```

-> $O(2n) \equiv O(n)$ y también $\Omega(2n) \equiv \Omega(n)$ por ende, tenemos que => $\Theta(2n) \equiv \Theta(n)$.
* Nótese que tenemos realmente $O(1) \times$ *cantidad de llamados a la recursivo*. => $O(n)$.


**Approach Bottom-Down:**
```
def fib(n: int) -> int:
	memoria = [-1] * (n + 1)
	memoria[0] = 0
	memoria[1] = 1

	 for i in range(2, n + 1):
	 memoria[i] = memoria[i-1] + memoria[i-2]

return memoria[n]

// Igual podemos mejorar la implementación porque simplemente necesitamos 3 elementos en total para calcular fibonacci

// [anterior, siguiente, actual] -> actual = anterior + siguiente.

// En vez de dedicarle un array más largo, podría ir pisando los valores del anterior y siguiente e ir realizando las iteraciones.


def fib(n: int) -> int:
	anterior = 0
	resultado = 1

 for _ in range(2, n + 1):
	 nuevo = resultado + anterior
	 anterior = resultado
	 resultado = nuevo
 
 return resultado

```

-> Complejidad temporal: $O(n)$ porque siempre termino recorriendo los n elementos.
-> Complejidad espacial: $O(1)$ ya sabemos que solamente nos va a ocupar 3 lugares.

![[Pasted image 20240403182636.png]]


### Caminos en una matriz
***
Definamos primero un poco la noción formal de *camino*.

![[Pasted image 20240403185942.png]]
* Las *partes* de una *solución óptima* a un problema, deben ser *soluciones óptimas* de las correspondientes *subproblemas*.
* Permite obtener una solución óptima al problema original a partir de las solucion óptima al problema original a partir de soluciones óptimas de los subproblemas.

**Principio de Optimalidad** => *Clave*
Sea *P* un camino de mínimo costo en una matriz *M* entre dos posiciones diferentes $(a,b)$ y $(c,d)$. Sea *Q* un sufijo de *P*, y sea $(a', b')$ el primer elemento de *Q*. El camino *Q* es de mínimo costo entre todos los caminos desde $(a', b')$ hasta $(c,d)$.

$$\begin{pmatrix}   20 & 10 & 5\\    9& 2 & 1  \\  30 & 8 & 5\end{pmatrix}$$

Camino mínimo = \[20 -> 9 -> 2 -> 1 -> 5\]
* Me paro en la posición $(0, 0)$ y verifico. (0, 1) -> Derecha o (1, 0) -> Abajo
	* Me quedo con el menor de los dos y me muevo a ese.


Encontramos una expresión general para plantear la fórmula recursiva para camino mínimo de una matriz (ni en pedo demuestro todo ja).

![[Pasted image 20240403193827.png]]

**Teorema (correctitud de minCamino):** minCamino(fila, col, M) es el costo de un camino mínimo desde (fila, col) hasta (m, n) siendo *M* la matriz, para toda fila $\in \{0, ..., m\}$ y col $\in \{0, ..., n\}$.
