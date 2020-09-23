# Stored Procedure

Hemos aprendido a utilizar **procedimientos almacenados** en JDBC mientras analizamos el capítulo JDBC - Declaraciones. Este capítulo es similar a esa sección, pero le brindará información adicional sobre la sintaxis de escape de JDBC SQL.

Así como un objeto Connection crea los objetos Statement y PreparedStatement, también crea el objeto CallableStatement, que se usaría para ejecutar una llamada a un procedimiento almacenado de base de datos.

## Crear objeto CallableStatement

Supongamos que necesita ejecutar el siguiente procedimiento almacenado de Oracle:

```sql
CREATE OR REPLACE PROCEDURE getEmpName
   (EMP_ID IN NUMBER, EMP_FIRST OUT VARCHAR) AS
BEGIN
   SELECT first INTO EMP_FIRST
   FROM Employees
   WHERE ID = EMP_ID;
END;
```

```note
**NOTE**: Above stored procedure has been written for Oracle, but we are working with MySQL database so, let us write same stored procedure for MySQL as follows to create it in EMP database −
```

```sql
DELIMITER $$

DROP PROCEDURE IF EXISTS `EMP`.`getEmpName` $$
CREATE PROCEDURE `EMP`.`getEmpName`
   (IN EMP_ID INT, OUT EMP_FIRST VARCHAR(255))
BEGIN
   SELECT first INTO EMP_FIRST
   FROM Employees
   WHERE ID = EMP_ID;
END $$

DELIMITER ;
```

Existen tres tipos de parámetros: IN, OUT e INOUT. El objeto PreparedStatement solo usa el parámetro IN. El objeto CallableStatement puede usar los tres.

Aquí están las definiciones de cada uno:

Parámetros | Descripción
--- | ---
IN | Un parámetro cuyo valor se desconoce cuando se crea la instrucción SQL. Vincula valores a parámetros IN con los métodos setXXX().
OUT | Un parámetro cuyo valor es proporcionado por la declaración SQL que devuelve. Recupera valores de los parámetros OUT con los métodos getXXX().
INOUT | Un parámetro que proporciona valores de entrada y salida. Vincula variables con los métodos setXXX() y recupera valores con los métodos getXXX ().

El siguiente fragmento de código muestra cómo emplear el método **Connection.prepareCall()** para crear una instancia de un objeto **CallableStatement** basado en el procedimiento almacenado anterior:

```java
CallableStatement cstmt = null;
try {
   String SQL = "{call getEmpName (?, ?)}";
   cstmt = conn.prepareCall (SQL);
   . . .
}
catch (SQLException e) {
   . . .
}
finally {
   . . .
}
```

La variable de cadena SQL representa el procedimiento almacenado, con marcadores de posición de parámetros.

Usar objetos CallableStatement es muy parecido a usar objetos PreparedStatement. Debe vincular valores a todos los parámetros antes de ejecutar la instrucción, o recibirá una SQLException.

Si tiene parámetros IN, simplemente siga las mismas reglas y técnicas que se aplican a un objeto PreparedStatement; utilice el método setXXX() que corresponda al tipo de datos Java que está vinculando.

Cuando usa los parámetros OUT e INOUT, debe emplear un método CallableStatement adicional, registerOutParameter(). El método registerOutParameter() vincula el tipo de datos JDBC al tipo de datos que se espera que devuelva el procedimiento almacenado.

Una vez que llama a su procedimiento almacenado, recupera el valor del parámetro OUT con el método getXXX() apropiado. Este método convierte el valor recuperado del tipo SQL en un tipo de datos Java.

## Cerrar el objeto CallableStatement

Al igual que cierra otro objeto Statement, por la misma razón también debería cerrar el objeto CallableStatement.

Una simple llamada al método close () hará el trabajo. Si primero cierra el objeto Connection, también cerrará el objeto CallableStatement. Sin embargo, siempre debe cerrar explícitamente el objeto CallableStatement para garantizar una limpieza adecuada.

