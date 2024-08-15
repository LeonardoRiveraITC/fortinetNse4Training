==policy & objects > IPv4 DOS Policy==
Usado en [[Lab 3 - Mitigar ddos]]

Las politicas se aplican automaticamente al DOM
Un ataque de denegación de servicio consiste en sobrecargar el objetivo de x o y manera de forma que no pueda responder a peticiones de clientes legitimos

- Un ejemplo de esto son los ataques DDOS que buscan gastar los recursos de ancho de banda de el objetivo
- Otro ejemplo es el uso de payloads creadas para poner en hacke el sistema, como una petición in-procesable por el software que dejará colgado el sistema 
![[Pasted image 20240613122645.png]]

## DOS por recursos
Para contrarestar el primer ejemplo
Que consiste en dejar sin recursos de procesamiento, ram, ancho de banda y puertos, se contraresta de la siguiente manera

El DOS filtering toma curso temprano en el procesamiento de un paquete, ya que toma lugar a nivel de kernel


## Tipos de ataque

- TCP SYN/FLOOD [[TCP SYN (pendiente)]]
	El atacante envia miles de conexiones TCP incompletas lo que ocaciona que el target se agote de puertos para conectar. Esto llena su tabla de conexiones en RAM
	![[Pasted image 20240613131907.png]]

- TCP sweep 
		Un barrido es enviar peticiones a un rango o segmento para descubrir hosts en la red
		![[Pasted image 20240613131806.png]]

- TCP [[Esceneo de puertos(pendiente)]]
		El atacante envía solicitudes de conexion a distintos puertos del host atacado
		Despues se mapean los servicios descubiertos en base a información obtenida para proseguir el ataque

- DDOS 
		La denegación de servicio distribuida es más compleja, ya que es una red distribuida de tráfico entrante al host, lo que lo hace más díficil de detectar y bloquear
## Uso
![[Pasted image 20240613122823.png]]

Cuando el número limite de conexiones ocurre, toma accion la politica (conexiones half-open, hosts,source address, dest address, puertos, etc)


##### Configuración
- Se pueden establecer limites para las distintas anomalias configurables
- Flood
	- Número del mismo tipo de tráfico
- Sweep/scan
	- Detecta intentos de sweep
- Source
	- Volumen de tráfico de una misma sesión
- Dest
	- Volumen de tráfico hacía un mismo host

Hay que tener cuidado, ya que en una red de alto uso o en horas pico podríamos bloquear accidentalmente el servicio

Podemos logear el tráfico hasta obtener métricas normales y luego aplicarlas a las reglas de DOS

