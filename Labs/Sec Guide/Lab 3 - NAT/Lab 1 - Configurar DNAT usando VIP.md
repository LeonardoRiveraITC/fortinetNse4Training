1. Reestablecer configuración inicial en ambos dispositivos
2. Crear una VIP con las siguientes especificaciónes
	1. interfaz: puerto 1 - (10.200.1.1)
	2. Nombre: VIP-internal-host
	3. ip externa: 10.200.1.200
	4. ip interna: 10.0.1.10
	
==Nota: Esta configuración funciona así ya que  la ip externa se encuentra en la subred de la ip interna y se esta en un entrono de redes locales. Esta configuraciones NO se debe hacer para internet, ya que generalmente la ip asignada por el ISP es unica, y lo más seguro es que las otras ips bajo la subred pertenezcan a alguien más. A menos que se tenga contratada toda la subred==

![[Pasted image 20240627113759.png]]

3. Crear politica de firewall con lo siguiente
	1. Incoming interface: port 1
	2. Outgoing interface: port 3
	3. Source : any
	4. Dest: VIP-INTERNAL-HOST
==Al seleccionar el VIP, fortigate se encargara de hacer el mapeo, por su parte deshabilitamos el nateo por obvias razones y habilitamos logeo a todas las sesiones para probar la politica==

![[Pasted image 20240627115147.png]]


Y usando el comando 
```
get system session list
```
Podemos ver la sesión
![[Pasted image 20240627115705.png]]

#### Al hacer S[[NAT]] con una VIP Fortigate utiliza la VIP mapeada en lugar de la de la ip de la interfaz

Para ver esto en accion
1. Ejecutamos 
```
diagnose sys session clear
```
2. Volvemos a iniciar sesion a local fortigate
3. Iniciamos sesiones con páginas web desde vm-local
4. Comprobamos que, a la hora de natear el tráfico, la [[VIP]] (que es estática), toma precedencia sobre la ip de la interfaz 
5. ![[Pasted image 20240627120634.png]]