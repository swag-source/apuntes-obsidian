***

### Checkpoint 1

a) La convención de llamada se entiende como la forma de enviar y recibir información usando funciones de usuario. En el caso de la ABI (Interfaz Binaria de Aplicación) de System V para 64 y 32 bits, tenemos dos convenciones de llamada particulares:
=> **Para 64 bits:** Utiliza los registros de propósito general y la pila.
**=> Para 32 bits:** Solo se utiliza la pila.

b) El encargado de que se cumpla la convención de llamada en C es el compilador. En Assembler es el programador el encargado de cumplir la convención.

c) El **Stack Frame** es la región delimitada por el puntero EBP (Base Stack Pointer) y el ESP (Register Stack Pointer) en la arquitectura de 32 bits y en 64 bits los registros RBP y RSP con las mismas funcionalidades respectivamente. La misma se utiliza para organizar y gestionar el flujo de ejecución y almacenamiento de datos durante la ejecución de una función del programa.
Por otro lado, el **prólogo** es donde se reserva espacio en la pila para datos temporales, agregado de padding y mantenerla alineada a 16 bytes; el **epílogo** es donde restauramos los valores de registros no volatiles y devolvemos la pila a su estado inicial.

d) Las variables temporales se guardan en la pila en posiciones relativas al RBP para poder acceder a las mismas.

e) 