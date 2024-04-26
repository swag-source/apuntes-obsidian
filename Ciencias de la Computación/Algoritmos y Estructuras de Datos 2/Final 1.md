***
**Ejercicio 1**
Se cuenta con n elementos que contienen una clave primaria y una secundaria. Hay m<<n claves secundarias distintas. Proponga dos formas eficientes y distintas que permitan ordenar los n elementos, en base a las dos claves (primero la primaria, luego la secundaria). Una de las dos formas tiene que utilizar solo arreglos.

**Ejercicio 2**
Se desea implementar un TAD que permita representar un conjunto de números racionales, donde a las tradicionales operaciones de Agregar(S,x) y Borrar(S,x) se agrega la siguiente: CLOSERTOAVG(S), que toma como input un conjunto S y devuelve el valor contenido en S más cercano al promedio de los valores contenidos en S. Discutir la implementación de este TAD utilizando variantes de al menos 4 estructuras de datos vistas en clase para representar conjuntos/diccionarios standard.


**Ejercicio 3**
Decir si es V o F (justificar o dar contraejemplo) 

-Sea S arreglo de claves representado por max-heap. Sean S\[i\] y S\[j\] claves del heap / i < j y S\[i\] < S\[j\] → el arreglo obtenido intercambiar de S\[i\] y S\[j\] sigue siendo max-heap. 

-Sea S arreglo de claves representado por max-heap. Sean S\[i\] y S\[j\] claves del heap / i < j y S\[i\] > S\[j\]→ el arreglo obtenido intercambiar de S\[i\] y S\[j\] sigue siendo max-heap.


**Ejercicio 4**
Explique la relación entre invariante de representación y complejidad algorítmica, y entre función de abstracción y la demostración de que el diseño es correcto con respecto a la especificación.


#### Respuestas
**Ejercicio 1**

Mi idea es: al recibir un arreglo de claves de la forma <(c1, c2), ... , (c1,c2) > será crear un arreglo res el cual recibirá todas las claves de manera ordenada. 

Primero, inicializo un array de tuplas res con los n elementos de entrada => O(n)

A los valores que recibimos como entrada les aplicamos el algoritmo de Array-To-Heap (pedimos que sea un min-heap) para luego extraer los mínimos e insertarlos en el array que creamos res. => O(n) extraer los mínimos + O(logn) heapificar => O(nlog(n))

``` Pseudocódigo
funcion sortClaves(n: array<Tupla<c1, c2>>) {
	Tupla[] res;

	array2Heap(n); //O(n)

	for(int i = 0; i < n.length(); i++){
		elem = n.extraerMinimo()   //O(nlog(n))
		res.insertar(elem)    //O(n)
	}

	return res;

=> O(n + log(n)) =  O(nlog(n))
}

```



**Ejercicio 2**

La implementación del TAD consiste en utilizar 3 estructuras de datos distintas. En primer lugar utilizamos un conjunto lineal el cual almacenará los valores racionales de manera simple. La ventaja es que acceder al mismo y realizar operaciones de borrado, agregado llevan tiempo lineal. 

Por otro lado, realizamos una extensión del conjunto donde guardaremos los números racionales y su respectiva "distancia" al valor promedio. Por último, guardamos en una variable de tipo float el promedio de los elementos de esta lista. 

Nuestra idea es buscar de todos los números la menor de las distancias de los números racionales hacia el promedio. En el caso de que hubiera empate en distancia entre dos números racionales, tomamos la posición más cercana al inicio del conjunto (es decir, más a la izquierda).


