# Database Connections

Una vez que haya instalado el controlador adecuado, es hora de establecer una conexión de base de datos mediante JDBC.

La programación necesaria para establecer una conexión JDBC es bastante sencilla. Estos son estos simples cuatro pasos:

- Importar paquetes JDBC: agregue declaraciones de importación a su programa Java para importar las clases requeridas en su código Java.

- Registrar controlador JDBC: este paso hace que la JVM cargue la implementación del controlador deseada en la memoria para que pueda cumplir con sus solicitudes JDBC.

- Formulación de URL de base de datos: esto es para crear una dirección con el formato adecuado que apunte a la base de datos a la que desea conectarse.

- Crear objeto de conexión: finalmente, codifique una llamada al método getConnection () del objeto DriverManager para establecer una conexión real a la base de datos.

## Importar paquetes JDBC

Las declaraciones de importación le dicen al compilador de Java dónde encontrar las clases a las que hace referencia en su código y se colocan al principio de su código fuente.

Para usar el paquete JDBC estándar, que le permite seleccionar, insertar, actualizar y eliminar datos en tablas SQL, agregue las siguientes importaciones a su código fuente:

```bash
import java.sql.* ;  // for standard JDBC programs
import java.math.* ; // for BigDecimal and BigInteger support
```

## Registrar controlador JDBC

Debe registrar el controlador en su programa antes de usarlo. El registro del controlador es el proceso mediante el cual el archivo de clase del controlador de Oracle se carga en la memoria, por lo que se puede utilizar como una implementación de las interfaces JDBC.

Necesita hacer este registro solo una vez en su programa. Puede registrar un conductor de dos formas.

## Enfoque I - Class.forName()

El enfoque más común para registrar un controlador es utilizar el método Class.forName () de Java, para cargar dinámicamente el archivo de clase del controlador en la memoria, que lo registra automáticamente. Este método es preferible porque le permite hacer que el registro del controlador sea configurable y portátil.

El siguiente ejemplo usa Class.forName () para registrar el controlador de Oracle:

```java
try {
   Class.forName("oracle.jdbc.driver.OracleDriver");
}
catch(ClassNotFoundException ex) {
   System.out.println("Error: unable to load driver class!");
   System.exit(1);
}
```

Puede usar el método **getInstance()** para evitar las JVM que no cumplen con las normas, pero luego tendrá que codificar dos excepciones adicionales de la siguiente manera:

```java
try {
   Class.forName("oracle.jdbc.driver.OracleDriver").newInstance();
}
catch(ClassNotFoundException ex) {
   System.out.println("Error: unable to load driver class!");
   System.exit(1);
catch(IllegalAccessException ex) {
   System.out.println("Error: access problem while loading!");
   System.exit(2);
catch(InstantiationException ex) {
   System.out.println("Error: unable to instantiate driver!");
   System.exit(3);
}
```

## Enfoque II - DriverManager.registerDriver()

El segundo enfoque que puede utilizar para registrar un controlador es utilizar el método estático **DriverManager.registerDriver()**.

Debe utilizar el método **registerDriver()** si está utilizando una JVM que no es compatible con JDK, como la proporcionada por Microsoft.

El siguiente ejemplo usa **registerDriver()** para registrar el controlador de Oracle:

```java
try {
   Driver myDriver = new oracle.jdbc.driver.OracleDriver();
   DriverManager.registerDriver( myDriver );
}
catch(ClassNotFoundException ex) {
   System.out.println("Error: unable to load driver class!");
   System.exit(1);
}
```

## Formulación de URL de base de datos

Una vez que haya cargado el controlador, puede establecer una conexión mediante el método **DriverManager.getConnection()**. Para una referencia sencilla, permítame enumerar los tres métodos **DriverManager.getConnection()** sobrecargados:

- getConnection(String url)

- getConnection(String url, Properties prop)

- getConnection(String url, String user, String password)

Aquí cada formulario requiere una **URL** de base de datos. Una URL de base de datos es una dirección que apunta a su base de datos.

La formulación de una URL de base de datos es donde ocurren la mayoría de los problemas asociados con el establecimiento de una conexión.

