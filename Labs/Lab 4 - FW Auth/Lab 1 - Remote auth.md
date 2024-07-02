1. Reiniciar a configuración inicial
2. Configurar el servidor [[LDAP]] en ==Users & auth > LDAP==
3. ![[Pasted image 20240701230826.png]]
En este lab no se cuenta con un fortiauthenticator, pero el probar conexion debe de ser correcto
4.  ![[Pasted image 20240701230935.png]]

5. Ahora agregaremos un grupo de usuarios remotos a un grupo de usuarios local
![[Pasted image 20240701231333.png]]

![[Pasted image 20240701231349.png]]

6. Finalmente, se agrega el grupo de usuarios a la politica de firewall
![[Pasted image 20240701231634.png]]

7. Ahora cuando se matchee la politica prompteara al usuario a autenticarse primero

==Se pueden ver y manipular sesiones activas en Dashboard > Users & Devices==
