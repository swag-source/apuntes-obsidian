***
### Conceptos clave:
1. Eficiencia
2. Costo
3. Tiempo de computación
4. Modelo de cómputo
5. Notación O grande

### Análisis de la Complejidad de Algoritmos
***
* Nos interesa modelar correctamente un problema y pensar algoritmos correctos que resuelvan el problema.
* Además queremos que no solamente cumplan lo que queremos, sino que lo hagan eficientemente (en términos de consumo de recursos).
* Entramos en el campo del diseño para TADs.
* Recursos que nos interesan a la hora de ver algoritmos:
	-> Tiempo de ejercución.
	-> Espacio (memoria/complejidad espacial).
	-> # Procesadores (multithreading, algoritmos paralelos).
* También puede interesarnos:
	-> "Elegancia" de la solución.

*¿Cuándo podemos elegir que un algoritmo es mejor que otro?*

* Podemos hacer un análisis de forma **Empírica o experimental** o **Teórica**.

**Empírica**
* Medimos el tiempo que tarda un programa en terminar dado una entrada *S* y una computadora *C*.
* Usando un cronometro y viendo el consumo de una computadora.
* **Ej:** programa X cuesta 3GB de memoria y tarda 1,5 segundos en ejecutarse.

**Teórica**
* Proponemos una medida teórica del comportamiento de un algoritmo.

**Ejemplo:** buscamos un elemento X en un array A (asumiendo X en A)
*Busqueda Secuencial (linear search)*

-> \[2,3,6,8,3\] buscamos X = 6
-> i = 0 es X? NO ---> i = 1 es X? NO ---> i = 2 es X? SI ---> return i

#### Ventajas del análisis teórico
***
-> No necesitamos ejecutar el algoritmo para saber su "costo" o cuánto tarda.
-> Vale para cualquier instancia del problema.
-> No depende del lenguaje de programación y de la maquina donde se ejecuta.
-> Se basa en un "modelo de máquina" y ponemos foco en la dependencia en el tamaño del input.
-> Nos interesa ver cómo se comporta asintóticamente (cuanto más grande sea el N a qué tiende, es decir)
$\lim_{n \to \infty} \\ Complexity(n) \to$ $f(n)$.

* Veamos lo que sucede punto por punto.

### Modelo de cómputo
***
* Planteamos una idea abstracta de computadora "universal" donde implementamos nuestro algoritmo.
* Esta máquina permite hacernos de la noción de tiempo y espacio para nuestro algoritmo.
-> **Tiempo:** número de pasos que se ejecutan dado un determinado input.
-> **Espacio:** número de posiciones de memoria utilizadas para ese input (cuanta memora "alocamos").

* Nuestra computadora abstracta necesita que se definan ciertas unidades de tiempo para algunas **Operaciones Elementales** (tardan una unidad de tiempo).

#### Operaciones elementales

![[Pasted image 20231009124134.png]]

**Consideraciones generales:**

* Consideramos que una OE tiene por definición, 1 unidad de costo elemental .
* El tiempo de ejecución de una secuencia consecutiva de instrucciónes se calcula sumando los tiempos de ejecución de cada instrucción.

*Ej de busqueda secuencial: ¿Cuánto nos tarda encontrar X = 5 en 
A = \[2,6,3,5,8\]*

* Si hacemos la cuenta de cuántas operaciones elementales se están ejecutando, llegamos a 30.
* Cada vez que el arreglo se hace más grande y el número más al fondo está, más nos damos cuenta que hacer todo el cálculo es engorroso.
* Nos interesa empezar a dar una idea más general de las cosas que nos pueden llegar a pasar (que el número esté en la primera posición o en la última, si el arreglo tiene 10 posiciones no va a poder estar en la posición 11, etc.)
* **El número exacto no termina sirviendo tanto, sino el comportamiento "aproximado" que dependa de ese input**.

 ![[Pasted image 20231009132323.png]]
 
![[Pasted image 20231009132344.png]]

### Tamaño del input (y su importancia)
***
* Como dijimos, no nos interesa tanto un numerito en particular (no nos aporta tanta información) sino ver cómo se comporta nuestro algoritmo respecto a los distintos tamaños que le pasemos.
* Podemos tener infinitos inputs para un algoritmo y queremos ver *"qué es lo peor que puede pasar"*, *"qué es lo mejor que puede pasar"* o (cuidadosamente) *"qué pasa en promedio"*

