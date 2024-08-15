1. Crearemos un nuevo usuario, este servira para entrar a la VPN en web mode
	![[Pasted image 20240728175542.png]]

2. Configurar la VPN SSL
	1. En SSL VPN settings configurar lo siguiente a la vpn (esta vpn será de forti como servidor con clientes web)

		IF : Port1
		Puerto: 10443
		Allow any
		Server certificate: Fortigate default
		Idle logout: 3000 seconds

		En tunnel activaremos automáticamente asignar ip
	![[Pasted image 20240728180139.png]]
	![[Pasted image 20240728180151.png]]

2. Cambiar el portal para all other users/groups ![[Pasted image 20240728180302.png]]
	Si vemos el portal configurado, podemos ver que como dice el nombre se trata de un vpn de web access
	![[Pasted image 20240728180557.png]]

3. Finalmente, debemos configurar politicas de firewall que permitan el tráfico entre la vpn 
	![[Pasted image 20240728181034.png]]
	Podemos notar que el incoming interface es una interfaz virtual llamada SSL-VPN tunnel access ssl.root, esto debido a que es la interfaz de la vpn, que esta atada a el puerto1, y la salida sera por puerto 3 hacia el local subnet

Tambien notamos que en source además de las direcciones necesitamos un usuario o grupo de usuarios

Finalmente habilitamos inspeccion por flow mode con el siguiente comando
![[Pasted image 20240728181321.png]]


	