==Policy & Objects>Antivirus==
==Requiere subscripcion a fortiguard antivirus==
Parte del [[UTM]]
Para que el antivirus tome acción, como todo en fortigate, se debe primero habilitar en la politica de firewall


Además de activarlo en la politica, hay que activar el ssl inspection para verdaderamente tomar provecho de esta caracteristica

Requisitos:

1. Licensia de AV
2. [[SSL inspection]]
3. Activar en politica de firewall
		Policy & Objects>Antivirus

El uso de [[SSL inspection]] es para poder inspeccionar los contenidos con el antivirus


Imagen ilustrativa:
![[Pasted image 20240611160429.png]]


### Funcionamiento
El AV funciona con 3 métodos de detección

- Firmas: Compara con base de datos de firmas por hashes de virus conocidos
	- Se puede complementar la bd con firmas de [[Fortisandbox]] 
- Grayware : Bloquea software no deseado
- AI: Usa un modelo entrenado en fortigate para detectar virus de dia zero, todos los virus detectados con ai se identifican con W32.AI.Pallas.Suspicious y esta habilitado por default. Aunque consuma recursos vale la pena si se es target de ataques

El orden de escaneo es tal como se presenta: Firmas > Grayware > AI


### Versiones de bases de datos de firmas
- Extendida
	- default
	- Disponible para todos los modelos
	- Contiene virus activos y algunos adicionales
- Extrema
	- Solo disponible en high end devices

##### Bases de datos extras
- virus outbreak prevention
	==Se requiere de una licencia ZHVO zero hour virus outbreak==
	1. Scanunit daemon hashea el archivo
	2. urlfilter daemon revisa su cache por hashes 
	3. Se envia a fortiguard para su analisis 
	4. En lo que se reporta el archivo se espera o se deja morir la peticion
- Base de datos de fortisandbox

### Comportamiento
[[Proxy based inspection]]

Dos modos 
- default - stream based scanning
	- Mejora el escaneo de contenidos nesteados sin bufferear al analizarlos en la marcha. Extrae y analiza. Optimizado para uso en memoria
	- Soporta FTP,SFTP,SCP
- legacy
		Bufferea todo el archivo para analizar
- Ambos usan la bd de firmas

1. En este modo se espera a recibir todo el archivo antes de analizarlo
2. Se guarda en un buffer mientras se esperan todos los contenidos
	- Es por esto que puede causar latencia
3. Si el archivo es más grande que el buffer solo se pueden tomar dos acciones
	1. Block
	2. Allow
		- En cualquier caso se crea un log
4. Una vez que se recibe el archivo entero, se empieza a analizar por el motor AV, si se detecta un virus se envia un bloque de reemplazo
5. ![[Pasted image 20240707010224.png]]


[[Flow based inspection]]
==CDR - content disarming, MAPI, SSH, MS office docs==
- Modo hibrido con [[Proxy based inspection]]
	1. A medida que recibe los paquetes se inspeccionan
	2. Se guarda una copia y se forwardea, esto continua hasta el ultimo paquete
	3. Al recibir el ultimo paquete, no se forwardea y se envia a IPS para desarmar y enviar al AV
	4. 1.Si al recibir el ultimo paquete se detecta un virus, fortigate reinicia la conexión y no envia el ultimo paquete, tambien guarda la url de origen por si se intenta repetir el ataque
	5. 2. Si se detecta un virus en el escaneo de paquetes antes del final se envia un bloque de reemplazo inmediatamente 
	![[Pasted image 20240707005435.png]]


### Opciones
- APT protection options
	- Tratar windows ejecutables en attachments de mail como maliciosos
		- ==Esta habilitado por default ==
	- Enviar archivos a analizar a fortisandbox
- Virus outbreak prevention
- External malware block list
	- [[Security fabric]] Connector que soporta hashes md5,sha1,sha256

### Comandos
Estatus de actualizaciones
```
diagnose autoupdate status
```
Estatus de base de datos
```
diagnose autoupdate version
```
Establecer BD extrema
```
config antivirus settings
	set use-extreme-db
end
```
