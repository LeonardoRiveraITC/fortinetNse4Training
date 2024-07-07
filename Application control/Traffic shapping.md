==Policy & objects > Traffic shaping > Traffic shaping policies==
Parte de [[Application control]]
==Debe activarse en [[Visibilidad]]==

Esta es un caracteristica que permite manejar trafico especifico de aplicaciones para asegurarnos de que no haya overhead en la red, por ejemplo

Para youtube, lo que consume mucho ancho de banda son los videos, sin embargo, las demas paginas son meramente información, por lo que se desea hacer traffic shaping a los videos

Se puede coincidir en base a 
- Categorias 
- Applicacion
- Grupo de aplicaciones

### Funcionamiento
Se debe crear un shaper donde se especifique el limite del ancho de bando y que sera usado para las politicas

El shaper se hace una politica, que debe coincidir con las politicas de firewall, esta politica de shaper balanceara el trafico en las aplicaciones y URLs definidas en ella

- src-ip
- dst-ip
- Applications
- urls

### Modos de operacion

- Shared shaper 
	- Aplica el balanceo a todas las politicas que hagan referencia al shaper
- per ip shaper 
	- Aplica shaper para todas las ips dentro del grupo y se divide de manera uniforme

- Egress-to-ingress: Util para restringir ancho de banda de subida
- Ingress-to-egress o reverse shaper : util para ancho de banda de descargas o streaming

