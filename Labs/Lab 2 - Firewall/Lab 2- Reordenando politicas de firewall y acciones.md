
*Recordemos que fortigate matchea politicas de arriba a abajo, por lo que debemos poner las más especificas hasta arriba de la lista*

#### 1. Configurar [[Politicas de Firewall (Pendiente)]] para bloquear tráfico ICMP
Lo que hace esta [[Politicas de Firewall (Pendiente)]] es bloquear todo el tráfico de salida ICMP que vaya hacia la máquina de linux y logea las incidencias que violan la politica

![[Pasted image 20240617142853.png]]

#### 2. Probar la [[Politicas de Firewall (Pendiente)]]
Al crearla aparecera por debajo de las demás políticas, por lo que tendremos que moverla al tope para que esta tome lugar antes que la politica general
![[Pasted image 20240617142918.png]]
Podemos ver que efectivamente se bloquea el tráfico
![[Pasted image 20240617143001.png]]


![[Pasted image 20240617143342.png]]


Caso contrario, colocamos la politica por debajo del internet_access
![[Pasted image 20240617143043.png]]

Podemos ver que la [[Politicas de Firewall (Pendiente)]] que se matchea es internet_access por lo que la otra no toma efecto

*Recordemos que las politicas tienen un id que sirve a fortinet para identificar politicas, este id no tiene efecto en el orden en que se evaluan las politicas. Si no que es el orden de la tabla*

