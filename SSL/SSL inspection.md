### ==Security Profiles > SSL inspection==
==No tiene sentido habilitar SSL inspection por si solo, ya que este solo decifra el trafico, para ello es necesario habilitar algun otro perfil para decirle a la politica que debe analizar==
==De otra manera ni siquiera se intentara el SSL inspection==


Configurado en [[Lab 1 - SSL inspection on outbound traffic]]
Parte del [[UTM]]
La inspección ssl permite a fortigate actuar como Man in the middle para así inspeccionar los contenidos que se encontrarían cifrados de otra manera
Por ejemplo, inspeccionar un malware que de otra manera no podria ser inspeccionado por estar cifrado

Este es un perfil de seguridad, por lo que para usarlo debe de activarse en una [[Politicas de Firewall]] 


Funciona de la siguiente manera:

![[Pasted image 20240610225900.png]]

Como se puede ver en la imagen, el SSL inspection funciona interceptando el proceso de [[TLS - Transport layer security]] las llaves del cliente y el servidor

Una vez que se intercepta el client hello, se envia un client hello desde el dispositivo para actuar como cliente, mientras que al cliente real se le responde el server hello desde el dipositivo, con un certificado de fortigate que se hace pasar por el certificado del sitio usando el campo de dns

De esta manera tenemos dos conexiones TLS y a la vez podemos inspeccionar el tráfico

Esto se hace debido a que TLS 1.3 tiene maneras de circuventar el robo de llaves, por lo que es más sencillo interceptar el tráfico de esta manera

### Operaciones
- Certs no confiados
	- Permitir
		- Esta opcion permite los certificados no confiados (incluye los self signed por ejemplo),y envia al navegador un certificado Fortinet_CA_Untrusted, el cuál causara que el navegador de una advertencia dejando al usuario aceptar el riesgo o no
	- Bloquear
		- Bloquea todo certificado no confiado
	- Ignorar
		- Siempre responde con Fortinet_CA_SSL_Certificate 
![[Pasted image 20240703232118.png]]


### Problemas con Inspección
Es posible que no se desee usarla por razones como que el sitio use HPKP (HTTP public key pinning), lo cual previene la inspeccion SSL, o por razones legales que eviten el uso de ssl inspection

Para eso usamos la opción ==Excempt from ssl inspection==


### Otras aplicaciones
Mayormente SSL se usa para paginas web, pero existen otros servicios y aplicaciones que utilizan ssl, por lo que de deben tomar en cuenta estos casos tambien
- FTPS
- SMTPS
- STARTTLS
Apps:
- Outlook
- Drop box


### Notas
Cada que un cliente solicita un sitio web externa fortigate genera un nuevo par de llaves y certificado temporales para enviar al cliente interceptado

Se puede habilitar decrypted traffic mirror, lo cual reenvia el trafico descifrado a una interfaz y muestra al usuario los terminos y condiciones de usar esta red

==Cumple con RFC5280==
![[Pasted image 20240703231315.png]]

