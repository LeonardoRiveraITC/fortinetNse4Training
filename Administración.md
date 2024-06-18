==System > Administration==

- Por medio de https, ssh o consola
- Se soportan otros protocolos como SNMP, pero solo en modo lectura


#### Crear un usuario admin
==Ejercicio realizado en [[Lab 1 fortigate introduction]]==
Create new > Administrator | REST API admin | SSO admin

- Usuario locales
	- User-pass
	- Hacer una contraseña robusta y Testear la contraseña ante ataques de fuerza bruta con johntheripper y l0pthcrack 
- Usuarios en servidor de auth remoto
- Usuarios autentificados por certificados emitidos por un CA interno


#### Permisos
A los usuarios se les asignan roles, de los cuales heredan permisos. Existen roles predefinidos o podemos crear custom

###### Perfiles default
- super_admin - acceso a todo el dispositivo
- prof_admin - acceso completo a su [[Vdom]] 

##### Timeout
Existe un timeout default para los usuarios de 5 minutos, se puede cambiar el tiempo por perfil de usuario con la opcion

```
config system accprofile
```


### Reiniciar contraseñas de admin
Para hacer esto se debe entrar al modo de mantenimiento

1. Hacer un hard reboot (Desconectando el power bank)
2. Durante los primeros 60 segundos se podra ingresar a la cuenta mantainer por medio de un cable consola
3. user: mantainer - pass: bcpb[serial-number]

Esta cuenta se puede deshabilitar en caso de no poder asegurar seguridad fisica

```
config sys global
	set admin-mantainer disable
end
```


### Trusted sources
==Ejercicio Realizado en [[Lab 3 - Configuración de cuentas de administración]]==
Podemos configurar cuentas de admin

Se pueden levantar direcciones ips confiadas las cuales tendran acceso a la cuenta de admin

El wildcard 0.0.0.0/0 funciona


### Acceso administrativo
![[Pasted image 20240618112802.png]]

Se pueden configurar cosas como los servicios y los puertos que se pueden usar para el acceso administrativo

- Password policy
Las reglas que necesitará tener una contraseña para ser considerada como valida
- longitud
- caracteres
- expiración
- reuso

Buenas prácticas de contraseñas, aplica a usuarios locales, ipsec

También podemos cambiar el timeout por usuario

###### Protocolos
Cómo ya se menciono, existe acceso a consola de admin por medio de telnet, ssh y web. Este acceso se puede deshabilitar por interfaz en [[Redes]]

Además existen protocolos de administración usados por [[Security fabric]]

- CAPWAP - fortigate telemetry, FORTIAP,FORTISWITCH,FortiExtender
- FMGAccess - Fortimanager, cuando maneja multiples fortigate
- RADIUS - paquetes de autenticación
- FTM - forti token
- LLDP - Layer Discovery Protocol, para descubrir dispositivos upstream



