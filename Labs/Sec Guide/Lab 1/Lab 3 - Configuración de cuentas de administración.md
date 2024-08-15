## Configuración de cuentas de administración


### 1.  
Iremos a system>admin profiles y agregamos uno nuevo
![[Pasted image 20240617102803.png]]

Se le dará permisos solo de lectura a casi todo
![[Pasted image 20240617102945.png]]

### 2.
Crear un nuevo administrador

Lo que hicimos hace un momento fue solo para crear un perfil, ahora deberemos crear una cuenta administrador que heredara los permisos de ese perfil

Navegar a System > Administrators  
Y creamos un usuario con el rol que acabamos de crear

*En la contraseña de un administrador no podemos incluir caracteres especiales*

Y listo, el usuario fue creado y levantado

### 3. 
Restringir acceso a solo subredes confiadas

En ocaciones es necesario restringir el acceso a solo subredes confiables en las que sabemos que se encuentra un administrador, este, entre otras opciones como el 2fa nos ayudan a fortalecer la seguridad de usuarios de fortigate

![[Pasted image 20240617103655.png]]

Bloque de comandos para agregar una subred confiada
![[Pasted image 20240617110458.png]]

![[Pasted image 20240617110729.png]]
Con este cambio, ahorta podremos acceder a esta cuenta desde local

