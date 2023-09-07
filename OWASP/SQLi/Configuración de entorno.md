----------
Tag: #sql #database #db #mysql #apache #php

-----------
Instalamos los paquetes necesarios:
```bash
apt install mariadb-server apache2 php-mysql
```

Iniciamos los distintos servicios y verificamos que se han iniciado (es posible que tengas que ejecutar estos comandos como root):
```bash
service mysql start
lsof -i:3306
service apache2 start
lsof -i:80

mysql -uroot -p
```

Creamos un usuario para la sesión php:
```msql
create user 'alfesito'@'localhost' identified by 'alfesito';
grant all privileges on hack4u.* to 'alfesito'@'localhost';
```

Creamos un archivo php, llamado searchUsers.php, para que realice peticiones GET a la base de datos buscando por el id y devolviendo el usuario que tiene dicho id:
```php
<?php
	$server = "localhost";
	$username = "alfesito";
	$password = "alfesito";
	$database = "hack4u";
	// Conexión a la base de datos
	$conn = new mysqli($server, $username, $password, $database);
	$id = $_GET['id'];
	echo "[+] Tu valor introducido es " . $id . "<br>----------------------------------------<br>";
	$data = mysqli_query($conn, "select username from users where id = '$id'");
	$response = mysqli_fetch_array($data);
	echo $response['username'];
?>
```

Para sqli basada en errores:
```php
<?php
	// ini_set('display_errors', 1);
	$server = "localhost";
    $username = "alfesito";
    $password = "alfesito";
    $database = "hack4u";
    // Conexión a la base de datos
    $conn = new mysqli($server, $username, $password, $database);
    $id = $_GET['id'];
    echo "[+] Tu valor introducido es " . $id . "<br>----------------------------------------<br>";
    $data = mysqli_query($conn, "select username from users where id = '$id'") or die(mysqli_error($conn));
    $response = mysqli_fetch_array($data);
    echo $response['username'];
?>
```
(Puede que sea necesario cambiar el archivo /etc/php/8.2/apache2/php.ini edite la linea "display_errors" = On, para que salgan errores en el navegador)

Podemos realizar una "sanitización" del campo id:
```php
<?php
	$server = "localhost";
	$username = "alfesito";
	$password = "alfesito";
	$database = "hack4u";
	// Conexión a la base de datos
	$conn = new mysqli($server, $username, $password, $database);
	// Sanitización (escapa la ')
	$id = mysqli_real_escape_string($_GET['id']);
	echo "[+] Tu valor introducido es " . $id . "<br>----------------------------------------<br>";
	$data = mysqli_query($conn, "select username from users where id = '$id'");
	$response = mysqli_fetch_array($data);
	echo $response['username'];
?>
```

Aunque a veces la programación del php puede tener errores
```php
<?php
    $server = "localhost";
    $username = "alfesito";
    $password = "alfesito";
    $database = "hack4u";
    // Conexión a la base de datos
    $conn = new mysqli($server, $username, $password, $database);
    // Sanitización (escapa la ')
    $id = mysqli_real_escape_string($conn, $_GET['id']);
    echo "[+] Tu valor introducido es " . $id . "<br>----------------------------------------<br>";
	$data = mysqli_query($conn, "select username from users where id = '$id'");
    $response = mysqli_fetch_array($data);
    echo $response['username'];
?>
```

(No siempre algunas inyecciones empiezan por comillas)

En el siguiente código no hay un output, pero si que tenemos un cambio del código de estado. En este caso, el código de respuesta es 200 si existe el id que se pone y 404 si no existe el id.
```php
<?php
	$server = "localhost";
	$username = "alfesito";
	$password = "alfesito";
	$database = "hack4u";
	// Conexión a la base de datos
	$conn = new mysqli($server, $username, $password, $database);
	$id = mysqli_real_escape_string($conn, $_GET['id']);
	$data = mysqli_query($conn, "select username from users where id = $id");
	$response = mysqli_fetch_array($data);
	echo $response['username'];
	
	if(! isset($response['username'])){
			http_response_code(404);
        }
?>
```
