1. Security profiles>SSL inspection
	2. Create new
	3. Allow invalid SSL certificates
	4. Asignarle el nombre Custom_SSL_Inspection
		![[Pasted image 20240705210246.png]]
		![[Pasted image 20240705210258.png]]
2. Habilitar en la politica de firewall Full_Access
	1. ![[Pasted image 20240705211148.png]]
	2. Debido al hambre de fortinet, este lab  no funcionara si no se tiene una license vigente
	3. ![[Pasted image 20240705211357.png]]

3.En caso de tener la licencia, veriamos lo siguiente debido a que el self-signed certificate no esta disponible ![[Pasted image 20240705212220.png]]
4. Para eso lo debemos instalar, descargandolo desde System>Certificates, y instalandolo  importandolo al navegador