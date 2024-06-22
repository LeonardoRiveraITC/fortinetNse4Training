### ==Firewall & Objects > Firewall Policy==

## ¿Qué son?
Las politicas de firewall dictan cómo se maneja un flujo de paquetes al llegar a fortigate. Las acciones más básicas y de un firewall tradicional son: ==Deny y Allow==
en base a direcciones IP

### ==Fortigate es un stateful firewall, lo que quiere decir que funciona en capa 3 y 4 al funcionar dando seguimiento a sesiones. Esto quiere decir que para cada outbound request, debe de haber un inbound response, la cual el firewall deduce bajo las reglas ya creadas para la outbound request. Haciendo uso de la sesión creada para la petición puede deducir las reglas, además de cosas como análisis del paquete. Esto es especialmente útil para políticas de salida a internet, ya que solo necesitaremos hacer una politica de salida, porque al estar la dirección nateada a la interfaz, nunca iniciara una sesión por fuera de todas maneras==

#### ==*La acción default para todo paquete entrante es denegar, para permitir la entrada de un paquete, y acciones subsecuentes como análisis de [[AV]] se debe de crear una politica de firewall*==

#### ==*Las politicas de fortigate se evaluan de arriba a abajo, lo que significa que mientras más alto en la lista, más prioridad tendrá. Una vez que se encuentre una politica coincidiente, fortigate la aplicara y por subsecuente politicas de menos prioridad no tendran efecto*==

*Recordemos que las politicas tienen un id que sirve a fortinet para identificar politicas, este id no tiene efecto en el orden en que se evaluan las politicas. Si no que es el orden de la tabla*

Existen varios tipos de politicas, el más común siendo politicas de firewall (IPv4,IPv6)

Otros tipos de politicas se deben configurar bajo visibilidad o interfaz:
- Virtual wire pair policy - para pares de cables virtuales
- Proxy policy 
- Multicast policy - el flujo de paquetes multicast entre interfaces
- local-in-policy - Controla el tráfico entrante a una interfáz y puede ser usado para bloquear acceso administrativo
- DoS policy - [[Denial of service]]


### Politicas de firewall
###### ==*Al igual que los [[objetos]] cuentan con un [[UUID(pendiente)]] que los identifica a lo largo de fortigate y con conectores de [[Security fabric]]==. Este uuid es First come First served*

Las politicas de firewall son el punto central de nuestros [[objetos]]. Es aqui donde se juntan para orquestrar la seguridad de la red. Esto nos permite dirigir acciones a tomar cuando se encuentra con la el escenario especificado por los objetos, como permitir o denegar tráfico, activar perfiles de [[UTM]] (AV, IPS, etc). Entre otras cosas como activar el nateo

Las politicas de firewall deben de tener un nombre para su identificación y administración fácil. Es posible deshabilitar este requisito con una feature de [[Visibilidad]] oculta. En caso de usar CLI, este requisito no aplica pero si editamos una politica sin nombre en GUI nos hara ponerla.

```
config firewall policy
edit 1
	set name "Training"
	set uuid [uuid]
```
*Para agregar uuids personalizados en GUI debemos activar el feature en [[Visibilidad]]*
###### Estado en tiempo real
Al entrar a una politica en GUI, podemos ver estadisticas en tiempo real que incluyen:
- ID
- Ultimo uso
- Primer uso
- Sesiones activas
- Hit count
- Bytes totales
- Ancho de banda actual
- Grafo de uso
	![[Pasted image 20240621173026.png]]


## Tabla de politicas
Es la primera vista de ==Objects & policy== aqui es donde vemos de inicio la tabla de las politicas de firewall, donde se evaulan paquetes de arriba a abajo.

Por default en esta versión, se tiene habilitada la vista consolidada, lo que significa que la tabla de politicas aplicara a ipv4 e ipv6 indiscriminadamente

