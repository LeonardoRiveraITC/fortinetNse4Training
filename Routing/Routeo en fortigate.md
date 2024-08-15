==En fortigate se maneja el trafico de la misma manera que los routers, cada interfaz debe estar en una subred distinta y cada subred forma un dominio sitinto de broadcast, lo mismo ocurre al router, cambiando el src mac address por el de la interfaz==

Fortigate cuenta con dos modos de operacion como dispositivo de red. [[Modo transparente]] y [[modo NAT]]


- Soporta routeo de ipv4 y ipv6
- Trafico de firewall
	- El trafico que se genera desde y hacia el firewall
- Trafico local-out
	- El trafico saliente de la red local


==El routeo predece la mayoria de acciones de seguridad==

==La mejor ruta es la ruta más especifica al destino==


==Orden de routeo==
- RIB
- ISDB routes
- SDWAN routes
- FIB entries

### Tablas de routeo
Fortigate funciona con 3 tablas de [[Routeo]], estas se comportan igual que una tabla de routeo regular, con su next hop, arp etc. Pero con la diferencia de que fortigate determina que tabla de routeo usar en que situaciones.

##### RIB - Routing information base
Compuesta mayormente de las mejores rutas, vivas, estaticas y dinamicas

##### FIB - Forwarding information base
Es la tabla de routeo desde el punto de vista de kernel
Compuesta de RIB además de entradas especificas del sistema
- Rutas muertas (que estan establecidas pero no se puede establecer comunicación)
- Rutas de menos prioridad 
	- Rutas estaticas con menos prioridad
	- Rutas dinamicas con menos prioridad

##### Policy routes
Estas rutas engloban a lo que es 
- Policy routes
	- Se asigna un id de 1 a 65535
- ISDB routes
	- Se asigna un id mayor a 65535
- SDWAN routes
	- Se asigna un id mayor a 65535
	- Se le asigna el camplo vw1_service, que indica el id desde la perspectiva de SDWAN

==El maximo de policy rotues depende del modelo, puede ser 512==

Una tabla de rutas especial con presedencia sobre la tabla de routeo normal, esta tiene la particularidad de que, además de subred destino podemos agregar otros parametros granulares, como src addresses, protocolos de transporte, dst addresses, puertos, tipos de servicio y configurar la accion a tomar en hit 

- Stop policy routing 
	- Usa el FIB 
- Forward traffic
	- Forwardea usando lo configurado en la politica

#### Tabla de routeo en fortigate
![[Pasted image 20240707211326.png]]
- Source - primera col
	- K - kernel
	- S - estatica
	- C - Conectada
	- R - RIP
	- B - BGP
	- O - OSPF
	- I - IS-IS
	- V - BGP VPNv4
	
- Red destino - segunda col
- (Distancia/metrica) - tercera col
- via - cuarta col - ip de salto
- IF - quinta col
- Prioridad/Peso - ultima col

### Route lookup
Busqueda de ruta, como su nombre implica
1. Fortigate hace dos busquedas de ruta, para el primer paquete del originador
2. Y para el paquete proveniente del responsor
3. Luego fortigate guarda esto a una ==Tabla de sesiones==, donde a partir de ese momento todos los paquetes pasaran a traves de esa ruta, a menos que haya un cambio que impacte estas rutas


### Rutas estaticas 
==Network > static routes==
Rutas estaticas configurables por el administrador
Estan atadas a una interfaz y indican el siguiente salto para una busqueda de red

El destino de estas rutas puede ser 
- Subnet
- IP range
- FQDN
- Geography
- Dynamic
- Device (MAC)
Los mismos que el [[objetos]] address
- [[Internet service database]] object

### Rutas dinamicas
Existen protocolos para no configurar todas las rutas de manera estatica
==Debe activarse en [[Visibilidad]]==

- [[RIP - Routing interface protocol]]
- [[BGP - Border gateway protocol]]
- [[OSPF - Open shortest path first]]
- IS-IS - Solo en CLI

Al activar este feature tambien activamos visibilidad sobre
- [[Policy routes]]
- [[Routing objects]]
- [[Multicast]]

### Distancia
Usado en [[Lab 1 - Configuring route failover]]

El primer tiebreaker para determinar mejor ruta
- Establecido por el admin
- Mientras más bajo el valor, mejor la ruta
- Si existen dos rutas de misma distancia pero de distinto protocolo, se mantiene la ultima (evitar)

Distancias default para rutas:
- Conectado - 0
- Static (SD-WAN)- 1 
- Static (DHCP) - 5
- Static (Manual) - 10
- Static (IKE) - 15
- EBGP - 20
- OSPF - 110
- IS-IS 115
- RIP - 120
- IBGP - 200

### Metrica
El segundo tiebreaker de rutas con misma distancia y mismo destino
Mientras menos metrica, mejor la ruta
==Solo para rutas del *mismo* protocolo==

El calculo de metrica difiera entre protocolos, RIP usa saltos, OSPF usa costo, que se determina por el ancho de banda en el link, etc

### Prioridad
==Una vez llegados a este punto, todas las rutas son instaladas en el RIB==
==No aplica a rutas conectadas==
El tercer tiebreaker
Este busca romper el empate entre rutas ECMP (Equal distance, priority duplicate routes)
==Como en los demás, mientas la prioridad sea más baja, más alta sera la prioridad==

Para todos los protocolos se esta hardcodeado el valor a 1, y solo se puede cambiar en BGP.
El valor de rutas estaticas esta por default en 1, pero se puede configurar en la gui en rutas estaticas

Si existen rutas ECMP con prioridad igual, fortigate las instala todas al RIB y hace balanceo de cargas sobre de ellas

##### Balanceo de cargas ECMP
- IP - default
	- Sesiones de la misma ip usan la misma ruta

- Source-dest ip
	- Sesiones con la misma ip y con el mismo destino usan la misma ruta

- Weighted
	- ==Solo a rutas estaticas==
	- Mientras mas peso tenga una ruta, más sesiones se dirigiran por esa ruta
	- Se distribuyen sesiones basandose en peso o interfaz
- Usage
	- Se usa otra ruta hasta que se agote el spillover threshold de la primera

### IPv6
==Se debe habilitar en [[Visibilidad]]==


### Monitor
==Dashboard > Network > Routing > Static & dynamic routing==
![[Pasted image 20240707191923.png]]
- La tabla de routeo refleja la RIB
- La tabla de politica contiene las rutas de politica


### Herramienta de busqueda de rutas
- Dst address (obligatorio)

- Dst port 
- src address
- src interface
- protocol

==Si solo se pone el dst address, solo se considera el RIB==
==Si se ponen todos los campos, tambien se considera el policy routing==

### Comandos

imprimir FIB
```
get router routing-table all
```

imprimir la base de datos de rutas
Se indica con un asterisco la mejor ruta, parte del FIB
```
get router info routing-table database
```

Imprimir tabla de routeo de kernel
```
get router info kernel
```

imprimir la tabla de rutas policy
```
diagnose firewall proute list
```

==Solo si esta deshabilitado SD-wan se puede habilitar el balanceo ECMP==
```
config system settings 
	set v4-ecmp-mode [source-ip-based | weight-based | usage-based | source-dest-ip based]
end
```

Configurar pesos de interfaz

```
config system interface
	edit <if-name>
		set weight <0-255>
	next
end
```

```
config router static
	edit <id>
		set weight <0-255>
	next
end
```