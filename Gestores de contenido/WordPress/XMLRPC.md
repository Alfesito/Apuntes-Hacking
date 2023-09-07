----------------
- Tags: #web #scripting #wordpress 
--------------------------
Si quisierams realizar fuerzabruta en un WordPress de la misma forma que hace Wpsan pero de forma manual para descubrir credenciales validas sería necesario realizar una peticion POST al archivo xmlrpc.php tramitando una estructura en XML tal que así:

```xml
<Files "xmlrpc.php">
  Require all denied
</Files>
<Directory "/path/to/site-that-needs-xmlrpc">
  <Files "xmlrpc.php">
    Require all granted
  </Files>
</Directory>
<Directory "/path/to/other-site-that-needs-xmlrpc">
  <Files "xmlrpc.php">
    Require all granted
  </Files>
</Directory>
```


