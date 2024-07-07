==Objects & policy > Firewall policy > Inspection mode==

Usado en [[Lab 1 - Configurando web filtering]]
Se llama así porque actua como un intermediario o *proxy*, ya que completa dos sesiones TCP entre el cliente y el servidor. Además de que escanea los contenidos de los paquetes as a *whole*, en lugar de paquete por paquete
La comunicación termina en capa 4

Desventajas
- Esto tiene la desventaja de causar overhead en la red
- Resource consumption

Ventajas
- Inspección de contenidos, como archivos, usando un buffer
- Alteración de contenidos como headers HTTP
![[Pasted image 20240704002308.png]]
