***
Vimos a lo largo de la materia distintas metodologías para desarrollar algoritmos:
* *Backtracking* (Imponemos podas a nuestra función recursiva para no "entrar" en todos los casos).
* *Programación Dinámica* (Hacemos uso de la memoria para no tener que volver a computar información que ya hicimos)
* *Divide & Conquer* (Dividimos cada problema en subproblemas de menor tamaño proporcionales a la entrada por una constante $c$)
* *Heuristica* (Sabemos que mi problema no tiene solución computable, entonces buscamos una solución **posible** pero que sea una aproximación a la solución exacta, no exactamente la misma) -> "*Psicología de los objetos cotidianos*"

Decimos que un algoritmo es "$\epsilon-\text{aproximado}$" si:
$$
|\frac{X_{A}  \ - \ X_{optimo}}{X_{optimo}}| < \epsilon
$$
Es decir, si la distancia entre nuestra solución aproximada y nuestra solución optima es cada vez menor a un valor $\epsilon$, entonces la tomamos como una solución válida pero aproximada.

**Introducción al Algoritmo Greedy**
*¿Cómo definimos un algoritmo greedy?* Vemos un ejemplo con el problema de la mochila.

-> **Algoritmo greedy**: Mientras no se haya excedido el peso de la mochila, agregar a la mochila el $i$-ésimo objeto que...
	->... tenga mayor beneficio $b_{i}$
	->... tenga menor peso $p_{i}$
	-> ... maximice $\frac{b_{i}}{p_{i}}$

*¿Qué podemos decir en cuanto a la **calidad** de las soluciones obtenidas por estos algoritmos?*
	-> Filosóficamente podemos decir que si en cada paso busca maximizar localmente la solución, entonces la esperanza es que globalmente lleguemos a una solución óptima.

* Supongamos que los objetos están ordenados de mayor a menor cociente $b_{i}/p_{i}$.
***
```
L <- C
i <- 1
while(L > 0 and i <= n):
	x <- min{1, L/p_{i}};
	Agregar una fracción de x del objeto i a la solución:
	L <- L - x * p_{i};
	i++;
end while
```
***
**Teorema:** El algoritmo greedy por *cocientes* encuentra solución óptima del problema de la mochila fraccionario.

=> No puedo entonces asegurar entonces que una solución que resuelva un problema a partir de un algoritmo greedy sea una solución óptima a menos que sepa que la estructura con la que trabajo sea una **subestructura óptimal**. Quedamos que:

$$
\text{es solución óptima por greedy} \iff \text{es una estructura suboptimal}
$$

### Problema del vuelto
***
Supongamos que queremos dar el vuelto a un cleinte usando el mínimo número de monedas posibles utilizando molnedas de 1, 5, 10 y 25 centavos. Por ejemplo, si el monto es $0.69, deberemos entregar 8 monedas: 2 monedas de 25 centavos, una de 10 centavos, una de 5 centavos y cuatro de un centavo.

-> **Algoritmo goloso (la del cajero):** Seleccionar la moneda de mayor valor que no exceda la cantidad restante por devolver, agregar moneda a la lista de la solución, y sustraer la cantidad correspondiente a la cantidad que resta por devolver (hasta que sea 0).


### Tiempo de espera total de un sistema
*** 
Un servidor tiene *n* clientes para atender, y los puede atender en cualquier orden. Para $i = 1, ..., n$, el tiempo necesario para atender al $i$-ésimo cliente es $t_{i}$. El objetivo es determinar en qué orden se deben atender los clientes para minimizar *la suma de los tiempos de espera* de los clientes.

-> Si $I$ = ($i_{1}, ... , i_{n}$) es una permutación de los clientes que representa el orden de atención, entonces la suma de los tiempos de espera es:

**Algoritmo goloso:** En cada paso, atender al cliente pendiente que tenga el menor tiempo de atención.

1. Este altoritmo retorna una permutación $I_{GOL}$ = $(i_{1}, ..., i_{n})$ tal que $t_{i_{j}} \leq t_{i_{j+1}}$ para $j = 1, ..., n-1$
2. ¿Cuál es la *complejidad* de este algoritmo?

**Teorema.** El algoritmo goloso por menor tiempo de atención proporciona una solución óptima del problema de minimizar el tiempo total de espera de un sistema.

$$
T = t_{i_{1}} \ + \ (t_{i_{1}} \ + \ t_{i_{2}}) \ + \ (t_{i_{1}} \ + \ t_{i_{2}} \ + \ t_{i_{3}})
$$

*¿Cómo vamos a hacer para encarar un ejercicio de Algoritmo Greedy?*
1. Identificar si nuestro problema requiere una solución **Minimal** o **Maximal**.
2. Cuando implementamos, nos preguntamos:
	1. El valor ¿es una solución posible? => Si lo es, la incluimos. Si no, la descartamos.
		1. Podemos definir una función *f(x) = posible(x) : Bool*

```
a = [a1, ..., a5]
n = 5

Algoritmo Greedy(a, n):
	for (int i = 0; i < len(a); i++){
		x = a[i];
		if(esFactible(x)){
			Solución = Solución + x;
		}
	}
```