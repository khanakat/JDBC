# Batch Processing

El procesamiento por lotes le permite agrupar declaraciones SQL relacionadas en un lote y enviarlas con una llamada a la base de datos.

Cuando envía varias declaraciones SQL a la base de datos a la vez, reduce la cantidad de sobrecarga de comunicación, mejorando así el rendimiento.

- Los controladores JDBC no son necesarios para admitir esta función. Debe utilizar el método DatabaseMetaData.supportsBatchUpdates () para determinar si la base de datos de destino admite el procesamiento de actualizaciones por lotes. El método devuelve verdadero si su controlador JDBC admite esta función.

- El método addBatch() de Statement, PreparedStatement y CallableStatement se utiliza para agregar declaraciones individuales al lote. El executeBatch() se utiliza para iniciar la ejecución de todas las declaraciones agrupadas.

- ExecuteBatch() devuelve una matriz de números enteros, y cada elemento de la matriz representa el recuento de actualizaciones para la declaración de actualización respectiva.

- Así como puede agregar declaraciones a un lote para su procesamiento, puede eliminarlas con el método clearBatch (). Este método elimina todas las declaraciones que agregó con el método addBatch(). Sin embargo, no puede elegir de forma selectiva qué declaración eliminar.

## Procesamiento por lotes con objeto de declaración

A continuación, se muestra una secuencia típica de pasos para utilizar el procesamiento por lotes con el objeto de declaración:

- Cree un objeto Statement utilizando los métodos createStatement().

- Establezca el compromiso automático en falso usando setAutoCommit().

- Agregue tantas declaraciones SQL como desee en el lote utilizando el método addBatch() en el objeto de declaración creado.

- Ejecute todas las sentencias SQL usando el método executeBatch() en el objeto de sentencia creado.

- Finalmente, confirme todos los cambios usando el método commit().

> Ejemplo
El siguiente fragmento de código proporciona un ejemplo de una actualización por lotes utilizando el objeto Statement:

```java
// Create statement object
Statement stmt = conn.createStatement();

// Set auto-commit to false
conn.setAutoCommit(false);

// Create SQL statement
String SQL = "INSERT INTO Employees (id, first, last, age) " +
             "VALUES(200,'Zia', 'Ali', 30)";
// Add above SQL statement in the batch.
stmt.addBatch(SQL);

// Create one more SQL statement
String SQL = "INSERT INTO Employees (id, first, last, age) " +
             "VALUES(201,'Raj', 'Kumar', 35)";
// Add above SQL statement in the batch.
stmt.addBatch(SQL);

// Create one more SQL statement
String SQL = "UPDATE Employees SET age = 35 " +
             "WHERE id = 100";
// Add above SQL statement in the batch.
stmt.addBatch(SQL);

// Create an int[] to hold returned values
int[] count = stmt.executeBatch();

//Explicitly commit statements to apply changes
conn.commit();
```

Para una mejor comprensión, estudiemos el código de ejemplo por lotes.

## Procesamiento por lotes con el objeto PrepareStatement

A continuación, se muestra una secuencia típica de pasos para usar el procesamiento por lotes con el objeto PrepareStatement:

1. Cree declaraciones SQL con marcadores de posición.

2. Cree el objeto PrepareStatement utilizando los métodos prepareStatement().

3. Establezca el compromiso automático en falso usando setAutoCommit().

4. Agregue tantas declaraciones SQL como desee en el lote utilizando el método addBatch() en el objeto de declaración creado.

5. Ejecute todas las sentencias SQL usando el método executeBatch() en el objeto de sentencia creado.

6. Finalmente, confirme todos los cambios usando el método commit().

El siguiente fragmento de código proporciona un ejemplo de una actualización por lotes utilizando el objeto PrepareStatement:

```java
// Create SQL statement
String SQL = "INSERT INTO Employees (id, first, last, age) " +
             "VALUES(?, ?, ?, ?)";

// Create PrepareStatement object
PreparedStatemen pstmt = conn.prepareStatement(SQL);

//Set auto-commit to false
conn.setAutoCommit(false);

// Set the variables
pstmt.setInt( 1, 400 );
pstmt.setString( 2, "Pappu" );
pstmt.setString( 3, "Singh" );
pstmt.setInt( 4, 33 );
// Add it to the batch
pstmt.addBatch();

// Set the variables
pstmt.setInt( 1, 401 );
pstmt.setString( 2, "Pawan" );
pstmt.setString( 3, "Singh" );
pstmt.setInt( 4, 31 );
// Add it to the batch
pstmt.addBatch();

//add more batches
.
.
.
.
//Create an int[] to hold returned values
int[] count = stmt.executeBatch();

//Explicitly commit statements to apply changes
conn.commit();
```

---

:octocat: [Repositorio en Github](https://github.com/FernandoCalmet/JDBC)

[![ko-fi](https://www.ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/T6T41JKMI)
