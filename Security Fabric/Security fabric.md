# ==Security Fabric > Fabric connectors==

![[Pasted image 20240613155259.png]]


## *Security fabric es el acercamiento de fortigate para crear una red de seguridad centralizada*


Esto quiere decir que se conectan todas nuestras fuentes de inteligencia de amenazas, firmas, todo. Para lograr un manejo centralizado y simple de la seguridad. A su vez *Permitiendo que las amenazas sean erradicadas al momento que aparece una en una de nuestras fuentes*

Es
- Amplia - Centraliza todas las soluciones en una para efectivamente bloquear ataques
- Integrada - Reduce la complejidad de la red al centralizar la inteligencia de amenzas
- Automatizada - El intercambio de inteligencia de amenzas ocurre en tiempo real
- Abierta - La api es open source para integrar cualquier vendor


### Soluciones de fabric connector
![[Pasted image 20240613160833.png]]
Se ofrecen 8 categorias de productos para proteger todo tipo de vectores
 - Network access
 - Security WLAN/LAN
 - Public and private cloud infraestructure
 - Applications
 - Endpoint
 - Security Operations
 - Open fabric ecosystem
 - Fabric managment center


## Composición de un security fabric
![[Pasted image 20240614160036.png]]

- En el centro debe de haber al menos dos dispostivos fortigate y al menos un fortianalyzer o fortigate cloud
- Recomendado es tener distintas soluciones como
	- fortimanager, fortiap, fortiswitch, forticlient, forticlient ems, fortisandbox fortimail, fortiweb, fortideceptor
- Extendido - otros vendors


# Uso
Se debe configurar una [[raiz]] y [[downstream]]s 

En el fortigate [[raiz]]

En los [[downstream]]  


![[Pasted image 20240614170042.png]]

## Conflictos de objetos
1. Mensaje de notificaciones avisando del conflicto
2. Resoulción Automatico o manual
3. Automatico agrega Remote-* al objecto downstream


# Multi vdom

- Para que el fabric pueda detectar el vdom de downstream debe de estar configurado el fortigate en multivdom
- Se debe habilitar device detection para encontrar puertos habilitados y conectados
- VDoms sin puertos y con dispositivos conectados no serán encontrados

# Identificacion de devices

Fortigate detecta los dispositvos de red, incluyendo los 3rd party y los agrega a la topologia

Para esto hay dos tacticas, agentless y agent
Network > interfaces - enable device identification

Security Fabric > Logical Topology
- agentless
		Este solo funciona si hay comunicación directa entre forti y el dispositivo en cuestión, no debe de haber intermediarios capa 3
		La forma de identificar es por medio de varios metodos, headers http 3, dirección MAC, TCP Fingerprinting, DHCP, Microsoft web browser service, SIP user agent, Simple Service Discovery protocol, QUIC, FORTI-OS VM detecion

- agent 
		- Usa forti client para recibir información de dispositivos


![[Pasted image 20240614172401.png]]

## Automatización de eventos
Para esto se usan [[stitches]]

Las stitches nos permiten actuar de manera automática cuándo se detecta una amenaza en nuestra security fabric


## Conectores externos
Permiten integrar distintas soluciones de nube
![[Pasted image 20240614173704.png]]


## Se puede ver el estado del security fabric en ==Dashboard > Status > Security fabric connectors==


### ==Dashboard > Security ratings==
Consiste de 3 scores
- postura de seguridad
	- Provee de tips para mejorar la seguridad de fabric connector a través de security rating en ==security fabirc > security rating==
	-  

- Covertura de fabrica
	-   
- Optimización

![[Pasted image 20240614174538.png]]


Los chequeos de security rating se ejecutan cada 4 horas y se deshabilitan con 
![[Pasted image 20240614175144.png]]

O ejecutar manualmente con 
`diagnose report-runner trigger`

### Topología
==Security Fabric > Physical Topology==

Nos permite ejecutar tareas administrativas sobre dispositivos, la topologia completa solo esta disponible bajo el fortigate raiz
![[Pasted image 20240614175520.png]]

