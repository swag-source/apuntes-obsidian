***
## Correctitud de un programa
***
>[!Definición]
> Decimos que un programa *S* es *correcto respecto de una especificación* dada por una una precondición *P* y una postcondición *Q*, si siempre que un programa comienza en un estado que cumple la precondición *P*, el programa **termina su ejecución**, y en su estado final satisface Q.

Utilizamos la notación siguiente para demostrar que un programa es correcto respecto de su especificación:

**Notación**: Cuando *S* es correcto respecto de la especificación (*P, Q*), lo denotamos con la siguiente **Tripla de Hoare**:

$$\{P\} \ S \ \{Q\}.$$

El objetivo de utilizar la tripla de Hoare para demostrar la correctitud de un programa a partir de la misma es obtener una formula lógica que denotamos con la letra $\alpha$ tal que 

$$\alpha \ \text{es valida} \ \iff \{P\} \ S \ \{Q\}.$$

## Weakest precondition
***
Al momento de buscar un mecanismo para demostrar "automáticamente" la correctitud de un programa respecto a la especificación, nos interesa buscar de todo el universo de precondiciones posibles *P* para mi programa *S* que satisfaga *Q*, la precondición que menores restricciones imponga a nuestro programa sin perder valores posibles que satisfagan *Q*.

![[Pasted image 20240220122034.png]]

**Notación:** notamos $wp(\textbf{S}, Q)$ a la precondición más debil del programa *S* que satisface la postcondición *Q* de nuestro programa.

Si nosotros podemos probar que la precondición $P$  se encuentra "contenida" dentro de la $wp(\textbf{S}, Q)$, entonces la Tripla de Hoare $\{P\} \ S \ \{Q\}$ es válida.

$$P \Longrightarrow_{L} wp(\textbf{S}, Q)$$

Entonces necesitamos un mecanismo para obtener la Weakest Precondition para mi programa *S* a partir de la postcondición *Q*.

* Vemos que la precondición más debil se *deriva* a partir de la postcondición que le pedimos a nuestro programa.
* Puede ocurrir al revés, pero suele ser más dificil.

![[Pasted image 20240220141617.png]]
![[Pasted image 20240220141759.png]]

*Enunciar la propiedad de monotonía de la Weakest Precondition y explicar la intuición detrás de la propiedad.*


La propiedad de monotonía de la Weakest Precondition establece que
$$P \Longrightarrow_{L} Q \ \therefore \ wp(\textbf{S}, P) \ \Longrightarrow_{L} wp(\textbf{S}, Q) \ $$
aquí observamos que este corolario relaciona los predicados P y Q con sus precondiciones más débiles. La noción detrás de este corolario es demostrar que si P es un predicado que se encuentra contenido dentro del predicado Q (por la implicancia), entonces sus precondiciones deberán "automáticamente" cumplir la misma propiedad.

**Demostración**
Sabemos que $P \Longrightarrow_{L} wp(\textbf{S}, P)$ por Axioma 1 y $Q \Longrightarrow_{L} wp(\textbf{S}, Q)$. Por la premisa sabemos que $$P \Longrightarrow_{L} Q$$por ende $$(P \Longrightarrow_{L} Q) \Longrightarrow_{L} (wp(\textbf{S}, P) \ \Longrightarrow_{L} wp(\textbf{S}, Q))$$por transitividad, como se quería probar.


### Teorema del Invariante y Terminación del ciclo
***

Al momento de verificar si un ciclo dentro de un programa *S* es correcto respecto a la especificación (es decir, no se ejecuta infinitamente), introducimos la herramienta del **Teorema del Invariante** y **Terminación del ciclo** para probar los siguientes puntos:

**Teorema del Invariante** -> Al entrar al ciclo se cumple correctamente la precondición
1. $P_{c} \Longrightarrow_{L} I$
2. $\{I \land B\} \ S \ \{I\}$
3. $I \land \neg B \Longrightarrow_{L} Q_{c}$

**Teorema de Terminación** -> Si entramos al ciclo, garantizamos que también saldremos del mismo.
4. $\{I \land B\ \land \text{f}_{v} = v_{0}\} \ S \ \{\text{f}_{v} < v_{0}\}$
5. $\{I \ \land \ \text{f}_{v} \leq v_{0} \} \Longrightarrow_{L} \neg B$ 

Donde *I* es un invariante, el cual describe un estado que se satisface cada vez que
comienza la ejecución del cuerpo de un ciclo y también se cumple cuando la ejecución del cuerpo del ciclo concluye.

* El invariante del ciclo representa la Hipotesis Inductiva (H.I) del ciclo, es decir, todo aquello que no cambia durante las sucesivas ejecuciones de *S*.

***Explicación punto por punto***
1. $P_{c} \Longrightarrow_{L} I$: La precondición del ciclo deberá ser un "subconjunto" o estar contenida dentro del Invariante (es decir, no puede no cumplir lo que el invariante del ciclo te pide, sino sería incorrecto o inválido el estado en el que ingreso al ciclo).

2. $\{I \land B\} \ S \ \{I\}$: Durante toda la ejecución del ciclo, es decir, mientras la guarda *B* sea verdadera, la intersección entre el Invariante y B deberán estar contenidos dentro del Invariante. No puede no cumplirse el invariante en alguna de las iteraciones del ciclo.

3. $I \land \neg B \Longrightarrow_{L} Q_{c}$: En el momento que se deja de cumplir la condición de la guarda (es decir, salimos del ciclo), el estado en el que salimos del ciclo deberá estar contenido dentro del conjunto de postcondiciones $Q_{c}$ válidas; es decir, tiene que salir en un estado que la cumpla.

4. $\{I \land B\ \land \text{f}_{v} = v_{0}\} \ S \ \{\text{f}_{v} < v_{0}\}$: ??

5. $\{I \ \land \ \text{f}_{v} \leq 0 \} \Longrightarrow_{L} \neg B$ : Cuando la función variante es menor o igual que 0, es decir, nuestro contador "ya terminó" sus iteraciones, entonces el estado en el que se acaba la ejecución de las iteraciones deberá estar contenido dentro de la negación de la guarda B (cuando se sale del ciclo).