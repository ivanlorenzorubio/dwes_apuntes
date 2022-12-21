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
El mapeado objeto-relacional (Object-Relational  mapping o ORM) es una técnica de programación para convertir datos entre un lenguaje de programación orientado a objetos y una base de datos relacional como motor de persistencia.

Esto posibilita el uso de las características propias de la orientación a objetos, podremos acceder directamente a los campos de un objeto para leer los datos de una base de datos o para insertarlos o modificarlos.

__Laravel incluye su propio sistema de  ORM  llamado Eloquent__. Para cada tabla de la base datos tendremos que definir su correspondiente modelo, el cual se utilizará para interactuar desde código con la tabla.

##### Definición de un modelo
Para definir un modelo que use Eloquent únicamente tenemos que crear una clase que herede  de la clase Model. Podemos hacerlas “a mano”, pero es mucho más fácil y rápido crear los  modelos usando el comando make:model de Artisan:
```php
php artisan make:model User
```
##### Nombre del modelo
En general el nombre de los modelos se pone en singular con la primera letra en mayúscula, mientras que el nombre de las tablas suele estar en plural.

Gracias a esto, al definir un modelo no es necesario indicar el nombre de la tabla asociada, sino que Eloquent automáticamente buscará la tabla transformando el nombre del modelo a  minúsculas y buscando su plural (en inglés).

En el ejemplo anterior que hemos creado el modelo User buscará la tabla de la base de  datos llamada users y en caso de no encontrarla daría un error.
Si la tabla tuviese otro nombre lo podemos indicar usando la propiedad protegida $table del modelo:
```php
class User extends Model
{
protected $table = 'my_users';
}
```
##### Clave primaria
Laravel también asume que cada tabla tiene declarada una clave primaria con el nombre id.
En el caso de que no sea así y queramos cambiarlo tendremos que sobrescribir el valor de la
propiedad	protegida	$primaryKey	del	modelo.
```php
protected	$primaryKey='my_id';.
```
##### Timestamps
Otra propiedad que en ocasiones tendremos que establecer son los timestamps automáticos.
Por defecto Eloquent asume  que todas las  tablas contienen  los campos updated_at y  created_at (los cuales los podemos añadir muy fácilmente con Schema añadiendo $table->timestamps() en la migración).

Estos campos se actualizarán automáticamente cuando se cree un nuevo  registro o se  modifique.

En el caso de que no queramos utilizarlos (y que no estén añadidos a la tabla) tendremos que  indicarlo en el modelo o de otra forma nos daría un error. 

Para indicar que no los actualice  automáticamente tendremos que modificar el valor de la propiedad pública $timestamps a false, por ejemplo: 
```php
public $timestamps = false;
```
Ejemplo:
```php
class User extends Model
{
    protected $table = 'my_users';  
    protected $primaryKey = 'my_id';  
    public $timestamps = false;
}
```
##### Uso de un modelo de datos
El sitio correcto donde realizar estas acciones es en el __controlador__, el cual se los tendrá que pasar a la vista ya preparados para su visualización.

Es importante indicar al inicio de la clase el espacio de nombres del modelo o modelos a  utilizar.
Por ejemplo, si vamos a usar los modelos User y Orders tendríamos que añadir:
use App\User;
use App\Orders;

##### Consultar datos
Para obtener todas las filas de la tabla asociada a un modelo usaremos __el método all()__:
Ejemplo:
```php
$users = User::all();  
foreach( $users as $user ) {
    echo $user->name;
}
```
Este método nos devolverá __un array de resultados__, donde __cada item del array__ será una __instancia del modelo User__. Gracias a esto al obtener un elemento del array podemos acceder  a los campos o columnas de la tabla como si fueran propiedades del objeto ($user->name).

También podremos utilizar __where, orWhere, first, get, orderBy, groupBy, having, skip, take,__  etc. para elaborar las consultas.

Eloquent también incorpora __el método find($id)__ para buscar un elemento a partir del  identificador único del modelo, por ejemplo:
```php
$user = User::find(1);
```
Si queremos que se lance una excepción cuando no se encuentre un modelo podemos utilizar  __los métodos findOrFail o firstOrFail__. Esto nos permite capturar las excepciones y mostrar un  error 404 cuando sucedan.
```php
$model = User::findOrFail(1);
$model = User::where('votes', '>', 100)->firstOrFail();
```
A continuación se incluyen otros ejemplos de consultas usando Eloquent con algunos de los  métodos:
```php
// Obtener 10 usuarios con más de 100 votos
$users = User::where('votes', '>', 100)->take(10)->get();
// Obtener el primer usuario con más de 100 votos
$user = User::where('votes', '>', 100)->first();
```
También	podemos	utilizar	los	métodos	agregados	para	calcular	el	total	de	registros obtenidos, o el máximo, mínimo, media o suma de una determinada columna. Por ejemplo:
```php
$count = User::where('votes', '>', 100)->count();
$price = Orders::max('price');
$price = Orders::min('price');
$price = Orders::avg('price');
$total = User::sum('votes');
```
##### Insertar datos
Para insertar un  dato en  una  tabla de la base de datos tenemos  que crear una __nueva instancia__ de dicho modelo, __asignar los valores__ y guardarlos con el __método save()__:
```php
$user = new User(); $user->name = 'Juan’; $user->save();
```
Para obtener el identificador asignado en la base de datos después de guardar, lo podremos recuperar accediendo al campo id del objeto que habíamos creado, por  ejemplo:
```php
$insertedId = $user->id;
```
##### Actualizar datos
Para actualizar una instancia de un modelo sólo tendremos que recuperar la instancia  que queremos actualizar, a continuación modificarla y por último guardar los datos:
```php
$user = User::find(1); $user->email = 'juan@gmail.com’; $user->save();
```
##### Borrar datos
Para borrar fila de una tabla en la base de datos tenemos que usar su __método delete()__:
```php
$user = User::find(1);	
$user->delete();
```
Si queremos  borrar un conjunto de resultados también podemos usar el método  delete():
```php
$affectedRows = User::where('votes', '>', 100)->delete();
```
### Seeders
Los	seeders	sirven para	rellenar la	base de	datos con datos iniciales.
Para ello se puede rellenar __el método run__ de database/seeds/DatabaseSeeder.php
Luego ejecutaremos la migración con:
```php
php artisan db:seed
```
Pero lo habitual es crear distintos	seeders para cada modelo de nuestra base de datos. Para crearlos usamos:
```php
php artisan make:seeder UserSeeder
```
Nos creará __una clase con el método run__. Ahí escribiremos los datos de creación de objetos.
Por	último,	desde el método	run	de la clase	DatabaseSeeder llamaremos a las clases Seeder creadas:
```php
$this->call(UserSeeder::class);
```


