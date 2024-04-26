***
### Lenguajes de programación
* A la hora de entender el funcionamiento de un microprocesador, tenemos que entender que la única interpretación del mundo es a partir de dos estado:
	* V o F
	* 1 o 0 
	* Tensión V o Tensión 0


**Lenguaje Ensamblador: Una sentencia = una instrucción**
* Como al ser numano se le dificulta leer grandes instrucciones en binario (no somos computadoras), se desarrollan lenguajes más accessibles al léxico humano.
	* Lenguaje ensamblador.
	* Se denomina este programa *código fuente*.
	* *Instrucciones como:* MOV, ADD, STR.
	
* Entre este lenguaje entendible por nosotros y el binario, necesitamos un intermediario => **Assembler**. Esta herramienta toma nuestras instrucciones y encuentra una expresión para la misma equivalente en código binario.

![[Pasted image 20240404172922.png]]

**Lenguajes de alto nivel: Una sentencia = varias instrucciones** 
* Observamos que hay un cambio en el paradigma de la programación. Buscamos que en un lenguaje de programación, una sola sentencia => varias instrucciones dentro del lenguaje de ensamblador.
* Con ayuda de un **Compilador** este texto (código) se pasa a números binarios, explotando más las instrucciones del microprocesador.

*Overview hasta ahora*
```
Código alto nivel -> Compilador -> Binario.
Código assembly -> Ensamblador -> Binario.
```


En un programa en C, tenemos ciertos componentes lógicos básicos que caracterizan nuestra implementación:
-> **Funciones:** Sentencias que definen las diferentes operaciones que se ejecutan una a una.
-> **Variables:** Contienen los datos del programa que deseamos almacenar, los cuales se modificarán (o no) luego de su operación.
* Tenemos una función "obligatoria" que debemos tener en nuestro código: *función main()*. Todo programa comienza su ejecución a partir de la función *main*.
* Las funciones que utilizamos en *main* podemos escribirlas en el mismo archivo o mismo *importarlas* desde otro código (objeto). 

-> **Bibliotecas:** Son conjunto de código preescrito y compilado que proporciona funciones, clases y rutinas reutilizables para facilitar el desarrollo de software. Estas bibliotecas contienen implementaciones de algoritmos comunes, estructuras de datos, funciones de utilidad y otros componentes que los desarrolladores pueden utilizar en sus propios programas para realizar tareas específicas sin tener que escribir todo el código desde cero.

-> **Directivas:** *ej: \#include*. Esta "instrucción" o *directiva* no es levantada por el procesador sino por el compilador. Pide que al momento de compilar mi programa, deberá incluir el archivo cabecera con las definiciones que nos trae esa biblioteca (particularmente la biblioteca **libc**).

``` Ejemplo en C
#include <stdio.h>

int main(void){
	printf("hello world");
	return 0;
}
```

>[!Concepto Importante]
>¡stdio.h no contiene el codigo de la biblioteca! Es un archivo de texto en el que solamente se declaran las funciones que componen la biblioteca para que el compilador pueda conocer la sintaxis correcta para su invocacion desde los programas. La  biblioteca de codigo está en otro archivo (binario). El código fuente de las funciones que componen esta biblioteca, tampoco está en stdio.h. No olvidar este concepto.

* De alguna manera podemos a pensar que las bibliotecas son como un libro donde compilador, al momento de compilar, simplemente busca en esa biblioteca el nombre de la función que necesitará compilar para luego buscar donde estará su "implementación".
* En ningún momento contiene el código de dicha implementación, sino simplemente información o definciones a esas implementaciones (guardadas en otro lado).


### Compilador
***
El compilador es una herramienta la cual verifica que un programa fuente siga con la sintaxis del lenguaje de programación. Si todo se cumple adecuadamente, generará un archivo de tipo *objeto* para poder ser ejecutado.
* Reemplaza nombres lógicos que adoptamos (por ejemplo, i = 4) por las direcciones de memoria donde se ubican las mismas.
* Solamente lee lo que está el archivo (es decir, si tenemos una función auxiliar "importada", simplemente dejará un espacio luego para el que el Linker se encargue). No puede leer *referencias* a otros archivos
* El compilador antes de "compilar" el programa, se encarga de enviar el programa al *preprocesador* el cual se encarga de eliminar comentarios y reemplazar macros.

### Linker
***
 Es un programa que toma aquel objeto generado por el compilador y vincularlo con las bibliotecas de código que contienen a las funciones que contiene nuestro programa. 

### Modelo de Ejecución SIMD en IA-32
***
-> *Fundamentos* (Series de Fourier, Lagrance y Laplace)
-> *Procesamiento de Señales digitales*
-> *Modelo de ejecución SIMD*
-> *Implementaciones SIMD en x86*
-> *Instrucciones*


