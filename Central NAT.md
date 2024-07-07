La [[Visibilidad]] se debe habilitar en ==System>Settings>central snat==
![[Pasted image 20240625223422.png]]
==Si estan habilitados ambos, DNAY y SNAT, fortigate usa politicas SNAT para outgoing traffic==

==En caso de tener VIP o ip pools previas se deben de eliminar==

O con el comando

```
config system settings
	set central-nat enable
end
```

Mandatorio para activar el modo [[NGFW Mode]] policy based 

Visto en [[Lab 3 - Configurando central SNAT]]