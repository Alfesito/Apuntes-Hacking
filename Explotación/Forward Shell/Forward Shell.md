----
Tags: #firewall #pipe #FIFO #forwardshell #python #scripting #nc #netcat 

----
Esta técnica se utiliza cuando no se pueden establecer conexiones Reverse o Bind debido a reglas de Firewall implementadas en la red. Se logra mediante el uso de **mkfifo**, que crea un archivo **FIFO** (**named pipe**), que se utiliza como una especie de “**consola simulada**” interactiva a través de la cual el atacante puede operar en la máquina remota. En lugar de establecer una conexión directa, el atacante redirige el tráfico a través del archivo **FIFO**, lo que permite la comunicación bidireccional con la máquina remota.

------------
![[Captura de pantalla 2023-08-28 a las 11.42.35.png]]

---------
## Pipes con mkfifo

```bash
mkfifo input; tail -f input | /bin/sh 2>&1 > output
```

-------
## [TtyOverHttp](https://github.com/s4vitar/ttyoverhttp)
Con esta herramienta, evitamos tener que hacer uso de una reverse shell para obtener una TTY posteriormente completamente interactiva. A través de archivos '**mkfifo**', jugamos para simular una TTY interactiva sobre HTTP, logrando manejarnos sobre el sistema cómodamente sin ningún tipo de problema.

--------------
