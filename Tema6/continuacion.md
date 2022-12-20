### Base de datos
Laravel facilita la configuración y el uso de diferentes tipos  de base de datos: MySQL,PostgreSQL, SQLite y SQL  Server.

En el fichero de configuración __config/database.php__  tenemos que indicar todos los parámetros de acceso a  nuestras bases de datos y además especificar cuál es la  conexión que se utilizará por defecto.

En Laravel podemos hacer uso de varias bases de datos a  la vez, aunque sean de distinto tipo. Por defecto  se  accederá a la que especifiquemos en la configuración y si  queremos acceder a otra conexión lo tendremos que indicar  expresamente al realizar la consulta.

#### Configuración
Lo primero que tenemos  que hacer para trabajar con bases de datos es completar la
configuración.

Si editamos el fichero con la configuración __config/database.php__ podemos ver en primer lugar  la siguiente línea: 'default' => env('DB_CONNECTION', 'mysql'),

Este valor indica el tipo de base de datos a utilizar  por defecto. El método   env('DB_CONNECTION', 'mysql') lo que hace es obtener el valor de la variable  DB_CONNECTION del __fichero .env__.

En el fichero de configuración __config/database.php__, dentro de la sección connections, podemos encontrar  todos los campos utilizados para configurar cada tipo de base de datos, en concreto la base  de datos tipo mysql tiene los siguientes valores:

```php
 'mysql' => [
            'driver' => 'mysql',
            'url' => env('DATABASE_URL'),
            'host' => env('DB_HOST', '127.0.0.1'),
            'port' => env('DB_PORT', '3306'),
            'database' => env('DB_DATABASE', 'forge'),
            'username' => env('DB_USERNAME', 'forge'),
            'password' => env('DB_PASSWORD', ''),
            'unix_socket' => env('DB_SOCKET', ''),
            'charset' => 'utf8mb4',
            'collation' => 'utf8mb4_unicode_ci',
            'prefix' => '',
            'prefix_indexes' => true,
            'strict' => true,
            'engine' => null,
            'options' => extension_loaded('pdo_mysql') ? array_filter([
                PDO::MYSQL_ATTR_SSL_CA => env('MYSQL_ATTR_SSL_CA'),
            ]) : [],
        ],

```
Podríamos modificar directamente los valores del archivo config/database.php,  pero cuando trabajamos en el desarrollo de una aplicación, la mayoría de las  veces,__las credenciales de la base de datos de nuestro entorno local es diferente  de las credenciales de nuestro entorno de producción o pruebas__.

Por lo que no es conveniente tener datos de configuración variables dentro de  nuestro código.

Para solventar este problema podemos hacer uso de las variables de entorno __contenidas en el fichero .env__

Así que para configurar la base de datos vamos a cambiar las siguientes líneas en  el __fichero .env de la raíz del proyecto__:

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=(nombre de la base de datos)laravel_zoologico
DB_USERNAME=(nombre de usuario)root
DB_PASSWORD=(contraseña de acceso)

#### Migraciones: Schema Builder
Laravel utiliza __las migraciones__ para poder definir y crear las  tablas de la base de datos desde código, y de esta manera tener  un control de las versiones de las mismas.

Permiten que un equipo trabaje sobre una base de datos  añadiendo y modificando campos,manteniendo un histórico de los  cambios realizados y del estado actual de la base de datos. 

Las  migraciones se utilizan de forma conjunta con __la herramienta Schema builder__ para gestionar el esquema de base de datos de la  aplicación.

Para poder empezar a trabajar con las migraciones es necesario en  primer lugar crear la tabla de migraciones. 
Para esto tenemos que  ejecutar el siguiente comando de Artisan:
```php 
php artisan migrate
```
Para crear una nueva migración se utiliza el comando de Artisan make:migration,
al cuál le pasaremos el nombre del fichero a crear:
```php 
php artisan make:migration create_users_table
```
Esto nos creará un fichero de migración en la carpeta database/migrations con el  nombre __*TIMESTAMP*_create_users_table.php__.

Al añadir un timestamp a las migraciones el sistema sabe el orden en el que tiene
que ejecutar (o deshacer) las mismas.

Si lo que queremos es añadir una migración que modifique los campos de una  tabla existente tendremos que ejecutar el siguiente comando:
```php 
php artisan make:migration add_votes_to_user_table --table=users
```
En este caso se creará también un fichero en la misma carpeta, con el nombre __*TIMESTAMP*_add_votes_to_user_table.php__ preparado para modificar los campos de dicha tabla.

Por defecto, al indicar el nombre del fichero de migraciones se suele seguir siempre  el mismo patrón (aunque el realidad el nombre es libre).
* Si es una migración que  crea una tabla el nombre tendrá que ser __create_*table-name*_table__ 
* Si es una  migración que modifica una tabla será __*action*_to_*table-name*_table__

##### Estructura de una migración

El fichero o clase PHP generada para una migración siempre  contiene __los métodos up y down__.
* En el método up es donde tendremos crear o modificar la tabla
* En el método down tendremos que deshacer los cambios que se hagan  en el up (eliminar la tabla o eliminar el campo que se haya añadido).
  
