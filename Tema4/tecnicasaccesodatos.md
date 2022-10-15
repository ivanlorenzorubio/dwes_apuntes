# UT4 UTILIZACIÓN DE TÉCNICAS DE ACCESO A DATOS

## Índice

## Acceso a bases de datos desde PHP

PHP soporta más de 15 **sistemas gestores de bases de datos**: SQLite, Oracle, SQL Server, PostgreSQL, IBM DB2, MySQL, etc
Hasta la versión 5 de PHP, el acceso a las bases de datos se hacía principalmente utilizando extensiones específicas para cada sistema gestor de base de datos (**extensiones nativas**).

A partir de la versión 5 de PHP se introdujo en el lenguaje una extensión para acceder de una forma común a distintos sistemas gestores: **PDO**.

La gran ventaja de PDO es que podemos seguir utilizando una misma sintaxis aunque cambiemos el motor de nuestra base de datos.

Mientras PDO ofrece un conjunto común de funciones, las extensiones nativas normalmente ofrecen más potencia (acceso a funciones específicas de cada gestor de base de datos) y en algunos casos también mayor velocidad.

## MySQL /MariaDB

**MySQL** es un sistema de gestión de bases de datos relacional desarrollado bajo **licencia dual**: Licencia pública general/Licencia comercial por Oracle Corporation y está considerada como la base de datos de código abierto más popular del mundo,​​ y una de las más populares en general junto a Oracle y Microsoft SQL Server.

![Logo de MySQL](img/mysql.png)

**MariaDB** es un sistema de gestión de bases de datos derivado de MySQL con licencia GPL (General Public License). Es desarrollado por Michael (Monty) Widenius —fundador de MySQL—, la fundación MariaDB y la comunidad de desarrolladores de software libre.

![Logo de MariaDB](img/mariadb.png)

Ambas bases de datos incorporan múltiples motores de almacenamiento siendo alguno de ellos:

**MyISAM** (**Aria** en MariaDB): motor que se utiliza por defecto. Muy rápido pero a cambio no contempla integridad referencial ni tablas transaccionales. 

**InnoDB** (**XtraDB** en MariaDB): es un poco más lento pero sí soporta tanto integridad referencial como tablas transaccionales.

## MySQLi

Esta extensión se desarrolló para aprovechar las ventajas que ofrecen las versiones 4.1.3 y posteriores de MySQL, y viene incluida con PHP a partir de la versión 5. 

Ofrece un interface de programación dual, pudiendo accederse utilizando objetos o funciones.
Por ejemplo, para establecer una conexión con un servidor MySQL y consultar su versión, podemos utilizar cualquiera de las siguientes formas:

- Utilizando objetos

```php
//utilizando constructores y métodos de la programación orientada a objetos
$conexion = new mysqli('localhost', 'usuario', 'contraseña', 'base_de_datos');
print $conexion‐>server_info;
```
- Utilizando funciones
  
 ```php 
// utilizando llamadas a funciones 
$conexion = mysqli_connect('localhost', 'usuario', 'contraseña', 'base_de_datos');
print mysqli_get_server_info($conexion);
```
## PHP Data Objects(PDO)

PDO se basa en las características de orientación a objetos de PHP pero, al contrario que la extensión MySQLi, no ofrece un interface de programación dual. 

Para acceder a las funcionalidades de la extensión hay que emplear los objetos que ofrece, con sus métodos y propiedades. No existen funciones alternativas.

### PDO: establecimiento de conexiones





