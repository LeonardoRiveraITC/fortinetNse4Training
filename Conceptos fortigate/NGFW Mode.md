==System > Settings > NGFW Mode==
Para cambiar de modo una vez en producción deberemos de borrar todas las politicas en lugar

Se aplica de forma global o por VDOM

### Policy based
Permite crear Application control y Web filtering directamente en una politica de  firewall
- ==Se debe de habilitar [[Central NAT]] para poder activar este modo, para así tener politicas de nateo donde pueda salir el tráfico==
- ==Solo soporta [[Flow based inspection]]==
Se recomienda habilitar full [[SSL inspection]] en este modo para poder hacer filtrado propio sobre las aplicaciones y web filter
De igual manera se trabaja con el concepto de Security policy, que vendria a ser parecido a las [[Politicas de Firewall]]

##### Funcionamiento de filtrado [[Application control]] en policy based 

1. Por default se permiten todas las sesiones hasta que se identifique la aplicacion de la que provienen
	- En este momento solo se inspeccionan headers ipv4
	- Agrega una flag may_dirty a la sesion
2. Se envian las sesiones al motor IPS
	- Identifica la aplicación y agrega una nueva flag
		- dirty - se envia al kernel para reevaluar la sesión
		- app_valid - la aplicacion es valida
3. Esta vez se usan headers de capa 4 y capa 7 y se toma la accion configurada


### Profile based
Es el modo estudiado en la mayoria de esta guia, donde se debe crear un perfil/[[objetos]] para aplicarlo a una politica de firewall

- ==Soporta [[Flow based inspection]] y [[Proxy based inspection]]==

