1. Reiniciar a configuración inicial
2. Configurar el servidor [[LDAP]] en ==Users & auth > LDAP==
3. ![[Pasted image 20240701230826.png]]
En este lab no se cuenta con un fortiauthenticator, pero tenemos un active directory disponible en windows local
![[Pasted image 20240706193628.png]]

5. Ahora agregaremos un grupo de usuarios remotos a un grupo de usuarios local
![[Pasted image 20240706193731.png]]
![[Pasted image 20240706193825.png]]

6. Ahora agregamos a la politica de firewall

![[Pasted image 20240706194005.png]]

7. Ahora si intentamos navegar, deberemos autenticarnos
8. ![[Pasted image 20240706194114.png]]0a


==Se pueden ver y manipular sesiones activas en Dashboard > Users & Devices==
