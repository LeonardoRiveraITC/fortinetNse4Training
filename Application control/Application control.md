
El application control es un perfil de seguridad aplicable a las politicas de firewall y parte del [[UTM]]

==Detectar aplicaciones tales como facebook, skype, p2p, proxy==

==Funciona con el [[IPS - Intrution Prevention System]] engine==

==Usa escaneo [[Flow based inspection]]==

==Requiere de [[SSL inspection]] para inspeccionar tráfico cifrado==
La manera en que lo logra es al detectar patrones correspondientes a paquetes de aplicaciones y protocolos usados por estas, es por eso que es capaz de detectar incluso si se pasa a traves de un proxy

##### ==Orden de escaneo ==
Application and filter overrides > Categories

De igual manera, application control ocurre primero que [[Web filter]], por lo que incluso si se permite una aplicación en application control puede que se bloquee despues por este

Lo mismo ocurre con acciones de [[Web filter]], puede que se exente a una url, pero esto no tomara efecto para application control, por lo que application control le bloqueara

### P2P
A diferencia de las aplicaciones tradicionales, cliente-servidor. El p2p esta diseñado para funcionar por medio de puertos aleatorios, pinholes y uso de protocolos de cifrado, eso además de que en P2P se tienen multiples nodos distribuidos a través de muchos puntos

Se pueden buscar patrones para aplicaciones P2P


![[Pasted image 20240706200646.png]]

Estas aplicaciones pueden consumir mucho ancho de banda


### Firmas
==Se requiere de una suscripcion a servicios de fortiguard para poder actualizar la base de datos de firmas de aplicaciones (diferente a la base de datos IPS)==

Podemos ver la lista de firmas en fortiguard.com junto a las categorias y calificaciones asignadas por fortinet

==Para identificar aplicaciones especificas se requiere de [[Inspeccion de certificados]]==


###### Presedencia

La precedencia de firmas es de la siguiente manera
Social Media > Facebook/Linkedin > Facebook_Chat

Audio / Video > Youtube > Video_play-

Dentro de las aplicaciones puede haber acciones especificas, brindando control granular sobre aplicaciones y acciones en lugar de solo categorias



![[Pasted image 20240706201156.png]]


### Filtros

Se pueden aplicar overrides de aplicaciones para cambiar la categoria de una aplicación

De igual manera se pueden aplicar filtros para encapsular más de una aplicación como puede ser el nivel de amenaza

- Categorias
	- Se agrupan aplicaciones por similaridad - ej: redes sociales, acceso remoto
- Overrides
	- Override de aplicaciones
- Override de filtro
	- Filtros customizables para cuando no existe una categoria que englobe lo que buscamos, puede bloquear por popularidad hasta servicios de aplicaciones

==Las aplicaciones no conocidas se clasifican como unknown_applications==



##### Opciones extra

- Allow and log DNS traffic
- QUIC
	- Protocolo de google que usa UDP para tráfico web, esta opcion al permitir detecta y logea el trafico QUIC, bloquearlo fuerza a QUIC a usar [[TLS - Transport layer security]], y bloquea el trafico quic, esta es la accion default
- Mensajes de reemplazo
	- Para HTTP/HTTPS muestra un mensaje conveniente al usuario, para otros protocolos solo dropea los paquetes y reinicia la conexión

### Protocol enforcment
Permite enforcar protocolos en puertos especificos y si se llega tráfico de algún protocolo por otro puerto, este tomara la accion configurada en el perfil 

Pasa lo mismo si por el puerto enforcado se detecta trafico de algun protocolo que no sea el especificado

![[Pasted image 20240706202442.png]] ![[Pasted image 20240706202449.png]]

### Acciones
Para cada filtro en el perfil se pueden aplicar las siguientes acciones: 
	- Allow
	- Monitor - Genera mensaje de log
	- Block - Dropea el trafico detectado y genera log
	- Quarintine - Bloquea el trafico atacante y genera un mensaje de log después de pasar el tiempo de bloqueo


### Pagina de bloqueo
- Categoria
- Website host y url
- User name
- Group name
- Policy UUID

### [[NGFW Mode]] mode policy 
==Si se habilita un perfil de [[URL Filtering]], esta politica se vera limitada a tecnologias de navegador==
==Solo disponible en flow mode==

### Mejores practicas
- Aplicar solo a politicas que lo requieran
- No aplicar a trafico interno
- Habilitar [[SSL inspection]] deep inspection
- Usar aceleración de hardware para signature matching

### Troubleshooting
Se requiere una conexion estable a update.fortiguard.net puerto TCP/443

Actualiza la bd de firmas
```
execute update now
```




