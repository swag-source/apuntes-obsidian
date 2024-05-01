***
### Registros y tipos de datos
***
-> Registros
* XMM0 a XMM15 de 16 bytes (128 bits)

-> Tipos de datos
* Enteros de 8, 16, 32, 64, 128 bits.
* Puntos flotantes 32 (float) 64 (Double)
### Operaciones Load/Store
***

![[Pasted image 20240429113435.png]]

-> Las instruccionnes con **A** significa que mueven registros alineados. **U** mueve registros desalineados.
* Las **U** son más ineficientes ya que son instrucciones en posiciones desalineadas.
* El procesador tiene que elegir entre mover enteros a registros de punto flotante o de enteros (dependiendo la instrucción).
	* Para guardar enteros, usamos **MOVDQA/MOVDQU**
	* Punto flotante **MOVAPS/D** y **MOVUPS/D**
-> **S** significa *single*, **D** significa *double*, **P** significa packed.

*Ejemplos*
![[Pasted image 20240429113920.png]]

-> También tenemos instrucciones encargadas de *extender* la precisión de un número (por ejemplo, pasar un número de tipo *single* a *double*, *double* a *quad*). Podemos levantar un número de un registro y extenderlo en el mismo.

![[Pasted image 20240429114351.png]]

*Ejemplo*: PMOVSXBD XMM0, XMM0 (extiende un número de precisión simple a precisión double)
![[Pasted image 20240429114845.png|500]]

### Operaciones aritméticas
***
Tenemos instrucciones para operar entre números de distintos tipos.

![[Pasted image 20240429115007.png|500]]

*Ejemplo:* Podemos querer multiplicar y guardar en un registro particular solamente la parte baja de un registro. Utilizamos la instrucción PMULLD (multiply low double).

*PMULLD xmm0, xmm1*
![[Pasted image 20240429115256.png|500]]

### Ejemplos prácticos
***
>[!Ejercicio]
> Dado un vector de $n$ enteros sin signo de 16 bits. Incrementar en 1 unidad cada uno y almacenar el resultado en un vector de 16 bits. Considerar que $n \equiv 0  \ (mod \ 8)$
> *void suma1(uint16_t \*vector, uint16_t \*resultado, uint8_t n)

```
section .data
uno: times 8 dw 1 ; no sabemos si uno es una dirección alineada a 8 bytes.

section .text

suma1: ; tenemos cargado rdi = vector, rsi = resultado, rdx = n
	; prologo
	push rbp
	mov rbp, rsp
	movzx rcx, rdx ; extiende con 0s el registro rcx
	shr rcx, 3 ; (shifteo el registro en 8 bytes, es decir, divido por 2³)
	movdqu xmm8, [uno] ; xmm8 = | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 |

	.ciclo:
	movdqu xmm0, [rdi] 
	; xmm0 = | d7 | d6 | d5 | d4 | d3 | d2 | d1 | d0 | usamos movdqu ya que no sabemos si el vector está alineado.
	
	paddw xmm0, xmm8  
	; xmm0 = | d7+1 | d6+1 | d5+1 | d4+1 | d3+1 | d2+1 | d1+1 | d0+1 |

	movdqu [rsi], xmm0
	add rdi, 16
	add rsi, 16

loop .ciclo
pop rbp
ret
```

>[!Sumar Posiciones]
> Dado un vector de $n$ enteros con signo de 16 bits. Incrementar en 2 unidad cada uno y almacenar el resultado en un vector de 32 bits. Considerar que $n \equiv 0  \ (mod \ 8)$
> 
> *void suma2(uint16_t \*vector, uint32_t \*resultado, uint8_t n)

```
section .data
dos: times 4 dd 2

section .text

suma2: ; rdi = vector, rsi = resultado, rdx = n
	push rbp
	mov rbp, rsp
	movzx rcx, rdx
	shr
	movdqu xmm8, [dos] ; | 2 | 2 | 2 | 2 |

	.ciclo:
		pmovsxwd xmm0, [rdi] ; xmm0 = | d3 | d2 | d1 | d0 |
		padd xmm0, xmm8 ; xmm0 = | d3+2 | d2+2 | d1+2 | d0+2 |
		add rdi, 8 ; nos movemos 4 words
		add rsi, 16
loop .ciclo
pop rbp
ret
```

>[!Incrementar Brillo]
> Dado una imagen de $32 \times 32$ pixeles de un byte en escala de grises, incrementar cada pixel en 10 unidades. 
> void incrementarBrillo(uint8_t \*imagen)

-> *Recordemos:* Cuando queremos incrementar el brillo, tenemos que aumentar en 10 unidades cada pixel pero no queremos "pasarnos" del brillo posible para ese pixel (ya que sino devolveríamos más brillo y se iría a negro).

