##### ==Log & Report==

Los logs son pieza fundamental de la ciberseguridad, ya que es através de estos que podemos hacer cosas como troubleshooting, o analizar anomalias. Los logs los podemos ver presentes a lo largo de fortigate en la mayoria de las opciones que hemos explorado, notablemente en las [[Politicas de Firewall]], que podemos habilitar logs para solo eventos de seguridad o para todo el trafico coincidiente

Los logs son almacenados en un archivo, que se almacena de forma local o se envia a un dispositivo externo, como fortianalyzer

==Es imprortante tener un buen NTP para tiempo preciso de logs]]

==Los logs son muchos en volumenes grandes, por lo que intentar logear cada evento puede tener un impacto en el performance==
Para determinar la frecuencia de logeo estadistico se hace desde CLI con 

```
config system global
	set sys-perf-log-interval <0-15>
```
Donde 1-15 son minutos, y 0 para deshabilitar


### Tipos de logs
Visto en [[Lab 4 - Viendo logs en fortigate]]
Por la misma razón de que existen logs para todo, estos se dividen en tres categorias

- Traffic: Eventos relacionados al flujo de informacion, fortinet graba la petición y de ser necesario la respuesta
	- Forward
	- Local
	- Sniffer
- Evento: Eventos relacionados al sistema
	- Endpoint
	- [[High Availabilty]]
	- General system - ([[Configuración]])
	- User - [[Firewall authentication]] y admins 
	- Router 
	- [[VPN]]
	- [[SDWAN]]
	- WIFI
	- CIFS
	- [[Security rating]] ([[IPS]])
	- SDN Connectors ([[Security fabric]])
- Seguridad: Eventos relacionados a incidentes de seguridad
	- [[Application control]]
	- [[AV]]
	- [[DNS]] query
	- [[File filter (pendiente)]]
	- [[Web filter]]
	- [[IPS - Intrution Prevention System]]
	- Anomalias
	- [[SSL inspection]]
		- [[SSH]]
		###### Estos a su vez cuentan con un nivel de seguridad
		0. Emergencia - sistema inestable
		1. Alerta - Accion inmediata
		2. Critico - funcionalidad afectada
		3. Error - Fallas afectando ciertos sectores
		4. Warning - posiblemente afectada
		5. Notif - Información de eventos
		6. Information - Info
		7. Debug - Diagnostico

### Estructuras de un mensaje de log

##### Header
Comun en la mayoria de los logs, contiene información del log en si y de su contenido
- date
- time
- type
- subtype
- eventtype
- level
- vd
![[Pasted image 20240702141244.png]]

##### Body
Este campo depende del tipo determinado en el header, pero en general contiene las razones del log y las acciones tomadas por fortigate
![[Pasted image 20240702141502.png]]
En este caso vemos que es el log de una politica de firewall debido a sus campos
- policyid
- sessionid
- user
- srcip
- srcport
- srcintf
- dstip
...

Y podemos ver la acción, bloqueo, y la razón en msg



### Almacenamiento


#### Logs
Es común que los dispositivos de gama media tengan un disco duro o más, en caso de no contar con uno, o que se este usando para otra cosa como WAN optimization, podemos enviar los logs a un almacenamiento remoto como [[FortiAnalyzer]]

El sistema reserva un 25% de disco para rendimiento y usa el sobrante para logs
podemos ver esto en cli con

```
diagnose sys logdisk usage
```
##### Local
==Por default, los logs más viejos a 7 dias se eliminan==
Esto se puede cambiar con 
```
config log disk setting
	set maximum-log-age <dias>
```



Puede o no venir activado por default dependiendo del modelo, en caso de querer activarlo esto se hace en
==Log & report > Log settings ==

O en cli 
```
config log disk setting
	set status enable
```

##### Remoto
Podemos almacenar logs en los siguientes dispositivos
- Syslog - Un servidor syslog
- [[FortiAnalyzer]] - [[SIEM - Security information and event managment]] y Dispositivo dedicado a alamacenar y analizar logs
- [[FortiManager]] - Dispositivo dedicado a controlar dispositivos de red
- [[FortiCloud]] - Solución en la nube con tier gratis

==El puerto default es UDP 514 y compresión LZ4 para CLI y TCP para GUI (reliable)==

##### Funcionamiento
Transmisión de logs ==reliable== consiste en enviar los logs en una cola con un seq_no, un numero secuencial para dar seguimiento al ultimo log enviado, una vez se confirma se eliminan de la cola los seq_no hasta el ultimo recibido

==Log & report > Log settings==
Configurar envio de logs a [[FortiAnalyzer]] o [[FortiManager]]
![[Pasted image 20240703091842.png]]

