Los objetos son "piezas de rompecabezas" usadas por fortigate, como su nombre indica son objetos

Estos se crean para poder aplicarlos a las politicas de firewall y reflejan algún recurso en la red. Por ejemplo un recurso de address refleja una dirección ip en la red.

Los objetos, además de su valor contienen un campo nombre y descripción. De esta manera simplificando mucho la administración de los recursos y dandoles un seguimiento de manera sencilla

Los objetos se aplican a una politica de firewall una vez creados

==Un objeto no se puede eliminar sin antes eliminar todas las referencias que se hacen a ellos. Esto se facilita con la columna referencias en la vista del objeto en cuestión==

==Tanto en politicas como objetos, se debe tener un nombre menor a 35 carácteres, con caracteres alfanúmericos. Y caracteres especiales solo de - y __==
### Direcciones y rangos de direcciones
Las direcciones son un objeto que refleja direcciones ip. Estos pueden ser rangos o subredes. Para agregar una ip unica, debemos agregar una subred, ingresar la ip deseada y poner la mascara de red como puros 1s (/32). En este grupo de objetos también entran los ==FQDN, las IPs geográficas, las IPs dinamicas y direcciones de capa 2 (MAC) y los grupos de direcciones==

### Servicios
Un [[Servicio de red (pendiente)]] tiene la caracteristica de estar definido por el par ==Protocolo de transmision/Puerto Destino== (En los casos de aplicaciones legacy, seguramente puerto source tambien, ya que este se elige de forma aleatoria en el esquema actual de las redes). Por ejemplo [[HTTP (pendiente)]] es TCP/80, mientras que [[HTTPS]] es TCP/443. Sin embargo esto no esta escrito en piedra, y de ser necesario podemos crear nuevos objetos de servicio para acomodar a las reglas de firewall. Por ejemplo. HTTP-PORTAL-ORG: TCP/8080
### Schedules
Las schedules son horarios aplicables a politicas de firewall durante los cuales entrara en vigor la politica en cuestion

Se puede programar por recurrentes o una vez; horas en dias especificos

- Recurrente. Aplica en los dias indicados en las horas indicadas. Si la hora de termino es más temprano que la hora de inicio, se terminara a la hora de fin del siguiente dia. Ejemplo: Permitir redes sociales en horas de comida
 
 - Una vez: Politicas que se aplicaran solo una vez Con fecha y hora de inicio y fecha y hora de fin. En este caso la fecha fin obviamente no puede ser antes de la inicio. Ideal para eventos especiales: por ejemplo: dia del niño en la planta. ==Este ocaciona que se genere un log  por dia de 1 a n dias (maximo 100) en preexpiration event log==

### Perfiles de seguridad
Los perfiles de seguridad son parte de los [[Servicios]] que ofrece fortigate para la protección extendida de la red. Una vez que la politica acepto el paquete, puede pasar por varios perfiles de seguridad, los cuales analizaran paquete por paquete en dos modos
- [[Proxy based inspection]] ó
- [[Flow based inspection]]
Cada uno con sus caracteristicas
Entre los perfiles que podemos aplicar tenemos
- [[Antivirus]]
- [[Web filter]]
- [[Video filtering]]
- [[Web filter]]
- [[Application control]]
- [[IPS - Intrution Prevention System]]
- [[File filter (pendiente)]]
- [[VoIP (pendiente)]]
- [[Web Application Firewall WAF (pendiente)]]
- [[SSL inspection]]

Estos cada uno con cuenta con su proposito y deben ser usados con caucion ya que consumen recursos de cpu.

*Por default VoIp, WAF y Video filter deben de activarse en [[Visibilidad]]*

### Internet Service Database
Es una base de datos, parte de los [[Servicios]] de fortiguard  que contiene  multiples servicios de internet. ==Sus Ips, puertos, servicios más usados==. Y es por ello que es mutuamente exclusivo con direcciones y servicios, ya que un objeto ISDB ya contiene esta información hardcodeada 
Esta base de datos se mantiene en constante actualizacion desde fortiguard

También podemos crear ISDB custom

#### Objetos geograficos de servicios de internet
Estos son rangos de ip de algun servicio de internet que se encuentran en un pais

##### ISDB Usado en [[Lab 3]]

