
### Ejercicio 1
***
El objetivo del ejercicio consiste en recibir un array de números guardados en half-word (el cual accedemos a partir de la etiqueta *arr*) e ir sumando los elementos tal que se cumpla que:
* Se enmascare únicamente los bits que se encuentran en posiciones pares con el número 0x5555 (puesto que trabajamos en half-word).

Paso 1: **Main**

Inicializo mi programa utilizando los registros a0 para llevar tracking de la suma, a1 con la dirección donde tengo cargado el array *data*, a2 con la cantidad de elementos que tendré en el array (el cual me sirve luego de iterador) y a3 hardcodeando la constante utilizada en el AND.

Llamo luego a suma_arreglo, mi función que se encarga de realizar el ejercicio.

![[Pasted image 20231121102530.png]]


Paso 2: **Suma_arreglo y loop**

En mi primer etiqueta, el objetivo es guardarnos en otros 4 registros los valores que habíamos cargado anteriormente en las posiciones \[a0, ... , a3\] para luego poder manipularlos y hacer la ejecución del loop.

En el cuerpo del loop, la lógica es la siguiente:
* Cargo en $t_{0}$ el valor almacenado en $S_{1}$ con el offset en 0.
* Realizo el *AND* entre el número guardado y la constante 0x5555.
* Acumulo en mi registro $S_{0}$ la suma de los elementos.
* Me muevo 2 bytes para recorrer mi arreglo puesto que estamos trabajando en *half-word*.
* Decremento la cuenta de mi iterador/longitud en 1.
* Repito el loop.

*Aclaración*: la etiqueta beqz por cada iteración está checkeando si mi longitud es 0 (es decir, no tengo más elementos para sumar), en el caso que lo fuera, salta a la etiqueta return.

![[Pasted image 20231121102755.png]]


Paso 3: **Finalización**

Habiendo evaluado que $S_{2}$ es cero, la etiqueta return nos mueve el contenido que teníamos en $S_{0}$ a la dirección de retorno a0 (por convención) y luego llama a la etiqueta halt para aguardar instrucciones (y en definitiva cortar el programa).

![[Pasted image 20231121104005.png]]


### Ejercicio 2
***
El objetivo de este ejercicio consiste en tomar dos arreglos *src* y *dst* cuya longitud es la misma, realizar un OR sobre ambos números y guardar el valor resultante en la i-esima posición de *dst*.

Paso 1: **Main**
Al igual que el ejercicio 1, cargamos los valores que utilizaremos en las posiciones a0 hasta a2. Puesto que la convención dice que a0 y a1 son direcciones de store y retorno, guardé mi dirección a *dst* en a1.

![[Pasted image 20231121104449.png]]



Paso 2: **Array_or y Loop**

![[Pasted image 20231121105512.png]]

Misma idea, cargamos desde $S_{0}$ hasta $S_{2}$ con las respectivas posiciones de $A$ y entramos al loop.

*Secuencia del loop*:
* Movemos temporalmente a $t_{0}$ la constante cero (mi valor resultante)
* Cargamos en a3 y a4 los inmediatos $S_{0}$ y $S_{1}$ (nuestros i-ésimos elementos de ambos arreglos).
* Realizamos el OR entre ambos inmediatos y los cargamos en $t_{0}$ para luego guardarlos en el registro $S_{1}$.
* Realizo un "offset" a la posición de memoria de 4 bytes tanto en *src* como en *dst* y vuelvo a realizar el loop la cantidad de elementos que tenga en el array.
* Siempre checkeo en la primer linea que mi longitud no sea igual a 0, si lo fuera salto a la etiqueta return.

**Paso 3: Finalización**

Puesto que nos interesa devolver la posición del array modificado (en nuestro caso *dst*), la instrucción return mueve al registro *ra* (return address) la posición donde se encuentra *dst* (guardada en a1) y salta a la etiqueta halt donde finaliza mi ejecución.

![[Pasted image 20231121110305.png]]




