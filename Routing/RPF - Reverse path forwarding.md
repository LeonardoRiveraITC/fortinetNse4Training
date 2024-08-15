==habiltiado por default==
RPF es una medida de protección contra ataques de IP spoofing

Funciona al buscar una ruta de retorno a la ip src en el primer paquete de la sesion

Si no se encuentra una ruta se dropea el paquete

Tiene dos modos
- Feasible path - foormerly loose
	- El return path no debe de ser el mejor path
- strict
	- El return path debe de ser el mejor


### Comandos
Habilitar RPF 
```
config system settings
	set strict-src-check [enable | disable]
	end
```

```
config system interface
edit <if>
	set src-check disable
	next
	end
```