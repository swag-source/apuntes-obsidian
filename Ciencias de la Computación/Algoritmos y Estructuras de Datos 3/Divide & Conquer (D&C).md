***
**Algunas características de algoritmos D&C**
* Considerada una de las técnicas más intuitivas y elementales de la programación recursiva/programación.
* Entiende que cada problema puede ser dividido en subproblemas (más pequeños) del mismo tipo que el original.

**Ejemplo típico de D&C (MergeSort):**
![[Pasted image 20240408181203.png | 500]]

*Fórma general:* $F(X)$ es una función que toma una instancia de $X$.
* Si $X$ es suficientemente chico, solucionar el problema de manera ad hoc.
* Si no,
	* Dividir a $X$ en $X_{1}, X_{2}, ..., X_{k}$
	* $\forall i \leq k$, hacer $Y_{i} = F(X_{i})$.

Pero ¿Cómo calculamos la complejidad de un algoritmo de D&C?

### Teorema maestro (Complejidad de recursividad)
***
El costo de un algoritmo de D&C de tamaño $n$ se puede expresar como T($n$), que debe considerar:
* Dividir el problema en $k$ subproblemas de tamaño máximo $n / c$, siempre con algún $n/c$ > $n_{0}$.
* El costo de el algoritmo será simplemente tomar la complejidad de los subproblemas menores que se nos presentan en cada paso recursivo (entradas de tamaño menor).
* Si ese costo es $b$ cuando $n = 1$, entonces podemos expresarlo como $b \ \times n^{d}$ , para algún d. => (En español, es mi caso base. Evaluar en una instancia de tamaño 1 es a lo que queremos llegar).
* Luego hay que resolver los subproblemas: $a \ T(n/c)$ donde $c$ es la constante en la cual dividiremos el subproblema.

Entonces:
$$ T(n) = a \ T(n/c) + b \ n^{d}$$
Supongamos que tenemos que $n = c^{k}$ para algún $k$ y analicemos la recurrencia.
$$
T(c^k) = a \ T(c^{k-1}) \ + \ b \ (c^{k})^{d}
$$
$$
T(c^{k}) = a^{j} \ T(c^{k-j}) \ + \ b \ \sum_{}^{} 
$$
