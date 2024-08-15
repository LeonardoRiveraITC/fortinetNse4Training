1. Reiniciar a valores default
2. Cambiar el perfil de AV default a Proxy based y habilitar logging para ssh
	1. Debemos correr los siguientes comandos
```
config antivirus profile 
	edit default 
	set feature-set proxy
end
```
![[Pasted image 20240707155911.png]]

3. Configura una politica de fw con este perfil
	1. Para poder elegir el perfil de inspeccion recien configurado, debemos cambiar el modo de inspección de la politica a [[Proxy based inspection]] tambien
	```
	config firewall policy
		edit 1
			set inspection-mode proxy
		end
	```
![[Pasted image 20240707160346.png]]

4. Probar el perfil
	1. Para esto usaremos el EICAR file, un archivo diseñado para probar antivirus 
	2. Al intentar descargar EICAR, tenemos la siguiente advertencia ![[Pasted image 20240707164235.png]]
5. 