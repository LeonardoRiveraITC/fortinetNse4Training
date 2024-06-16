### IPS
- Control granular 
	Se deben aplicar reglas de IPS de forma granular, no tiene sentido analizar firmas de windows en un equipo macos y viceversa. De igual forma hay firmas que solo aplican a servers y otras a clientes

-  Analizar el requerimento de la red
	Aplicar las politicas solo a los recursos que sea necesario y en donde sea necesario. 
- Análisis continuó de requerimentos
	Evaluar constantemente la red para cambios de IPS. No tiene sentido mantener politicas que ya no sirven. Como el caso de los parches
- Evitar análisis de tráfico interno

IPS gasta muchos recursos de CPU por lo que es un arma de doble filo. Si se abusa puede costar overhead en la red

### [[SSL inspection]]
Se recomienda activarla en  la misma politica para así poder analizar el contenido de tráfico cifrado


## [[Hardware (pendiente)]]
La aceleración de hardware se puede activar para 
- NP6,NP7, NP8 y SoC4; Acelerar el procesamiento
- CP8,CP9, aligerar la carga sobre el cpu y pasarla a estos chips
config system global
	np-accel-mode
end

config ips global
	set np-accel-mode [basic | none]
	set cp-accel-mode [basic | advanced | none]
end
	