```
Modulo ConjuntoExt implementa ConjuntoExtImpl {
	var promedio : float;
	var conjuntoExtendido : conjuntoLog<float, float>;
}

function closerToAvg(c: conjuntoExt){

import random


def crear_conjunto_racionales(n):

# Crea un conjunto de números racionales con 'n' elementos

conjunto = set()

for _ in range(n):

# Genera un número racional aleatorio (en este caso, un número entero aleatorio)

racional = random.uniform(0,1) # Modifica el rango según tus necesidades

conjunto.add(racional)

return conjunto

  

def calcular_promedio(conjunto):

# Calcula el promedio de los números en el conjunto

if conjunto:

return sum(conjunto) / len(conjunto)

else:

return 0 # Si el conjunto está vacío, devuelve 0 como promedio

  

def crear_diccionario_diferencias(conjunto):

# Calcula el promedio del conjunto

promedio = calcular_promedio(conjunto)

# Crea un diccionario con las claves como los valores del conjunto

# y los valores como la diferencia en módulo con el promedio del conjunto

diccionario_diferencias = {}

for valor in conjunto:

diferencia = abs(valor - promedio)

diccionario_diferencias[valor] = diferencia

return diccionario_diferencias

  

# Ejemplo de uso

conjunto = crear_conjunto_racionales(10)

print("Conjunto de números racionales:", conjunto)

  

promedio = calcular_promedio(conjunto)

print("Promedio del conjunto:", promedio)

  

diccionario_diferencias = crear_diccionario_diferencias(conjunto)

print("Diccionario de diferencias con el promedio:", diccionario_diferencias)

  

def encontrar_mas_cercano_al_promedio(diccionario):

if not diccionario:

return None # Devolver None si el diccionario está vacío

# Encontrar la clave con la mínima diferencia al promedio

min_diferencia = float('inf')

mas_cercano = None

for clave, diferencia in diccionario.items():

if diferencia < min_diferencia:

min_diferencia = diferencia

mas_cercano = clave

return mas_cercano

  

# Ejemplo de uso

mas_cercano = encontrar_mas_cercano_al_promedio(diccionario_diferencias)

print("Clave con la mínima diferencia al promedio:", mas_cercano)


}
```

**Ejercicio 3**
F. Supongamos que tenemos el arreglo \[21,18,20,4,3,8,19\] representado por Max-Heap. Vemos que si tomamos i = 4, j = 7 (indexado en 1) => s\[i\] = 4 < s\[j\] = 19 y se cumple el invariante del max-heap. 
Luego, si realizamos el intercambio,  \[21,18,20,19,3,8,4\], vemos que i < j se cumple pero s\[i\] = 19 < s\[j\] = 4 (falso) rompe con el invariante del max-heap.

F. Supongamos que tenemos el arreglo \[30,29,9,28,27,3,2\] representado por Max-Heap. Vemos que si tomamos i = 2, j = 3 (indexado en 1) => s\[i\] = 29 > s\[j\] = 9 y se cumple el invariante del max-heap. 
Luego, si realizamos el intercambio,  \[30,9,29,28,27,3,2\], vemos que i < j se cumple pero s\[i\] = 9 > s\[j\] = 29 (falso) rompe con el invariante del max-heap.


**Ejercicio 4**
Explique la relación entre invariante de representación y complejidad algorítmica, y entre función de abstracción y la demostración de que el diseño es correcto con respecto a la especificación.

A la hora de realizar una implementación de un Tipo Abstracto de Dato (TAD), es necesario reconocer que existen propiedades estructurales las cuales dicha implementación deberá garantizar previo, durante y posterior a la ejecución de la misma; esta noción la reconocemos como Invariante de Representación. 
Cuando elegimos la forma en la cual buscamos implementar dicho TAD, hacemos elección de estructuras de datos las cuales garantizan resolver una determinada operación con un costo de operación determinado. De esta manera, como nuestra implementación posee ciertas restricciones impuestas por nuestro invariante de representación (ya que en el caso de que no se cumpla, la misma no sería una instancia válida del TAD) y nuestra elección de las estructuras que mejor implementa ese TAD, podemos observar como claramente el Invariante de Representación determina directamente la complejidad algoritmica de nuestra implementación del TAD. 

Por otro lado, a la hora de verificar que nuestro diseño es correcto con respecto a la especificación, dicha tarea se hace a partir de la función de abstracción. La función de abstracción es una función que mapea elementos de un conjunto de instancias (implementadas) válidas a un conjunto de instancias abstractas del mismo tipo de dato. En sencillo, conecta el mundo de la implementación con el mundo del TAD siempre verificando que dichas instancias cumplan el invariante de representación. Para ello, la verificación consiste en demostrar que, siendo X una instancia del TAD y abs(X) la instancia abstracta de X, entonces al aplicar f a la instancia abstracta de X y a X luego de realizar una operación sobre la implementación, el resultado de la operación deberá ser observacionalmente igual para ambas instancias.

