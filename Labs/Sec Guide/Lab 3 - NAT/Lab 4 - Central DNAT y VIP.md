1. Crear una VIP con la siguientes caracteristicas:
	1. name:central-DNAT
	2. interface:port 1
	3. type: static nat
	4. External ip:10.200.1.50
	5. Mapped ip: 10.0.1.10
	![[Pasted image 20240627141203.png]]

2. Si intentamos crear una politica de firewall, veremos que no podemos usar el VIP como destino, esto ya que al usar central NAT se crea una regla desde kernel para hacer el nateo
3. Al usar la politica de outbound creada, podemos ver que efectivamente se natea a través del [[Central NAT]]
![[Pasted image 20240627143949.png]]

Lista de sesiones
![[Pasted image 20240627144837.png]]

==Si estan habilitados ambos, DNAY y SNAT, fortigate usa politicas SNAT para outgoing traffic==