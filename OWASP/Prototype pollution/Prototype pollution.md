---
tags:
  - prototypepollution
---
El **ataque Prototype Pollution** es una técnica de ataque que aprovecha las vulnerabilidades en la implementación de objetos en JavaScript. Esta técnica de ataque se utiliza para modificar la propiedad “prototype” de un objeto en una aplicación web, lo que puede permitir al atacante ejecutar código malicioso o manipular los datos de la aplicación.

En JavaScript, la propiedad “prototype” se utiliza para definir las propiedades y métodos de un objeto. Los atacantes pueden explotar esta característica de JavaScript para modificar las propiedades y métodos de un objeto y tomar el control de la aplicación.

El ataque de Prototype Pollution se produce cuando un atacante modifica la propiedad “prototype” de un objeto en una aplicación web. Esto se puede lograr mediante la manipulación de datos que se envían a través de formularios o solicitudes AJAX, o mediante la inserción de código malicioso en el código JavaScript de la aplicación.

Una vez que el objeto ha sido manipulado, el atacante puede ejecutar código malicioso en la aplicación, manipular los datos de la aplicación o tomar el control de la sesión de un usuario. Por ejemplo, un atacante podría modificar la propiedad “prototype” de un objeto de autenticación de usuario para permitir el acceso a una cuenta sin la necesidad de una contraseña.

El impacto de la explotación del ataque Prototype Pollution puede ser significativo, ya que los atacantes pueden tomar el control de la aplicación o comprometer los datos de los usuarios. Además, dado que el ataque se basa en vulnerabilidades en la implementación de objetos en JavaScript, puede ser difícil de detectar y corregir.

A continuación, se proporciona el enlace directo al proyecto de Github que utilizamos en esta clase para desplegar un entorno vulnerable con el que poder practicar:
- SKF-LABS: https://github.com/blabla1337/skf-labs
-----
![[Captura de pantalla 2023-09-13 a las 18.48.31.png]]
![[Captura de pantalla 2023-09-13 a las 18.54.16.png]]

------

----------
# Enlaces
- https://medium.com/node-modules/what-is-prototype-pollution-and-why-is-it-such-a-big-deal-2dd8d89a93c