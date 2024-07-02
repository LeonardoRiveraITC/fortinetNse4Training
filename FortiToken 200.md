![[Pasted image 20240701232821.png]]
Los fortitoken usan un algoritmo sencillo de ==seed + tiempo== que cambia cada 60 segundos. Una vez se dispara el 2fa después de validar pass y user. El servidor que esta sincronizado con el fortitoken, validara el token regenerandolo con el mismo algoritmo

==Se recomienda un buen servidor NTP==