## Backups
Aplicado en la práctica [[Lab 1 fortigate introduction]]
En fortigate la configuración se guarda como una archivo de texto plano para su fácil administración y recuperación

Esta configuración se basa en almacenar los comandos necesarios para llegar al estado actual del sistema, podemos almacenarla en yaml o formato de fortinet

Podemos elegir cifrar la configuración, así como ofuscar información sensible como contraseñas


Para acceder al menu de configuración vamos al menu de usuario

menu > configuration > Backup | restore | revisions | scripts

- ###### Las configuraciónes se pueden almacenar por [[Vdom]]

#### Información del backup
[[Lab 2 Generando backups de configuración]]
El header de un archivo de backup siempre tendrá información sobre. ==El modelo, el firmware y el build== de los que se generó esa configuración. Esto ayuda a fortigate a saber lo que requiere para aplicar la configuración. En el caso de archivos cifrados, además de la contraseña se requiere del mismo modelo y build de fortigate.

#### Opcionalmente podemos usar YAML
Esto se puede hacer solo por CLI

```
execute backup yaml-config {ftp|tftp} <filename> server <username> 
<password>
```

```
execute restore yaml-config {ftp|tftp} <filename> server <username> 
<password>
```

##### Tips

- `ris:Checkbox` Siempre hay que respaldar, aunque se trate de un cambio menor, ya que no hay forma de deshacer configuraciones. 

- `ris:Checkbox` Aunque no se use el cifrado, las contraseñas almacenadas están hasheadas y ofuscadas

- `ris:Knife` Y el cifrado es un arma de doble filo, si bien mantiene seguras tus credenciales e información sensbile, hace imposible el troubleshooting ya que para decifrarlo se requiere de la contraseña Y un fortigate del mismo modelo
### Configuración inicial
- Modo NAT por default
- Para entrar a la consola de administrador
	- IP default 192.168.1.99 
	-  Por HTTPS y SSH, ping habilitado tambien ó consola por medio de puerto consola o MGMT usb
	- Modelos de entrada
		- Conectar al puerto 1 del switch de fortigate, estos ya tienen un servidor [[DHCP]], por lo que la asignación de la ip es automatica
		- Entrar a  https://192.168.1.99
	- Modelos mid a alto: Conexión por medio de interfaz MGMT
	- user: admin, pass: espacio-blanco

Lo mejor es deshabilitar Telnet

### Gateway
[[Redes]] >  Static routes
Para integrar a la red, fortigate debe conocer el default gateway. Este se conoce automaticamente si se trata de [[DHCP]] o pppoe

### *Hay que asegurarse de que exista una ruta que coincida a todas las ips (0.0.0.0/0) y las envie a internet, o al siguiente router *
![[Pasted image 20240618145619.png]]


## Actualizaciones

#### Firmware

A partir de la versión 7, esto se maneja desde [[Security fabric]]. Alternativamente podemos ver la version con 
```
get system status
```

Para actualizar debemos seleccionar el dispositivo > upgrade en security fabirc. Y elegir el archivo o imagenes directamente de fortiguard

Debemos asegurarnos de seguir un ==upgrade path== para asegurarnos de que no haya breaking changes

