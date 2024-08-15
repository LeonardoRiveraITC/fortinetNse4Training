Zero trust access network
- RBAC authentication 
- client auth
- Zero trust logs

### Modos
#### Access proxy
Centrado a usuarios remotos
Acceso a recursos por un proxy cifrado [[TLS - Transport layer security]] /SSL 

#### IP/Mac based access control
- centrado a usuarios locales
Agrega un TAG IP address/Mac address para identificacion


![[Pasted image 20240707225600.png]]

Se usa un forticlient EMS que enviara la información a fortigate security fabric sobre los usuarios

### Roles
- Forticlient
	- Endpoint client que da informacion de dispositivo, usuarios, seguridad opposture

- Forticlient EMS 
	- Emite y firma certificados cliente
	- Syncroniza con fortigate
	- Usa tagging para endpoints
- Fortigate 
	- mantiene conexion con EMS
	- FortiWAD daemon usa esta informacion para el trafico ZTNA
