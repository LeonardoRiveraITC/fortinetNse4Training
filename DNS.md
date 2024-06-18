Domain name service

Es un servicio que permite asociar direcciones Ip a dominios 

Podemos habilitar este servicio a una interfaz de [[Redes]] LAN, no es optimo usarlo más que para resoluciones internas, ya que resolver DNS puede tomar mucho tiempo de cpu

Tiene 3 modos de operacion
- forward: Reenvia las peticiones DNS a otros servidores y las comunica al solicitante
- Non-recursive: Responde solo con su tabla de direcciones
- Recursive: Intenta responder con sus dns conocidos, y si no encuentra coincidencia reenvia la peticion