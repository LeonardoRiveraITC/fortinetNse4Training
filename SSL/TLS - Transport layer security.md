[What is TLS (Transport Layer Security)? - Cloudflare](https://www.cloudflare.com/learning/ssl/transport-layer-security-tls/)

Transport layer security

Propuesto por la IEEE en 1999, TLS es un protocolo de seguridad diseñado para ofrecer confidencialidad e integridad para conexiones de internet. Es ampliamante usado para aplicaciones web

TLS es usado para cifrar conexiones, y es usado también en aplicaciones como email, voip, etc

La forma en que esto se lográ es por medio de [[TLS handshake]]

TLS hace uso de [[Cifrado asimetrico]]
## SSL o TLS
En realidad TLS es una evolución de SSL, SSL era propiedad de netscape por lo que para su versión 3.1 se le cambio el nombre a TLS

# El objetivo de TLS
TLS cumple 3 objetivos

1. Integridad de los datos
2. Autenticación (quien entrega los datos es quien dice ser)
3. Confidencialidad de los datos (cifrado)

## Certificados
Los certificados SSL (en realidad TLS, SSL por convención de nombre). Es distribuido por una autoridad que es dueña del dominio, que además de la información contiene la llave pública del servidor, la cuál es importante para un intercambio seguro

Otro campo importante incluido en los certificados es el MAC (Message-Authentication-Code), el cual es una firma que confirma que el tráfico proviene del sitio