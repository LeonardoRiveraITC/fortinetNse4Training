1. Reiniciar a valores de fabrica
2. Se requiere una licencia activa de sevicios de fortiguard, con la cual no cuenta el laboratorio pero se hara como si se tuviera
	1. ![[Pasted image 20240706131052.png]]
3. Habilitar el [[Proxy based inspection]] mode 
	Para habilitarlo, por default no podemos verlo en gui por lo que ejecutaremos lo siguiente
	```
	config firewall policy
		edit 1
			set inspection-mode prxoy
		end
	```
	Y se vera habilitado
	![[Pasted image 20240706133832.png]]
		2. Ademas asignar el perfil de web filter default a la politica
	
3. Ahora buscaremos categorias en fortiguard.com/webfilter
	![[Pasted image 20240706134643.png]]
	1. En el perfil default bloqueamos social networking
		![[Pasted image 20240706134830.png]]
4. Comprobar el funcionamiento
	1. Verificar categorias
	```
	get webfilter status
	```
	Podemos ver el fortimanager como uno de los servidores de webfiltering
		![[Pasted image 20240706140138.png]]
5. Creaundo un override
	1. Los overrides nos ayudan a cambiar la categoria de un sitio para así no permitir la categoria entera
	2. ![[Pasted image 20240706140513.png]]