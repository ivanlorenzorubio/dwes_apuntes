# UT6 GENERACIÓN DINÁMICA DE PÁGINAS WEB 

## Índice

## Patrón arquitectónico MVC (Modelo-Vista-Controlador)

En muchas ocasiones se mezcla el código propio de la lógica de la aplicación, con el código necesario para crear el interface web que se presenta a los usuarios.

Existen varios métodos que permiten separar la lógica de presentación (en nuestro caso, la que
genera las etiquetas HTML) de la lógica de negocio, donde se implementa la lógica propia de
cada aplicación.

El más extendido es el patrón de diseño Modelo – Vista – Controlador (MVC). Este patrón pretende dividir el código en tres partes, dedicando cada una, a una función definida y diferenciada de las otras:

* __Modelo__: es el encargado de manejar los datos propios de la aplicación.Debe proveer mecanismos para obtener y modificar la información del mismo. Si la aplicación utiliza algún tipo de almacenamiento para su información (como un SGBD), tendrá que encargarse de almacenarla y recuperarla.

* __Vista__: es la parte del modelo que se encarga de la interacción con el usuario. En esta parte se encuentra el código necesario para generar el interface de usuario (en nuestro caso en HTML), según la información obtenida del modelo.

* __Controlador__: en este módulo se decide qué se ha de hacer, en función de las acciones del usuario con su interface. Con esta información,interactúa con el modelo para indicarle las acciones a realizar y, según el resultado obtenido, envía a la vista las instrucciones necesarias para generar el nuevo interface.

### Funcionamiento MVC

1. El usuario envía petición al controlador vía una URL
2. El controlador solicita al modelo los datos
3. El modelo devuelve los datos
4. El controlador selecciona una vista
5. Se devuelve la vista seleccionada al controlador
6. El controlador devuelve una vista (página php) que carga los datos del modelo seleccionado

![Modelo Vista Controlador](img/modelomvc.png)

Aunque se puede programar utilizando MVC por tu cuenta, es más habitual utilizar el patrón MVC en  conjunción con un framework o marco de desarrollo. Existen numerosos frameworks disponibles en PHP,  muchos de los cuales incluyen soporte para MVC.

__La arquitectura MVC__ separa __la lógica de negocio__ (el modelo) y __la  presentación__ (la vista) por lo que se consigue un mantenimiento  más sencillo de las aplicaciones.

Si por ejemplo una misma aplicación debe ejecutarse tanto en un  navegador estándar como un navegador de un dispositivo móvil,  solamente es necesario crear una vista nueva para cada  dispositivo; manteniendo el controlador y el modelo original. 

__El  controlador se encarga de aislar al modelo y a la vista de los  detalles del protocolo__ utilizado para las peticiones (HTTP, consola  de comandos, email, etc.). __El modelo se encarga de la abstracción  de la lógica relacionada con los datos__, haciendo que la vista y las  acciones sean independientes de, por ejemplo, el tipo de gestor de  bases de datos utilizado por la aplicación.

Para poder entender las ventajas de utilizar el patrón MVC, se va a  transformar una aplicación simple realizada con PHP en una  aplicación que sigue la arquitectura MVC

## Laravel

Laravel es un framework de código abierto para el  desarrollo de aplicaciones web en PHP que posee una  sintaxis simple y elegante.

Características:
* Creado en 2011 por Taylor Otwell.
* Está	inspirado	en	__Ruby	on	rails__	y	__Symfony__,	de	quien	posee  muchas dependencias.
* Está diseñado para desarrollar bajo el patrón __MVC__
* Posee	un	sistema	de	mapeado	de	datos	relacional	llamado __Eloquent ORM__.
* Utiliza un sistema de procesamiento de plantillas llamado __Blade__,  el cuál hace uso de la caché para darle mayor velocidad

### Instalación

Para la utilización de Laravel en primer lugar necesitamos tener instalado
lo siguiente:

