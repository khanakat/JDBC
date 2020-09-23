# Exceptions Handling

El manejo de excepciones le permite manejar condiciones excepcionales, como errores definidos por programa, de manera controlada.

Cuando ocurre una condición de excepción, se lanza una excepción. El término arrojado significa que la ejecución del programa actual se detiene y el control se redirige a la cláusula catch aplicable más cercana. Si no existe una cláusula catch aplicable, la ejecución del programa finaliza.

El manejo de excepciones de JDBC es muy similar al manejo de excepciones de Java, pero para JDBC, la excepción más común con la que lidiará es **java.sql.SQLException**.

## Métodos de SQLException

Una SQLException puede ocurrir tanto en el controlador como en la base de datos. Cuando ocurre una excepción de este tipo, un objeto de tipo SQLException se pasará a la cláusula catch.

El objeto SQLException pasado tiene los siguientes métodos disponibles para recuperar información adicional sobre la excepción:

Método | Descripción
--- | ---
getErrorCode() | Obtiene el número de error asociado con la excepción.
getMessage() | Obtiene el mensaje de error del controlador JDBC para un error, manejado por el controlador u obtiene el número de error de Oracle y el mensaje para un error de base de datos.
getSQLState() | Obtiene la cadena XOPEN SQLstate. Para un error del controlador JDBC, este método no devuelve información útil. Para un error de base de datos, se devuelve el código XOPEN SQLstate de cinco dígitos. Este método puede devolver un valor nulo.
getNextException() | Obtiene el siguiente objeto de excepción en la cadena de excepciones.
printStackTrace() | Imprime la excepción actual, o arrojable, y su seguimiento a un flujo de error estándar.
printStackTrace(PrintStream s) | Imprime este elemento arrojadizo y su seguimiento en el flujo de impresión que especifique.
printStackTrace(PrintWriter w) | Imprime este elemento arrojadizo y es un seguimiento del escritor de impresión que especifique.

Al utilizar la información disponible en el objeto Exception, puede detectar una excepción y continuar con su programa de manera adecuada. Aquí está la forma general de un bloque try:

```java
try {
   // Your risky code goes between these curly braces!!!
}
catch(Exception ex) {
   // Your exception handling code goes between these
   // curly braces, similar to the exception clause
   // in a PL/SQL block.
}
finally {
   // Your must-always-be-executed code goes between these
   // curly braces. Like closing database connection.
}
```

### Ejemplo

Estudie el siguiente código de ejemplo para comprender el uso de los bloques try....catch...finally

```java
//STEP 1. Import required packages
import java.sql.*;

public class JDBCExample {
   // JDBC driver name and database URL
   static final String JDBC_DRIVER = "com.mysql.jdbc.Driver";  
   static final String DB_URL = "jdbc:mysql://localhost/EMP";

   //  Database credentials
   static final String USER = "username";
   static final String PASS = "password";

   public static void main(String[] args) {
   Connection conn = null;
   try{
      //STEP 2: Register JDBC driver
      Class.forName("com.mysql.jdbc.Driver");

      //STEP 3: Open a connection
      System.out.println("Connecting to database...");
      conn = DriverManager.getConnection(DB_URL,USER,PASS);

      //STEP 4: Execute a query
      System.out.println("Creating statement...");
      Statement stmt = conn.createStatement();
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

Cuando ejecuta **JDBCExample**, produce el siguiente resultado si no hay ningún problema; de lo contrario, se detectará el error correspondiente y se mostrará el mensaje de error:

```bash
C:\>java JDBCExample
Connecting to database...
Creating statement...
ID: 100, Age: 18, First: Zara, Last: Ali
ID: 101, Age: 25, First: Mahnaz, Last: Fatma
ID: 102, Age: 30, First: Zaid, Last: Khan
ID: 103, Age: 28, First: Sumit, Last: Mittal
C:\>
```

Pruebe el ejemplo anterior pasando un nombre de base de datos incorrecto o un nombre de usuario o contraseña incorrectos y verifique el resultado.

---

:octocat: [Repositorio en Github](https://github.com/FernandoCalmet/JDBC)

[![ko-fi](https://www.ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/T6T41JKMI)
