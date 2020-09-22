# Statements, PreparedStatement and CallableStatement

Una vez obtenida la conexión podemos interactuar con la base de datos. Las interfaces JDBC Statement, CallableStatement y PreparedStatement definen los métodos y propiedades que le permiten enviar comandos SQL o PL / SQL y recibir datos de su base de datos.

También definen métodos que ayudan a salvar las diferencias de tipos de datos entre los tipos de datos Java y SQL utilizados en una base de datos.

La siguiente tabla proporciona un resumen del propósito de cada interfaz para decidir qué interfaz utilizar.

Interfaces | Uso recomendado
--- | ---
Statement | Utilice esto para el acceso de propósito general a su base de datos. Útil cuando utiliza sentencias SQL estáticas en tiempo de ejecución. La interfaz de declaración no puede aceptar parámetros.
PreparedStatement | Use esto cuando planee usar las sentencias SQL muchas veces. La interfaz PreparedStatement acepta parámetros de entrada en tiempo de ejecución.
CallableStatement | Úselo cuando desee acceder a los procedimientos almacenados de la base de datos. La interfaz CallableStatement también puede aceptar parámetros de entrada en tiempo de ejecución.

## Los objetos de declaración

Crear objeto de declaración Antes de que pueda utilizar un objeto Statement para ejecutar una sentencia SQL, debe crear una utilizando el método **createStatement()** del objeto Connection, como en el siguiente ejemplo:

```java
Statement stmt = null;
try {
   stmt = conn.createStatement( );
   . . .
}
catch (SQLException e) {
   . . .
}
finally {
   . . .
}
```

Una vez que haya creado un objeto Statement, podrá utilizarlo para ejecutar una sentencia SQL con uno de sus tres métodos de ejecución.

- **boolean execute (String SQL)**: devuelve un valor booleano verdadero si se puede recuperar un objeto ResultSet; de lo contrario, devuelve falso. Utilice este método para ejecutar sentencias DDL de SQL o cuando necesite utilizar SQL verdaderamente dinámico.

- **int executeUpdate (String SQL)**: Devuelve el número de filas afectadas por la ejecución de la instrucción SQL. Utilice este método para ejecutar sentencias SQL para las que espera que se vean afectadas varias filas, por ejemplo, una sentencia INSERT, UPDATE o DELETE.

- **ResultSet executeQuery (String SQL)**: devuelve un objeto ResultSet. Utilice este método cuando espere obtener un conjunto de resultados, como lo haría con una instrucción SELECT.

## Objeto de declaración de cierre

Al igual que cierra un objeto Connection para ahorrar recursos de la base de datos, por la misma razón también debe cerrar el objeto Statement.

Una simple llamada al método close () hará el trabajo. Si primero cierra el objeto Connection, también cerrará el objeto Statement. Sin embargo, siempre debe cerrar explícitamente el objeto Statement para garantizar una limpieza adecuada.

```java
Statement stmt = null;
try {
   stmt = conn.createStatement( );
   . . .
}
catch (SQLException e) {
   . . .
}
finally {
   stmt.close();
}
```

Para una mejor comprensión, le sugerimos que estudie el tutorial Declaración - Ejemplo.

## Los objetos PreparedStatement

La interfaz PreparedStatement amplía la interfaz Statement, lo que le brinda una funcionalidad adicional con un par de ventajas sobre un objeto Statement genérico.

Esta declaración le brinda la flexibilidad de proporcionar argumentos de forma dinámica.

## Crear objeto PreparedStatement

```java
PreparedStatement pstmt = null;
try {
   String SQL = "Update Employees SET age = ? WHERE id = ?";
   pstmt = conn.prepareStatement(SQL);
   . . .
}
catch (SQLException e) {
   . . .
}
finally {
   . . .
}
```

Todos los parámetros en JDBC están representados por? símbolo, que se conoce como marcador de parámetro. Debe proporcionar valores para cada parámetro antes de ejecutar la instrucción SQL.

Los métodos setXXX () vinculan valores a los parámetros, donde XXX representa el tipo de datos Java del valor que desea vincular al parámetro de entrada. Si olvida proporcionar los valores, recibirá una SQLException.

Cada marcador de parámetro se refiere a su posición ordinal. El primer marcador representa la posición 1, la siguiente posición 2 y así sucesivamente. Este método difiere del de los índices de matriz de Java, que comienza en 0.

Todos los métodos del objeto Statement para interactuar con la base de datos (a) **execute()**, (b) **executeQuery()** y (c) **executeUpdate()** también funcionan con el objeto PreparedStatement. Sin embargo, los métodos se modifican para usar sentencias SQL que pueden ingresar los parámetros.

## Cerrar objeto PreparedStatement

Del mismo modo que cierra un objeto Statement, por la misma razón también debe cerrar el objeto PreparedStatement.

Una simple llamada al método **close()** hará el trabajo. Si primero cierra el objeto Connection, también cerrará el objeto PreparedStatement. Sin embargo, siempre debe cerrar explícitamente el objeto PreparedStatement para garantizar una limpieza adecuada.

```java
PreparedStatement pstmt = null;
try {
   String SQL = "Update Employees SET age = ? WHERE id = ?";
   pstmt = conn.prepareStatement(SQL);
   . . .
}
catch (SQLException e) {
   . . .
}
finally {
   pstmt.close();
}
```

## Los objetos CallableStatement

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
**NOTA**: El procedimiento almacenado anterior se ha escrito para Oracle, pero estamos trabajando con la base de datos MySQL, por lo tanto, escribamos el mismo procedimiento almacenado para MySQL de la siguiente manera para crearlo en la base de datos EMP:
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

Parámetro | Descripción
--- | ---
IN | Un parámetro cuyo valor se desconoce cuando se crea la instrucción SQL. Vincula valores a parámetros IN con los métodos setXXX ().
OUT | Un parámetro cuyo valor es proporcionado por la declaración SQL que devuelve. Recupera valores de los parámetros OUT con los métodos getXXX ().
INOUT | Un parámetro que proporciona valores de entrada y salida. Vincula variables con los métodos setXXX () y recupera valores con los métodos getXXX ().

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

La variable de cadena SQL, representa el procedimiento almacenado, con marcadores de posición de parámetros.

Usar los objetos CallableStatement es muy similar a usar los objetos PreparedStatement. Debe vincular valores a todos los parámetros antes de ejecutar la instrucción, o recibirá una SQLException.

Si tiene parámetros IN, simplemente siga las mismas reglas y técnicas que se aplican a un objeto PreparedStatement; utilice el método setXXX() que corresponda al tipo de datos Java que está vinculando.

Cuando usa los parámetros OUT e INOUT, debe emplear un método CallableStatement adicional, **registerOutParameter()**. El método **registerOutParameter()** vincula el tipo de datos JDBC al tipo de datos que se espera que devuelva el procedimiento almacenado.

Una vez que llama a su procedimiento almacenado, recupera el valor del parámetro OUT con el método getXXX () apropiado. Este método convierte el valor recuperado del tipo SQL en un tipo de datos Java.

## Cerrar el objeto CallableStatement

Al igual que cierra otro objeto Statement, por la misma razón también debería cerrar el objeto CallableStatement.

Una simple llamada al método close() hará el trabajo. Si primero cierra el objeto Connection, también cerrará el objeto CallableStatement. Sin embargo, siempre debe cerrar explícitamente el objeto CallableStatement para garantizar una limpieza adecuada.

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

---

:octocat: [Repositorio en Github](https://github.com/FernandoCalmet/JDBC)

[![ko-fi](https://www.ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/T6T41JKMI)
