==VPN>SSL-VPN/SSL-VPN portal==
Una VPN SSL se basa en crear un tunnel de comunicación [[VPN]] sobre [[TLS - Transport layer security]]

Para monitorear sesiones activas ==Dashboard > Network > SSL VPN==
Para logs ==System logs > user events/vpn events==

### Web mode
- Requiere solo de un navegador web
- Solo soporta Http/s, RDP, SMB,SSH, Telnet, VHS y ping
- La interacción con la vpn esta limitada al navegador por medio del portal 
- Cookies habilitadas

Actua como un server side reverse proxy o un gateway simple seguro https

La sección bookmarks contiene recursos utiles al usuario, alternativamente puede usar quick connection para acceder a una url deseada

Se autentica un usuario, el cual puede tener distintos permisos y distintos portales que otros usuarios


### Tunnel mode
- Accesible a través de forticlient
- Requiere de un adaptador virtual en el cliente - fortissl
- Cada que un cliente se conecta a la vpn, este recibira una IP dinámica de una pool
- Forticlient debe de ser compatible con la version de firmware de fortigate
De igual manera al web mode requiere de que el usuario se autentique para formar la conexión

##### Fortigate como cliente
- Se crea una interfaz de tipo tunnel SSL VPN en el fortigate cliente
- Requiere de una instalación propia de CA de certificados, ya que el cliente usa PSK (preshared key) y PKI (public key ifraestructure) para autenticarse
- Se crean rutas dinámicas a los recursos detrás del VPN server de forma automática
- Se le asigna una ip dinámica de una pool al fortigate cliente
- Permite configurar tecnologia hub-and-spoke entre el cliente y el servidor
- Cuando se crea un tunnel se agrega una ruta dinamica default como ECMP para la default ya existente

![[Pasted image 20240714233226.png]]

##### Split tunneling
- Deshabilitado
	- Todo el tráfico pasa a través del fortigate server, incluyendo el tráfico de internet, lo que causa congestión en la red
- Habilitado
	- Solo el tráfico dirigido a las subredes del servidor pasan a través del tunel, las demás pasan por internet

##### Client integrity checkup
==VPN>SSL-VPN>portal-name>host check==
Revisa la integridad del cliente por software instalado, para este proceso se depende del centro de seguridad de windows por lo que se requiere de software reconocido o ingresar el guid indicado, de otra manera no conectara a la vpn

- Revisar por AV
- Revisar por FW
- Revisar por GUID especificos

### Setup
#### SSL VPN user as client
![[Pasted image 20240714234917.png]]
##### 1. Configurar cuentas de usuario y grupos
-  Se soportan todos los métodos de autenticación menos FSSO debido a las limitaciones a la red local al usar este

##### 2. Configurar portales SSL vpn
Portal configurable para usuarios y o grupos
- Modo web
	- Solo sirve en HTTP/S, ping, SSH, FTP, VNC, RDP,SMB, TELNET
- Modo tunnel
	- Asigna una ip de un pool a cada cliente
	- Al usar split tunneling se debe habilitar una de las siguientes dos opciones
		- Policy destination
			- Solo permite tráfico que coincide a las reglas de la politica
		- Trusted destination
			- Solo permite tráfico que NO coincide con las reglas de la politica

##### 3. Configurar settings SSL VPN
Una vez que se tiene configurado el portal se debe configurar la VPN
- Mapear la vpn a una interfaz, la cual sera la que los usuarios usaran para conectar a la vpn
- El puerto default es 443, si por alguna razón se sobreponen el portal VPN y el portal administrativo, el portal vpn tiene prioridad
- SSL VPN timeout - default 300 segundos (5 mins)
- Por default se usa un certificado self signed
- Asignamiento de IPs
	- Se toman ips de un pool y hay dos algoritmos para el asignamiento
		- First come first serve (default)
		- Round robin
		- Configurar round robin se debe hacer desde la cli, solo puede usar un pool que tambien debe de ser configurado desde cli

- Resolución de DNS
	- Puede ser un servidor interno o WINS

- Portales de autenticación
	- Establecer portales de autenticación default o para distintos usuarios y o grupos

