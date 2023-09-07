------
Tags: #firewall #iptables 

-------
## Definir reglas con iptables

Intalación:
```bash
apt install iptables
```

En caso de error, busca en internet, ya que seguramente hay que utilizar unas flags adicionales. 
Para borrar todas las reglas:
```bash
iptables --flush
```

Configurar una regla para aceptar toda la comunicación TCP que tengo destino el puerto 80:
```bash
iptables -A INPUT -p tcp --dport 80 - j ACCEPT
iptables -A OuTPUT -p tcp --dport 80 - j ACCEPT
```

Para bloquear todo lo demás:
```bash
iptables -A INPUT -p tcp --dport 0:65535 -m contrack --ctstate NEW - j DROP
iptables -A OUTPUT -o eth0 - j DROP
```
