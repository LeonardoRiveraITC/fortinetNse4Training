Objetivos del handshake

1. Determinar version de TLS (1.0,1.1...)
2. Decidir el conjunto de algos de cifrado
3. Autenticar la identidad del web server con la llave pública y la firma digital
4. Generar llaves de sesión

![[Pasted image 20240610235958.png]]

Un handshake TLS toma lugar luego de un handshake TCP*

El handshake puede variar dependiendo del algoritmo, por ejemplo RSA (inseguro) toma los siguientes pasos

1. client hello: El cliente envía la versión de TLS, cyphers soportados y el client random, una cadena aleatoria usada para poner de acuerdo llaves
2. server hello: El servidor envia su certificado, el cypher elegido y el server random, otra cadena aleatoria. El certificado  contiene la llave públioca
3. authenticación: Se verifica la identidad del servidor con la autoridad que expidio el certificado, esto confirma que el servidor es quien dice ser
4. Premaster secret: El cliente envia una cadena aleatoria cifrada por la llave publica, esta solo puede ser decifrada por la llave privada, esto sirve para volver a corroborar que el servidor es quien dice ser
5. Llave privada: EL servidor decifra el premaster
6. Se generan las llaves de sesión: client random + server random + premaster secret
7. Client - ready : El cliente envia un mensaje de listo cifrado con una llave de sesión
8. Server - ready : El servidor envia un mensaje de listo cifrado con una llave de sesión 

### Delfie-hillman
Por su lado, delfie hillman no usa la llave privada para la creación de llaves de sesión

1. Client - hello: El cliente envía la versión de TLS, cyphers y el client  random
2. Server - hello: El servidor responde con el cipher elegido, su certificado y el server random
3. Además, el servidor computa una firma de todos los mensajes intercambiados hasta ahora, esto se envia con el server hello
4.  Cliente: Verificar la firma enviada por el servidor
5. Cliente: Enviar parametro DH
6. Cliente y Server: Calcular el premaster secret usando los parametros DH
7. Se crean las llaves de sesión
8. Client ready
9. server ready
10. Cifrado simetrico de sesión
