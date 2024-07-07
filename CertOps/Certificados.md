==System > Certificates==
Un certificado es un documento digital que identifica a una persona o dispositivo

Los certificados funcionan con [[Cifrado asimetrico]] para asegurar los datos, y requieren de un CertificateAuthority (CA) que valide el certificado

Entre los campos que tiene un certificado, los más destacables son:
- Para hacer [[Cifrado asimetrico]]
	- Version
	- Algoritmo
	- Llave pública
- Para reconocer al dueño del certificado
	- Subject Name o Distinguished name (DN) 
	- Alternate subject name 
	- Llave de CA - Authroity key identifier (autoridad que valida el certificado)
	- Subject key identifier (llave identificador de sujeto)
- Reconocer la validez del certificado
	- Fecha de inicio de validez
	- Validez de la firma digital
	- Fecha de fin de validez
	- Issuer - Quien expide ek certificado
	- Validez del serial en listas CRL (Certificate revocation list) de manera local o de forma remota con OCSP (Online Certificate Status Protocol)
![[Pasted image 20240703164907.png]]
#### Validar una firma
Para validar una firma se siguen los siguientes pasos aprovechando los usos del [[Cifrado asimetrico]]

1. El CA genera un valor, el cuál es hasheado. Este es el *original hash*
2. El original hash es cifrado usando la llave privada y enviado 
3. El dispositivo recibe el certificado, y usando el algoritmo de hasheo (incluido en el certificado), hashea el certificado, obteniendo un nuevo hash
4. Se intenta descifrar los contenidos del hash generado en paso 2 con la llave pública, si no se puede descifrar, la verificación falla
5. Si el paso 4 es exitoso, se compara los resultados del hash decifrado con el generado en el paso 3. Si coinciden el certificado es valido, si no, el certificado es invalido


### Certificados invalidos
Cuando fortigate encuentra un certificado invalido por
- Expirado 
- Cert revocado
- Verificación fallida
- Timeout de validación
Se pueden tomar las siguientes acciones
- Mantenener como inconfiable y permitir
	- Permite pasar el trafico y elegir al usuario la accion a tomar
- Bloquear
- Permitir y confiar

### Inbound y outbound
Explicado en [[Tráfico]]
Para cuando llega una peticion inbound, fortigate tambien puede interceptar la comunicación haciendo uso del [[SSL inspection]], pero esta vez para un servidor dentro de la red

Para esto, fortigate usa un certificado configurado con el SNI y/o CN que coincida a nombre del web server, si no existe, agrega el primer certificado en la lista de certificados

Para esto se debe usar la opcion de ==Protecting SSL Server==
Usado en [[Lab 2 - SSL inspection en inbound]]
![[Pasted image 20240703234311.png]]

### CA Stores


### CA Root

### Operaciones
- [[Inspeccion de certificados]]
- [[SSL inspection]]


![[Pasted image 20240703231327.png]]
