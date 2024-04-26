***


![[Pasted image 20240410113224.png]]

```
esElMismo(lista: int[], i: int) : B {
	if(|lista| == 1){
		if(lista[1] == 1)
			return True;
		else
			return False;
	} else
	m = |lista| // 2 -> División Entera redondea para abajo
	if(lista[m] > m + i){
		return esElMismo(lista[m+1 :], m + i);
	} else if(lista[m] < m + i){
		return esElMismo(lista[: m], i);
	} else {
		return True;
	}
}
```


![[Pasted image 20240410131004.png]]

```
expoRapida(a : int, b: int) { 
	if b == 1{
		return a;
	} else if (b == 0) {
		return 1;
	} else {
		if(b % 2 == 0) {
			return expoRapida(a, b/2) * expoRapida(a, b/2)
		} else {
			tmp = expoRapida(a, b/2) * expoRapida(a, b/2)
			return a * tmp
		}
	}
}	
```

