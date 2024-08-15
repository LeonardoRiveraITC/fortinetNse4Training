La topologia a crear en este laboratorio es la siguiente
![[Pasted image 20240719122403.png]]
1. Restablecer Fortigate a configuración inicial
2. Ahora podemos notar la presencia del menu de vdoms, tanto en la esquina superior derecha indicando el vdom sobre el que estamos trabajando, y en el menu principal, crearemos uno nuevo en System>VDOM>Create
	1. ![[Pasted image 20240719123521.png]]
	Una vez creado podremos notar la adición del vdom al menu

3. A continuación crearemos  un administrador solo para ese vdom
	![[Pasted image 20240719123756.png]]

4. Bindear interfaz 3 a vdom
	Recordemos que un vdom esta atado a una interfaz, un vdom puede tener varias 
	interfaces, pero una interfaz solo puede estar bindeada a un solo vdom
	
	1. ![[Pasted image 20240719124518.png]]
5. Habilitar DNS en customer vdom
	En esta configuración, la interfaz servira como servidor DNS usando la base de datos de dns 
	
	1. Habilitar DNS en feature visibility dentro del customer vdom
	2. Crear DNS service en network > DNS server
	![[Pasted image 20240719130146.png]]

6. Probar el usuario customer-admin

==Al usar CLI debemos especificar primero el VDOM a configurar, de otra manera los comandos no serviran==
```
config vdom
	edit <vdom>
```
De igual manera debemos tener cuidado con la opcion edit ya que los vdoms son case sensitive por lo que podriamos crear uno nuevo sin querer

7. Comparar las tablas de routeo de los vdoms
	Recordemos que los vdoms tienen su propio todo, a excepción de cosas que deben de ser globales, como NTP, acceso a fortiguard, [[Vdom]]

```
config vdom
	edit customer
		get router info routing-table all
	next

	edit root
		get router info routing-table all
	next
```
![[Pasted image 20240719132255.png|500]]

