1. Configurar IPv4 DOS policy
	1. Proveniente de puerto 1, con src de todos y dst a todo para todos los servicios
	2. Configurar thresholds de capa 4 (habilitar logs tambien)
		1. ICMP_SYN_FLOOD: Block - Threshold 250
		![[Pasted image 20240707175418.png]]
		3. Ejecutar el comando en linux
		```
		sudo ping -f 10.200.1.1
		```
		La flag -f indica enviar continuas peticiones sin esperar una respuesta
3. En logs, en anomalias podemos ver el evento
4. ![[Pasted image 20240707175702.png]]
5. 