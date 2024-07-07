Fortisandbox se encarga de reconocer virus en archivos 

Security fabric > Fabric connectors
==Activavle solo en gui==

```
config system fortisandbox
	set inline scan <enable | disable>
end
```

- Fortisandbox cloud
- Fortisandbox appliance

Para determinar que archivos enviar al fortisandbox se usa un feature de [[Proxy based inspection]]

##### inline inspection
Otro feature de [[Proxy based inspection]], este retiene un archivo hasta que se determine su seguridad

### CDR - Content disarming
Remueve contenido peligroso y lo reemplaza por contenido seguro
- PDS
- MS office
- IMAP
- POP3

Fortigate entonces envia al usuario el archivo seguro y envia a fortigate el archivo original para su inspeccion



### Malware block list
Security fabric > external connectors
- MD5
- SHA1
- SHA256

