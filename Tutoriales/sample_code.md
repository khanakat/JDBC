# Example Code

Este capítulo proporciona un ejemplo de cómo crear una aplicación JDBC simple. Esto le mostrará cómo abrir una conexión de base de datos, ejecutar una consulta SQL y mostrar los resultados.

Todos los pasos mencionados en este ejemplo de plantilla se explicarán en los capítulos siguientes de este tutorial.

## Creación de la aplicación JDBC

Hay los siguientes seis pasos involucrados en la construcción de una aplicación JDBC:

- **Importar los paquetes**: Requiere que incluya los paquetes que contienen las clases JDBC necesarias para la programación de la base de datos. La mayoría de las veces, basta con utilizar import java.sql. \*.

- Registrar el controlador JDBC: Requiere que inicialice un controlador para poder abrir un canal de comunicación con la base de datos.

- **Abrir una conexión**: Requiere usar el método DriverManager.getConnection () para crear un objeto Connection, que representa una conexión física con la base de datos.

- **Ejecutar una consulta**: Requiere usar un objeto de tipo Statement para construir y enviar una sentencia SQL a la base de datos.

- **Extraer datos del conjunto de resultados**: requiere que utilice el método ResultSet.getXXX () apropiado para recuperar los datos del conjunto de resultados.

- **Limpiar el entorno**: Requiere cerrar explícitamente todos los recursos de la base de datos en lugar de depender de la recolección de basura de la JVM.

Código de muestra
Este ejemplo de muestra puede servir como plantilla cuando necesite crear su propia aplicación JDBC en el futuro.

Este código de muestra se ha escrito en función del entorno y la configuración de la base de datos realizada en el capítulo anterior.

Copie y pegue el siguiente ejemplo en FirstExample.java, compile y ejecute de la siguiente manera:

```java
//STEP 1. Import required packages
import java.sql.*;

public class FirstExample {
   // JDBC driver name and database URL
   static final String JDBC_DRIVER = "com.mysql.jdbc.Driver";
   static final String DB_URL = "jdbc:mysql://localhost/EMP";

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
      System.out.println("Connecting to database...");
      conn = DriverManager.getConnection(DB_URL,USER,PASS);

      //STEP 4: Execute a query
      System.out.println("Creating statement...");
      stmt = conn.createStatement();
      String sql;
      sql = "SELECT id, first, last, age FROM Employees";
      ResultSet rs = stmt.executeQuery(sql);

      //STEP 5: Extract data from result set
      while(rs.next()){
         //Retrieve by column name
         int id  = rs.getInt("id");
         int age = rs.getInt("age");
         String first = rs.getString("first");
         String last = rs.getString("last");

         //Display values
         System.out.print("ID: " + id);
         System.out.print(", Age: " + age);
         System.out.print(", First: " + first);
         System.out.println(", Last: " + last);
      }
      //STEP 6: Clean-up environment
      rs.close();
      stmt.close();
      conn.close();
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
            stmt.close();
      }catch(SQLException se2){
      }// nothing we can do
      try{
         if(conn!=null)
            conn.close();
      }catch(SQLException se){
         se.printStackTrace();
      }//end finally try
   }//end try
   System.out.println("Goodbye!");
}//end main
}//end FirstExample
```

Ahora compilemos el ejemplo anterior de la siguiente manera:

```bash
C:\>javac FirstExample.java
C:\>
```

Cuando ejecuta FirstExample, produce el siguiente resultado:

```note
C:\>java FirstExample
Connecting to database...
Creating statement...
ID: 100, Age: 18, First: Zara, Last: Ali
ID: 101, Age: 25, First: Mahnaz, Last: Fatma
ID: 102, Age: 30, First: Zaid, Last: Khan
ID: 103, Age: 28, First: Sumit, Last: Mittal
C:\>
```

---

:octocat: [Repositorio en Github](https://github.com/FernandoCalmet/JDBC)

[![ko-fi](https://www.ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/T6T41JKMI)
