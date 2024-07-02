Son [[objetos]] usados para mapear DNATs. 
Por default es una ip estatica pero tambien puede ser un FQDN, y para aplicarlo en una politica de firewall se debe poner en el campo de destino.

==Cuando se usa una VIP, El trafico de ingreso es traducido sin importar el servicio,  mientras que la politica coincida al destino==

==De igual forma el trafico de egreso es mapeado a la ip mapeada en el vip en lugar de la interfaz de egreso==

==Se puede usar port DNAT con VIPs==

==Por default las VIP no se consideran iguales a los [[objetos]] de firewall, esto quiere decir que no se solapan. Es por ello que puede haber comportamiento inesperado, como por ejemplo. Si tenemos el implicit deny hasta arriba y el VIP hasta abajo, este entrara al VIP porque la regla de implicit  deny no coincide al VIP==

Para cambiar este comportamiento hay que correr 

```
config firewall policy
	edit <deny policy id>
	set match-vip enable
	next
end
```

### ARP reply
[[ARP (pendiente)]] esta habilitado por default para las VIP, ya que de esta manera se pueden circunventar muchos errores de routeo a la hoar de acceder una VIP