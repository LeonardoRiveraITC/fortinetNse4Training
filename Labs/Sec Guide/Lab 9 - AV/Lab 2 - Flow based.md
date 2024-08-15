1. Cambiar el modo del perfil en [[Lab 1 - Antivirus en proxy based mode]] por flow mode y habilitar FTP al perfil
2. ![[Pasted image 20240707165946.png]]
3. Al intentar descargar eicar desde un ftp en linux remote, podemos ver que falla la transferencia de archivo
4. ![[Pasted image 20240707170150.png]]
5. Y a su vez se crea un log de seguridad
 ![[Pasted image 20240707170232.png]]

6. Probar la IA
	1. Por default esta habilitado el escaneo por inteligencia artifical, para deshabilitarlo ejecutamos
	```
	config antivurus settings
		set machine-learning-detection disable
	end
	```
7. Descargamos un archivo desde ftp de nuevo
8. El archivo no existe pero la inspección no detectara a menos que activemos machine-learning-detection de nuevo
9. Recordemos que todos las detecciones de IA se marcan como W32/AI.Pallas.suspicious signature