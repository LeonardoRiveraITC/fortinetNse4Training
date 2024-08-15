- Detecta links muertos cuando el error esta más haya de la conexión

- Envia periodicamente pruebas a 4 servidores (beacons)
	- ICMP request - ==mas usado==
	- TCP
		puerto 7
	- UDP
		puerto 7
	- HTTP
		Get request
	- TWAMP - ==mas confiable==
		- TCP 862
		- UDP 862

Para fortigate los enlaces funcionan de la siguiente manera:
- Se marca como vivos
- Se envian 5 probes a los beacons, si no se puede establecer la conexión, se marca como muertos
	- Actualiza las rutas (estatica,politicas y cascada)
- Se envian 5 probes y si es exitoso se marca como vivo de nuevo

==Los 5 probes es la config default==

### Comandos
configurar link health monitor
```
config system link-monitor
	edit port1-health
	set srcintf port1
	set server 1.1.1.1
	set gateway-ip 10.200.1.254
	set protocol ping
	set update-cascade-interface enable
	set update-static-route enable
	set update-policy-route enable
next
end
```

Configurar iknterfaz de alerta
```
config system interface
	edit port1
	set fail-detect enable
	set fail-alert-method link-down
	set fail-alert-interfaces "port3"
```