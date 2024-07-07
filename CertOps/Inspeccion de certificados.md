==Security profiles > SSH/SSL inspection==
Es uno de los metodos de inspeccion SSL junto a [[SSL inspection]] full

Permite a fortigate validar certificados por medio del SNI (Server name indivation), subject o alternate subject name

Inspecciona los headers del certificado, esto sirve para verificar el certificado, además de 
- Verificar sitios bloqueados por web filter o app control y que no se use https como workaround
