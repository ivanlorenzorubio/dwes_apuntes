
# UT3 PROGRAMACIÓN BASADA EN LENGUAJES DE MARCAS CON CÓDIGO EMBEBIDO

## Índice

  - [Estructuras de control](#estructuras-de-control)
    - [Sentencias condicionales](#sentencias-condicionales)
    - [Sentencias repetitivas o bucles](#sentencias-repetitivas-o-bucles)
  - [Funciones](#funciones)
    - [Inclusión de las funciones en ficheros externos](#inclusión-de-las-funciones-en-ficheros-externos)
    - [Creación y ejecución de funciones](#creación-y-ejecución-de-funciones)
    - [Argumentos en las funciones](#argumentos-en-las-funciones)
  - [Tipos de datos compuestos](#tipos-de-datos-compuestos)
    - [Arrays](#arrays)
      - [Recorrer un array](#recorrer-un-array)
  - [Funciones relacionadas con los tipos de datos completos](#funciones-relacionadas-con-los-tipos-de-datos-completos)
  - [Formularios](#formularios)
    - [métodos GET y POST](#métodos-get-y-post)
      - [Método GET](#método-get)
      - [Método POST](#método-post)
    - [Recuperación de información](#recuperación-de-información)
      - [Con GET](#con-get)
      - [Con POST](#con-post)
    - [Validación de datos](#validación-de-datos)

## Estructuras de control

son de dos tipos:
* Condicionales
* Repetitivas o bucles

### Sentencias condicionales

* __if /elseif /else__: definir una expresión para ejecutar o no la sentencia o conjunto de sentencias siguientes
```php
<?php
    if ($a<$b)
        print "a es menor que b";
    elseif ($a>$b)
        print "a es mayor que b";
    else
        print "a es igual a b";
?>
```
> no es necesario poner {} porque solo ejecuta una sentencia en cada caso
* __switch__: similar a enlazar varias sentencias if comparando una misma variable con diferentes valores
  
```php
<?php
    switch ($a){
        case 0:
            print " a vale 0";
            break;
        case 1:
            print " a vale 1";
            break;
        default:
            print "a no vale 0 ni 1";
    }
?>
```
### Sentencias repetitivas o bucles

* __while:__:define un bucle que se ejecuta mientras se cumpla una expresión. La expresión se evalúa antes de comenzar cada ejecución del bucle. 
```php
<?php
    $a=1;
    while ($a <8)
        $a+=3;
    print $a // el valor obenido es 10
    
?>
```
> no es necesario poner {} porque solo ejecuta una sentencia 

* __do ... while__:similar al bucle while, pero la expresión se evalúa al final, con lo cúal se asegura que la sentencia/as del bucle se ejecutan al menos una vez.
```php
<?php
    $a=1;
    do 
        $a -=3;
    while ($a >10)
    print $a; // el valor obtenido es -2
?>
``` 
> no es necesario poner {} porque solo ejecuta una sentencia 

* __for__:compuesto por tres expresiones:
sintaxis
```php
    for(expr1;expr2;expr3){
        sentencia o conjunto de sentencias
    }
``` 
1. expr1: se ejecuta una vez al comienzo del bucle.
2. expr2: se evalúa para saber si se debe ejecutar o no la/as sentencia/as
3. expr3: se ejecuta tras ejecutar todas las sentencias del bucle
```php
<?php
    for ($a=5;$a<10;$a+=3){
        print $a // se muestran los valores 5 y 8
        print "<br/>";
    }    
?>
``` 
:computer: Hoja03_PHP_01

## Funciones

* Permiten __asociar una etiqueta__ (el nombre de la función) con un bloque de código a ejecutar. 
* Al usar funciones estamos ayudando a __estructurar mejor el código__. 
* Las funciones permiten crear variables locales que no serán visibles fuera del cuerpo de las mismas. 

### Inclusión de las funciones en ficheros externos

En ocasiones resulta más cómodo agrupar las funciones en ficheros externos al que se hará referencia.

Formas de incorporar ficheros externos:
* __include__: evalúa el contenido del fichero que se indica y lo incluye como parte del fichero actual en el punto de realización de la llamada. Se puede indicar la ruta de forma absoluta o de forma relativa. Se toma como base la ruta que se especifica en la directiva include_path del fichero php.ini, si no se encuentra en esa ubicación, se buscará en el directorio actual
  
* __include_once__: si por equivocación incluyes más de una vez un mismo fichero, lo normal es que obtengas algún tipo de error (por ejemplo, al repetir una definición de una función) 
  
* __require__: si el fichero que queremos incluir no se encuentra, include da un aviso y continua la ejecución del guión. La diferencia más importante al usar require es que en ese caso, cuando no se puede incluir el fichero, se detiene la ejecución del guion.
   
* __require_once__: es la combinación de las dos anteriores. 

Ejemplo
```php
<?php
   include 'funciones.inc.php';
   print fecha();
?>
``` 
### Creación y ejecución de funciones
Para crear tus propias funciones se usa la palabra __function__

```php
<?php
   function precio_con_iva(){
        global $precio;
        $precio_iva=$precio*1.21;
        print "el precio con IVA es ".$precio_iva;
   }
   $precio=10;
   precio_con_iva();
?>
``` 
No es necesario definir las funciones antes de usarlas excepto cuando están definidas condicionalmente:
```php
<?php
    $iva=true;
    $precio =10;
    precio_con_iva();// Da error, pues aquí aún no está definida 
    if ($iva){
        function precio_con_iva(){
            global $precio;
            $precio_iva=$precio*1.21;
            print "el precio con IVA es ".$precio_iva;
        }
    }
    precio_con_iva(); // aquí ya no da error
?>
``` 
### Argumentos en las funciones

* Siempre es mejor utilizar __argumentos o parámetros al hacer la llamada__. Además, en lugar de mostrar el resultado en pantalla o guardar el resultado en una variable global, las funciones pueden __devolver un valor usando la sentencia return__. 
* Cuando en una función se encuentra una sentencia return, termina su procesamiento y devuelve el valor que se indica 
```php
<?php
    function precio_con_iva($precio){
        return $precio*1.21;
    }
    $precio=10;
    $precio_iva=precio_con_iva($precio);
    print "el precio con IVA es ".$precio_iva;
?>
``` 
* Al definir la función, puedes indicar __valores por defecto__ para los argumentos, de forma que cuando hagas una llamada a la función puedes no indicar el valor de un argumento; en este caso se toma el valor por defecto indicado. 
  
```php
<?php
    function precio_con_iva($precio,$iva=0.21){
        return $precio*(1+$iva);
    }
    $precio=10;
    $precio_iva=precio_con_iva($precio);
    print "el precio con IVA es ".$precio_iva;
?>
``` 
* Los argumentos anteriores se pasaban __por valor__. Esto es, cualquier cambio que se haga dentro de la función a los valores de los argumento no se reflejará fuera de la función. 
* Si quieres que esto ocurra debes definir el parámetro para que su valor se pase __por referencia__, añadiendo el símbolo __& antes de su nombre__. 
  
```php
<?php
    function precio_con_iva(&$precio,$iva=0.21){
        return $precio*(1+$iva);
    }
    $precio=10;
    precio_con_iva($precio);
    print "el precio con IVA es ".$precio."<br/>";
?>
``` 
:computer: Hoja03_PHP_02

## Tipos de datos compuestos

Un tipo de datos compuesto es aquel que te permite almacenar más de un valor. En PHP puedes utilizar dos tipos de datos compuestos: el array y el objeto.

### Arrays

 __Un array__ es un tipo de datos que nos permite almacenar varios valores. Cada miembro del array se almacena en una posición a la que se hace referencia utilizando un valor clave. Las claves pueden ser numéricas o asociativas.

```php
<?php
   //array numérico
   $modulos1=array(0=>"Programación",1=>"Base de datos",....,9=>"Desarrollo web en entorno servidor");

   //array asociativo
   $modulos2=array("PR"=>"Programación","BD"=>"Base de datos",...,"DWES"=>"Desarrollo web en entorno servidor");
?>
``` 
* la función __print_r__, nos muestra todo el contenido del array que le pasamos.
* Para hacer referencia a los elementos almacenados en un array, hay que utilizar el valor clave entre corchetes:  

```php

$modulos1[9];
$modulos2["DWES"];

``` 
* En PHP se pueden crear arrays de varias dimensiones almacenando otro array en cada uno de los elementos de un array.
```php
<?php
   
   $ciclos=array(
    "DAW"=>array("PR"=>"Programación","BD"=>"Base de datos",...,"DWES"=>"Desarrollo web en entorno servidor"),
    "DAM"=>array("PR"=>"Programación","BD"=>"Base de datos",...,"AD"=>"Acceso a datos")
   );
?>
``` 
* Para hacer referencia a los elementos almacenados en un array multidimensional, hay que indicar las claves para cada una de las dimensiones:
```php
$ciclos["DAW"]["DWES"];

``` 
* No hay que indicar el tamaño del array para crearlo. Ni siquiera es necesario indicar que una variable concreta es de tipo array:

```php
$modulos1[0]="Programación";
$modulos1[1]="Base de datos";
$modulos1[9]="Desarrollo web en entorno servivor";

``` 
* Ni siquiera es necesario especificar el valor de la clave. Si se omite, el array se irá llenando a partir de la última clave numérica existente, o de la posición 0 si no existe ninguna:

```php
$modulos1[]="Programación";
$modulos1[]="Base de datos";
$modulos1[]="Desarrollo web en entorno servivor";

``` 
#### Recorrer un array 

Las cadenas de texto o strings se pueden tratar como arrays en los que se almacena una letra en cada posición, siendo 0 el índice correspondiente a la primera letra, 1 el de la segunda, etc.

```php
$modulo = "Desarrollo web en entorno servidor";
// $modulo[3] == "a";
``` 
Para recorrer un array se puede utilizar __el bucle foreach__. Utiliza una variable temporal para asignarle en cada iteración el valor de cada uno de los elementos del arrays. Puedes usarlo de dos formas.
*  Recorriendo sólo los elementos:

```php
foreach ($modulos1 as $modulo)
		echo $modulo . "<br/>";

``` 
* O recorriendo los elementos y sus valores clave de forma simultánea:

```php
foreach ($modulos2 as $codigo => $modulo)
		echo "El código del módulo ".$modulo." es ".$codigo."<br/";
``` 
### Objetos

el lenguaje PHP original no se diseñó con características de orientación a objetos

Las características de POO que soportan  a partir de la versión PHP 5 incluyen:
* Métodos estáticos
* Métodos constructores y destructores
* Herencia
* Interfaces
* Clases abstractas

No se incluye:
* Herencia múltiple (Java NO tiene)
* Sobrecarga de métodos (Java SÍ tiene)
* Sobrecarga de operadores (Java NO tiene)

#### Creación de clases en PHP

La declaración de una clase en PHP se hace utilizando la palabra __class__. A continuación y entre llaves, deben figurar los miembros de la clase. Conviene hacerlo de forma ordenada, primero las propiedades o atributos, y después los métodos, cada uno con su código respectivo.
```php
    class Producto{
        private $codigo;
        public $nombre;
        public $PVP;
        public function muestra(){
            return "<p>"..$this codigo .."</p">;

        }

}
}


## Funciones relacionadas con los tipos de datos completos

En PHP existen funciones específicas para comprobar y establecer el tipo de datos de una variable, __gettype__ obtiene el tipo de la variable que se le pasa como parámetro y devuelve una cadena de texto, que puede ser array, boolean, double, integer, object, string, null, resource o unknown type.

También podemos comprobar si la variable es de un tipo concreto utilizando una de las siguientes funciones: **is_array(), is_bool(), is_float(), is_integer(), is_null(), is_numeric(), is_object(), is_resource(), is_scalar() e is_string()**. Devuelven true si la variable es del tipo indicado.
```php
<?php
  //asignamos a las dos variables la misma cadena de texto 
	$a = $b = "3.1416"; 
	settype($b, "float"); //y cambiamos $b a tipo float
	print "\$a vale $a y es de tipo ".gettype($a);
	print "<br />"; 
	print "\$b vale $b y es de tipo ".gettype($b);
?>
``` 
Si sólo nos interesa saber si una variable está definida y no es null, puedes usar __la función isset__. 
__La función unset__ destruye la variable o variables que se le pasa como parámetro.

:omputer: Hoja03_PHP_03

## Formularios

Son la forma de hacer llegar los datos a una aplicación web.

Van encerrados en las etiquetas
```html
 <form> 
    ....
 </form>
 ```
Dentro de las etiquetas de formulario se pueden incluir elementos sobre los que puede actuar el usuario. Ejemplo:

```html
<input>
<select>
<textarea>
<button>

```
En el __atributo action__ del FORM se indica la página a la que se enviarán los datos del formulario
En el __atributo method__ se especifica el método usado para enviar la información:
* __get__: los datos se envían en la URI utilizando el signo __?__ como separador.
* __post__: los datos se incluyen en el cuerpo del formulario y se envían usando el protocolo HTTP

### métodos GET y POST

Son métodos del protocolo HTTP para intercambio de información entre cliente y servidor.
* __get__:  el método más usado, sin embargo en formularios está en desuso. Pide al servidor que le devuelva al cliente la información identificada en la URI.
* __post__: se usa para enviar información a un servidor. Puede ser usado para completar un formulario de autenticación, por ejemplo.
  
La principal diferencia radica en la codificación de la información.

#### Método GET 

Utiliza la dirección URL que está formada por:

* **Protocolo**: especifica el protocolo de comunicación
* **Nombre de dominio**: nombre del servidor donde se aloja la información.
* **Directorios**: secuencia de directorios separados por /que indican la ruta en la que se encuentra el recurso
* **Fichero**: nombre del recurso al que acceder.
* Detrás de la URL se coloca el símbolo __?__ Para indicar el comienzo de las variables con valor que se enviarán, separadas cada una de ellas por __&__.
* __Ejemplo__: http://www.example.org/file/example1.php?v1=0&v2=3 

![imagen explicacion get](img/metodoget.png)

```html
<!DOCTYPE html>
<html> 
<head> 
	<title>Ejemplo Formularios</title> 
</head> 
<body>
	<h1>Ejemplo de procesado de formularios</h1> 
	<form action="ejemplo1.php" method="get">
        <label for="nombre">Introduzca su nombre:</label>
		<input type="text" id="nombre" name="nombre">
		<br/> 
        <label for="apellido">Introduzca sus apellidos:</label>
		<input type="text" id="apellido" name="apellidos"><br/> 
		<input type="submit" value="Enviar">
	</form>
</body> 
</html> 
```
#### Método POST

La información va codificada en el cuerpo de la petición HTTP y por tanto viaja oculta.
* No hay un método más seguro que otro, ambas tienen sus pros y sus contras.
* Es conveniente usarlos GET para la recuperación y POST para el envío de información.
  
```html
<!DOCTYPE html>
<html> 
<head> 
	<title>Ejemplo Formularios</title> 
</head> 
<body>
	<h1>Ejemplo de procesado de formularios</h1> 
	<form action="ejemplo1.php" method="post">
        <label for="nombre">Introduzca su nombre:</label>
		<input type="text" id="nombre" name="nombre">
		<br/> 
        <label for="apellido">Introduzca sus apellidos:</label>
		<input type="text" id="apellido" name="apellidos"><br/> 
		<input type="submit" value="Enviar">
	</form>
</body> 
</html> 
```
:computer: Hoja3_PHP_04(ejercicio1 y ejercicio2)

### Recuperación de información 

#### Con GET
En el caso del envío de información utilizando el método GET existe una variable especial __$_GET__, donde se almacenan todas las variables pasadas con este método. 

La forma de almacenar la información es __un array en el que el índice es el nombre asignado al elemento del formulario__

```php
$_GET[‘nombre’];
$_GET[‘apellidos’];
```

También se puede recuperar con la función __print_r__ que muestra el array completo.

```php
<?php
	print_r($_GET);
?>
```
#### Con POST

Al igual que en el método GET, se utiliza una variable interna __$_POST__, donde se almacenan todas las variables pasadas con este método. 

La forma de almacenar la información, también es __un array en el que el índice es el nombre asignado al elemento del formulario__

```php
$_POST[‘nombre’];
$_POST[‘apellidos’];
```

También se puede recuperar con la función __print_r__ que muestra el array completo.

Existe otra variable __$_REQUEST__ que contiene tanto el contenido de $_GET como $_POST

:computer: Hoja03_PHP_04(ejercicio3 y ejercicio4)

### Validación de datos
Siempre que sea posible, es preferible __validar los datos que se introducen en el navegador antes de enviarlos. Para ello deberás usar código en lenguaje Javascript__. 

Si por algún motivo hay datos que se tengan que validar en el servidor, por ejemplo, porque necesites comprobar que los datos de un usuario no existan ya en la base de datos antes de introducirlos, __será necesario hacerlo con código PHP en la página que figura en el atributo action del formulario__. 

```html
<html> 
    <head> <title>Desarrollo Web</title> </head> 
    <body>
```
```php 
        <?php 
            if (isset($_POST['enviar'])) { 
	            $nombre = $_POST['nombre']; 
	            $modulos = $_POST['modulos'];
	            print "Nombre: ".$nombre."<br />"; 
		        foreach ($modulos as $modulo) { print "Modulo: ".$modulo."<br />"; }
            } 
            else {
        ?>
```
```html 
	    <form name="input" action="" method="post"> 
	        <label for="nombre">Nombre del alumno: </label>
            <input type="text" name="nombre" id="nombre" />
             <br />
	        <p>Módulos que cursa:</p> 
	        <input type="checkbox" name="modulos[]" value="DWES" />Desarrollo web en entorno servidor<br /> 
	        <input type="checkbox" name="modulos[]" value="DWEC" />Desarrollo web en entorno cliente<br /> <br /> 
	        <input type="submit" name="enviar" value="Enviar" />
	    </form>
```
```php
        <?php  	}   ?>
```
```html
    </body> 
</html>
```
> en el atributo action también se puede poner 
> ```php
> <?php echo $_SERVER['PHP_SELF'];
> ?>
>```
 
 :computer: Hoja03_PHP_04

