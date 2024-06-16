Recomendaciones para la implementación correcta del AV

## Escaneos en todo el tráfico
Si se esta usando un balanceador de cargas o conexiones redundantes, hay que asegurarse que todas las conexiones dentro-fuera y fuera-dentro tengan [[Antivirus]] activado

## Usar inspección deep en lugar de certificate based
Esto para asegurarse de que todo el contenido es escaneado

## Usar la base de datos de forticloud sandbox o fortisandbox

## No incrementar el buffer size a menos que sea necesario

Esto puede causar un overhead innecesario, ya que los virus usualmente viajan en archivos pequeños


## Asegurarse de que los perfiles con AV tengan log de eventos de seguridad activado

## Logear archivos pasados por el scanner por su tamaño

## Aceleración de hardware

La aceleración de hardware puede ayudar a aligerar la carga sobre el cpu

*Lo siguiente solo funciona para flow based 

Para ello se requiere de nturbo

config ips global
	set np-accel-mode [none | basic]
end


Adicionalmente con hardware CP8 y CP9
Podemos aligerar más la carga del cpu al descargar bases de datos de patrones del ips engine 

Las cuales serán analizadas por el hardware CP, de la siguiente manera

1. Llega el tráfico
2. El cpu redirigé al hardware CP
3. El hardware CP decide el modelo de seguridad basandose en servicio y politica
4. El hardware CP toma la decisión y la comunica al engine

El hardware CP8 y CP9 también pueden acelerar la [[SSL inspection]]

