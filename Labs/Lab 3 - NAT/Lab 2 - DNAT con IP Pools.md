Las IP pools son [[objetos]] para usar al momento de aplicar nateo. Estas nos sirven para tomar IPs desde una pool para aplicar NATeo, existen multiples modos de operacion, con el default siendo overload, que carga tantas sesiones como pueda a una sola ip antes de pasar a la siguiente, uno a uno, o asignación de puertos

1. Crear una IP pool con las siguientes caracteristicas
	1. name:INTERNAL-HOST-EXT-IP
	2. type: overload
	3. range: 10.200.1.100-10.200.1.100

![[Pasted image 20240627121651.png]]

4. Editar la politica full-access para usar la IP pool
![[Pasted image 20240627121805.png]]**

5. Navegamos a muiltiples sitios para ver el comportamiento del tráfico

==Podemos establecer filtros de session para no eliminar otras sesiones a la hora de limpiarlas==
```
diagnose sys session filter clear
diagnose sys session fliter src <ip>
diagnose sys session clear
```

6. Podemos ver el SNAT que se hace de la siguiente manera

 ![[Pasted image 20240627123224.png]]
Tomando así la siguiente presedencia
 Ip pool> VIP > NAT interface





