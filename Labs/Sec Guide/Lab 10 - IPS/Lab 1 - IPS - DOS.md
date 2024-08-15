1. Restaurar config inical
2. Configurar un perfil IPS (sensor) filter y agregar amenazas de nivel medio,alto, critico
![[Pasted image 20240707172253.png]]
3. Crear una VIP de 10.200.1.200 a 10.0.1.10
 ![[Pasted image 20240707172436.png]]
4. Finalmente, una politica de firewall para publicar la vip
 ![[Pasted image 20240707172803.png]]

No olvides agregar el pefil IPS WEBSERVER

5. Al usar la herramienta nikto en el linux server, podemos ver que se logea el trafico malicioso que configuramos en el sensor
![[Pasted image 20240707173412.png]]



