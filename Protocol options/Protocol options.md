Por default, se permite la entrada de archivos que sobrepasan el buffer, y es por ello que estos archivos pasan sin ser escaneados

config firewall profile-protocol-options
	edit <profile-name>
	config <protocol-name>
	set options oversize-limit <integer> (default 10 mb, el máximo es hw dependant)
	end
end

Obviamente un valor alto tiene el beneficio de escanear la mayoria de archivos, pero la desventaja de que el proxy estara ocupado este no se encargara de dirigir más tráfico hasta que se termine el análisis, por lo que será un overhead visible

Mientras un valor más bajo de buffer 
-Menos ram
-Más overhead

Es por eso que es deseable activar el logging cada que pase un archivo muy alto para el buffer

config firewall profile-protocol-options
	edit <profile_name>
	oversize-log enable <enable | disable>
end

Existen opciones de procesar 
[[Compressed files]]
