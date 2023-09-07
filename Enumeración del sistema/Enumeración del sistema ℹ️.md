--------
Tags: #sysenum #lse #pspy #herramientas #reconocimiento #peass #linenum

-------
Algunas de las herramientas de enumeración son:

- **LSE** (**Linux Smart Enumeration**): Es una herramienta de enumeración para sistemas Linux que permite a los atacantes obtener información detallada sobre la configuración del sistema, los servicios en ejecución y los permisos de archivo. LSE utiliza una variedad de comandos de Linux para recopilar información y presentarla en un formato fácil de entender. Al utilizar LSE, los atacantes pueden detectar posibles vulnerabilidades y encontrar información valiosa para futuros ataques.
- **Pspy**: Es una herramienta de enumeración de procesos que permite a los atacantes observar los procesos y comandos que se ejecutan en el sistema objetivo a intervalos regulares de tiempo. Pspy es una herramienta útil para la detección de malware y backdoors, así como para la identificación de procesos maliciosos que se ejecutan en segundo plano sin la interacción del usuario.

También se puede realizar algún script parecido a pspy con lo que se pueda ver las tareas que están corriendo en la máquina.
```bash
#!/bin/bash
old_process=$(ps -eo user, command) while true; do new_process=$(ps -eo user, command)

while true; do
	new_process=$(ps -eo user,command)
	diff <(echo "$old_process") <(echo "$new_process") | grep "[\>\<]" | grep -vE "command |kworker | procmon" old_process=$new_process
done
```


Asimismo, desarrollaremos un script en Bash ideal para detectar tareas y comandos que se ejecutan en el sistema a intervalos regulares de tiempo, abusando para ello del siguiente comando que nos chivará todo lo que necesitamos:

```bash 
ps -eo user,command
```

A continuación, se proporciona el enlace a estas herramientas:
- **LSE**: [https://github.com/diego-treitos/linux-smart-enumeration](https://github.com/diego-treitos/linux-smart-enumeration)
- **PSPY**: [https://github.com/DominicBreuker/pspy](https://github.com/DominicBreuker/pspy)
- **PEASS**: [https://github.com/carlospolop/PEASS-ng/tree/master](https://github.com/carlospolop/PEASS-ng/tree/master)
- **LinEnum**: [https://github.com/rebootuser/LinEnum](https://github.com/rebootuser/LinEnum)