Podemos enviar logs hasta a 3 [[FortiManager]] o [[FortiAnalyzer]] configurando en cli
Las opciones de ambos son similares
```
config log [fortianalyzer|fortianalyzer-cloud|fortianalyzer2|fortianalyzer3] setting
	set status enable
	set server <server_IP>
	set serial "serial"
	set enc-algorithm <high-medium|high|low>
	set upload-option <real-time|5-minute|1-minute>
	set reliable enable
	end
```

Para enviar logs a syslog 
```
config log syslogd<1-4> setting
	set status enable
	set server <syslog_ip>
	set format [default | csv | cef]
end
```
==reliable permite cifrar usando SSL OFTP==
##### Conectar a fortiguard con cli 
```
config log fortiguard setting
	set status enable
	set source-ip
	set upload-option <realtime|1-time|5-time>
	set enc-algorithm <high-medium|high|low>
```

#### Configurar filtros de logs
Podemos configurar filtros de logs que serviran para indicar quien recibira que
```
config log [fortianalyzer<2-3|cloud> | syslogd <1-4> ] filter
	set severity critical
	set local-traffic enable
```
Para filtros tenemos
- Nivel de severidad
- Habilitar/deshabilitar
	- trafico local
	- trafico forward
	- multicast  traffic
	- sniffer traffic
	- anomalias
	- voip
	- ztna-traffic
	- GTP
- Filter [string]
- Filter type [include | exclude]
#### Disponibilidad
Si por alguna razón el fortianalyzer remoto no esta disponible, el fortigate almacenara logs en un buffer hasta que este se llene, y luego borrara logs de más viejo a más nuevo. Y subira los logs en cache una vez que este disponible de nuevo

Este proceso esta a cargo de miglogd

Podemos ver el estado del fortianalyzer remoto con

diagnose test application milogd 6 
![[Pasted image 20240703114650.png]]


#### VDOMS
==Se puede configurar por vdom hasta== 
- ==3 fortianalyzers==
- ==4 syslog servers==
==Y de igual manera 3 y 4 de forma global==

###### Global
```
config global
	config log fortianalyzer setting
	set status enable
	set server 10.0.0.1
	end
```

##### Vdom
```
config vdom
edit Training
	config log setting
	set faz-override enable
	set syslog-override enable
```


### [[Politicas de Firewall]]
Para que se logeen eventos en politicas de firewall se deben tener uno o más perfiles de seguridad UTM habilitados, o logear TODO el tráfico permitido (consume recursos)

==Los logs para procesadores NP6 no logean estatisticas de trafico, si se desea habilitar se baja el performance==
==Procesadores NP7 tienen una mejoria en logs de red estadisticos==

### Esconder nombres de usuario en logs
En algunos paises, por ley se debe anonimizar nombres de usuario

```
config log setting
	set user-anonymize enable
	end
```

### Filltros de busqueda
Podemos ver logs en cli y en gui
En cli para facilitar la busqeuda podemos aplicar filtros con 

```
execute log filter
```
Y mostrar resultados con filtros aplicados con
```
execute log display
```

==Solo se muestran logs locales, al igual que en la gui==
==El máximo de logs a mostrar es 1000==

### Alertas
==System>Settings==

Usado en [[Lab 3 - Monitoreando alertas en email]]
Las alertas son importantes para estar al tanto de eventos de seguridad por ==correo==
Para esto se requiere de un servidor SMTP, Fortigate tiene uno por default pero lo mejor es levantar uno

![[Pasted image 20240703160919.png]]

```
config alertmail setting
	set username "fortigate@fortigate.com"
	set mailtol "admin@training.com" #Hasta 3 recipients configurables
	set filter-mode category | threshold
	set email-interval 1
	set IPS-logs enable
	set HA-logs enable
	set antivirus-logs enable
	set webfilter-logs enable
	set log-disk-usage-warning enable
end
```

### Backups
Podemos hacer backups de los logs desde gui o cli

Podemos usar: 
- TFTP
- USB
- FTP
O descargarlos desde gui

```
execute backup disk allogs [ftp|tftp|usb]
```

```
execute backup disk log [ftp|tftp|usb] <log-type>
```

### Log rolling
Se pueden rotar logs, similar a comprimir para reducir su espacio
==Por default es cuando un archivo llega a los 20 mb==
###### ==maximo de 100 mb==
###### ==schedule diario o semanal==


Podemos enviar logs rolleados a un servidor ftp o calendarizar una rotacióin 
```
config log disk setting
	set max-log-file-size <1-100>
	set roll-schedule [daily|weekly]
	set roll-time [hh:mm]
	set upload [enable|disbale]
	set upload-destination [FTP]
	set uploadip [ipv4]
	set uploadport [port]
	set sourceip [ipv4 ip]
	set uploaduser [user]
	set uploadpass [pass]
	set uploaddir [remote dir]
	set uploadtype [log type]
	set upload-delete-files [enable|disable] 
```




