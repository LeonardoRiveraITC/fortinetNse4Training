### ==Security Profiles > SSL inspection==
Parte del [[UTM]]
La inspección ssl permite a fortigate actuar como Man in the middle para así inspeccionar los contenidos que se encontrarían cifrados de otra manera
Funciona de la siguiente manera:

![[Pasted image 20240610225900.png]]

Como se puede ver en la imagen, el SSL inspection funciona interceptando el proceso de [[TLS - Transport layer security]] las llaves del cliente y el servidor

Una vez que se intercepta el client hello, se envia un client hello desde el dispositivo para actuar como cliente, mientras que al cliente real se le responde el server hello desde el dipositivo, con un certificado de fortigate que se hace pasar por el certificado del sitio

De esta manera tenemos dos conexiones TLS y a la vez podemos inspeccionar el tráfico

Esto se hace debido a que TLS 1.3 tiene maneras de circuventar el robo de llaves, por lo que es más sencillo interceptar el tráfico de esta manera