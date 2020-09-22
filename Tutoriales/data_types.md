# Data Types

El controlador JDBC convierte el tipo de datos Java al tipo JDBC apropiado, antes de enviarlo a la base de datos. Utiliza una asignación predeterminada para la mayoría de los tipos de datos. Por ejemplo, un int de Java se convierte en un INTEGER de SQL. Se crearon asignaciones predeterminadas para proporcionar coherencia entre los controladores.

La siguiente tabla resume el tipo de datos JDBC predeterminado al que se convierte el tipo de datos Java cuando se llama al método setXXX () del objeto PreparedStatement o CallableStatement o al método ResultSet.updateXXX ().

SQL | JDBC/Java | setXXX | updateXXX
--- | --- | --- | ---
VARCHAR | java.lang.String | setString | updateString
CHAR | java.lang.String | setString | updateString
LONGVARCHAR | java.lang.String | setString | updateString
BIT | boolean | setBoolean | updateBoolean
NUMERIC | java.math.BigDecimal | setBigDecimal | updateBigDecimal
TINYINT | byte | setByte | updateByte
SMALLINT | short | setShort | updateShort
INTEGER | int | setInt | updateInt
BIGINT | long | setLong | updateLong
REAL | float | setFloat | updateFloat
FLOAT | float | setFloat | updateFloat
DOUBLE | double | setDouble | updateDouble
VARBINARY | byte[] | setBytes | updateBytes
BINARY | byte[] | setBytes | updateBytes
DATE | java.sql.Date| setDate | updateDate
TIME | java.sql.Time| setTime | updateTime
TIMESTAMP | java.sql.Timestamp | setTimestamp | updateTimestamp
CLOB | java.sql.Clob | setClob | updateClob
BLOB | java.sql.Blob | setBlob | updateBlob
ARRAY | java.sql.Array | setARRAY | updateARRAY
REF | java.sql.Ref | SetRef | updateRef
STRUCT | java.sql.Struct | SetStruct | updateStruct

JDBC 3.0 tiene soporte mejorado para los tipos de datos BLOB, CLOB, ARRAY y REF. El objeto ResultSet ahora tiene métodos updateBLOB(), updateCLOB(), updateArray() y updateRef() que le permiten manipular directamente los datos respectivos en el servidor.

Los métodos setXXX() y updateXXX() le permiten convertir tipos específicos de Java en tipos de datos JDBC específicos. Los métodos setObject() y updateObject () le permiten mapear casi cualquier tipo de Java a un tipo de datos JDBC.

El objeto ResultSet proporciona el método getXXX() correspondiente para cada tipo de datos para recuperar el valor de la columna. Cada método se puede utilizar con el nombre de la columna o por su posición ordinal.

SQL | JDBC/Java | setXXX | getXXX
--- | --- | --- | ---
VARCHAR| java.lang.String| setString | getString
CHAR | java.lang.String | setString | getString
LONGVARCHAR | java.lang.String | setString | getString
BIT | boolean | setBoolean | getBoolean
NUMERIC | java.math.BigDecimal | setBigDecimal | getBigDecimal
TINYINT | byte | setByte | getByte
SMALLINT | short | setShort | getShort
INTEGER | int | setInt | getInt
BIGINT | long | setLong | getLong
REAL | float | setFloat | getFloat
FLOAT | float | setFloat | getFloat
DOUBLE | double | setDouble | getDouble
VARBINARY | byte[] | setBytes | getBytes
BINARY | byte[] | setBytes | getBytes
DATE | java.sql.Date | setDate | getDate
TIME | java.sql.Time | setTime | getTime
TIMESTAMP | java.sql.Timestamp | setTimestamp | getTimestamp
CLOB | java.sql.Clob | setClob | getClob
BLOB | java.sql.Blob | setBlob | getBlob
ARRAY | java.sql.Array | setARRAY | getARRAY
REF | java.sql.Ref | SetRef | getRef
STRUCT | java.sql.Struct | SetStruct | getStruct

## Tipos de datos de fecha y hora

La clase java.sql.Date se asigna al tipo SQL DATE y las clases java.sql.Time y java.sql.Timestamp se asignan a los tipos de datos SQL TIME y SQL TIMESTAMP, respectivamente.

El siguiente ejemplo muestra cómo las clases de fecha y hora dan formato a los valores de fecha y hora estándar de Java para que coincidan con los requisitos de tipo de datos SQL.

```java
import java.sql.Date;
import java.sql.Time;
import java.sql.Timestamp;
import java.util.*;

public class SqlDateTime {
   public static void main(String[] args) {
      //Get standard date and time
      java.util.Date javaDate = new java.util.Date();
      long javaTime = javaDate.getTime();
      System.out.println("The Java Date is:" +
             javaDate.toString());

      //Get and display SQL DATE
      java.sql.Date sqlDate = new java.sql.Date(javaTime);
      System.out.println("The SQL DATE is: " +
             sqlDate.toString());

      //Get and display SQL TIME
      java.sql.Time sqlTime = new java.sql.Time(javaTime);
      System.out.println("The SQL TIME is: " +
             sqlTime.toString());
      //Get and display SQL TIMESTAMP
      java.sql.Timestamp sqlTimestamp =
      new java.sql.Timestamp(javaTime);
      System.out.println("The SQL TIMESTAMP is: " +
             sqlTimestamp.toString());
     }//end main
}//end SqlDateTime
```

hora compilemos el ejemplo anterior de la siguiente manera:

```bash
C:\>javac SqlDateTime.java
C:\>
```

Cuando ejecuta JDBCExample, produce el siguiente resultado:

```bash
C:\>java SqlDateTime
The Java Date is:Tue Aug 18 13:46:02 GMT+04:00 2009
The SQL DATE is: 2009-08-18
The SQL TIME is: 13:46:02
The SQL TIMESTAMP is: 2009-08-18 13:46:02.828
C:\>
```

## Manejo de valores NULL

El uso de SQL de valores NULL y el uso de Java de null son conceptos diferentes. Entonces, para manejar valores SQL NULL en Java, hay tres tácticas que puede usar:

- Evite el uso de métodos getXXX() que devuelven tipos de datos primitivos.

- Use clases contenedoras para tipos de datos primitivos y use el método wasNull() del objeto ResultSet para probar si la variable de clase contenedora que recibió el valor devuelto por el método getXXX() debería establecerse en nulo.

- Utilice tipos de datos primitivos y el método wasNull() del objeto ResultSet para probar si la variable primitiva que recibió el valor devuelto por el método getXXX() debe establecerse en un valor aceptable que haya elegido para representar un NULL.

Aquí hay un ejemplo para manejar un valor NULL:

```java
Statement stmt = conn.createStatement( );
String sql = "SELECT id, first, last, age FROM Employees";
ResultSet rs = stmt.executeQuery(sql);

int id = rs.getInt(1);
if( rs.wasNull( ) ) {
   id = 0;
}
```

---

:octocat: [Repositorio en Github](https://github.com/FernandoCalmet/JDBC)

[![ko-fi](https://www.ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/T6T41JKMI)