##### 4. Crear politicas de firewall
Se debe permitir tráfico de salida y entrada a la vpn
- Se crea una interfaz virtual llamada ssl.\<vdom>\, la cual representa la vpn
- Si no estan habilitados los vdoms, se bajo root por default
- Incluir en la politica
	- Interfaz virtual de entrada
	- Interfaz fisica de salida
	- Usuarios
	- Perfiles al gusto
![[Pasted image 20240715002110.png]]


##### 5. Crear politicas para permitir salida a internet (opcional)



#### Fortigate como servidor - fortigate como cliente

##### Servidor
###### 1. Establecer cuentas de usuario y grupos 
- Para esto se debe  habilitar login por medio de psk y pki
	- Para habilitar el pki, se debe crear primero por cli un usuario pki
	- Al usuario pki debemos de darle un CN, ya que de otra manera cualquier certificado firmado por el ca es valido
- Crear una cuenta local y otra remote
	- ==User & auth > User definition==
	- ==User & auth > PKI==
###### 2. Configurar portales
###### 3. Configurar settings vpn
###### 4. Crear políticas de entrada y salida de la vpn
###### 5. Crear políticas de acceso a internet de la vpn (opcional)
![[Pasted image 20240716092337.png]]

##### Cliente
###### 1. Crear usuario pki
- Establecer el mismo CN del fgt server
- Usar un certificado que permita completar la cadena
###### 2. Crear una interfaz tunnel SSL VPN 
- Interfaz ssl.root
###### 3. Configurar
- Client name
- Interfaz SSL
- SSL de fgt server
- Establecer un certificado apropiado
###### 4. Crear politicas de FW que permitan el tráfico hacia la vpn
![[Pasted image 20240716092353.png]]


### Timers
Cuando una sesión es cerrada, ya sea por idle o por el usuario, fortigate elimina todas las sesiones de la entrada para evitar el reuso de sesiones

Una sesión se considera idle cuando ya no ve paquetes de la sesión

El idle del vpn es diferente al timeout de autenticación de usuario, si se expira una sesión de usuario fuerza a cerrar la sesión vpn debido a politicas del firewall

Cuando se esta en redes de alta latencia, es posible que fortigate termine la sesión sin siquiera establecerla debido a timeouts para recibir respuesta, es por eso que se puede configurar

- login timeout - default de 30 segundos
- dtls hello timeout - 10-60
- https-request-header-timeout
- https-request-body-timeout

Los timers tambien ayudan a mitigar vulnerabilidades como slowloris o R-U-Dead-yet


### Comandos
Configurar portal
```
config vpn ssl web portal
	edit <name>
		set tunnel-mode <enable|disable>
		set web-mode <enable|disable>
	end
```
Habilitar round robin
```
config vpn ssl settings
	set tunnel-addr-assigned-method first-available/round-robin
end
```
Agregar ip pool para round robin
```
config vpn ssl settings
	set tunnel-ip-pools/tunnel-ipv6-pools
end
```

Habilitar usuario pki
```
config user peer
	edit pki
		set ca "CA_Cert_1"
		set cn "FGTXXXXX"
	end
```

COnfigurar host check
```
config vpn ssl web 
	edit <portal-name>
		set host-check [none|av|fw|av-fw|custom]
		set host-check-interval <seconds>
	end
```

Revisar host check

```
config vpn ssl web host-check-software
```

Cambiar timers para el inicio de sesion vpn
```
config vpn ssl settings
	set login-timeout <10-180>
	set dtls-hello-timeout <10-60>
	set http-request-header-timeout <1-60>
	set http-request-body-timeout <1-60>
end
```

Habilitar aceleracion de hardware
```
config firewall policy
edit <id>
	set auto-asic-offload [enable|disable]
```

##### Debug commands
Listar conexiones activas
```
diagnose vpn ssl list
```
Info general
```
diagnose vpn ssl info 
```
Estadisticas de uso de recursos
```
diagnose vpn ssl statistics
```
Debug filter
```
diagnose vpn ssl debug-filter
```
Estado de aceleracion de hardware
```
diagnose vpn ssl hw-acceleration-status
```
Habilitar o deshabilitar forma legacy de alojamiento de ip
```
diagnose vpn ssl tunnel-test
```
Habilitar o deshabilitar ID aleatorio de sesion para vpn web
```
diagnose vpn ssl web-mode-test
```
Habilitar o deshabilitar logs de vpn
```
diagnose debug application sslvpn -1
```