
### *Es normal que un dispositivo con muchas politicas, perfiles o VDOMS tenga alto uso, pero se debe aislar la problemática en caso de presentar un uso alto todo el tiempo


# [[IPS - Intrution Prevention System]]
Similar a otros sistemas de firmas[[Troubleshooting - All]]

- Las actualizaciónes llegan desde update.fortiguard.net en el puerto 443

Debug
```
diagnose debug application update -1
diagnose debug enable
excute update-now
```


```
diagnose test application ipsmonitor <Integet>
```

- 1 - Mostar info de ips engine
- 2 - Habilitar/deshabilitar ips engine
- 3 - Restart log
- 4 - Clear restart log 
- 5 - Bypass status (Se mantiene activo el engine pero no análiza tráfico)
- 6 - Submit attack characteristics now 
- 10 - cola de IPS 
- 11 - Clear cola de ips IPS 
- 12 - IPS L7 socket characteristics
- 13 - Lista de sesiones IPS
- 14 - IPS NTurbo statistics
- 15 - IPSA statistics
- 97 - Start all ips engines
- 98 - Stop all ips engines
- 99 - restart all ips engines and monitor (monitorear que se reinicien bien)


### [[IPS - Intrution Prevention System]] Fail Open

El [[IPS - Intrution Prevention System]] Falla cuándo el buffer se queda sin espacio para continuar analizando paquetes, existen dos comportamientos cuando esto ocurre

1. Permitir pasar ciertos paquetes hasta que se desocupe el buffer
2. Dropear paquetes hasta que se desocupe el buffer

Claro esta, este comportamiento no es deseado y hay que afinar las reglas de firewall para que esto no ocurra

Esto se configura con el comando

```
config ips global
	set fail-open <enable|disable>
end
```