Esto nos permitirá poder ir añadiendo y eliminando cambios sobre la  base de datos y tener un control o histórico de los mismos.

Para especificar la tabla a crear o modificar, así como las columnas  y tipos de datos de las mismas, se utiliza __la clase Schema__.

Esta clase tiene una serie de métodos que nos permitirá especificar  la estructura de las tablas independientemente del sistema de base  de datos que utilicemos.

##### Crear y borrar una tabla
En __la sección up__ para añadir una nueva tabla a la base de datos se utiliza el siguiente
constructor:
```php
 Schema::create('users', function (Blueprint $table) {
            $table->id();
            $table->string('name');
 });
```
Donde el primer argumento es el nombre de la tabla y el segundo es una  función que recibe como  parámetro __un objeto del tipo Blueprint__ que  utilizaremos para configurar las columnas de la tabla.
En __la sección down__ de la migración tendremos que eliminar la tabla que  hemos creado, para esto usaremos el método:
```php
Schema::dropIfExists('users');
```
Al crear una migración con el comando de Artisan __make:migration__ ya nos  viene este código añadido por defecto, la creación y eliminación de la  tabla que se ha indicado y además se añaden un par de columnas por  defecto (id y timestamps).


##### Añadir columnas
El constructor Schema::create recibe como segundo parámetro una función que nos permite especificar las columnas que va a tener dicha tabla.
En esta función podemos ir añadiendo todos los campos que queramos,  indicando para cada uno de ellos su tipo y nombre, y además si queremos  también podremos indicar una serie de modificadores como  valor por  defecto, índices, etc. 
Ejemplo:
```php
 Schema::create('users', function (Blueprint $table) {
            $table->id();
            $table->string('name');
            $table->string('email')->unique();
            $table->timestamp('email_verified_at')->nullable();
            $table->string('password');
            $table->rememberToken();
            $table->timestamps();
        });
```
Podemos consultar [todos los tipos de datos](https://laravel.com/docs/master/migrations#creating-columns)

##### Añadir índices
Schema soporta los siguientes tipos de índices:
|Comando| Descripción |
| ------------- | ------------------------------ |
|$table->primary('id');|Añadir una clave primaria|
|$table->primary(array('first','last'));|Definir una clave primaria compuesta|
|$table->unique('email');|Definir el campo como UNIQUE|
|$table->index('state');|Añadir un índice a una columna|

En la tabla se especifica como añadir estos índices  después de crear el campo, pero también permite  indicar estos índices a la vez que se crea el campo como figura en el ejemplo anterior al definir el campo email.

##### Claves ajenas
Con Schema también podemos definir claves ajenas entre tablas:
```php
$table->integer('user_id')->unsigned();
$table->foreign('user_id')->references('id')->on('users');
```
En este ejemplo en primer lugar añadimos la columna "user_id" de tipo UNSIGNED  INTEGER (siempre tendremos que crear primero la columna sobre la que se va a  aplicar la clave ajena).

A continuación creamos la clave ajena entre la columna "user_id" y la columna "id"  de la tabla "users".
También podemos especificar las acciones que se tienen que realizar para "on  delete" y "on update":
```php
$table->foreign('user_id')->references('id')->on('users')->onDelete('cascade');
```
Para eliminar una clave ajena, en __el método down__ de la migración tenemos que utilizar el siguiente código:
```php
$table->dropForeign('posts_user_id_foreign');
```
Para indicar la clave ajena a eliminar tenemos que seguir el siguiente patrón para especificar el __nombre *tabla*_*columna*_foreign__ donde:
* "tabla" es el nombre de  la tabla actual
* "columna" el nombre de la columna sobre la que se creo la clave  ajena.

##### Ejecutar migraciones
Después de crear una migración y de definir los campos de la tabla tenemos que lanzar la
migración con el siguiente comando:
```php
php artisan migrate
```
>Si nos aparece el error "class not found" lo podremos solucionar indicando a composer que vuelva a compilar el autocargador. Desde la carpeta del proyecto, ejecutamos este comando:
```php
composer dump-autoload
```
El comando migrate aplicará la migración sobre la base de datos. Si hubiera más de una migración pendiente se ejecutarán todas.

Para cada migración se llamará a su método up para que cree o modifique la base de datos.  

Posteriormente en caso de que queramos deshacer los últimos cambios podremos ejecutar:
```php
php artisan migrate:rollback
```
O si queremos deshacer todas las migraciones:
```php
php artisan migrate:reset
```
Un comando interesante cuando estamos desarrollando un nuevo sitio web es migrate:fresh, el
cual deshará todos los cambios y volver a aplicar las migraciones:
```php
php artisan migrate:fresh
```
Además si queremos comprobar el estado de las migraciones, para ver las que ya están  instaladas y las que quedan pendientes, podemos ejecutar:
```php
php artisan migrate:status
```
:computer: Hoja06_MVC_07

#### Modelos de datos mediante ORM
