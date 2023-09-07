----------------
- Tags: #web #wordpress #reconocimiento #herramientas 
----------------
La herramienta **Wpscan**[^1] es conocido por realizar reconocimiento a un WordPress.

Su modo de uso es muy sencillo, podríamos realizar un escaneo a modo de prueba ejecutando el siguiente comando de bash:

```bash
wpscan --url http://localhost
```

Para enumerar usuarios válidos potenciales existentes:
```bash
wpscan --url http://localhost --enumerate u
```

Otra de las utilidades de esta herramienta es que es capaz de aplicar fuerza bruta para descubrir credenciales válidas en los paneles de autenticación.


--------------
## Referencias

[^1]: Enlace al proyecto en GitHub: [Enlace](https://github.com/wpscanteam/wpscan)
[^2] Este procedimiento puede ser abusado manualmente con **XMLRPC** 