* Un servidor web Apache
* PHP
* MySQL
* Composer(https://getcomposer.org)
    * Permite descargar y gestionar las dependencias del Framework.
    * Es un administrador de dependencias para PHP que nos permite descargar paquetes  desde un repositorio para agregarlo a nuestro proyecto.
    * Por defecto, se agregan a una carpeta llamada /vendor. De esta manera evitamos  hacer las búsquedas manualmente y el mismo Composer se puede encargar de  actualizar las dependencias que hayamos descargado por una nueva versión.
  * al instalar composer indicar la dirección del php ejemplo en windows
  
![Path en compososer de PHP](img/composerphp.png)
  
* La librería de laravel
  * Usando la terminal de GitBash vamos a la carpeta de publicación c:/xampp/htdocs y escribimos:
    * composer global require laravel /installer
  
![instalar laravel](img/instalaravel1.png)

Instala la última versión de Laravel;con el paso del tiempo conviene ir actualizando la version de Laravel con el comando: composer global update laravel/installer

* comprobamos la versión del instalador de lavarel

![version instalador laravel](img/instalaravel2.png)
      
* Adicionalmente también es recomendable instalar Node.js. Se instala la herramienta NPM (Node Package Manager), herramienta que permite instalar librerías de JavaScript, como BootStrap o jQuery. La página oficial es https://nodejs.org/es/download.
    * comprobamos la versión de node.js instalada

![version de node.js](img/instanodejs.png)

Ahora podemos comprobar que todo lo instalado es correcto y que podemos crear nuestro primer proyecto de prueba en Laravel (lpruebas), aunque anteriormente ya estabamos posicionados en la carpeta de publicación del XAMPP hasta ahora no era necesario pero ahora sí.

* laravel new lpruebas

![comando para crear proyecto laravel](img/crearproyecto.png)

También tenemos la opción de solo realizar en un paso la instalación de Laravel y la creación del proyecto, poniendo en la carpeta de publicación: 

* composer create-project laravel/laravel lpruebas

Entramos en la carpeta __public__  y comprobamos que se ha instalado correctamente

![pagina web de laravel](img/paginawebl.png)

Como entorno de desarrollo vamos a utilizar Visual Studio code e instalar la extensión Laravel extension pack que a su vez nos instala 12 plugins.

### Patrón de Laravel

Laravel se basa en un diseño  MVC

![patrón de laravel](img/patronlaravel.png)

### Estructura de Laravel

Vamos a ver la estructura de nuestro proyecto, para así entender que hay dentro de las principales carpetas:
* __app__ - contiene los controladores, modelos, vistas y  configuraciones de la aplicación. En esta carpeta escribiremos la  mayoría del código para que nuestra aplicación funcione. En la  carpeta Models tenemos un solo modelo: “User.php” que se crea  por defecto.
* __app/Http__
  * __Controllers__ son los que interaccionan con los modelos
  * __Midleware__ son filtros de seguridad cuando se envía una ruta, un formulario….
* __bootstrap__ son archivos del sistema. En esta carpeta se incluye el código que se carga para procesar cada una de las llamadas a nuestro proyecto. Normalmente, no tendremos que modificar nada de esta carpeta.
* __config__ todos los archivos de configuración del sistema
  * __app.php__ tenemos los namespace para acceder a las librerías internas de Laravel,si descargamos algo nuevo para nuestra aplicación hay que  instalar los namespace aquí
  * __database.php__ se configura la B.D. que ya contiene mysql, pero también  tiene sqlite y otras
  * __filesystems.php__ maneja discos internos en laravel: imágenes, videos,…
* __database__
  * __migrations__ se crea la estructura para la BD,tablas...
* __lang__ en esta carpeta se guardan archivos PHP que contienen arrays con los textos de nuestro sitio web en diferentes lenguajes; solo será necesario utilizarlo en caso de que se desee que la aplicación se pueda traducir
* __public__ es la única carpeta a la que los usuarios de la  aplicación pueden acceder. Todas las peticiones y solicitudes a la aplicación pasan por esta carpeta, ya que en ella se encuentra el index.php, este archivo es el que inicia todo el  proceso de ejecución del framework. En este directorio también se alojan los archivos CSS, Javascript, imágenes y  otros archivos que se quieran hacer públicos.
* __resources__
  * __views__ las vistas. Welcome.blade.php que es la página de inicio
* __routes__ las rutas. web.php es la más importante, aquí se definen las rutas para interpretar las solicitudes que el usuario hace al sistema
* __storage__ discos internos de laravel. En esta carpeta almacena toda la información interna necesaria para la ejecución de la web, como los archivos de sesión, la caché, la compilación de las vistas, metainformación y logs del sistema.
* __tests__ esta carpeta se utiliza para los ficheros con las pruebas automatizadas. Laravel incluye un sistema que facilita todo el proceso de pruebas con PHPUnit.
* __vendor__ En esta carpeta se alojan todas las librerías que  conforman el framework y sus dependencias
  
Además, en la carpeta raíz también encontramos tres ficheros importantes que utilizaremos:
* __.env__ se utiliza para almacenar los valores de configuración que son propios de la máquina o instalación actual, lo que nos permite cambiar fácilmente la configuración según la máquina en la que se instale y tener opciones distintas para producción, para distintos desarrolladores.
* __composer.json__ este fichero es el utilizado por Composer para realizar la instalación de Laravel. En una instalación inicial únicamente se especificará la instalación de un paquete (el propio framework de laravel), pero podemos especificar la instalación de otras librerías o paquetes externos que añadan funcionalidad a Laravel.
* __package.json__ en este fichero se encuentran algunas dependencias por parte cliente( Bootstrap o jQuery), y se encuentran preinstaladas en la carpeta node_modules.
Estos tres archivos no deben subirse a ningun repositorio(GitHub) incluir en el __.gitignore__, porque si importamos un proyecto en laravel, podemos regenerar las dependencias de PHP con __composer install__ y las dependencias de JavaScript con __npm install__, es decir, los archivos composer.json y package.json actúan como indice de dependencias de PHP y JavaScript, respectivamente.

### Funcionamiento básico

El funcionamiento básico que sigue Laravel tras una petición web a una URL de nuestro sitio es el siguiente:
1. Todas las peticiones entran a través del fichero public/index.php, el cual en primer lugar comprobará en el fichero de rutas (routes/web.php) si la URL es válida y en caso  de serlo a qué controlador tiene que hacer la petición.
2. A continuación se llamará al método del controlador asignado para dicha ruta. Como hemos visto, el controlador es el punto de entrada de las peticiones del usuario.
3. Accederá a la base de datos (si fuese necesario) a través de los "modelos" para obtener datos (o para añadir, modificar o eliminar).
4. Tras obtener los datos necesarios los preparará para pasárselos a la vista.
5. Por último se mostrará al usuario
   
  ![funcionamiento básico](img/funcionamiento.png) 

### Artisan

Laravel incluye un interfaz de línea de comandos (CLI,  Command line interface llamado __Artisan__.

Esta utilidad nos va a permitir realizar múltiples tareas necesarias durante el  proceso de desarrollo o despliegue a producción de una aplicación, por lo que nos facilitará y acelerará el trabajo.

Para ver una lista de todas las opciones que incluye Artisan podemos ejecutar el siguiente  comando en una consola o terminal del sistema en la carpeta raíz de nuestro proyecto:
__php artisan list__ (o php artisan)

vamos a la terminal de Visual Studio Code y ejecutamos

![listado comandos artisan](img/artisan.png)

Para ver un listado con todas las rutas que hemos definido en el fichero routes.php  podemos ejecutar el comando:

__php artisan route:list__

Esto nos mostrará una tabla con el método, la dirección, la acción y los filtros definidos  para todas las rutas.

A través de la __opción make__ podemos generar diferentes componentes de Laravel (controladores, modelos, filtros, etc.) como si fueran plantillas, esto nos ahorrará  mucho trabajo y podremos empezar a escribir directamente el contenido del  componente.Por ejemplo, para crear un nuevo controlador tendríamos que escribir:

__php artisan make:controller TaskController__

Dentro de Laravel, ya tenemos un servidor interno que haría la función de servidor web: 

__php artisan serve__

Esto pondrá en marcha un servidor en el puerto 8000; podremos acceder anuestro proyecto a través de la URL http://localhost:8000

### Rutas

Las rutas se tienen que definir en el fichero __routes/web.php__.

Cualquier ruta no definida en este fichero no será válida, generado una excepción (lo que devolverá un error 404).

Las rutas, en su forma más sencilla, pueden devolver __directamente un valor__  desde el propio fichero de rutas, pero también podrán __generar la llamada  a una vista o a un controlador__.

Con las rutas, construimos las URL amigables( son fáciles de recordar y más seuguras) de nuestra aplicación, es importante para el posicionamiento web. 

Son un mecamismo que nos permite establecer el controlador al que debemos enviar la petición de una determinada URL. 

Además de definir la URL de la petición, también indican el método con el cual se ha de hacer dicha petición. Los dos métodos más utilizados son GET y tipo POST.

En el archivo __routes/web.php__ inicialmente ya existe una ruta predefinida a la raiz del proyecto
```php
Route::get('/', function () {
    return view('welcome');
});
```
Lo que hace dicha ruta es llamar al método __view__, que carga una vista o archivo final HTML situado en __resources/views/welcome.blade.php__ 

para definir una ruta, realizamos la llamada al método get/post de la clase Route

```php
Route::get('/',function()){
  return 'Hola mundo';
});
```
Este código se lanzaría cuando se realice una petición tipo GET a la ruta raíz  de nuestra aplicación.

Para definir una ruta tipo POST se realizaría de la misma forma pero cambiando el verbo  GET por POST:
```php
Route::post('foo/bar',function(){
  return 'Hola mundo';
});
```
En este caso la ruta apuntaría a la dirección URL foo/bar
Si queremos que una ruta se defina a la vez para get y post lo podemos hacer añadiendo un  array con los tipos, de la siguiente forma:
```php
Route::match(array('GET','POST'),'/',function(){
  return 'Hola mundo';
});
```
O para cualquier tipo de petición HTTP utilizando el método any
```php
Route::any('/',function(){
  return 'Hola mundo';
});
```
Si queremos añadir parámetros a una ruta simplemente los tenemos que indicar
entre llaves {} a continuación de la ruta, de la forma:
```php
Route::get('user/{id}',function($id){
  return 'User'.$id;
});
```
Definimos la ruta /user/{id}, donde id es __obligatorio__ y puede ser cualquier valor.
En caso de no especificar ningún id se produciría un error. También podemos indicar  que un parámetro es opcional simplemente añadiendo el símbolo ? al final (y en  este caso no daría error si no se realiza la petición con dicho parámetro):
```php
Route::get('user/{name?}',function($name=null){
  return $name;
});
//tambien podemos poner algún valor por defecto
Route::get('user/{name?}',function($name='cic'){
  return $name;
});
```
Podemos definir rutas con alias o named routes, al definir la ruta, asociamos al método name el nombre que queramos. Esto es interesante cuando esta ruta forma parte de algún enlace en alguna página de nuestra aplicación
```php
Route::get('clientes',function(){return "listado";})->name('ruta_clientes');
```
Laravel también permite el uso de expresiones  regulares para validar los  parámetros que se le pasan a una ruta. Por ejemplo, para validar que un  parámetro esté formado sólo por letras o sólo por números:
```php
Route::get('user/{name?}',function($name){return $name;})->where('name','[A-Za-z]+');
```
### Controladores






