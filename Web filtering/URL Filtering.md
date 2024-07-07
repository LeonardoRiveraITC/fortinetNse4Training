El URL filtering nos permite tomar acciones basados en nombre de url (url, no dominio)

Por lo que podemos permitir todo un sitio y bloquear solo un usuario tal que http://pagina.com/user/123 por ejemplo

El URL filtering tiene 3 formas de matcheo
- Simple
	- Debe matchear exactamente
- Wildcard
	- Se usan wildcards, por ejemplo /user/*
- Regex
	- Se usan expresiones regulares

Las acciones a tomar son las siguientes:
- Allow
- Block
- Monitor
- Exempt

