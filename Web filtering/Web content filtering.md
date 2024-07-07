==Requiere de FULL [[SSL inspection]]==
==Maximo de 5000 patrones por filtro==
==Los filtros se manejan per policy==
El filtrado de contenido web bloquea contenidos de una pagina web
Soporta los siguientes metodos de match
- simple - match exacto
- Wildcard
- Regex - perl regex

Con las siguientes acciones:
- Exempt
- Block

Por ejemplo, bloquear contenidos de el cdn malicioso: cdn.polyfill.io

De igual manera se maneja un sistema de score, donde contenidos pueden valer más que otros, se suma el valor de los contenidos bloqueados y si se pasa un threshold, se bloquea 