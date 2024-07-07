#### ==Security profiles > Web filter==
==Para que web filtering funcione se debe de tener una licencia de  fortiguard activa==

El web filter es el filtrado de páginas web
Entre otras razones para 
- Aumentar productividad
- Prevenir congestiones de red
- Reducir acceso a sitios maliciosos
- Evitar acceso a sitios ilegales

### Funcionamiento
![[Pasted image 20240704121536.png]]
El bloqueo web entra cuando se obtiene tráfico del dominio HTTP

### Comportamiento en modos de operacion
Ambos pueden filtrar sitios web en base a categorias y urls, pero la manipulación de contenidos es exclusiva del [[Proxy based inspection]]

##### [[Proxy based inspection]] 
- [[Categorias]] 
	- Categorias Locales
	- Categorias Remotas
		- Bloquear
		- Permitir
		- Monitorear
		- Advertir
		- Autenticar
		
- Engines de busqueda
- [[Video filtering]]
- [[Web content filtering]]
- Opciones de proxy
- [[URL Filtering]]

##### [[Flow based inspection]] (profile based)
-  [[Categorias]]
	- Categorias Locales
	- Categorias Remotas
		- Bloquear
		- Permitir
		- Monitorear
		- Advertir
		- Autenticar
- [[URL Filtering]]
- [[Web content filtering]]
##### Policy mode ([[NGFW Mode]])
-  [[Categorias]]
	- Categorias Locales
	- Categorias Remotas
		- Bloquear
		- Permitir
- [[URL Filtering]]
- [[Web content filtering]]

![[Pasted image 20240704132058.png]]


### Search engine
==Proxy based feature==
==Requiere de FULL [[SSL inspection]]==
==Security profiles > Web Filter > Search engine==
Esta opcion se usa para habilitar la busqueda segura en los buscadores, al incluir el parametro &safe=active (google)

### Orden de evaluación

==Si la accion es exempt en [[URL Filtering]], se saltara los demás filtros==

[[URL Filtering]] > [[Categorias]] > Otros filtros

### Cache 
==System > Fortiguard > web filter cache==
==Por default se da un rating error si la peticion falla tras 15 segundo, se puede configurar a 30==

Fortigate puede guardar registros de fortiguard para reducir la carga en la red. El tiempo que se guardan estos registros estan determinados por el TTL

Por default se usa el puerto 443 para conectar a fortigaurd o fortimanager, tambien se puede usar 8888 o 53, pero se deben configurar apropiadamente


### Comandos
Mostrar estado de webfilter
```
get webfilter status
```

Mostrar categorias

```
diagnose debug rating
```



