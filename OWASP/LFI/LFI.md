---

---
--------------
Tags: #lfi

-------------
**Local File Inclusion** (**LFI**) es una vulnerabilidad de seguridad informática que se produce cuando una aplicación web **no valida adecuadamente** las entradas de usuario, permitiendo a un atacante **acceder a archivos locales** en el servidor web.

En muchos casos, los atacantes aprovechan la vulnerabilidad de LFI al abusar de parámetros de entrada en la aplicación web. Los parámetros de entrada son datos que los usuarios ingresan en la aplicación web, como las URL o los campos de formulario. Los atacantes pueden manipular los parámetros de entrada para incluir rutas de archivo local en la solicitud, lo que puede permitirles acceder a archivos en el servidor web. Esta técnica se conoce como “**Path Traversal**” y se utiliza comúnmente en ataques de LFI.

En el ataque de Path Traversal, el atacante utiliza caracteres especiales y caracteres de escape en los parámetros de entrada para navegar a través de los directorios del servidor web y acceder a archivos en ubicaciones sensibles del sistema.

Por ejemplo, el atacante podría incluir “**../**” en el parámetro de entrada para navegar hacia arriba en la estructura del directorio y acceder a archivos en ubicaciones sensibles del sistema.

Para prevenir los ataques LFI, es importante que los desarrolladores de aplicaciones web validen y filtren adecuadamente la entrada del usuario, limitando el acceso a los recursos del sistema y asegurándose de que los archivos sólo se puedan incluir desde ubicaciones permitidas. Además, las empresas deben implementar medidas de seguridad adecuadas, como el cifrado de archivos y la limitación del acceso de usuarios no autorizados a los recursos del sistema.

A continuación, se os proporciona el enlace directo a la herramienta que utilizamos al final de esta clase para abusar de los ‘**Filter Chains**‘ y conseguir así ejecución remota de comandos:
- **PHP Filter Chain Generator**: [https://github.com/synacktiv/php_filter_chain_generator](https://github.com/synacktiv/php_filter_chain_generator)

------------
## LFI básico
```php
<?php
	$filename = $_GET['filename'];
	include("/var/www/html" . $filename);
?>
```

## LFI con filtros
```php
<?php
	$filename = $_GET['filename'];
	$filenmae = str_replace("../", "", $filename)
	include("/var/www/html" . $filename);
?>
```
El archivo anterior es vulnerable a la petición http://localhost/index.php?filename=....//....//....//....//etc/passwd.

## LFI con null byte
Es vulnerable con  http://localhost/index.php?filename=....//....//....//....//etc/passwd%00.

## LFI con wrappers
### php://filter
```bash
echo file_get_contents("php://filter/convert.base64-encode|convert.base64-decode/resource=file:///etc/passwd");
```

El resultado de la sentencia anterior en el código en base64, por lo tanto, si lo copiamos en el archivo data:
```bash
cat data | base64 -d | sponge data
```

Hay más wrappers: https://book.hacktricks.xyz/v/es/pentesting-web/file-inclusion#lfi-rfi-utilizando-envoltorios-y-protocolos-php

### Filter chains
![[Pasted image 20230906133738.png]]
**php://memory** y **php://temp** son lujos de lectura-escritura que permiten almacenar datos temporales en una envoltura similar a un fichero.

![[Pasted image 20230906134048.png]]
Para luego usar la herramienta: [PHP Filter Chain Generator](https://github.com/synacktiv/php_filter_chain_generator)

-----------
# Enlaces
- https://book.hacktricks.xyz/v/es/pentesting-web/file-inclusion
