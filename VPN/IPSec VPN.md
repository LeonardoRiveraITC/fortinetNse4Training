e=VPN>IPSec==


Funciona en base a IPSec[[vpn]]
==De los protocolos usados en IPsec vpn, fortigate no usa AH==
Proporciona de 
- Autorización (Por contraseña o por certificado)
- Confidencialidad (Cifrado de datos)
- Integridad de datos (Chequeo de integridad HMAC)

==Las politicas de firewall se evaluan antes que los slectors de fase 2, por lo que hay que establecer politicas de firewall que permitan el tráfico de la vpn==
==Por lo mismo, para establecer un tunnel se deben incluir politicas de firewall de ingreso y egreso==

==Cuando el gateway remoto es estatico, las rutas aparecen desde que se establece la fase 1, para que el trafico coincidiente a la ruta dispare una negociación de SA fase 2==

==Cuando remote gateway se establece como dial up, se agrega la ruta hasta que ocurre el tunnel fase 2 SA, con una distancia default de 15, además de que se eliminaran una vez se cierre el tunnel==

### IKE 1 vs IKE 2
#### IKE 1
- Autenticación simétrica
	- PSK
	- Certificate
	- Extended authentication XAUTH
- NAT-T - Soportado como extension
- Inconfiable, los mensajes no son reconocidos
- Dial up phase 1
	- Peer id + aggressive mode + PSK
	- Peer id + main mode + certificate signature
- No traffic selector
##### Main mode
Total de 9 mesnajes (6 de fase 1 y 3 de fase 2)
- Es más seguro debido a que el preshared key hash se envía cifrado

##### Aggressive mode
Total de 6 mensajes (3 de fase 1 y 3 de fase 2)
- ==Negociación más rápida==
- ==No tan seguro==
- Requerido cuando se revisa peer id
Un caso de uso de este modo es cuando se tienen multiples tuneles dial up hacia la misma ip, ya que al no conocer la ip iniciadora, en aggressive mode el id peer se envia en el primer paquete lo que hace que se diriga al tunel correcto, mientras que en main mode el peer id se envia al ultimo paquete 

#### IKE 2
- Solo un procedimiento de intercambio
- Autenticación asimetrica
	- PSK
	- Certificate
	- EAP (Extendible authentication protocol)
	
- NAT-T nativo
- Confiable - Los mensajes se reconocen
- Dial up phase 1
	- Peer ID
	- Network ID
- Traffic selector



### Setups comunes
#### Acceso remoto
- Funciona sobre dial-up

En este modo de acceso solo un usuario puede iniciar una conexión VPN ya que su ip comunmente será dinámica, además de requerir un cliente como forticlient

#### Site to site
##### Simple
![[Pasted image 20240717135422.png]]
Un tunnel simple site to site

##### Hub and spoke 
![[Pasted image 20240717135555.png]]
Un setup relativamente simple para conectar multiples sitios, donde el unico dispositivo que requiere de buenos recursos es el hub, ya que los firewalls solo se encargan de mantener un tunnel, mientras que el hub and spoke se encargara de manejar los otros 4 tuneles. Puede ser lento si el site esta muy apartado del sitio local

##### Full mesh and partial mesh
![[Pasted image 20240717135910.png]]

- Full mesh: todos los dispositivos comunican uno con otro
- Partial mesh: solo algunos dispositivos se conectan

Esta opcion es más adhoc que las otras, además de que puede ser complicado el routeo por lo que se debe planear bien, además de generar menos overhead que un hub

#### Auto discovery vpn
![[Pasted image 20240717140221.png]]
Combina las ventajas de un mesh y un hub, al conectar los spoke a un hub y dejar que las rutas se descubran automaticamente y luego levantando tuneles de manera dinámica entre sitios 

