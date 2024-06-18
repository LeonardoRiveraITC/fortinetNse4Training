## ==Network==

### Interfaces

##### Roles de interfaces
Las interfaces pueden tomar un rol dependiendo de su lugar en la topología de la red, este rol nos simplifica la configuración de la interfáz

Por ejemplo, si se habilita el rol de wan, no tiene sentido que esta interfáz funja como servidor DHCP

Entre los roles disponibles tenemos: [[LAN]], [[WAN]], [[DMZ]] y undefined. Undefined permitiendo hacer lo que queramos con la interfáz para edge cases

### Modos de operación

Nat [[Modos]], al ser de capa 3. Es obvio que se necesitara de una ip para operar. Para poder así ser visible ante capa 3 y routear tráfico

Podemos asignar una Ip estática, [[DHCP]] o [[PPPoE]] a una interfáz


