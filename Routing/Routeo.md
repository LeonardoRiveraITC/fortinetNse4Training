### Funcionamiento

![[Routing-drawing]]

### Tabla de routeo
Una tabla que contiene las rutas a tomar para llegar a una red. Existen varios tipos de rutas pero todas son lo mismo, un salto para llegar a una red, estos saltos se dan entre routers hasta llegar al routeador final. A un router no le importa quien tiene el destino final, siempre y cuando sepa quien conoce el siguiente salto, para dejarle a el el problema

Tipos de ruta
- L - local 
	- Mis propias IPS
- C - connected 
	- IPS conectadas
- S - Static
	- Rutas estaticas (configuradas)

De igual manera, no es necesario descubrir manualmente rutas, para esto existen protocolos que automatizan el proceso con distintas caracteristicas cada uno

- R - [[RIP - Routing interface protocol]] (Routing interface protocol)
- BGP - [[BGP - Border Gateway Protocol]] 
- OSPF - [[ OSPF - Open Shortest Path First]]

### Estructura de una tabla de routeo
- Red
- Gateway IP
- Interfaces
- Distancia
- Metrica
- Prioridad
![[Pasted image 20240707192443.png]]