### Setup 
- Wizard
	- ==VPN>IPSec Wizard==
	- Existe un wizard con los setups comunes que simplifica la creación de un ipsec vpn
		- Esto lo hace con plantillas visibles en ==VPN>IPSEc tunnel template==
	- De igual manera, podemos elegir custom, lo que nos llevara directamente a configurar un tunel phase 1 y phase 2

#### Custom
##### Fase 1
###### Network
-  IP version (La versión a usar de salida, encapsulación)
		- IPV4
		- IPV6
- Remote gateway
- No se esta limitado a una sola opcion por vpn
	- Static
		- Ip address
	- Dial up
	- Se usa cuando se trabaja con ips dinamicas, por lo que el iniciador (cliente), será el que deberá inicar la negociación de las llaves
			- Fortigate a fortigate
			- Usuario a usuario
	- Dynamic DNS
		- Usar un FQDN
- Interface
	- La interfaz a la que se bindeara la VPN, generalmente es una WAN, hay que tener por seguro que esta tenga una ruta a el sitio remoto
- Local Gateway
	- El gateway de salida local, se usa cuando la interfaz elegida tiene más de una IP
		- First
		- Second
		- Specify 
			- Cuando se quiere usar una ip diferente a las de la interfaz
- Keep alive frequency
- Dead peer connection
	- ==Mecanismo para detectar un tunnel muerto, util en tunneles redundantes==
	- On demand
		- Se envian Dead peer Detection (DPD) probes cuando no hay traficode entrada en el tunnel
	- On idle 
		- Se envian DPD si no hay trafico en el tunnel
	- Disabled
		- Solo responder a probes, y no enviarlas
- Forward Error Connection
- Avanzado
	- Agregar ruta
		- No usar con dynamic protocols
	- Auto discovery sender
		- Habiltar en HUB para facilitar ADVPN
	- Exchange interface IP
		- Intercambio de IP addresses de interfaces IPsec, esto permite multi point hub and spoke
	- Device creation
		- Crea una interfaz para cada cliente dial up
	- Miembro agregado
		- Agregar multiples tuneles a una interfaz
- IKE mode config
	- ==Para que funcione se debe habilitar en ambos puntos==
	- Funciona como en DHCP, en que por medio de IKE messages se enviara información sobre IP, DNS, mascara de red
- NAT transversal
	- ESP tiene problemas pasando redes nateadas, ya que este usa numero de protocolo en lugar de puerto TCP/UDP. Por lo que el nat transversal se agrego como medida a esto
	- NAT transcersal es capaz de detectar si hay dispositivos de nateo de por medio, y si los hay utiliza ESP por medio de UDP/4500 y IKE por medio de TCP/4500
	- La opcion de forzar hara que siempre usen estos puertos

###### Authentication
- Metodos
	- Preshared key
	- Signature
		- Para esto, el CA debe estar instalado en el otro peer
- IKE
	- v1
		- Aggressive y main mode
	- v2
		- No aplica
###### Propuesta
- Cifrado
	- El algoritmo de cifrado a usar
		- AES128
		- AES192
		- DES
		- 3DES
- Autenticación
	- Algoritmo para verificar la integridad de la authenticación
		- SHA256
		- SHA384
		- SHA512
		- SHA1
- Grupos diffie hellmann
	- Mientras más altos más seguros, pero más tardado de computar
- Key lifetime
	- Tiempo de vida del tunnel ipsec security association phase 1 (IKE SA)
- Peer ID
	- Si el punto acepta un ID especifico, llenar el campo

###### XAUTH
Agrega una capa de seguridad extra al forzar autenticación de usuarios

Cuando se usa fortigate como dialup server, se debe de establecer un usuario y contraseña, además de elegir el tipo de server, PAP, CHAP y automatico

Luego se puede elegir que tipo de match de usuarios se desea
- Choose
	- Te permite elegir un grupo de usuarios de fortigate que se podran autenticar
	- La desventaja de este es que tendrías que crear un nuevo tunnel para cada grupo de usuarios que deseas conectar