La siguiente tabla enumera los nombres de los controladores JDBC más populares y la URL de la base de datos.

| RDBMS  | JDBC driver name                | URL format                                          |
| ------ | ------------------------------- | --------------------------------------------------- |
| MySQL  | com.mysql.jdbc.Driver           | jdbc:mysql://hostname/ databaseName                 |
| ORACLE | oracle.jdbc.driver.OracleDriver | jdbc:oracle:thin:@hostname:port Number:databaseName |
| DB2    | COM.ibm.db2.jdbc.net.DB2Driver  | jdbc:db2:hostname:port Number/databaseName          |
| Sybase | com.sybase.jdbc.SybDriver       | jdbc:sybase:Tds:hostname: port Number/databaseName  |

Toda la parte resaltada en formato URL es estática y solo debe cambiar la parte restante según la configuración de su base de datos.

## Crear objeto de conexión

Hemos enumerado tres formas del método **DriverManager.getConnection()** para crear un objeto de conexión.

## Usar una URL de base de datos con un nombre de usuario y contraseña

La forma más utilizada de **getConnection()** requiere que pase una URL de base de datos, un nombre de usuario y una contraseña:

Suponiendo que está utilizando el controlador ligero de Oracle, especificará un valor host: puerto: databaseName para la parte de la base de datos de la URL.

Si tiene un host en la dirección TCP / IP 192.0.0.1 con un nombre de host amrood, y su oyente de Oracle está configurado para escuchar en el puerto 1521, y el nombre de su base de datos es EMP, la URL completa de la base de datos sería:

```bash
jdbc:oracle:thin:@amrood:1521:EMP
```

Ahora debe llamar al método **getConnection()** con el nombre de usuario y la contraseña adecuados para obtener un objeto de conexión de la siguiente manera:

```java
DriverManager.getConnection(String url);
```

Sin embargo, en este caso, la URL de la base de datos incluye el nombre de usuario y la contraseña y tiene la siguiente forma general:

```bash
jdbc:oracle:driver:username/password@database
```

Entonces, la conexión anterior se puede crear de la siguiente manera:

```java
String URL = "jdbc:oracle:thin:username/password@amrood:1521:EMP";
Connection conn = DriverManager.getConnection(URL);
```

## Usar una URL de base de datos y un objeto de propiedades

Una tercera forma del método DriverManager.getConnection () requiere una URL de base de datos y un objeto Propiedades -

```java
DriverManager.getConnection(String url, Properties info);
```

Un objeto de propiedades contiene un conjunto de pares de palabras clave y valores. Se utiliza para pasar las propiedades del controlador al controlador durante una llamada al método getConnection (). Para hacer la misma conexión hecha por los ejemplos anteriores, use el siguiente código:

```java
import java.util.*;

String URL = "jdbc:oracle:thin:@amrood:1521:EMP";
Properties info = new Properties( );
info.put( "user", "username" );
info.put( "password", "password" );

Connection conn = DriverManager.getConnection(URL, info);
```

## Cerrar conexiones JDBC

Al final de su programa JDBC, es necesario cerrar explícitamente todas las conexiones a la base de datos para finalizar cada sesión de la base de datos. Sin embargo, si lo olvida, el recolector de basura de Java cerrará la conexión cuando limpie los objetos obsoletos.

Confiar en la recolección de basura, especialmente en la programación de bases de datos, es una práctica de programación muy pobre. Debe tener el hábito de cerrar siempre la conexión con el método **close()** asociado con el objeto de conexión.

Para asegurarse de que una conexión esté cerrada, puede proporcionar un bloque 'finalmente' en su código. Un bloque finalmente siempre se ejecuta, independientemente de que ocurra una excepción o no.

Para cerrar la conexión abierta anterior, debe llamar al método **close()** de la siguiente manera:

```java
conn.close();
```

Cerrar explícitamente una conexión conserva los recursos del DBMS, lo que hará feliz al administrador de su base de datos.

---

:octocat: [Repositorio en Github](https://github.com/FernandoCalmet/JDBC)

[![ko-fi](https://www.ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/T6T41JKMI)
