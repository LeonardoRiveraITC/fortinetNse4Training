Esta es una carácteristica de [[Flow based inspection]]

Un protocolo es una ==especificación== de reglas las cuales nos permiten la comunicación, por lo que un protocol decoder es un decodificador de protocolo. Nos permite decodificar y detectar anomalias en un mensaje de un protocolo.

La forma de detectar ataques con el protocol decoder es la siguiente:

![[Pasted image 20240612142741.png]]

1. Recibir paquete
2. Detectar protocolo (a veces debe configurarse a cierto puerto, casi siempre es automático) 
3. Decodificar paquete
4. Ver si el paquete va de acuerdo al estandar, si no rechazarlo

Por ejemplo, muchos headers HTTP


