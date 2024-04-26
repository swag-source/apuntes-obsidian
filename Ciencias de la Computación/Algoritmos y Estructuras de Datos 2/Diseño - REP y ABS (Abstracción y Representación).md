****
### Conceptos clave
1. *Abstracción*
2. *Contratos*
3. *Instancias*
4. *Observadores*
5. *Funciones*
6. ***Cómo** hacemos* no *qué hacemos*
7. Encapsular
8. Interfaz

**Introducción**
* Cuando tenemos que resolver sistemas complejos, la idea es realizar *Abstracción* para simplificar un problema sin abrumar de información a la hora de resolver un problema. Se rescatan los conceptos más importantes de un *sistema*.
* Al operar con alguna herramienta de programación, hay partes del programa que utilizamos sin ver cómo está implementada/representada (no nos importa).
* Escribimos *contratos* entre el desarrollador y el creador para la utilización de ciertas estructuras.

## ¿Qué es un TAD?
***
Un **TAD**, Tipo Abstracto de Datos, es un conjunto de valores y las operaciones asociadas al conjunto en cuestión.
* Es abstracto ya que, para utilizarlo, no necesitamos conocer los detalles de representación interna ni cómo están implementadas.
* Describe el *qué* y no el *cómo*.
* Son una forma de modularizar datos.

*Ejemplo:* 
El TAD **conjunto** es una abstracción de un conjunto matemático, que contiene "cosas" (del mismo tipo), es decir, sus elementos.
-> Tengo las operaciones: agregar, sacar, ver pertenencia, cantidad, sumar, restar, etc.
 + En esta instancia no me interesa la forma en la que se implementa el TAD (una lista, un arreglo, etc.)

### Observadores
+ El estado de una *instancia* de un TAD lo describimos a través de *variables* o *funciones de estado* llamadas **observadores**.
+ En un instante de tiempo, el estado de una instancia del TAD estará dado por el estado de todos sus observadores.
+ El conjunto tiene que ser *completo*. Tenemos que poder observar todas las características que nos *interesan* de las instancias.
+ A partir de los observadores, se tiene que poder distinguir si dos instancias son distintas.

>[!Instancias]
>Al final del día, una instancia es un elemento, o algo que viene en particular de un determinado conjunto. Es decir, si tengo un TAD "conjunto", entonces la secuencia S = <19,1,4,5> es una **Instancia** del Tipo Conjunto.


### Operaciones de un TAD
***
* Las operaciones del TAD indican qué se puede hacer con una instancia de un TAD (cómo podemos operar con una instancia).
* Para *especificar* estas operaciones, usamos precondiciones y postcondiciones.
* Es importantisimo **Inicializar** un conjunto (es decir, defino un constructor para crear un conjunto, *del ejemplo de Conjunto(T)*). Para eso tengo que definir la inicialización.
* Para evitar la subespecificación, tenemos que describir el estado *completo* 
* Los observadores son **solo** para especificar. No operaciones que utilizamos en el código.

![[Pasted image 20230920171003.png]]


* Cuando planteamos un TAD, buscamos representar una cierta estructura de datos a partir de las operaciones que la misma puede hacer.
* Partimos de un modelo teórico sobre qué debe hacer la estructura (especificación) y planteamos una forma de implementar ese TAD (diseño). Toda estructura tiene un conjunto de operaciones contenidas en sí.
* Estas operaciones se encuentran dentro de la ***interfaz***.

#### Interfaz
***
-> La interfaz de un módulo es una capa "intermedia" entre las estructuras y los algoritmos que implementan el TAD y el usuario que las utiliza.

* Estos modulos con una determinada interfaz tienen:
	* Signatura (tipos de los parametros de *in/out*.)
	* *Pre* y *Post* de las operaciones.
	* Complejidad (espacial, temporal).

### Esructura de representación
***
-> Todo diseño de TAD debe representar una ***estructura de representación***, es decir, debe respetar las "consignas" de la estructura abstracta que estamos buscando implementar.
* No solo representamos la estructura, sino el valor (output) de las operaciones sobre dicha estructura.

-> Relación entre **Representación** y **Abstracción**

### Invariante de representación
***
>[!Definición]
>El Invariante de Representación indica las condiciones necesarias que debe cumplir una instancia de un TAD previo, durante y posterior a la ejecución del mismo. Estas condiciones definen propiedades estructurales del tipo y, de no cumplirse el invariante de representación, existiría una falla en la representación de la clase.

* El invariante de representación es un predicado/función booleana que nos devuelve *True* <=> el conjunto de valores pasados como parámetro representan instancias válidas de la implementación.
* **El dominio sería el genero de representación de nuestro TAD y la imagen un booleano**.
* Establece una relación entre la idea que buscamos representar y el valor representado por el TAD.
* **Asegura coherencia entre las diferentes partes de nuestra estructura (todo debe permanecer bajo los confines del invariante, si algo se escapa, entonces ya no representaríamos la idea que queremos).**
* Debe cumplirse en la Pre y la Post de las operaciones.

Informalmente: 
* invRep(i: InstanciaDelTAD) = True <=> i $\in$ Genero de representación.

### Función de abstracción
***
*¿Podemos demostrar que una TAD es correcto respecto a su especificación?*

-> Nos interesaría probar "la vuelta" de el invariante de representación. Es decir, dada alguna estructura abstracta, queremos una función que mapee a alguna instancia asbtracta en particular de nuestro TAD.

* funcAbs(estructuraDeRepresentación) -> valorAbstracto

-> Es decir, agarra una estructura que utilizamos para representar un valor abstracto y me devuelve esa estructura abstracta en cuestión.

![[Pasted image 20231019135000.png]]
* Si yo tengo un elemento X, la If **implementación** de X y la Abs **abstracción** de X deberán mapear **OBSERVACIONALMENTE** a lo mismo.

>[!Definición]
>La Función de Abstracción es una función que mapea elementos de un dominio abstracto (TAD) a elementos de un dominio físico o implementativo. Esta función describe la relación entre la representación interna de los datos y su significado o semántica en el contexto del problema que se está resolviendo.

