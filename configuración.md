En fortigate la configuración se guarda como una archivo de texto plano para su fácil administración y recuperación

Esta configuración se basa en almacenar los comandos necesarios para llegar al estado actual del sistema, podemos almacenarla en yaml o formato de fortinet

Podemos elegir cifrar la configuración, así como ofuscar información sensible como contraseñas


# Uso
Para acceder al menu de configuración vamos al menu de usuario

menu > configuration > Backup | restore | revisions | scripts

### Tips

- `ris:Checkbox` Siempre hay que respaldar, aunque se trate de un cambio menor, ya que no hay forma de deshacer configuraciones. 

- `ris:Knife` Y el cifrado es un arma de doble filo, si bien mantiene seguras tus credenciales e información sensbile, hace imposible el troubleshooting ya que fortigate no puede leerlo

