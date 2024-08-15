==Global > System > VDOM==
==Por default todas las interfaces en un vdom pertenencen a un mismo broadcast, se puede cambiar al cambiar el dominio en cli==
### *Los dominios virtuales segmentan el firewall esencialmente en dominios distintos, esto quiere decir que para cada dominio se tendran politicas de firewall separadas, tablas de routeo, y etc *

- Por default el tráfico no es intercambiable entre vdoms
- Por default fortigate soporta hasta 10 vdoms


![[Pasted image 20240618110948.png]]

### Habilitar VDOM

```
config system global
	set vdom-mode [no-vdom | multi-vdom]
end
```

### Bindear a interfaces
==Cada interfaz puede pertenecer a un solo vdom==
![[Pasted image 20240707221522.png]]


### Multi vdom mode

#### Tipos de VDOM
##### Administrativo
- ssh
- https
- No pasa trafico
==por default el vdom de administracion es root==
==Todo el trafico originado de fortigate pasa por el managment vdom==
==requiere acceso a servicios globales==
- NTP
- Fortiguard services
- SNMP
- DNS filter
- LOGS

#### Trafico
- Procesa trafico de red


### Indepentendt vdoms
- No hay comunicación entre vdoms
- Cada vdom tiene su propia interfaz fisica

### Meshed VDOM
Solo se necesita un VDOM con salida a internet
Los VDOMS estan interconectados
![[Pasted image 20240707215022.png]]
Es más dificil de configurar por las politicas de firewall y rutas necesarias pero provee de flexibilidad 


En esta topologia existen vdom links pero solo hacia un solo vdom, lo que quiere decir que todo el routeo, al igual que el acceso a internet ocurren en to_internet
![[Pasted image 20240707215423.png]]


### Administración
==Solo [[Administración]] y super[[Administración]] tienen acceso a todos los vdoms==

Se puede asignar acceso de administrador per vdom

##### Asignar vdoms a perfil
![[Pasted image 20240707215628.png]]


### Configuraciones globales y VDOM
- global
	- Hostname
	- HA settings
	- Fortiguard settings
	- NTP
	- Admin accs
	- Perfiles de seguridad globales
		- deben de iniciar el nombre con g- para que se reconozcan
		- Son de solo lectura para admins de vdom
		- [[AV]]
		- [[IPS]]
		- [[Web filter]]
		- [[Application control]]
- VDOM
	- Modo de operacion (Transparente/NAT)
	- Modo [[NGFW Mode]]
	- Rutas e interfaces de red
	- Firewall policies
	- Security policies


### Links
- Se puede hacer
	- Nat a nat
	- Transparent a transparten

T a T no se puede debido a loops potenciales
![[Pasted image 20240707222442.png]]

### Recomendaciones
- Limitar recursos, debido a que un solo vdom puede comerse todo el sistema
![[Pasted image 20240707222650.png]]


##### Aceleracion
npu0_vlink0
npu0_vlink1

npu1_vlink0
npu_vlink1




### Comandos
==Habilitar vdoms==
```
config system global
set vdom-mode [no-vdom|multi-vdom]
end
```

Crear vdom

```
config vdom
edit <vdom>
config system settings
	set vdom-type <traffic/admin>
end
```

Cambiar cominio de broadcast 
Todos las interfaces con el mismo id de dominio comparte un broadcast
```
config system interface
	edit <interface>
		set forward-domain <id>
	end
```

Bindear vdom a interfaz
```
config global
	config system interface
		edit <if>
			set vdom <vdom-name>
		end
```

acceder a global config
```
config global
```

acceder vdom config 
```
config vdom
edit <vdom>
```