Podemos usar los mismos objetos para interfaces source y destino, igualmente servicios y schedules. PERO, para crear una politica de ipv4 e ipv6 debemos de incluir un SRC y un DST para ambos tipos de direcciones. No podemos incluir uno de cada uno 
- Opcionalmente podemos ver la lista de politicas por pares o en orden, los pares son pares de interfaces src -> dst. Mientras que en orden muestra el orden en que se evaluan las politicas


### Uso
Visto en [[Lab 1. Creando objetos y politicas]]
Las politicas de firewall funcionan haciendo uso de los [[objetos]] creados

Para crear una politica de firewall se requieren de:
- Interfaces entrantes y salientes 
- Source -  [[objetos]] de tipo address
- Destination - [[objetos]] de tipo address
- Servicio - [[objetos]] de tipo servicio
- Schedule - [[objetos]] de tipo schedule

- Accion - Permitir o denegar el tráfico. Si se habilita, se habilitan también los [[objetos]] de perfil de seguridad. Los cuales pueden hacer analisis de amenazas sobre el paquete

==*Denegar dropea el paquete*==

#### Coincidir por source
El source es de donde se origina la petición. Este puede ser un:
[[objetos]] de tipo dirección

Opcionalmente, podemos usar los datos de usuarios o grupos de usuarios como source
- [[LDAP (pendiente)]],[[Radius (Pendiente)]],[[Active directory (pendiente)]]

==Se pueden combinar usuario y direcciones/servicios. Pero direcciones y servicios son mutuamente exluyentes==
###### Identificación con usuarios
Cuando se usa usuario como source, se debe primero validar al usuario, el usuario puede ser:
- local - usuario y contraseña alojados en fortigate
- radius,ldap,av,radius - el usuario es autenticado por el server AAA como el usuario remoto y se actualiza a fortigate de la autenticacion
- FSSO - single sign on de fortinet


#### Coincidir por destino
El destino es igual al source, con la diferencia de que no puedes usar usuarios por obvias razones de direccionamiento
De igual manera podemos usar cualquier [[objetos]] de tipo dirección

Opcionalmente, podemos usar los datos de usuarios o grupos de usuarios como source
- [[LDAP (pendiente)]],[[Radius (Pendiente)]],[[Active directory (pendiente)]]

==Se pueden combinar usuario y direcciones/servicios. Pero direcciones y servicios son mutuamente exluyentes==
##### Identificación con usuarios
Cuando se usa usuario como source, se debe primero validar al usuario, el usuario puede ser:
- local - usuario y contraseña alojados en fortigate
- radius,ldap,av,radius - el usuario es autenticado por el server AAA como el usuario remoto y se actualiza a fortigate de la autenticacion
- FSSO - single sign on de fortinet


#### Coincidir por servicio
###### ==Policy & objects > Services==

Los servicios son [[objetos]], que reflejan un servicio de manera Protocolo de transmision/puerto

### Logging

Para las politicas de firewall existen dos situaciones de logeo:
- Allow
	- En este caso se puede logear todo ó solo los eventos de seguridad detectados por los perfiles de seguridad. Al logear todo debemos tener cuidado con el performance
	- Podemos habilitar logear desde que se inicia una sesión (no disponible sin storage interno)
	- Podemos habilitar capturar paquetes
- Deny
	- En el caso de deny solo  podemos activar si queremos logear el tráfico delincuente.

En el caso de que se detecte tŕafico bloqueado o que se disparen los perfiles de seguridad. Fortigate inmediatamente logeara la violación . 


==Para guardar tiempo de cpu, podemos habilitar una tabla de sesiones bloqueada. fortigate tendra esta sesión bloqueada durante un tiempo default de 30 segundos, donde los cuales cualquier paquete es dropeado==.
La tabla se habilita con 

```
config system setting 
	ses-dennied-traffic [enable|disable]
end
```

Este tiempo se puede cambiar en cli con 
```
config system global
	set block-session-timer [1-300]
end
```




