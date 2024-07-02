Existen dos tipos de comportamiento de VPNs. 
### Policy based VPN
Se usan reglas para determinar si un paquete viajara por la vpn, esto sirve para granularidad, ya que podemos pasar paquetes sobre diferentes VPNs basandonos en [[Politicas de Firewall]]

### Route based VPN
VPN en rutas, sirve para situaciones en las que hay que pasar muchas politicas o trafico general bajo una misma VPN, en este caso se usan las rutas para determinar que ruta tendra vpn


## IPSEC
 Las VPN - virtual private network son redes virtuales con el proposito de crear una conexión segura, ==Autenticable y cifrada== a través de redes públicas y no confiables, como el internet

Bases de IPSec.
Muy parecido a [[TLS - Transport layer security]], IPSec usa llaves [[Cifrado asimetrico]] para poder llevar a cambio un intercambio de llaves simetricas para la sesión


### Fase 1
En el caso de VPN, el equivalente a [[TLS handshake]], seria el protocolo [[IKE - Internet Key Exchange (pendiente)]]


1. Se identifica por medio de una contraseña o certificado
2. Se genera una llave diffie-hellman en cada lado de manera local
	1. Llave privada local + llave publica remota + secretos= Diffie Hellmann
3. Se llega a un acuerdo de llave simetrica por medio de la llave Diffie-Hellmann [[TLS handshake]]
4. Se crea un tunnel Security Assosiation (tunnel fase 1)

![[Pasted image 20240623201624.png]]

### Fase 2
1. Se usan las llaves acordadas en fase 1
2. Acordar métodos de cifrado y llaves usadas para el tráfico de bulto (tráfico de red)
3. Se crea un tunnel IPPSEC Security Association (tunnel fase 2)

La razón de dividir las fases en dos es para poder crear y destruir tuneles de fase 2 facilmente. Cuando ya no exista tráfico interesante, tirar el tunel hasta que de nuevo haya tráfico, esto no toma tiempo ya que ya se llego a un acuerdo desde fase 1.

![[Pasted image 20240623201821.png]]

