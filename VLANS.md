#### *Una VLAN es una red local virtual, esto quiere decir que a un puerto fisico le podemos asignar dos o más redes virtuales, esto nos sirve para segmentar la red, aumentar la seguridad y  el rendimiento de la misma al reducir el dominio de broadcast y isolando la comunicación entre vlans*


### Crear una vlan
##### ==Network > interfaces >New Interface>Type vlan ==
Se pueden separar vlans de vdoms



![[Pasted image 20240618132457.png]]


[https://www.youtube.com/watch?v=n8j9UcDsV2g](https://www.youtube.com/watch?v=n8j9UcDsV2g)

Las vlans son “virtuales”, porque el segmentamiento ocurre a nivel logico

Es decir, podemos isolar y segmentar una red sin necesidad de usar un switch fisico

Las vlans tambien sirven para solucionar el problema de broadcasting (muchos paquetes de broadcast saturando la red), ya que también seccionan el broadcast por cada vlan correspondiente

¿Porque vlans y no más switches?

Los switches son costosos, y no solo eso, si no que también conectar switches cuesta el medio fisico, por lo que podemos terminar con muchos switches con muchas interfaces sin usar, para ello podemos usar vlans, para virtualizar las funciones de un switch

Siempre que hagamos redes debemos seguir el principio KISS (keep it simple stupid)

¿Bueno, ahora que tenemos vlans, como las interconectamos ?

Fácil, con un routeador a como lo hariamos con dos redes

O podemos aprovechar y hacer enlaces troncales que sacan el provecho al máximo de las VLANs

Pero, si queremos conectar varias vlans, debemos asignar un puerto por vlan, que a la larga no es conveniente

![[Pasted image 20240618132956.png]]

Para eso se inventaron los troncales, que nos permite pasar trafico de muchas vlans en un solo puerto, como quiera estamos hablando de direccionamiento logico

Lo que se hace para habilitar un puerto trunk es habilitar el protcolo 802.1q, que etiqueta los paquetes dependiendo la vlan

Una vez que el paquete etiquetado llega a otros switches, se recorta la etiqueta ya que el switch sabe a que vlan va y no es necesario para el paquete en el end device

El paquete se reenviara por el switch de forma regular usando las direcciones MAC

### Esencialmente, las subredes separan una red fisicamente, mientras que las VLAN las separan logicamente


![[Pasted image 20240618133644.png]]
![[Pasted image 20240618133651.png]]
Para crear una vlan debe estar creada en ambos switches y luego configurar los puertos para extenderlos entre si

### VLAN Nativa

Esto se usa para que los dispositivos viejos que no soportan 802.1q, o cualquier trama en general sin etiquetar, pueda pasar por un puerto troncal

Lo que hacen las vlans nativas es, que a cualquier trama sin etiquetar se le asignara una vlan, por ejemplo, en cisco al puerto 1 se le asigna la vlan nativa 1, y cualquier trama que pase por el puerto 1, se le asignara la vlan 1

Ahora tambien se puede cambiar la vlan nativa, y se suele aprovechar de la siguiente manera

![[Pasted image 20240618133731.png]]

En este ejemplo tenemos el troncal, con la vlan 10 ,20 y 30, como sabemos etiquetara la trama dependiendo de la vlan correspondiente

Sin embargo, al cambiar la vlan nativa, al recibir una trama de vlan 30, no etiquetara la trama el primer switch

Finalmente el segundo switch al recibir una trama sin etiquetar, asumira que la trama va dirigida a la vlan 30

Y por eso es importante configurar el troncal en ambos switches para tener la misma vlan nativa, de otra forma puede haber un native vlan mismatch

Todos los puertos de acceso pertenecen a la default vlan y no se pueden configurar como troncales, pero si se pueden asignar a vlans

(Existen puertos de acceso y puertos troncales)

![[Pasted image 20240618133742.png]]