***
#### ¿Qué significa lo siguiente?

*Esta arquitectura tiene direcciones de 16 bits, direcciona de a cuatro bytes y opera con palabras de 64 bits.*

-> Direcciones
-> Direccionar a
-> Word/Palabra

* Cuando decimos *direccionar a* nos referimos al tamaño de los índices (cintita de 4 bytes).
* Al decir *direcciones de 32 bits* significa que puedo definir mis direcciones en n bits (en este caso, tengo 16 índices para guardar información 2³² direcciones posibles).
* La palabra/word es el tamaño que podemos alocar en un "bloquecito" para representar información (en este ejemplo, tenemos un word de 32 bits.)
![[Pasted image 20230928092825.png]]

* Podemos llegar a tener la situación que querramos almacenar en una palabra mayor cantidad de bits del que se puede. Es decir, en un word de 32 bits alocar palabras de 64 bits.

*Ejemplo:*
![[Pasted image 20230928094104.png]]

* Pasamos de una dirección real a una dirección virtual (donde interpreta que un bloque virutal son dos bloques virtuales, representando que dos word de 32 bits ahora es un word de 64 bits).
* Si tenemos \[AABB,CCDD,EEFF\] -> Direccionamiento físico 0<sub>x</sub>0 = AA
 --> M = memoria
 --> T = tamaño de mis tipos
 --> F = tamaño unidad direccionables
 
Entonces, si queremos calcular mi dirección física a partir de mi dirección virtual/lógica:
	DirecciónFísica = M <sub>t</sub>= \[i * T/f\]


### Introducción a Assembler
* Utilizamos una simplificación de Intel x86.
* Tiene instrucciones con 2 operandos, 1 operando, 0 operandos y saltos.
* Además posee diferentes modos de direccionamiento.

### Ejercicio
* Tenemos almacenado **vector de enteros de 16 bits.**
* La dirección del vector se identifica por la etiquetta **VECTOR**.
* Queremos calcular la cantidad de ceros que hay almacenados en el vector.
* El tamaño del vector se encuentra en la posición de memoria dado por la etiqueta **SIZE**.

 