##### ==User & authtentication==

La autenticación de firewall es necesaria debido a que la autenticación tradicional no da para los NGFW, ya que no se puede saber quien esta detrás meramente por Ip

==Para todo usuario remoto debe existir su contraparte local, la creación del objeto en fortigate, o para su sencillez; un grupo== ya que tratamos con objetos que obviamente serán usados en [[Politicas de Firewall]], en este caso de source


==De forma general se habilita el uso de dns de forma libre a los usuarios, ya que es un servicio base y es probable que sea usado para comunicar al servidor de autenticación en primer lugar==

![[Pasted image 20240702132035.png]]
De forma similar, es común incluir los protocolos HTTP,HTTPS y FTP, ya que son usados para las formas de autenticación activas
### Autenticación local
La más simple de todas, se alamacena en el fortigate de manera lcoal y puedes habilitar 2fa por medio de sms, token, email

### Autenticación de servidor externo (RADIUS,LDAP,AD,POP3,TACACS)
Cuando se usa esta opcion tenemos dos opciones:
- Agregar un usuario local con autenticación externa
- Agregar el servidor a un grupo de usuarios, donde todos los usuarios serán parte de ese servidor
- POP3 solo se puede configurar por CLI
- POP3 es el unico que requiere del correo completo, los demás dependera de la configuración depender de 

##### Creando grupos de usuarios como servidores externos
###### Usando usuarios
Esto se hace cuando se quiere agregar 2fa al usuario

1. Crear usuario local
2. Agregar servidor remoto
![[Pasted image 20240627075228.png]]

###### Usando grupos
1. Crear grupo
2. Crear grupo eligiendo como objetivo el servidor remoto
![[Pasted image 20240627080005.png]]
- Si se usa algo como LDAP podemos controlar el acceso a grupos especificos 

#### [[LDAP]]

La configuración de LDAP dependerá de la configuración del servidor externo
- Common name identifier: el identificador de nombre de usuario, puede ser userid, sAMAccountName, cn

- Distinguished name: El directorio más alto del arbol en donde se encuentran los usuarios, generalmente es un valor dc, pero puede ser ou

- Bind type: Tipo de bind, regularmente general

- Para una conexión segura se debe habilitar la opcion, escoger el protocolo usado por el remote server y establecer el certificado del servidor remoto para comprobar su identidad

![[Pasted image 20240627094855.png]]

Test con cli:
```
diagnose test authserver ldap External_Server aduser11 Training!
```

### [[Radius]]
Cuando un cliente se autentica (fortigate) se envia un ACCESS-REQUEST, del cual se puede responder

Para configurar un server radius debemos de insertar
- Primary server/IP
- Metodo de autenticacion
	- chap,pap,mschap,mschap2 (default)
==La opcion incluir en cada grupo habilita a todos los grupos de usuarios locales a autenticarse contra radius, por lo que debe evalurase si es necesario==

Test con CLI
```
diagnose test authserver radius FortiAuth-RADIUS pap student fortinet
```


### 2FA con token o certificado
==User & authentication > Fortitokens==

La autenticación de firewall soporta el uso de Tokens 2fa o [[OTPs]]s. Estos se pueden enviar por correo o SMS a ambos; users y admins. Para el token, además tenemos la opción de [[FortiToken 200]] de hardware o fortitoken mobile. 

Los [[OTP]] también pueden ser usados como un segundo factor

==Todos los fortigates incluyen dos fortitoken gratuitos==

==Para usar un token en dos o más fortigates, se debe usar un fortiautenticator que actue como servidor de autenticación==

Para activar los tokens debemos de sincronizarlos y habilitar el 2fa para el usuario o grupo de usuarios 
![[Pasted image 20240701233433.png]]



### Autenticación pasiva

Significa que los usuarios no deben de autenticarse manualmente y al contrario se les da acceso transparente 
==Si la autenticación pasiva falla, se va al fallback, que es la autenticación activa==
- [[FSSO]]
- [[RSSO]]
- [[NTLM]]

### Grupos de usuarios
==User & auth > User groups==
Los grupos de usuarios son usados para la fácil administración de usuarios, como agregar todos los usuarios de LDAP a un grupo de LDAP, o casos más granulares
En fortigate existen cuatro tipos de grupos de usuarios

- Local
	- Usuarios locales que pueden conectarse a un servidor externo para facilitar la administración
- Guest
	- Usuarios temporales, se pueden crear a mano o en masa
- [[FSSO]]
- [[RSSO]]

### Flujo de autenticación

1. Revisar source
2. Si el usuario esta autenticado y es parte del grupo, coincidir el tráfico y pasarlo por la politica
3. Si no, Siguiente politica
4. Regresar al punto 1 hasta que coincida una politica, en caso de todas tener autenticación se prompteara la ultima

Para cambiar este comportamiento podemos :

##### Usar auth-on-demand
Esta es una opción exclusiva de cli que se activa de la sig manera:
```
config user setting
set auth-on-demand <always|implicit>
```

Implicito: Comportamiento default, de ser posible va al fallback

Siempre: Siempre que sea requerido pedir autenticación

Si usamos esta opción, deberemos de poner politicas de autenticación pasiva por encima de politicas de autenticación activa

##### Activar portal captativo 
Se habilita un portal en una interfaz por donde todos los usuarios pasarán

### Monitoreo de usuarios
==Dashboard > User & devices > firewall users==
![[Pasted image 20240702135256.png]]



