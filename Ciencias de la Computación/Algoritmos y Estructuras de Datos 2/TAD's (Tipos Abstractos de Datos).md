***
Durante la clase veremos la noción de *especificación de TAD's*.
## Conceptos clave
1. *Abstracción*
2. *Contratos*
3. *Instancias*
4. *Observadores*
5. *Generadores*

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