- Inherit from policy
	- Hereda de las politicas de firewall los usuarios que podran conectarse, similar a SSL VPN, de esta manera puedes tener varias politicas para un solo tunel


##### Fase 2
La fase 2 es un tunnel "desechable" que se establece y tira de forma constante, ya - que se hace sobre el tunnel fase 1 ike ya establecido
- Perfect forward secrecy recalcula las llaves DH para evitar reusar
###### Selectores
En la fase 2 se debe proteger el tráfico interesante, por lo que las configuraciones de esta sección son relevantes a eso
Además, si se trata de un punto a punto los parametros de red de ambos peers deben coincidir

- Local Address y remote address
	- Las subredes locales y remotas, por logica, la subred remota sera la local de punto B y viceversa. Es fácil permitir cualquier subred a cualquier subred porque
		- Si fortigate no encuentra un selector que coincida con el trafico entrante IPSec, se dropea el tráfico, primero  se revisan las politicas de firewall
- Avanzado
	- Protocolo
		- por default all
	- Puerto
		- Por default all

###### Propuestas
- Cifrado
	- Para cada selector es posible que se deban configurar 1 o más propuestas, la propuesta es donde se propone el conjunto de algoritmos de cifrado a usar, esto afectara directamente al performance, siendo que algoritmos como 3AES son tardados de cifrar y decifrar. Además de que cifrados cómo CHACHA20POLY1305 no tienen soporte de aceleración de hardware
- Expiración del tunel basado en
	- Segundos
	- Kilobytes
	- Ambos (el primero en expirar)
- Auto key keep alive
	- Mantiene vivo un tunel cuando hay tráfico interesante
- Auto negotiate
	- Levanta un tunnel SA antes de que expire el actual para no tener perdida de paquetes, y lo usa de inmediato, al habilitar este es implicito el uso de Auto key keep alive




### Route based IPSec VPN
- policy based routing
	- No recomendado y se mantiene solo por soporte legacy
#### Route based
Cuándo route based esta habilitado Fortigate automáticamente agrega una interfaz virtual con el nombre de la VPN, esto fácilita la creación de routeo basado en politicas y de politicas de firewall
- Redundancia
- Soporte para
	- Protocolos de  routeo dinamico
	- L2TP-over-IPSEC
	- GRE-over-IPSEC

###### Dial up user
Cuando se esta configurado como dial up user podemos configurar add-route (habilitado por default), lo que hace add route es agregar rutas estaticas de forma automatica cuando se establece el tunnel fase 2 SA de la red local presentada durante la negociación del cliente, con una distancia default de 15
Una vez que se tira el tunnel, se remueve esta ruta estatica

Si se deshabilita se usan protocolos de routeo dinamico

```
config vpn ipsec phase1-interface
	edit "Dialup"
		set add-route enable | disable
	next
```

###### Static ip / Dynamic DNS
Cuando se configura de esta manera, se deben hacer rutas estaticas usando la interfaz virtual de la vpn
![[Pasted image 20240718010941.png | 500]]





### Redundancia
![[Pasted image 20240718011540.png]]
- Parcial
	- Un peer con dos o más tuneles
- Full
	- Cada peer tiene dos o más tunneles

#### Configuración
- Crear dos tunneles con su fase 1 y fase 2 respectivamente. Y habilitar DPD en ambos peers
- Agregar una ruta estatica para cada tunnel, con una prioridad menor para el fallback
- Configurar politicas de firewall para ambos


### Status
==Dashboard > Netowork > Ipsec==
Este widget nos ofrece varias opciones, como elegir un selector de fase 2 especifico, bajar o levantar tuneles, etc



### Comandos
Cambiar puerto de ike
```
config system settings
	set ike-port <port>
end
```

Offloading
```
config vpn ipsec phase1-interface
	edit ToRemote
		set npu-offload disable
	next
end
```