**Notamos**
-> **T(n)**: complejidad temporal respecto al input n.
-> **S(n)**: complejidad espacial (cuánto me cuesta el almacenamiento) respecto a n.
 ![[Pasted image 20231009174205.png]]

*Del video de Feuerstein en Algoritmos y Estructuras de Datos 2 pandemia
* $\text{T}_{mejor}(n) = 9$
* $\text{T}_{prom}(n) \approx 8 + \frac{5n}{2}$

**¿Si tuvieramos que hacer una busqueda si el arreglo está ordenado?**
*Rta: No, no cambia ESTE algoritmo en particular para los tipos de análisis hechos.*

* Pero también tenemos otros algoritmos de busqueda el cual cambia su COMPLEJIDAD TEMPORAL dependiendo de si el arreglo está ordenado o no.

**Busqueda binaria (binary search)**
* Vamos a ver cuánto tarda la busqueda de mi algoritmo (en términos de complejidad temporal).


### Principio de invarianza
***
* Dado un algoritmo y dos máquinas (abstractas, como veníamos viendo) $\text{M}_{1}$ y $\text{M}_{2}$, que tardan $\text{T}_{1}(n)$ y $\text{T}_{2}(n)$ respectivamente sobre inputs de tamaño n, existe una CONSTANTE k > 0 y un $\text{n}_{0} \in \mathbb{N}$ tales que $\forall n \geq n_{0}$ se verifica que:
	$T_{1}(n) \leq k * T_{2}(n)$

-> Esto me dice que DOS ejecuciones distintas para un mismo algoritmo, solamente van a diferir en una constante $K$ para valores de entrada suficientemente grandes. (Si tengo una complejidad O(n), otra instancia no puede tardarme O(n log(n)) porque si).

-> Hace que no necesitemos ninguna unidad temporal para medir el tiempo (no necesitamos que O(...) tenga tiempo (ms, mins, horas))


### Análisis asintótico
***
* Los distintos algoritmos para resolver un problema (dos algoritmos diferentes) pueden tener mucha diferencia de tiempo.
* Cuando el tamaño de nuestra información es pequeño, no nos daremos cuenta de "cuan lento" o "cuan rápido" es nuestro algoritmo.
* Cuando el **tamaño se vuelve muy grande**, podemos observar los **costos** del algoritmo pueden comenzar a variar.

**Conceptos clave del análisis asintótico**
-> **Orden de T(n):** mide la complejidad de un algoritmo en función de su magnitud MÁS dominante. Si el costo de un algoritmo es similar a, por ejemplo, $T(n) = 5*n² + n + 1$ => Orden cuadrático (porque el término dominante es $n²$).

-> **Comportamiento asintótico:** comportamiento para valores de la entrada suficientemente grandes.

*¿Pero si mejora mi arquitectura, no tardaría menos tiempo en resolver los algoritmos? ¿No sería más rápido ejecutar ahora las operaciones?*

* Planteamos un modelo de medidas de complejidad algoritmica.
-> O (O grande) cota superior.
-> $\Omega$ (omega) cota inferior.
-> $\Theta$ (theta) orden exacto de la función (una función h acotada por otras dos funciones f y g, similar al sandwich de análisis 1).

### O grande
***
* Se considera a una cota superior para el tiempo de ejecución de un algoritmo (no podrá pasar de ese valor).
* f $\in$ O(g) nos dice que f **no crecerá NUNCA** más rápido que g => g es cota superior de f.

![[Pasted image 20231009184043.png]]

![[Pasted image 20231009184217.png]]

### $\mathbf{\Omega}$ **Omega**
***
Se considera a una cota superior para el tiempo de ejecución de un algoritmo (no podrá pasar de ese valor).
* f $\in \mathcal{\Omega}(g)$ nos dice que f **CRECERÁ SIEMPRE** más rápido que g => g es cota Inferior de f.

![[Pasted image 20231011193419.png]]
![[Pasted image 20231011193353.png]]

### **$\mathbf{\Theta}$ Theta (Complejidad justa)**
***

![[Pasted image 20231011193844.png]]

![[Pasted image 20231011193904.png]]

![[Pasted image 20231011193927.png]]

### Definiciones y propiedades para tener a mano
***
**Definición de las Cotas**
![[Pasted image 20231011191754.png]]

**Propiedades de las O**
![[Pasted image 20231011191739.png]]


