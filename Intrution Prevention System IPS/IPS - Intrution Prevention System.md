Parte del [[UTM]]
==Usa [[Flow based inspection]]==
==A los perfiles se les llama sensores==

Es usado para detectar [[Anomalia]] y [[Exploit]]

==El motor de IPS tambien es responsable por la mayoria de operaciones de seguridad==
- [[Application control]]
- [[Antivirus]]
- [[Web filter]]
- [[Email filter]]

El motor de IPS es el motor del cuál fortigate detecta exploits en base de firmas y [[Protocol Decoder (Flow-based)]] y es responsable de la mayoria de analisis llevados a cabo en fortigate

## Uso 
Para aplicar un perfil IPS debemos configurarlo primero en 
==Security profiles > Intrusion prevention system ==

Después debemos aplicar el perfil en la politica de firewall deseada tal que:

![[Pasted image 20240613121623.png]]



### Modos
##### Fltro
Filtra por categorias de amenaza y protocolo

- critical
- high 
- medium
- low
- informational
Protocolo
- SSH
- FTP
- HTTP
- SMB
Applicaciones
- Skype
- Mysql
- MS
SO
- Linux
- Windows

##### Firmas
Filtra por las firmas especificadas, clasificación y threshold de nivel de amenaza
Usado en [[Lab 2 - Rate based]]

### Updates

La actualización de la base de datos de firmas incluye

- Se actualiza la base de datos de firmas para reconocer exploits
- Por su parte un protocolo no se actualiza a menos que cambie la especificación (MUY rara vez)
- El motor se actualiza con más frecuencia pero no es tan seguido
- ==Por su parte, la base de datos de ip botnets es parte de [[ISDB]], y la base de datos de dominios botnets es parte de AV==
- La frecuencia de las actualizaciones a partir de 7.0 se calcula en base a las licencias validas y el modelo con un intervalo de una hora
- Como todo producto, si caduca la licencia se podrá usar pero dejará de ser actualizado

## Bases de datos
==system>fortiguard==

Por default se tiene solo la base de datos la cual contiene los ataques más comunes

Por otra parte existe la base de datos extendida, la cual causa problemas de rendimiento y no esta disponible para equipos pequeños


## Lista de [[Firmas (Pendiente)]] 
==Security Profiles > Intrusion prevention==
En esta lista podemos ver la lista de firmas y acciones para las firmas detectadas, en su mayoria estas acciones son la correctas con dos edge cases

- El vendor libero un parche, por lo cual no tiene sentido gastar recursos escaneando esa firma
- Tienes una aplicación que puede disparar la firma en cuestión

## Configuración de sensores IPS
Se pueden agregar firmas de forma individual o por medio de un flitro

El filtro sirve de manera de englobar varias firmas al mismo tiempo
![[Pasted image 20240613084028.png]]

Una feature muy útil es el uso de rate-based signatures, que al exceder cierto limite de matches de firmas bloqueara al host en cuestión por cierto tiempo

Durante este periodo de tiempo, no se logeara tráfico del host*


# Forma de detección
Los sensores son la manera en la que se elige que firmas detectar, estas firmas de forma similar a las [[Politicas de Firewall]], se análizan  de arriba a abajo, y mientras menos y más arriba esten las frecuentes menos tiempo de computo llevara el sensor


## Excepciones 
Bajo de cada firma podemos poner excpeciones de ip fuente o destino, esto solo se puede hacer una por una

## Acciones

- Permitir 
- Monitorear
- Bloquear
- Reset (envia un tcp rst)
- Default (acciones default de cada firma)
- Cuarentena (Envia el host ip a cuarentena por x tiempo)

## Opciones
```
config ips sensor
	edit "cve"
	set comment "cve"
	config entries
		edit 1
			set cve "cve-2010-0177"
			set status enable
			set log-packet enable
			set action block
			next
		end
	next
end
```

Como se puede ver en este ejemplo, podemos agregar firmas basandose en su nombre de cve e inclusive por wildcard en numero de cve

## Proteccion de [[Botnets (pendiente)]]

- Es parte de la licencia ISDB 
- Se debe habilitar en el perfil IPS que exista sobre la politica deseada

## [[Logs/Logs|Logs]]
==Log & report > security events  > intrusion prevention==

