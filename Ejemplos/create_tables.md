# Create Tables

Este capítulo proporciona un ejemplo sobre cómo crear una tabla usando la aplicación JDBC. Antes de ejecutar el siguiente ejemplo, asegúrese de tener lo siguiente en su lugar:

- Para ejecutar el siguiente ejemplo, puede reemplazar el nombre de usuario y la contraseña con su nombre de usuario y contraseña reales.

- Su MySQL o cualquier base de datos que esté utilizando está en funcionamiento.

## Pasos requeridos

Los siguientes pasos son necesarios para crear una nueva base de datos utilizando la aplicación JDBC:

Importar los paquetes: Requiere que incluya los paquetes que contienen las clases JDBC necesarias para la programación de la base de datos. La mayoría de las veces, basta con utilizar `import java.sql.*`.

Registrar el controlador JDBC: Requiere que inicialice un controlador para poder abrir un canal de comunicaciones con la base de datos.

Abrir una conexión: Requiere usar el método `DriverManager.getConnection()` para crear un objeto Connection, que representa una conexión física con un servidor de base de datos.

Ejecutar una consulta: Requiere el uso de un objeto de tipo Declaración para construir y enviar una declaración SQL para crear una tabla en una base de datos seleccionada.

Limpiar el entorno: Requiere cerrar explícitamente todos los recursos de la base de datos en lugar de depender de la recolección de basura de la JVM.

## Código de muestra

Copie y pegue el siguiente ejemplo en JDBCExample.java, compile y ejecute de la siguiente manera:

```java
//STEP 1. Import required packages
import java.sql.*;

public class JDBCExample {
   // JDBC driver name and database URL
   static final String JDBC_DRIVER = "com.mysql.jdbc.Driver";
   static final String DB_URL = "jdbc:mysql://localhost/STUDENTS";

   //  Database credentials
   static final String USER = "username";
   static final String PASS = "password";

   public static void main(String[] args) {
   Connection conn = null;
   Statement stmt = null;
   try{
      //STEP 2: Register JDBC driver
      Class.forName("com.mysql.jdbc.Driver");

      //STEP 3: Open a connection
      System.out.println("Connecting to a selected database...");
      conn = DriverManager.getConnection(DB_URL, USER, PASS);
      System.out.println("Connected database successfully...");

      //STEP 4: Execute a query
      System.out.println("Creating table in given database...");
      stmt = conn.createStatement();

      String sql = "CREATE TABLE REGISTRATION " +
                   "(id INTEGER not NULL, " +
                   " first VARCHAR(255), " +
                   " last VARCHAR(255), " +
                   " age INTEGER, " +
                   " PRIMARY KEY ( id ))";

      stmt.executeUpdate(sql);
      System.out.println("Created table in given database...");
   }catch(SQLException se){
      //Handle errors for JDBC
      se.printStackTrace();
   }catch(Exception e){
      //Handle errors for Class.forName
      e.printStackTrace();
   }finally{
      //finally block used to close resources
      try{
         if(stmt!=null)
            conn.close();
      }catch(SQLException se){
      }// do nothing
      try{
         if(conn!=null)
            conn.close();
      }catch(SQLException se){
         se.printStackTrace();
      }//end finally try
   }//end try
   System.out.println("Goodbye!");
}//end main
}//end JDBCExample
```

Ahora, compilemos el ejemplo anterior de la siguiente manera:

```bash
C:\>javac JDBCExample.java
C:\>
```

Cuando ejecuta JDBCExample, produce el siguiente resultado:

```bash
C:\>java JDBCExample
Connecting to a selected database...
Connected database successfully...
Creating table in given database...
Created table in given database...
Goodbye!
C:\>
```

---

:octocat: [Repositorio en Github](https://github.com/FernandoCalmet/JDBC)

[![ko-fi](https://www.ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/T6T41JKMI)