```
section .data
diez: times 16 db 10

section .text

incrementarBrillo10: ; rdi = imagen -> al vector lo pensamos como una tira continua en vez de arreglo de arreglo. -> [n1, ... , n(32x32)]
	push rbp
	mov rbp, rsp
	
	mov rcx, (32*32 >> 4)

	movdqu xmm8, [diez] ; xmm8 = | 10 | ... | 10 |

	.ciclo: 
		movdqu xmm0, [rdi] ; xmm0 = | d15 | ... | d0 |
		paddusb xmm0, xmm8 ; xmm0 = | d15 + 10 | ... | d0 + 10 |
		movqdu [rdi], xmm0 ; rdi =  | d15 + 10 | ... | d0 + 10 |
		
		add rdi, 16

loop .ciclo
pop rbp
ret
```

### Operaciones aritméticas horizontales
***
![[Pasted image 20240429130343.png]]
![[Pasted image 20240429130723.png]]

### Operaciones lógicas
***
![[Pasted image 20240429131056.png]]
-> Son utilizadas en general para crear máscaras las operaciones lógicas entre registros. 
-> Actúan lógicamente sobre todos los registros sin importar el tamaño de los mismos.
-> Las operaciones **PD** y **PS** son exactamente la misma instrucción solo que contiene mayor *meta* información para el procesador. 

**Técnica: Calculo de máscara**.

-> Enmascarar información es útil para esconder información o "pintarla" con el fin de que no sea de entendimiento directo. Utilizamos operaciones lógicas para calcular máscaras y aplicarlas a registros.
* Calculamos la máscara (vemos formas luego)
* Queremos aplicar la máscara a dos registros *src* y *dst*
* Not(Mascara) se lo aplicamos a *src*, mascara a *dst*.
* Combinamos los resultados.

![[Pasted image 20240429132938.png|500]]

>[!Ejercicio]
> Dado un vector de 128 enteros con signo de 16 bits. Sumar todos los valores pares y retornar el resultado de la suma en 32 bits.
> 
> int32_t sumarPares(int16_t \*v)

```
sumarpares: ; rdi = int16_t *v
	push rbp
	mov rbp,rsp
	mov rcx, (128 >> 2) ; rcx = 128 / 4
	pxor xmm8, xmm8 ; xmm8 = | 0 | 0 | 0 | 0 |
	
.ciclo: 
	pmovsxwd xmm0, [rdi] ; xmm0 = | 00001233 | 00007314 | 00003011 | FFFF9311 |
	; convierto los números en valor absoluto con "pabs"
	pabs xmm1, xmm0 ; xmm1 = | 00001233 | 00007314 | 00003011 | 00006CEE |
	
	; shifteo a la derecha el byte en las posiciones impares
	pslld xmm1, 31 ; xmm1 = | 80000000 | 00000000 | 80000000 | 00000000 |
	
	; aplico una máscara sobre xmm1 para "exponer" los valores que no queremos
	psrad xmm1, 31 ; xmm1 = | FFFFFFFF | 00000000 | FFFFFFFF | 00000000 |
	
	pandn xmm0, xmm1 = | 00000000 | 00007314 | 00000000 | 00000000 |

	add rdi, 8

loop .ciclo
phaddd xmm8, xmm8 ; xmm8 = | ... | ... | SUM3+SUM2 | SUM1+SUM0 |
phaddd xmm8, xmm8 ; xmm8 = | ... | ... | ...| SUM3+SUM2+SUM1+SUM0 |

mov eax, xmm8 ; eax = suma(xmm8) -> no rax porque tiene que ser de 32 bits.
pop rbp
ret
```
### Operaciones de desempaquetado (unpacked)
***
-> Son una serie de operaciones que combinan partes **altas** de ciertos registros con parte **baja** de otro registro.
-> El registro de *src* siempre quedará en la parte **alta** del registro *dst* y el registro *dst* quedará en la parte **baja** del registro *dst*.
-> Por ejemplo, **IMPORTANTE**. Si tenemos que realizar una multiplicación y tenemos dos registros para parte alta y parte baja, entonces el *src* estará en la parte alta y *dst* en baja.

![[Pasted image 20240429145058.png]]
![[Pasted image 20240429145017.png]]
![[Pasted image 20240429145024.png]]

**Técnica: Operatoria de desempaquetado y empaquetado**

-> Esta filosofía consiste en agarrar un dato de un tamaño $n$. A partir de una operación de *extensión*, aumentar el tamaño del mismo para realizar *operaciones vérticales* eficientes. Una vez que tenemos el resultado en un registro "más grande", utilizar instrucciones de **saturación/empaquetado** para reducir el tamaño del dato.

![[Pasted image 20240429150157.png]]
>[!Ejercicio]
> Dado dos vectores de 128 enteros con signo de 16 bits. Multiplicar cada uno de ellos entre si y almacenar el resultado en un vector de enteros de 32 bits.
> 
> void mulvec(int16_t \*v1, int16_t *v2, int32_t \*resultado)

