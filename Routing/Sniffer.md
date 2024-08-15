==network > Diagnostics > Packet caputre==
Un sniffer nos sirve para debugear el trafico de red

Usa la sintaxis BPF Berkley packet filter, conocida por tcpdump

```
diagnose sniffer packet <interface> <filter> <verbosity> <count> <timestamp>
<framesize>
```

- Interface
	- any o puerto fisico o logico
	- filter - BPF syntax
	- verbosity 
![[Pasted image 20240707212918.png]]


- count-conteo de paquetes
- timestamp 
	- 1 tiempo local
	- a timestamp
- framesize
	- Tamaño maximo de frame siendo 65k el maximo
	- 