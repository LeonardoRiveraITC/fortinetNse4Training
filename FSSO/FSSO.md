==Security fabric > FSSO/Poll from ad server==
==Los agentes usados en este modo se pueden descargar en support.fortinet.com==
==Para que fortigate logee eventos de login, se requiere configurar [[Logs/Logs|Logs]] de eventos de al menos nivel 6 o informacional==


### Fortinet single sign on

Es un modo de autenticación ==pasivo==
SSO es un proceso que permite iniciar sesión una sola vez, single sign on

- Se puede aplicar a usuarios, grupos, etc

==Comunmente se implementa con AD o novell==

==FSSO proporciona a fortigate datos tales como ip, usuario y hostname para almacenar en una base de datos==

- El dns local debe ser capaz de resolver los hostnames para poder enviar esta información en chequeos de workstation debido a que la resolución de eventos de login se hace por medio de dns y no por ip
- Debe haber conexión con cada workstation para verificar eventos de login
- Debe estar abierto SMB (TCP 445) y su fallback TCP 135,139. UDP 137

### Agentes
Los agentes son usados para el DC mode, existen dos

#### DC agent
Un Domain controller agent al cuál se le instala un dcagent.dll en system32, este agente servira para escuchar eventos de login de grupos para despues ser enviados al collector agent usa SMB (TCP 445) y sus fallbacks TCP 135,139  UDP 1317

#### Collector agent
El collector agent se encarga de escuchar a multiples DC agents para preparar la información a enviar a fortigate, envia eventos por medio de TCP 8000 y recibe de fortigate por medio de UDP 8002 o DC agents por medio de SMB 

###### Group filters
El collector agent puede filtrar que eventos de que grupos enviar a fortigate, esto es especialmente útil para VDOMs, esto ya que escuchar el grupo entero puede ser ineficiente en la mayoria de los casos

###### Ignore list
Una lista de usuarios ignorados de la cual el collector agent no enviara eventos, esto es util para cuentas de soporte que puede que no inicien sesión desde el mismo dispositivo, y al hacerlo sobreescriban la base de datos, haciendo que haya problemas con identities y policy matching

###### Timers
- Workstation verify - Verifica el estado del usuario al revisar cada 5 minutos (configurable) el estado del workstation en el que estaba, de no encontrarlo el estado cambia a no verificado, usando SMB
- Dead entry timeout interval - El tiempo en el cual sera eliminado el entry para el workstation-user de la base de datos, para fortigate no hay diferencia para ok y not verified, ambos son validos
- Ip address change verify interval - El intervalo en el que se revisa el cambio de ips, esto es importante en un entorno DHCP para no bloquear al usuario en cuestin, DNS debe de ser capaz de resolver propiamente los hostnames
- Cache user group lookup result - El intervalo en el que se actualiza las membrecias del usuario

##### Access mode
Existen dos modos de acceso
- Standard mode
	- Usa la notación Dominio/Usuario de windows
	- No soporta nested groups
	- Se filtra desde el collector agent

- Advanced mode
	- Usa la notación LDAP
		- CN=user,OU=organizational unit,DC=domain

	- Soporta usuarios nesteados (lo que significa control más granular)
		- Por ejemplo aplicar UTM a solo un usuario bajo un grupo
		- O aplicar UTM a todos los usuarios bajo un grupo nesteado
	- Filtrado
		- Desde collector o desde fortigate como LDAP client


###### Soporte de grupos
- Grupos de seguridad
- Grupos universales
- Grupos dentro de OU
- Grupos locales o globales que contienen grupos universales de dominios hijo

==Si un usuario no es parte de un grupo y esta habilitado SOLO la autenticación pasiva por default se maneja como parte de SSO_guest_users, de otra manera se debera autenticar a un usuario debido a los registros usuario-host -> ip==


### Active directory
#### Modos
##### Domain controller agent mode
==Security Fabric > FSSO > Collector agent==


==Collector a fortigate default TCP 8000==
==Comunicar a collector default UDP 8002==
- Escalable
- El más recomendable
- Requiere de un dcagent.dll instalado en cada windows Domain Controller, en system32 
	- El domain controller agent es responsable de Monitorear eventos de login y enviarlos al collector agents

- Collector agent recibe de los domain controllers para enviar lo siguiente a fortigate
	- Verificación de grupos
	- Chequeo de workstations
	- Actualizar login records a fortigate
		- User name
		- Host name
		- Ip address
		- User groups
	- Enviar domain local security group y organizational units OU a fortigate
	- Manejar busquedas DNS
	![[Pasted image 20240714115541.png]]


