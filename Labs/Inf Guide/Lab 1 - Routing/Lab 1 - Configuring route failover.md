1. Habilitar la vista de distancia y prioridad
	1. Network > Static routes
	2. Click derecho en la tabla y habilitar seccion
	 ![[Pasted image 20240716121704.png]]
2. Habilitar una segunda ruta default standby con distancia de 20 y prioridad de 5 por el puerto 2
	![[Pasted image 20240716124823.png]]
3. Habilitar logueo de todo el tráfico en la politica de firewall default y crear una segunda politica para permitir tráfico del puerto 2 al puerto 3 (nueva ruta)
	![[Pasted image 20240716165506.png]]

	![[Pasted image 20240716165653.png]]


4. Analizar la tabla de routeo
	1. Imprimir la tabla de routeo RIB
	```
	get router info routing-table all
	```
	Podemos ver que solo esta la ruta default inicial
	![[Pasted image 20240716170030.png]]
	
	2. Imprimir la tabla de routeo FIB (database)
	```
	get router info routing-table database
	```
	![[Pasted image 20240716170127.png]]
	Aqui podemos ver ambas rutas, con su distancia de cada una (10 y 20) y su prioridad (1 y 5)


5. Configurar monitores de salud
	1. Puerto 1
	```
	config system link-monitor
		edit port1-monitor
			set srcintf port1
			set server 4.2.2.1
			set gateway-ip 10.200.1.254
			set protocol ping
			set update-static-route enable
		next
	end
	```
	2. Puerto 2
	```
	config system link-monitor
		edit port2-monitor
			set srcintf port2
			set server 4.2.2.2
			set gateway-ip 10.200.2.254
			set protocol ping
			set update-static-route enable
		next
	end
	```

6. Verificar que funciona el failover
	1. Hacer que el monitor de port 1 pingee a un servidor invalido
		![[Pasted image 20240716171840.png]]

	2. Podemos ver que el failback toma efecto
	![[Pasted image 20240716172041.png]]
