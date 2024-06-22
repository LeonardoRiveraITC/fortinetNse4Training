## ==Network ==

### Interfaces

##### Roles de interfaces
Las interfaces pueden tomar un rol dependiendo de su lugar en la topología de la red, este rol nos simplifica la configuración de la interfáz

Por ejemplo, si se habilita el rol de wan, no tiene sentido que esta interfáz funja como servidor DHCP

Entre los roles disponibles tenemos: [[LAN]], [[WAN]], [[DMZ]] y undefined. Undefined permitiendo hacer lo que queramos con la interfáz para edge cases

### Modos de operación

Nat [[Modos]], al ser de capa 3. Es obvio que se necesitara de una ip para operar. Para poder así ser visible ante capa 3 y routear tráfico

Podemos asignar una Ip estática, [[DHCP]] o [[PPPoE]] a una interfáz


### Interfaces y zonas
##### ==Network > Interfaces==

Recordemos que las interfaces son puertos fisicos enumerados en fortigate. Tenemos del port1 al puertoN dependiendo del modelo
![[Pasted image 20240619101952.png]]

Para facilitar la administración de las interfaces, especialmente en [[Politicas de Firewall]], podemos agrupar las interfaces en grupos logicos, llamados Zonas

Estas se usan para las [[Politicas de Firewall]], En donde siempre debe de haber una interfaz de ingreso y egreso y serán usadas a la hora de buscar reglas coincidientes para un paquete entrante (o el wildcard any en su defecto).

==Para agregar una interfaz a una zona debe de tener 0 referencias. Además de que una vez en una zona, no se puede referenciar individualmente==


Para activar el matcheo de any interface o de multiples interfaces, debemos de hacerlo [[Visibilidad]] en la gui
	![[Pasted image 20240620082513.png]]
