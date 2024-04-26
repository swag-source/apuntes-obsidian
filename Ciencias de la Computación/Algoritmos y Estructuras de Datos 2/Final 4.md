***
1) [AED] ¿Si probé la tripla de Hoare [P, C, Q], es cierto que [P', C, Q'] vale para P', Q' tales que P implica P' y Q implica Q'? Justificar

1) [AyED2] ¿Qué parámetros del esquema algorítmico de "divide and conquer" son importantes para calcular su complejidad cuando es aplicado a un problema?

2) ¿Para qué sirve el concepto de tipo abstracto de datos a la hora de presentar y usar una estructura de datos que lo implemente?

3) ¿Qué usos le dimos en la materia al invariante de representación de una estructura de datos?

4) ¿Qué propiedad se le podría pedir al mecanismo de gestión de colisiones elegido en el hashing abierto para garantizar que no se rechace un elemento si todavía hay lugar en el arreglo?

5) ¿Para qué usamos una versión levemente modificada de heap en el contexto de ordenamiento en memoria secundaria?

¿Si probé la tripla de Hoare [P, S, Q], es cierto que [P', S, Q'] vale para P', Q' tales que P implica P' y Q implica Q'? Justificar

Falso. Al momento de demostrar la validez de la Tripla de Hoare {P} S {Q}, nosotros probamos que P => wp(S,Q); en términos lógicos, estamos probando que P está contenido dentro del conjunto wp(S,Q). 
Ahora, como queremos ver si se cumple {P} S {Q} también se cumple {P'} S {Q'} tal que P => P' y Q => Q', observamos que lógicamente decimos que P se encuentra contenido dentro de P' y Q se encuentra contenido dentro de Q' (por la hipótesis del ejercicio).
Esto hace muy fácil observar que si bien P=>P' y Q=>Q', pueden haber elementos que pertenezcan a P' que no pertenezcan a P y elementos de Q que no estén en Q'; por esta razón es que existirá algún elemento de P' que no satisfaga la tripla de hoare {P} S {Q}



  
