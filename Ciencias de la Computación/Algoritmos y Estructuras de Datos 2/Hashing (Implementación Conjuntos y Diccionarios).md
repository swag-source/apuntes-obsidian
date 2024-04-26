***
### Conceptos clave
* Diccionario
* Criptografía
* Colisiones
* Direccionamiento cerrado
* Direccionamiento abierto
***

* *Motivación por ejemplo:* tenemos un conjunto de clientes y queremos poder almacenarlos y acceder a ellos rápidamente!
	-> Podríamos guardarlo en un arreglo con todas las posibles claves tal que 
	$0 \leq claves \leq$ \#\DNIposibles.
* Vemos que así acceder mi *complejidad $\mathcal{O}$(1).* (Buenisimo).
* Estamos desperdiciando MUCHISIMO espacio vacío (Malisimo).

*Planteamos dos problemas fundamentales*
	-> Solo sirve para claves que son enteros no negativos.
	-> Mucho desperdicio de memoria!

![[Pasted image 20231112150041.png]]
### Hashing perfecto y colisiones
***
* Idealmente para todo conjunto K *(de claves)*, queremos poder mapear una posición del arreglo H tal que si k1 != k2 => H(k1) != H(k2), es decir, evitar colisiones. (es decir, una función inyectiva)
* También que H(k1) != H(k2) => k1 != k2
* Encontrar esta función *perfecta* rara vez es posible, solo casos muy especiales y poco pragmáticos.

![[Pasted image 20231112151933.png]]

*Idealmente así debería verse una función de hashing perfecta*.

**Habitualmente nos enfrentamos con que**
* k1 != k2 => H(k1) = H(k2) -> colisión en la función de hash.
* Paradoja del cumpleaños

-> De alguna forma queremos encontrar algún método que se encargue de la *resolución de colisiones*

### Tablas de Hash
***
* Forma de implementar un Diccionario/Conjunto representado a partir de una tupla <T, h> donde: 
	-> $T$: arreglo de $n$ elementos 
	-> $h: K \rightarrow \{0, ..., n-1\}$ su función de hash con K claves posibles que nos permite calcular la posición de un elemento en un arreglo de $n$ elementos.
	
* Debe ser deterministica -> No puede valer que un valor de entrada produzca dos valores de salida en dos momentos diferentes. 

### Resolución de colisiones
***
* Como quisieramos idealmente tener una función que sea inyectiva =>  $K \leq |n|$ (pero no podemos), necesitamos poder manejar esas "colisiones".

*Ejemplo colisión*: ejemplo con mi hash H(k) = k mod 5.

![[Pasted image 20231112170511.png]]

* tenemos alternativas para manejar este tipo de colisiones: *Direccionamiento Cerrado* y *Direccionamiento abierto*.
* Idealmente nosotros buscamos construir una función de hashing cuya distribución de datos sea lo más uniforme posible. Esto implica que a mayor uniformidad, menor es la probabilidad de colisión => $\frac{1}{n}$ ya que tenderia a ser "equiprobable". 

### Direccionamiento por cerrado / concatenación 
***
*Idea principal:* creamos un array de {0, 1, ..., N-1} elementos tal que cada posición contenga una lista enlazada con los elementos colisionados.

![[Pasted image 20231112172731.png]]

Suponemos que tenemos mi función H(k) = k mod 5

* K => {k1 = 0, k2 = 4, k4 = 10, k5 = 9, k7 = 14} 
=> H(k) = {H(k1) = H(k4) = 0, H(k5) = H(k7) = H(k2) = 4}

Por lo que vemos, una función de hash a partir de aritmética modular no siempre es la mejor idea!

![[Pasted image 20231112173524.png]]

* La última linea nos indíca que *en promedio* cuando la cantidad de elementos tiende a la dimensión de la tabla de hash, entonces la complejidad -> $\mathcal{O}$(1).
### Direccionamiento abierto
***
* A diferencia del direccionamiento por concatenación, los valores los almacenamos en la posición de la tabla de hash y utilizamos un mecanismo diferente para salvar colisiones.
* Hay que considerar que la función de Hash tenemos que agregarle un parametro más i tal que: $H(k,i)$ donde $i$ es la $i$-esima iteración guardada.
* Si la posición $k$ no está disponible, busco otra disponible a partir del *barrido*. 

Es decir:
$$h: U \times \{0,1,.....,n-1\} \longrightarrow \{0,1,.....,n-1\}$$

-> Si $H(K_{1})=H(K_{2})=i$ entonces al elemento que más *"tarde"* llegó lo movemos a la posición siguiente (si está disponible).
 
***Algoritmos de inserción y borrado***
![[Pasted image 20231113105524.png]]

![[Pasted image 20231113105544.png]]


***aclaración**:* si observamos el algoritmo de insertar, en el caso de que eliminaramos el primer elemento $h(k_{0}$) de una lista de colisiones y luego quisieramos insertar no podríamos, entonces no tendríamos forma de acceder a los siguientes elementos $h(k_{i+1}$) por como tenemos planteado nuestro algoritmo.


*Problema*: si tuviéramos colisiones, existen casos donde tendríamos que barrer toda la tabla para encontrar una posición disponible (**INEFICIENTE**). Dependiendo el método de barrido, cambia nuestra implementación de la tabla de Hash.

* **Barrido lineal**: Una vez que encuentra colisión, busca linealmente el siguiente espacio disponible o vacío. La función de hash se define como $h(k)=(h'(k)+i)$ $mod$ $n$, donde $h'(k)$ es la función de hash original, $i$ es el número de intento y $n$ el tamaño de la tabla de hash.

* **Barrido cuadrático**: Al igual que el barrido lineal, en vez de tener el $i$ término lineal, tenemos $i^{2}$. La función de hash se define como $h(k)=(h'(k)+i^{2})$ $mod$ $m$. 

* **Hashing doble**: En el doble hashing, se utilizan dos funciones de hash. Cuando se produce una colisión, en lugar de simplemente buscar el siguiente espacio vacío en la tabla, se utiliza una segunda función de hash para calcular un desplazamiento adicional. La fórmula general es $h(k,i) = h_1​(k)+i⋅h_2​(k)$ $mod$ $n$ donde $h_1(k)$ es la primera función de hash, $h_2(k)$ es la segunda función de hash, $i$ es el número de intento y $n$ es el tamaño de la tabla hash. Este método generalmente produce una mejor distribución de los elementos en la tabla hash y reduce la probabilidad de agrupamiento de colisiones.

### Funciones de hashing
***
Además de ser una estructura de datos *súper* conveniente para almacenar valores a partir de pares (clave, valor) por su rapidez de cómputo, también es una excelente estructura para guardar información de manera segura.
* Si queremos tener un registro bancario con los usuarios y números de tarjeta de crédito, podríamos construir una tabla de hash donde cada usuario esté hasheado.

Para asegurarnos que ninguna persona pueda "*descifrar*" ese texto escondido, tenemos que construir una buena función de hashing. Para eso pedimos que la función que creemos cumpla ciertos requisitos.

-> ***uniformidad de claves***: para cualquier valor del dominio $K$ haya igual probabilidad de caer en $H$ al aplicar $h(k) = q$ sea $\frac{1}{n}$ con $H \in \{0, 1, ..., n-1\}$ 

-> ***universalidad de claves***: para cualesquiera valores $k_{1}$ y $k_{2}$ $\in K$, la probabilidad de que $h(k_{1}) = h(k_{2})$ sea como mucho $\frac{1}{n}$.  

