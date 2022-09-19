
# UT3 PROGRAMACIÓN BASADA EN LENGUAJES DE MARCAS CON CÓDIGO EMBEBIDO

## Índice

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
Ejemplo
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
No es necesario definir las funciones antes de usarlas excepto cuando están definida condicionalmente:
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


