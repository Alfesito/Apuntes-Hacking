---
tags:
  - nosqli
  - db
  - bbdd
  - python
  - scripting
---
------------
Las **inyecciones NoSQL** son una vulnerabilidad de seguridad en las aplicaciones web que utilizan bases de datos NoSQL, como MongoDB, Cassandra y CouchDB, entre otras. Estas inyecciones se producen cuando una aplicación web permite que un atacante envíe datos maliciosos a través de una consulta a la base de datos, que luego puede ser ejecutada por la aplicación sin la debida validación o sanitización.

La inyección NoSQL funciona de manera similar a la inyección SQL, pero se enfoca en las vulnerabilidades específicas de las bases de datos NoSQL. En una inyección NoSQL, el atacante aprovecha las consultas de la base de datos que se basan en documentos en lugar de tablas relacionales, para enviar datos maliciosos que pueden manipular la consulta de la base de datos y obtener información confidencial o realizar acciones no autorizadas.

A diferencia de las inyecciones SQL, las inyecciones NoSQL explotan la falta de validación de los datos en una consulta a la base de datos NoSQL, en lugar de explotar las debilidades de las consultas SQL en las bases de datos relacionales.

A continuación, se proporciona el enlace al proyecto de Github que nos descargamos para poner en práctica esta vulnerabilidad:
- Vulnerable-Node-App: https://github.com/Charlie-belmer/vulnerable-node-app
---------------
## Python script
![[Captura de pantalla 2023-09-11 a las 9.55.39.png]]
![[Captura de pantalla 2023-09-11 a las 9.57.07.png]]
-------------
## Enlaces
- https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/NoSQL%20Injection
