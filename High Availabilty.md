==System>HA==
![[Pasted image 20240721231323.png]]

La idea es siempre tener disponible un servicio, se sincroniza con hasta 4 dispositivos fortigate

Se sincroniza la informacioón del principal con los fallbacks

Al tomar el lugar, lo hace con toda la configuración del otro forti, para asegurar servicio (con algunas excepciones como licencias o configuración HA), llegando a tal extremo como adoptar las mismas direcciones MAC
### Active-passive
Solo el principal trabaja, y los demás monitorean el estado del principal, si algo ocurre al principal, se reemplaza a esto se le llama failover


### Active-active
Todos trabajan, y el primario distribuye trafico a los secundarios
Por default, solo inspecciones de proxy se distribuyen, para balanceo de trafico se debe habilitar
load-balance-all
	No se puede hacer balanceo de cargas sobre
	- ICMP
	- multicast,broadcast
	- SIP
	- ALG
	- IM
	- P2P
	- IPSec
	- HTTPS SI se usa proxy inspection
![[Pasted image 20240721235522.png]]


```
config system ha
	set schedule none|hub|leastconnection|round-robin|weight-round-robin|random|ip|hub|ipport
```

### Fortigate clustering protocol
- usado para
	- Member discovery
	- Primary eleciton
	- Data sync
	- Member health monitoring
- Eventos de failover
	- Dead member
	- Failed link
	- Failed remote link
	- High memory usage
	- Failed SSD
	- Admin triggered
- Heartbeat
	- 0x8990 NAT
	- 0x8991 Transparent
- Data sync
	- 0x8992
	- TCP 703
	- TCP 700
	- TCP 22

### HA requisitos
Todos deben tener
- Mismo modelo
- Mismo firewall version
- Licensamiento
	- Si hay licensamiento distinto, fortigate elige la manera más baja de licensamiento
		- Ej, si solo hay un web filtering, los demas acordaran no usar web filtering de manera general
- Configuración de firewall
- Modo operativo 
- Mismo HA group id, name , pass, hearth

- Usar al menos dos interfaces hearthbeat (maximo 8)


### Eleccion primario
![[Pasted image 20240721232959.png]]

- Override, prioridad primero que HA uptime
- ![[Pasted image 20240721233114.png]]
config system ha
	set override enable
end


### Tareas de fgt primario
Broadcast hello para monitoreo y discovery
- Sync
	- Configuracion
	- FIB
	- DHCP
	- ARP
	- Definiciones de fortiguard
	- IPSec tunnels SA
	- Sesiones


### Tareas fgt secundario
Sincroniza data del primario
Se elige un nuveo primario en failover


### Hearthbeat
Se asigna las siguientes direcciones
	- 169.254.0.1 para el serial number mas alto
	- 169.254.0.2 para el segundo mas alto
	- 0.3 para el tercero
La ip no cambia aunque se cambie el primario

La ip puede cambiar en caso de que se una o salga un miembro


### Sync
El principal compara el checksum de su conf con el de los secundarios, si nos son iguales se sincorniza

- Configuracion
	- VPN
		- SA tunnels y SSL web mode
	- FIB
	- DHCP
	- ARP
	- Definiciones de fortiguard
	- IPSec tunnels SA
	- Sesiones
		- TCP por default
		- UDP e ICMP opcionales
		- MULTICAST nono
```
config system ha
	set session-pickup enable
	set session-pickup-connectionless enable
	set multicast-ttl <5-36000>
```

NO se sincroniza
- HA managment interface csettings
- HA override
- HA device priority
- HA virutal cluster priority
- Host name
- Ping server HA
- Licencias
- Cache
- SSL tunnel


### Failovers
De muchos tipos, se disparan cuando

- Device
	- Se dejan de recibir paquetes del primario
- Link
	- Se detecta un enlace como down
- Remote link
	- Se hacen probes a un beacon
- Memoria
	- Se excede el threshold
- SSD
	- Error ext-fs
- Identity failover protection
	- SNMP, correos 


```
config system ha
	set hb-interval 1-20 (default 2, ms 100)
	set hb-lost-threshold 1-60 (default 6)
```


###### Remote link fail over usando link monitor

```
config system link-monitor
	edit "port1-ha"
		set srcintf
		set server
		set ha-priority 10 (penalty, inicialmente 0)
	next
```

```
config system ha
	set pingserver-monitor-interface port1
	set pingserver-failover-threshold 5
	set ping-server
```

### Virtual clustering
Para uso de VDOMs en fortinet

![[Pasted image 20240721235733.png]]

get system ha
diagnose system ha
execute ha manage


Para conectar a los miembros, siempre que conectemos a la ip del cluster contestara el principal, debemos ir al prinicipal y

Usar HA managment interface
Habilitar managment status
Configura ha interfaces
Bindea a un puerto y asigna gateway

Luego se configura el puerto atado para funcionar 
![[Pasted image 20240722000123.png]]

Se usa una interfaz interna de manejo

Usar in band managment interface
![[Pasted image 20240722000128.png]]



Uninterruptible upgrade