```
mulvec: ; rdi = *v1, rsi = *v2, rdx = *resultado
push rbp
mov rbp, rsp
mov rcx, (128 >> 2) ; rcx = 127 / 2² = 32
.ciclo:
	movdqa xmm0, [rdi] ; xmm0 = | a7 | a6 | a5 | a4 | a3 | a2 | a1 | a0 |
	movdqa xmm1, [rsi] ; xmm1 = | b7 | b6 | b5 | b4 | b3 | b2 | b1 | b0 |
	movdqa xmm2, xmm0  ; xmm2 = | a7 | a6 | a5 | a4 | a3 | a2 | a1 | a0 |
	
	pmulhw xmm2, xmm1  ; xmm2 = | hi(a7*b7)  ...  hi(a0*b0)  |
	pmullw xmm0, xmm1  ; xmm0 = | low(a7*b7) ...  low(a0*b0) |
	
	; siempre la parte baja será el registro de destino.
	movdqa xmm1, xmm1  ; xmm1 = | low(a7*b7) ...  low(a0*b0) |

	punpcklwd xmm0, xmm2 ; xmm0 = | hi:low(a3*b3) ... hi:low(a0*b0) |
	punpcklwd xmm1, xmm2 ; xmm1 = | hi:low(a7*b7) ... hi:low(a4*b4) |

	movdqa [rdx], xmm0
	movdqa [rdx + 16], xmm1
	add rdx, 32
	add rdi, 16
	add rsi

loop .ciclo
pop rbp
ret
```

***
# SIMD (parte 2)

### Shuffles
***
-> Las instrucciones de *Shuffle* permiten reordenar datos en un registro. Se considera un registro de destino (a reordenar) y una máscara la cual se encarga de realizar el shuffle.

![[Pasted image 20240429154858.png|500]]

```
PSHUFB (con operandos de 128 bit)

for i = 0 to 15:
	if(SRC[i*8 + 7] = 1) then  // el 0 <= SRC <= 127 
		DEST[(i*8)+7 ... (i*8)+0] <- 0
	else:
		index[3...0] <- SRC[(i*8)+3 ... (i*8)+0];
		DEST[(i*8)+7 ... (i*8)+0] <- DEST[(i*8+7) ...(i*8+0)];
	endif
```
![[Pasted image 20240429172819.png]]
* Me paro en una posición de *src*, miro el bit más significativo
	* Si es 1 => *dst* = 0
	* Si es 0 => busco el registro que me indican los 4 primeros bits (menos significativos) y lo pongo en esa posición. (1011 = 11).
* Es una instrucción costosa y requiere una máscara para operar. Es un shuffle **DE ENTEROS** no de **PUNTO FLOTANTE**.
* Consejo de Agus: Nunca mezclar operaciones de enteros con punto flotante y viceversa.


### Insert/Extract
***
-> Las instrucciones de *Insert* y *Extract*, permiten como su nombre indica, **insertar** y **extraer** valores dentro de un registro.

![[Pasted image 20240430110924.png|500]]
**INSERTPS:**
![[Pasted image 20240430111311.png|500]]

### Blend
***

### Conversiones
***

-> Son instrucciones de conversiones con la pinta: **CVTxx2yy** donde xx = tipo de dato 1, yy = tipo de dato 2.

-> xx e yy pueden valer:
* ps - Packed Single FP
* ss - Scalar Single FP
* pd - Packed Double FP
* sd - Scalar Double FP
* pi - Packed Integer
* si - Scalar Integer
* dq - Packed Dword

**Instrucciones de punto flotante**
* CVTSD2SS - Scalar Double FP to Scalar Single FP => CVTSD2SS xmm, registro32
* CVTSS2SD - Scalar Single FP to Scalar Double FP => CVTSS2SD registro32bits, xmm

* CVTSI2SD - Dword Integer to Scalar Double FP => CVTSI2SD xmm, registro64bits.
* CVTSD2SI - Scalar Double FP to Dword Integer => CVTSD2SI registro64, xmm

*  CVTDQ2PS - Packed Dword Integers to Packed Single FP (4X) => CVTDQ2PS xmm1, xmm2/m128
- CVTPS2DQ - Packed Single FP to Packed Dword Integers (4X) => CVTPS2DQ xmm1, xmm2/m128

- CVTDQ2PD - Packed Dword Integers to Packed Double FP (2X) => CVTDQ2PD xmm1, xmm2/m64
- CVTPD2DQ - Packed Double FP to Packed Dword Integers (2X) => CVTPD2DQ xmm1, xmm2/m128

**Instrucciones de truncado**
* CVTTSS2SI - Truncation Scalar Single FP to Dword Integer
* CVTTSD2SI - Truncation Scalar Double FP to Signed Integer
* CVTPS2DQ - Truncation Packed Single FP to Packed Dword Int.