==Al habilita SDWAN se esconde el ECMP load balancing==
==load-balance-mode reemplaza v4-ecmp-mode==
Software Defined WAN
Sirve para direccionar tráfico dependiendo de conjuntos de reglas
- Protocolos y servicios
- Application awareness
- Dynamic link selection

- Secure SDWAN

- Beneficios
	- Uso optimo de la wan
	- Performance de aplicacion mejorada
	- Reduccion de costos

==SD-WAN controla trafico de egreso, no trafico de entrada, por lo que puede que se responda a traves de otra interfaz==

Con SDWAN podemos habilitar el uso de multiples enlaces rapidos de poco ancho de banda para direccionar trafico a como sea más conveniente en lugar de depender de un solo enlace lento de mucho ancho de banda

SDWAN nos permite establecer reglas para direcciona trafico tal que
- Src - address . port
- Dst- address - port
- Servicio
- Aplicacion
- Balanceo de cargas
- Perfiles de seguridad
- VPN, IPSEC

### Reglas
==Se evaluan de arriba a abajo==
==Requieren de un firewall policy==

==Las reglas SDWAN son [[Policy routes]] rules==

La regla implicita indica routear el trafico como de cosumbre

### Diferencias de balanceo de carga con ECMP
load-banace-mode
	Soporta el algortimo volumen
	Usa el peso definido en SDWAN member
	Usa el spillover configurado en SDWAN

###### Algo de volumen
- Fortigate sigue el numero de bytes en un miembro y asignara más al de más peso

### Comandos
config system sdwan