```java
CallableStatement cstmt = null;
try {
   String SQL = "{call getEmpName (?, ?)}";
   cstmt = conn.prepareCall (SQL);
   . . .
}
catch (SQLException e) {
   . . .
}
finally {
   cstmt.close();
}
```

## Sintaxis de escape de JDBC SQL

La sintaxis de escape le brinda la flexibilidad de utilizar funciones específicas de la base de datos que no están disponibles para usted mediante el uso de métodos y propiedades estándar de JDBC.

El formato de sintaxis de escape SQL general es el siguiente:

```sql
{keyword 'parameters'}
```

Aquí están las siguientes secuencias de escape, que le resultarían muy útiles al realizar la programación JDBC:

## Palabras clave: d, t, ts

Ayudan a identificar los literales de fecha, hora y marca de tiempo. Como sabe, no hay dos DBMS que representen la hora y la fecha de la misma manera. Esta sintaxis de escape le dice al controlador que muestre la fecha u hora en el formato de la base de datos de destino. Por ejemplo:

```sql
{d 'yyyy-mm-dd'}
```

Donde aaaa = año, mm = mes; dd = fecha. El uso de esta sintaxis {d '2009-09-03'} es el 9 de marzo de 2009.

Aquí hay un ejemplo simple que muestra cómo INSERTAR la fecha en una tabla:

```java
//Create a Statement object
stmt = conn.createStatement();
//Insert data ==> ID, First Name, Last Name, DOB
String sql="INSERT INTO STUDENTS VALUES" +
             "(100,'Zara','Ali', {d '2001-12-16'})";
stmt.executeUpdate(sql);
```

Del mismo modo, puede utilizar una de las siguientes dos sintaxis, **t** o **ts**:

```sql
{t 'hh:mm:ss'}
```

Donde hh = hora; mm = minuto; ss = segundo. Con esta sintaxis {t '13: 30: 29 '} es 1:30:29 PM.

```sql
{ts 'yyyy-mm-dd hh:mm:ss'}
```

Esta es la sintaxis combinada de las dos sintaxis anteriores para 'd' y 't' para representar la marca de tiempo.

## escape palabra clave

Esta palabra clave identifica el carácter de escape utilizado en las cláusulas LIKE. Útil cuando se usa el comodín SQL%, que coincide con cero o más caracteres. Por ejemplo

```java
String sql = "SELECT symbol FROM MathSymbols
              WHERE symbol LIKE '\%' {escape '\'}";
stmt.execute(sql);
```

Si usa el carácter de barra invertida (\) como carácter de escape, también debe usar dos caracteres de barra invertida en su literal de cadena Java, porque la barra invertida también es un carácter de escape de Java.

## palabra clave: fn

Esta palabra clave representa funciones escalares utilizadas en un DBMS. Por ejemplo, puede usar la longitud de la función SQL para obtener la longitud de una cadena:

```sql
{fn length('Hello World')}
```

Esto devuelve 11, la longitud de la cadena de caracteres 'Hola mundo'.

## llamar palabra clave

Esta palabra clave se utiliza para llamar a los procedimientos almacenados. Por ejemplo, para un procedimiento almacenado que requiere un parámetro IN, use la siguiente sintaxis:

```sql
{call my_procedure(?)};
```

Para un procedimiento almacenado que requiere un parámetro IN y devuelve un parámetro OUT, use la siguiente sintaxis:

```sql
{? = call my_procedure(?)};
```

## palabra clave: oj

Esta palabra clave se utiliza para indicar combinaciones externas. La sintaxis es la siguiente:

```sql
{oj outer-join}
```

Where outer-join = table {LEFT|RIGHT|FULL} OUTERJOIN {table | outer-join} on search-condition. Por ejemplo −

```java
String sql = "SELECT Employees
              FROM {oj ThisTable RIGHT
              OUTER JOIN ThatTable on id = '100'}";
stmt.execute(sql);
```

---

:octocat: [Repositorio en Github](https://github.com/FernandoCalmet/JDBC)

[![ko-fi](https://www.ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/T6T41JKMI)
