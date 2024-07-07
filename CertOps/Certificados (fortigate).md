Fortigate hace uso de certificados con los siguientes motivos

- Establecer conexiones seguras a otros puntos como [[fortiguard]]
- Llevar a cabo [[SSL inspection]]
- Llevar a cabo [[Firewall authentication]] de usuarios con certificados
- Asegurarse de que los sitios son seguros

Fortigate puede usar varios campos para identificar el dueño de un [[Certificados]], entre ellos
- Subject 
- Subject alternate 
- Subject key identifier
- Authority (CA) key identifier

Para validar un certificado, fortigate
- Busca que tenga el certificado correspondiente al CA
- Busca el numero serial en listas CRL
- Verifica las fechas de expedición y expiración
- Validez de la firma digital

Para agregar un certificado a fortigate vamos a 
==Sec profiles > SSL inspection > View trsuted CA > Factory bundles==

Podemos configurar a fortigate como un OCSP 

Por default, fortigate usa un certificado ==self-signed==, lo que quiere decir que es un certificado valido pero no autorizado por un CA, por lo que saltaran advertencias en operaciones como intentar usar un sitio web, para remediar esto podemos 
- Agregar el certificado self-signed a las CA stores de los usuarios
- Comprar un certificado y asignarlo a fortigate

