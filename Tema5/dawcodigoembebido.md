# UT5 DESARROLLO DE APLICACIONES WEB UTILIZANDO CÓDIGO EMBEBIDO

## Índice

## Autenticación y control de acceso

Es importante verificar la identidad de los dos extremos de una comunicación.

Se debería utilizar el protocolo HTTPS.

En la mayoría de las aplicaciones web existe un mecanismo de control de acceso que obligue al usuario a identificarse.

### Mecanismos de autentificación

El protocolo HTTP ofrece un método sencillo para autentificar a los usuarios:

* El servidor web debe proveer algún método para **definir los usuarios** que se utilizarán y cómo se pueden autentificar.
* Cuando **un usuario no autentificado intenta acceder** a un recurso restringido, **el servidor web responde con un error** de "Acceso no autorizado" (código 401).
* El **navegador** recibe el error y **abre una ventana** para solicitar al usuario que se autentifique mediante **su nombre y contraseña**.
* La información de autentificación del usuario **se envía al servidor**, que **la verifica**y decide si permite o no el acceso al recurso solicitado. **Esta información se mantiene en el navegador** para utilizarse en posteriores peticiones a ese servidor.
  
En Apache existe la utilidad **htpasswd**
* Fichero con usuarios y contraseñas
* Crearlo en un lugar no accesible por los usuarios del servidor web

Pero ¿cómo le indicamos a Apache qué recursos tienen acceso restringido? Respuesta: Fichero .htaccess

*Además tendrás que asegurarte de que en la configuración de Apache se utiliza **la directiva AllowOverride** para que se aplique correctamente la configuración que figura en el **fichero .htaccess**.

Desde PHP puedes acceder a la información de autentificación HTTP que ha introducido el usuario utilizando el array superglobal **$_SERVER**
**$_SERVER['PHP_AUTH_USER']** Nombre de usuario que se ha introducido.
**$_SERVER['PHP_AUTH_PW']** Contraseña introducida.
**$_SERVER['AUTH_TYPE']** Método HTTP usado para autentificar. Puede ser Basic o Digest

En PHP puedes usar**la función header** para forzar a que el servidor envíe un error de "Acceso no autorizado" (código 401). De esta forma no es necesario utilizar ficheros .htaccess para indicarle a Apache qué recursos están restringidos.

En su lugar, puedes añadir las siguientes líneas en tus páginas PHP:

```php
if (!isset($_SERVER['PHP_AUTH_USER'])) {
	header('WWW-Authenticate: Basic Realm="Contenido restringido"');
	header('HTTP/1.0 401 Unauthorized');
	echo "Usuario no reconocido!";
	exit();
}
```
Te habrás dado cuenta en el ejercicio anterior que ahora se solicitan credenciales HTTP, pero el servidor no verifica la información. Deberemos ser nosotros los que lo comprobemos.

```php
if ($_SERVER['PHP_AUTH_USER']!='usuario' || $_SERVER['PHP_AUTH_PW']!='contraseña') {
	header('WWW-Authenticate: Basic Realm="Contenido restringido"');
	header('HTTP/1.0 401 Unauthorized');
	echo "Usuario no reconocido!";
	exit();
}
```
Pregunta: 
¿Creéis que esto es correcto? ¿Veis algún inconveniente? ¿Se os ocurre alguna alternativa?

Una solución mejor es utilizar **un almacenamiento externo** para los nombres de usuario y sus contraseñas. Para esto podrías emplear un **fichero de texto**, o mejor aún, **una base de datos**. La información de autentificación podrá estar aislada en su propia base de datos, o compartir espacio de almacenamiento con los datos que utilice tu aplicación web.

Pregunta
¿Cómo almacenaríais la contraseña?

**MD5** es un método para generar un resumen de un texto o un documento, de tal forma que a partir del resumen obtenido no es posible recuperar el texto original, ni hallar otro texto a partir del cual se obtenga el mismo resumen. Se llama hash al resumen obtenido al aplicar una función hash. Una de las funciones hash más extendidas es MD5, que genera 128 bits como resumen (normalmente se representa mediante una cadena de texto de 28 caracteres o mediante 32 dígitos hexadecimales).

En PHP puedes usar la función md5 para calcular el hash MD5 de una cadena de texto

```php
$str = 'contraseña';
if (md5($str) == '4c882dcb24bcb1bc225391a602feca7c') {
    echo "Contraseña correcta";
}
```