**Fundamientos matemáticos del procesamiento de señales (Serie de Fourier)**

* Dentro del procesamiento de señales, el trabajo de Fourier, Lagrange y Laplace es fundamental en el campo.
* Su descubrimiento demostró que $sen(\lambda \ x), cos(\lambda \ x) \ \forall \lambda \in \mathbf{z}$ son funciones ortogonales y permiten calcular cualquier otra función del tipo:   

![[Pasted image 20240409102909.png]]

El resultado de esto, es que nosotros podremos empezar a aproximar ciertas funciones a partir de otras fundamentales.

Observamos como a partir de $n$-armónicos aproximamos la función:

$$
 f(t) = \left\{
\begin{array}{ll}
      1 & -1 \leq t \leq 0 \\
      -1 & 0 \leq t \leq 1
\end{array} 
\right.
$$

![[Pasted image 20240409103455.png|200]]
![[Pasted image 20240409103817.png|200]]

Por ende mi función continua $f(t)$ pasa de ser una función continua en el tiempo a discreta en la frencuencia (*ver representación en el dominio de la frecuencia*). => **espectro de frecuencia**.

*Observaciones*
* Podemos descomponer cualquier tipo de señales en funciones seno y coseno.
* Llegamos a un punto donde eliminar armónicos (de orden muy alto) puede ser computacionalmente una buena idea. Estamos definiendo un *ancho de banda*.
* Esta idea de eliminar ciertas señales en el dominio de la frecuencia se denomina "eliminar bajos".

**Procesamiento de Señales Digitales**

* El proceso de digitalización de señales se corresponde al siguiente modelo.
* Por definición, tenemos que una señal es la *variación de una magnitúd en función de otra (en general, es una variable temporal) -> amplitud en función del tiempo*.

![[Pasted image 20240409105040.png]]

* La señal física tiene que poder ser digitalizada donde se toma un valor instantáneo de la señal y se "retiene" el valor de tensión en una capacidad. => **Sampleas un valor**.
* Tenemos ciertas arquitecturas que se encargarán especificamente del procesamiento de las señales.

-> Estas arquitecturas son una CPU de propósito dedicado, diseñado para cálculos y procesamientos de un único tipo de dato.

-> Se optimizan para el procesamiento de datos pequeños (8, 16, 24 o como mucho 32 bits). Por ejemplo, pixeles de imagen, valores de audio, valores térmicos.

-> Para aplicar un conjunto de algoritmos deseado sobre un conjunto de entradas que tomamos, necesitamos que estos algoritmos estén acompañados también de una arquitectura y métodos específicos para dichas tareas. => **Data Level Paralelism** o **Paralelismo a Nivel de Data**.


**Modelo de ejecución SIMD (Single Instruction Multiple Data)**

*A priori* nosotros cuando realizamos operaciones entre dos números de 8 bytes, por ejemplo, no hacemos el producto entre ambos números sino que tenemos que operar byte a byte. En grandes cantidades de datos, eso es muy *costoso* a nivel computacional. => **Single Instruction Single Data** 

*Ejemplo:* cada vez que querramos realizar una operación entre $a_{n}$ y $b_{n}$ tendríamos que mover todos los punteros de la estructura a sus siguientes posiciones.

![[Pasted image 20240409112057.png | 300]]

**La solución de SIMD:**
![[Pasted image 20240409112601.png | 500]]


### Implementación SIMD en x86
***
-> Arquitectura completa
-> Números reales
-> Formato de punto fijo
-> Formato de punto flotante
-> Codificación de reales
-> Extensiones AVX.

**Arquitectura completa**: Vemos una representación de la arquitectura x86 basado en SIMD. Esta ISA es la representación de los registros de arquitecturas SIMD en 32 bits.

![[Pasted image 20240409115325.png | 500]]
* Los registros de formato MMX son exactamente la parte baja de los registros de la FPU.

La siguiente arquitectura es de 64 bits.
-> Aumenta la cantidad de registros XMM de 8 a 16.

![[Pasted image 20240425143135.png|500]]

Cuando trabajamos con ciertos valores, podemos tener ciertos errores 
### Instrucciones
***
-> Transferencias.
-> Aritmética en algoritmos DSP.
-> Instrucciones de punto flotante.
-> Instrucciones para manejo de enteros en SSEn.
-> Instrucciones para manejo de cacheabilidad.

**Transferencias**
* MOVD: Mover un valor de 32 bits a la parte baja de un operando de 64 o 128 bits.
* MOVDQ: Mover valor de 64 bits a parte baja de un operando de 128 bits.
* MOVDQA/U: Mover valores de 64 bits *alineados* y *desalineado* respectivamente.
* MOVAPD: Mover 128 bits de Punto Flotante Double Precision *alineados*.
* MOVAPS: Mover 128 bits de Punto Flotante Simple *desalineados*. 

