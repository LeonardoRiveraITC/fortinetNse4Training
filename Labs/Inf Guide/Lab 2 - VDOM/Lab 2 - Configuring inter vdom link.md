1. Crear una nueva interfaz tipo vdom link
	1. Ir a configuración global > network > interfaces > new
	![[Pasted image 20240719133811.png | 700]]
	Para esto, tenemos que configurar los dos vdoms a conectar, y agregar una interfaz lógica de cada lado (vlink0 y vlink1). Estas interfaces saldrán del vdom y conectaran al otro vdom, por lo que deberan ser capaces de verse entre si
	![[Pasted image 20240719134138.png]]
	
2. Crear una ruta estatica en customer
	 Esta primea ruta estatica servira para que todo el trafico saliente a internet pase por vdom root, esto como default gateway
	 ![[Pasted image 20240720135953.png]]
3. Creamos una ruta estatica en root
	Esta ruta estatica servira para mapear todo el trafico dirigido al vdom customer

4. Habilitar politicas de firewall entre vdoms
	1. Customer a root
	![[Pasted image 20240720142104.png]]
	 2. root a customer
	![[Pasted image 20240720141633.png]]
	3.Y probamos que efectivamente funciona
	![[Pasted image 20240720142303.png]]
	 