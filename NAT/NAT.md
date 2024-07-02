==Network Address Translation==
La traducción de Direcciones de red consiste en un dispositivo como un router o un firewall reemplazando una ip y puerto por otra que esta mapeada.

El nateo nacio debido a el agotamiento de direcciones IPv4, y ofrece beneficios como

- Seguridad. Un dispositivo interno nunca dara a conocer su ip, ó sera siquiera alcanzable a menos que sea permitido por el dispositivo gracias a que este se encuentra detŕas de una ip nateada.
- Contramedida al agotamiento de IPs ipv4
- Redes privadas y externas. 
	- El concepto de redes privadas y externas se usa en gran medida con nateo. Las redes privadas son rangos de direcciones IP que nunca serán publicados a internet, por lo que siempre se mantienen dentro de la LAN. El nateo nos permite cambiar de direcciones ip públicas sin tener que modificar nada de nuestra infraestructura interna

SNAT: Traducción de IP y puerto source
- Para activar SNAT puedes usar NAT por la interfaz de salida en politicas de firewall o habilitar un Central nat para el [[Vdom]]
DNAT: Traducción de IP y puerto destinos
- Para DNAT podemos habilitar una [[VIP]]. En caso de usar central NAT debemos habilitar una [[VIP]] para Central NAT

PAT: Port Address Translation. Es una tecnica donde además de traducir la dirección IP. También traducimos puertos. Esta es particularmente útil cuándo tenemos multiples dispositivos detrás de un dispositivo NAT que requieren de traducción. De esta manera podemos mantener multiples sesiones con una misma IP, al asignar conexiones a los puertos y dar seguimiento a los paquetes de esta manera. 

##### ==Metodos de nateo==

- NAT 46
	Se refiere a la traducción de addresses entre ipv4 e ipv6. 4-6
	
NAT 64
	Se refiere a la traducción de addresses entre ipv6 e ipv4.6-4

NAT 66
	Traducción de addresses ipv6-ipv6

### NAT por [[Politicas de Firewall]]
Recomendado para infraestructuras con pocas salidas de NAT. Donde además los requisitos son granulares y los requisitos de nateo no conflictuan con politicas de firewall colindantes

Este método de nateo usa las politicas de firewall para dictar el comportamiento sobre una de estas. Para SNAT se habilita la salida por la ip de la interfaz de salida.
Para DNAT se usa una VIP para mapear a la ip de destino

#### SNAT
==Policy & Objects > Firewall policies > NAT==
Para esto podemos usar la interfaz de salida o una pool de IPS
![[Pasted image 20240623135236.png]]
##### Usar interfaz de salida
Con esta opcion todo paquete que coincida a la politica, su source ip sera traducida a la ip de la interfaz de salida. Se puede utilizar PAT para así traducir a la ip con puertos disponibles para establecer una sesión. Ó podemos usar puertos fijos, en  todo caso se deshabilitaria PAT. ==En caso de usar el ultimo, si dos conexiones se requieren sobre el mismo puerto, solo se atendera a una==
	![[Pasted image 20240623140110.png]]
#### Usar IP Pool
La ventaja de un IP pool es que soporta más conexiones a comparación de la interfáz de salida, con el uso de overloading. O atiende a necesidades más especificas de nateo


### NAT por [[Central NAT]]
Recomendado para aplicaciónes donde los requisitos de NAT son amplios, además de que muchas politicas de firewall colindantes requieren del uso de un mismo NATeo

Este método aplica por [[Vdom]]. En este método se usan reglas de nateo que aplicaran a todas las [[Politicas de Firewall]]



