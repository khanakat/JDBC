# Transactions

Si su conexión JDBC está en modo de confirmación automática, que es de forma predeterminada, entonces cada declaración SQL se confirma en la base de datos una vez completada.

Eso puede estar bien para aplicaciones simples, pero hay tres razones por las que es posible que desee desactivar la confirmación automática y administrar sus propias transacciones:

- Para aumentar el rendimiento.

- Mantener la integridad de los procesos comerciales.

- tilizar transacciones distribuidas.

Las transacciones le permiten controlar si los cambios se aplican a la base de datos y cuándo. Trata una sola declaración SQL o un grupo de declaraciones SQL como una unidad lógica y, si alguna declaración falla, toda la transacción falla.

Para habilitar la compatibilidad con transacciones manuales en lugar del modo de confirmación automática que utiliza el controlador JDBC de forma predeterminada, utilice el método **setAutoCommit()** del objeto Connection. Si pasa un booleano falso a setAutoCommit(), desactiva la confirmación automática. Puede pasar un booleano verdadero para volver a activarlo.

Por ejemplo, si tiene un objeto Connection llamado conn, codifique lo siguiente para desactivar la confirmación automática:

```java
conn.setAutoCommit(false);
```

## Commit & Rollback

Una vez que haya terminado con sus cambios y desee confirmar los cambios, llame al método commit () en el objeto de conexión de la siguiente manera:

```java
conn.commit( );
```

De lo contrario, para revertir las actualizaciones de la base de datos realizadas mediante la conexión denominada conn, utilice el siguiente código:

```java
conn.rollback( );
```

El siguiente ejemplo ilustra el uso de un objeto de confirmación y reversión:

```java
try{
   //Assume a valid connection object conn
   conn.setAutoCommit(false);
   Statement stmt = conn.createStatement();

   String SQL = "INSERT INTO Employees  " +
                "VALUES (106, 20, 'Rita', 'Tez')";
   stmt.executeUpdate(SQL);
   //Submit a malformed SQL statement that breaks
   String SQL = "INSERTED IN Employees  " +
                "VALUES (107, 22, 'Sita', 'Singh')";
   stmt.executeUpdate(SQL);
   // If there is no error.
   conn.commit();
}catch(SQLException se){
   // If there is any error.
   conn.rollback();
}
```

En este caso, ninguna de las instrucciones INSERT anteriores tendría éxito y todo se revertiría.

Para una mejor comprensión, estudiemos el código de ejemplo de compromiso.

## Usar puntos de guardado

La nueva interfaz JDBC 3.0 Savepoint le brinda un control transaccional adicional. La mayoría de los DBMS modernos admiten puntos de guardado dentro de sus entornos, como PL / SQL de Oracle.

Cuando establece un punto de guardado, define un punto de retroceso lógico dentro de una transacción. Si se produce un error más allá de un punto de guardado, puede utilizar el método de reversión para deshacer todos los cambios o solo los cambios realizados después del punto de guardado.

El objeto Connection tiene dos métodos nuevos que le ayudan a gestionar los puntos de guardado:

- **setSavepoint(String savepointName)**: define un nuevo punto de guardado. También devuelve un objeto Savepoint.

- **releaseSavepoint(Savepoint savepointName)**: elimina un punto de guardado. Tenga en cuenta que requiere un objeto Savepoint como parámetro. Este objeto suele ser un punto de guardado generado por el método setSavepoint().

Hay un método de rollback(String savepointName), que revierte el trabajo al punto de guardado especificado.

El siguiente ejemplo ilustra el uso de un objeto Savepoint:

```java
try{
   //Assume a valid connection object conn
   conn.setAutoCommit(false);
   Statement stmt = conn.createStatement();

   //set a Savepoint
   Savepoint savepoint1 = conn.setSavepoint("Savepoint1");
   String SQL = "INSERT INTO Employees " +
                "VALUES (106, 20, 'Rita', 'Tez')";
   stmt.executeUpdate(SQL);
   //Submit a malformed SQL statement that breaks
   String SQL = "INSERTED IN Employees " +
                "VALUES (107, 22, 'Sita', 'Tez')";
   stmt.executeUpdate(SQL);
   // If there is no error, commit the changes.
   conn.commit();

}catch(SQLException se){
   // If there is any error.
   conn.rollback(savepoint1);
}
```

En este caso, ninguna de las instrucciones INSERT anteriores tendría éxito y todo se revertiría.

---

:octocat: [Repositorio en Github](https://github.com/FernandoCalmet/JDBC)

[![ko-fi](https://www.ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/T6T41JKMI)
