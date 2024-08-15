1. Levantar la ruta default que se tiro en [[Lab 1 - Configuring route failover]]]], cambiando la ip del beacon por una ip valida
2. Cambiar la distancia administrativa del failover a la misma distancia y prioridad
	1. Y ahora podemos ver ambas rutas
	![[Pasted image 20240716184339.png]]
3. Cambiar el algoritmo ECMP
	```
	config system settings
		set v4-ecmp-mode source-dest-ip-based
	end
	```
	![[Pasted image 20240716184615.png]]

	Y ahora podemos ver las rutas en la tabla RIB
	![[Pasted image 20240718164122.png]]

4. Analizar el tráfico con un sniffer
	```
	diagnose debug enable
	diagnose sniffer packet any "not host 172.16.100.3 and tcp [13] &2==2       and port 80"
	```
	Podemos ver que se usan los puertos 2 y 1 lo que indica un balanceo
	![[Pasted image 20240718164704.png]]
5. Crear politica de routeo
	![[Pasted image 20240718165325.png]]
	![[Pasted image 20240718165340.png]]
Podemos ver el policy route en Dasboard > network > routing > policy
![[Pasted image 20240718165714.png]]

