[[Central NAT]]
1. Retornar a la configuración inicial
==Esto debido a que para activar [[Central NAT]] primero debemos eliminar toda referencia de VIP y IP pools==

2. Activar la [[Visibilidad]] en GUI o en CLI con
```
config system settings
set central-nat enable
end
```
3. Volver a crear el ip pool del [[Lab 2 - DNAT con IP Pools]] 
4. Una vez habilitado, aparecera la sección Central NAT bajo Firewall & objects
5. Creamos una nueva politica con la sig configuración
	1. Incoming interface: port 1
	2. Outgoing interface: port 3
	3. Source: All
	4. Dest: All
	![[Pasted image 20240627134030.png]]

6. Al ir a politicas de firewall podemos ver un aviso sobre el uso de Central NAT
 ![[Pasted image 20240627135617.png]]
 7. Viendo la lista de sesiones podemos ver que funciona
 ![[Pasted image 20240627135737.png]]
