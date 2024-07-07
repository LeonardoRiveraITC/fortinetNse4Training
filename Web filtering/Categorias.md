Las categorias son un rating a sitios que se les da por parte los servicios de fortiguard, para esto se requiere una licencia activa
Se puede revisar el rating de categorias en www.fortiguard.com/webfilter/categories
##### Override de categorias
==Security profiles > Web rating overrides==
Usado en [[Lab 1 - Configurando web filtering]]
Sobreescribe la clasificación de un sitio para así tomar acciones distintas para ese sitio
![[Pasted image 20240704132441.png]]

### Conexión a fortiguard

```
diagnose debug rating
```
Calculo de pesos: default=(diferencia en time zone) * 10
- peso
- Return trip rime
- Flags
	- D - Ip de dns
	- I - Contract server contacted
	- T - tiempo
	- F - fallido
- TZ - Time server zone
- Fortiguard-requests - Numero de peticiones enviadas
- curr lost - Se reinicia a 0 si se tiene un exito
- Total lost - Numero total de peticiones de fortiguard


### Notas
Bajo ==Security Profiles > Web filtering==
Podemos habilitar que por default, 
- se permita el sitio en caso de que falle la busqueda de rating
- Se busque no solo por dns, pero por ip tambien para sobrepasar intentos de esquivar al firewall