##### Polling mode
==Se encarga de esta tarea el dameon fssod==
==Para redes simples==
==Debe estar habilitado event polling en los DC, menos para NETApi==
Cada cierto tiempo fortigate pollea la información de usuarios desde los DC
- SMB (TCP 445)
- TCP 135,139, UDP 137 fallback

El polling se hace por medio de 
- NetAPI
- WinSecLog
- WMI

###### Polling agent-based 
Este modo es similar al collector based agent, pero no se requiere tener agentes instalados en cada uno de los DC
==Funciona sin agentes, estrictamente hablando, pero las tecnologías usadas reemplazan el rol, por lo que la función es distinta==
Del más recomendado al menos recomendado

- WMI
	API de windows donde el collector agent es un cliente WMI, que envia queries a el DC que en consecuencia es un WMI server; preguntando por login events, 
	
	DC envia eventos de login cada 3 segundos
	
	![[Pasted image 20240714121521.png]]


- WinSecLog
	Pollea eventos de seguridad desde el DC cada 10 segundos
	Lento pero confiable
	Requiere que los ID event de success se almacene en eventos de seguridad

- NETAPI
	Pollea eventos cada que un usuario inicia o cierra un sesión con la función NetSessionEnum de windows
	Pueden perderse eventos si la carga es demasiada

###### Polling agentless
==Security Fabric > External connectors> Poll active directory server==
==SMB , TCP 445==

Este modo funciona sin collector agents, lo que quiere decir que fortigate actuara como un collector agent recibiendo los eventos del DC, lo que causa que se requieran de recursos de RAM y CPU. Además de no tener carácteristicas adicionales como workstation checks y solo funcionar con dos eventos: 4768,4769
![[Pasted image 20240714122537.png]]
##### Terminal server agents


### Novell edirectory
- Se obtiene información desde [[LDAP]] o el novell api



### Instalaciones
#### Instalación de un collector agent
- Correr como administrator
- DomainName/Username
- Configurar para
	- Monitor logins
	- NTLM auth
	- Directory access
- Ejecutar la instalación de un DC agent en el collector inmediatamente después
##### Instalación de un DC agent
- IP address del collector agent
- Dominios a monitorear 
- Seleccionar usuarios a ignorar eventos, con distintos motivos como una actualización programada
- Elegir DC agent mode o polling mode, si se elige polling no se instala el DC 


### Otras instalaciones
Desde el collector agent en opciones avanzadas podemos habilitar
- Radius
- Citrix (Terminal Server)
	- Es como un servidor DC, que requiere de un controller para enviar eventos de login, además de usar los mismos puertos
- Exchange


### Logs de Collector agent
![[Pasted image 20240714224956.png]]

### Recomendaciones
- Asegurarse de que el puerto SMB y sus fallbacks esten abiertos TCP 445 SMB, UDP 139 , TCP 135, TCP 137, 389 (LDAP), 445 y 636 (LDAPS)
- Garantizar al menos 64 kbps de velocidad entre fortigate y los domain controllers
- Limpiar sesiones no activas en menor tiempo al default
- Asegurarse de que el DNS tenga records actualizados
- Agregar o quitar el SSO_guest_user group de acuerdo  

### Comandos
Lista de usuarios FSSO
```
diagnose debug authd fsso list
```
Filtro a aplicar al debug
```
diagnose debug authd fsso filter
```
Refrescar grupos
```
diagnose debug authd fsso refresh-groups 
```
Resumen de usuarios logeados
```
diagnose debug authd fsso summary
```
Eliminar estado de logins cacheados
```
diagnose debug authd fsso clear-logins
```
Resincronizar base de datos FSSO
```
diagnose debug authd fsso refresh-logins
```
Actualizar información de grupos de usuario
```
execute fsso refresh
```
Conexión a collector agent
```
diagnose debug authd fsso server-status
```

Limpar todos los usuarios filtrados
```
diagnose firewall auth clear
```
Filrar por grupo, id, usuario
```
diagnose firewall auth filter
```
Listar usuarios autenticados
```
diagnose firewall auth list
```

##### Comandos polling mode
Información del estado del polling mode
```
diagnose debug fsso-polling detail
```
Actualizar usuarios activos
```
diagnose debug fsso-polling refresh-user
```