#### 1. Primero levantamos el lab restableciendo las configuraciones iniciales

![[Pasted image 20240617122354.png]]



#### 2. Creación del primer objeto

*Por default fortigate contiene muchos [[objetos]] y politicas comúnes, pero se pueden crear nuevos para satisfacer las necesidades especificas*

Crearemos un objeto de tipo subnet, bajo policy objects > addresses

![[Pasted image 20240617125047.png]]
Podemos ver el [[objetos]] creado
![[Pasted image 20240617125523.png]]

Una vez que tenemos esto, podemos aplicarlo a una [[Politicas de Firewall]]

#### 3. Crear una politica de firewall

Esta config ya tiene una politica general que deshabilitaremos en objects & politics > firewall policy

La deshabilitamos
![[Pasted image 20240617125744.png]]


Y creamos la nueva politica

*Recordemos Fortinet tiene la acción por defecto de bloquear, por lo que si no agregamos una politica que permita el paso de tráfico, este entrara a la accion default*

Quedando así de la siguiente manera

![[Pasted image 20240617131706.png]]
![[Pasted image 20240617131746.png]]
#### 3. Probar la politica
Ahora que aplicamos la politica debemos ver que efectivamente funciona

Abriremos pestañas de navegador para esto

![[Pasted image 20240617135428.png]]
Podemos ver que efectivamente funciona


Ahora revisaremos los logs generados

Como se habilito el logeo de todo, incluyendo las sesiones iniciadas. Podremos ver la apertura de esta pestaña de google

![[Pasted image 20240617135517.png]]
Al visitar la página picoctf, podemos ver un log de esta politica con este destino
![[Pasted image 20240617140324.png]]

Asi comprobando que efectivamente se conecto