### [[Traffic Shaping]]
En fortigate podemos aplicar traffic shaping de 3 maneras:
- Por grupos 
- Por IPs
- Por aplicación
Una politica de traffic shaping se aplica a una politica de firewall, por lo que hay que asegurarse que ambas coincidan

### IP Pools
Un objeto usado al momento de hacer S[[NAT]]
Las IP pools son pools de IPs o rangos de IP, con el proposito de usar estos en lugar de la IP de la interfaz de salida.
##### ==Al usar IP pools debemos asegurarnos de usar ips en la misma subred a la interfaz de salida. O establecer routeo propio a la configuración, ya que de otra manera se pueden perder los paquetes al no haber forma de llegar a la ruta. Ejemplo: Interfaz de salida: 172.10.11.1. IP pool: 10.0.0.1. Al momento de regresar el paquete no se va a saber por que interfaz se debe de ir al no haber ruta ==

Existen 4 modos de operacion:
-Sobrecarga
-Uno a uno
-Rango arreglado de puertos
-Alojamiento de puerto a puerto

#### Overload
El modo default de operacion. Atiende un modelo de operación de muchos a uno. Donde muchas IPs son traducidas a una sola IP hasta que se agoten los puertos de esta y se pasa a la siguiente IP disponible en la pool


#### Uno a uno
Fortigate mapea una IP del pool a una ip interna solicitando ser nateada de manera FIFO. First come first serve. En caso de que se solicite una ip y no haya disponibles la conexión no se hara y se ignorara la petición

#### Fixed port range
Esta es una opcion útil para administradores e ISPs para dar seguimiento a que ips tienen que puertos, ya que en el default overloading es dificil saber quien tuvo acceso a que puerto más que logeando, una operación costosa

Lo que hace fixed port range es:
1. Indicar un rango de ips externas a ser mapeadas a un rango de ips internas
2. Se llama fixed port porque se calcula el bloque de puertos a asignar en base al rango de ips internas e ips externas
3. Estos bloques calculados de puertos, se asignan a las ips del rango interno


##### Asignación de puerto
![[Pasted image 20240624132251.png]]
![[Pasted image 20240624132331.png]]
Después, debemos calcular los puertos que se reservaran a una ip, esto es importante ya que determinara los puertos a los que se identificara una ip

![[Pasted image 20240624152038.png]]


##### Asignando puertos


```
diagnose firewall ippool list #listar tamaño y numero de bloques

diagnose firewall ippool-fixed-range list natip 70.70.70.71 #detalles de la ip externa y asignamiento de puertos por ip interna

diagnose firewall ippool-fixed range list natip 70.70.70.71 5900 # Agregar source port para obtener bloque especifico

```
![[Pasted image 20240624131455.png]]


#### Port block allocation

Es muy similar al alojamiento por rangos, con la diferencia de que en lugar de calcularse los bloques de puertos se asignan manualmente
Cuando se asigna un puerto a un host se crea una entrada de log

### Central SNAT
==Objeto activado al momento de activar el central NAT==
El nombre de central SNAT deriva de que ahora el nateo se hará a todo el tráfico en base a reglas que pongamos (source/dest interface, source/dest address, protocolo y a veces source port). Facilitando así la administración de nateos grandes  
Las reglas se revisan de arriba a abajo y si no matchea ninguno no se hace nateo

### DNAT & VIP

==Objeto activado al momento de activar el central NAT==
De manera similar al SNAT central, ahora fortigate buscara tráfico coinicidente al VIP 
![[Pasted image 20240625224656.png]]


### Protocol  options
==Objects & policy > Protocol options==
Usado en especial para [[Antivirus]], [[Web filter]] y otros perfiles de seguridad
Provee de una forma granular de especificar tipos de tráfico por ciertos puertos, para así manejar apropiadamente el tráfico en la politica 

Protocol port mapping 
	- HTTP
	- SMTP
	- POP3
	- IMAP
	- FTP
	- NFTP
	- MAPI
	- DNS
	- CIFS

##### opciones
- Confort clients
	Esta opcion consiste en enviar un poco de datos al cliente en lo que se analiza un archivo de av
- Block oversized files


##### comandos
Configurar protocol-options
```
config firewall profile-protocol-options
	edit <profile-name>
	config <protocol-name>
	set options oversize
	set oversize-limit <integer>
	set oversize-log <integer>
```

