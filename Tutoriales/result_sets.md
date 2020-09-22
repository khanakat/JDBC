# Result Sets

Las sentencias SQL que leen datos de una consulta de base de datos devuelven los datos en un conjunto de resultados. La instrucción SELECT es la forma estándar de seleccionar filas de una base de datos y verlas en un conjunto de resultados. La interfaz java.sql.ResultSet representa el conjunto de resultados de una consulta de base de datos.

Un objeto ResultSet mantiene un cursor que apunta a la fila actual en el conjunto de resultados. El término "conjunto de resultados" se refiere a los datos de fila y columna contenidos en un objeto ResultSet.

Los métodos de la interfaz ResultSet se pueden dividir en tres categorías:

- **Métodos de navegación**: se utilizan para mover el cursor.

- **Métodos de obtención**: se utilizan para ver los datos en las columnas de la fila actual a la que apunta el cursor.

- **Métodos de actualización**: se utilizan para actualizar los datos en las columnas de la fila actual. Las actualizaciones también se pueden actualizar en la base de datos subyacente.

El cursor se puede mover según las propiedades del ResultSet. Estas propiedades se designan cuando se crea la instrucción correspondiente que genera el ResultSet.

JDBC proporciona los siguientes métodos de conexión para crear declaraciones con el ResultSet deseado:

- createStatement(int RSType, int RSConcurrency);

- prepareStatement(String SQL, int RSType, int RSConcurrency);

- prepareCall(String sql, int RSType, int RSConcurrency);

El primer argumento indica el tipo de un objeto ResultSet y el segundo argumento es una de las dos constantes ResultSet para especificar si un conjunto de resultados es de solo lectura o actualizable.

## Tipo de conjunto de resultados

Los posibles RSType se dan a continuación. Si no especifica ningún tipo de ResultSet, obtendrá automáticamente uno que sea TYPE_FORWARD_ONLY.

Tipo | Descripción
--- | ---
ResultSet.TYPE_FORWARD_ONLY | El cursor solo puede avanzar en el conjunto de resultados.
ResultSet.TYPE_SCROLL_INSENSITIVE | El cursor puede desplazarse hacia adelante y hacia atrás, y el conjunto de resultados no es sensible a los cambios realizados por otros en la base de datos que ocurren después de que se creó el conjunto de resultados.
ResultSet.TYPE_SCROLL_SENSITIVE | El cursor puede desplazarse hacia adelante y hacia atrás, y el conjunto de resultados es sensible a los cambios realizados por otros en la base de datos que ocurren después de que se creó el conjunto de resultados.

## Concurrencia de ResultSet

Las posibles RSConcurrency se dan a continuación. Si no especifica ningún tipo de Concurrencia, obtendrá automáticamente uno que sea **CONCUR_READ_ONLY**.

Concurrencia | Descripción
--- | ---
ResultSet.CONCUR_READ_ONLY | Crea un conjunto de resultados de solo lectura. Este es el predeterminado
ResultSet.CONCUR_UPDATABLE | Crea un conjunto de resultados actualizable.

Todos nuestros ejemplos escritos hasta ahora se pueden escribir de la siguiente manera, que inicializa un objeto Statement para crear un objeto ResultSet de solo lectura y solo avance:

```java
try {
   Statement stmt = conn.createStatement(
                           ResultSet.TYPE_FORWARD_ONLY,
                           ResultSet.CONCUR_READ_ONLY);
}
catch(Exception ex) {
   ....
}
finally {
   ....
}
```

## Navegar por un conjunto de resultados

Hay varios métodos en la interfaz ResultSet que implican mover el cursor, que incluyen:

S.N. | Métodos | Descripción
--- | --- | ---
1 | public void beforeFirst() throws SQLException| Mueve el cursor justo antes de la primera fila.
2 | public void afterLast() throws SQLException | Mueve el cursor justo después de la última fila.
3 | public boolean first() throws SQLException | Mueve el cursor a la primera fila.
4 | public void last() throws SQLException | Mueve el cursor a la última fila.
5 | public boolean absolute(int row) throws SQLException | Mueve el cursor a la fila especificada.
6 | public boolean relative(int row) throws SQLException | Mueve el cursor el número dado de filas hacia adelante o hacia atrás, desde donde apunta actualmente.
7 | public boolean previous() throws SQLException | Mueve el cursor a la fila anterior. Este método devuelve falso si la fila anterior está fuera del conjunto de resultados.
8 | public boolean next() throws SQLException | Mueve el cursor a la siguiente fila. Este método devuelve falso si no hay más filas en el conjunto de resultados.
9 | public int getRow() throws SQLException | Devuelve el número de fila al que apunta el cursor.
10 | public void moveToInsertRow() throws SQLException | Mueve el cursor a una fila especial en el conjunto de resultados que se puede usar para insertar una nueva fila en la base de datos. Se recuerda la ubicación actual del cursor.
11 | public void moveToCurrentRow() throws SQLException | Mueve el cursor de regreso a la fila actual si el cursor está actualmente en la fila de inserción; de lo contrario, este método no hace nada

Para una mejor comprensión, estudiemos Navegar - Código de ejemplo.

## Ver un conjunto de resultados

La interfaz ResultSet contiene docenas de métodos para obtener los datos de la fila actual.

Hay un método de obtención para cada uno de los tipos de datos posibles, y cada método de obtención tiene dos versiones:

- Uno que tenga un nombre de columna.

- Uno que incluye un índice de columna.

Por ejemplo, si la columna que le interesa ver contiene un int, debe usar uno de los métodos getInt() de ResultSet -

S.N. | Métodos | Descripción
--- | --- | ---
1 | public int getInt(String columnName) throws SQLException | Devuelve el int de la fila actual en la columna denominada columnName.
2 | public int getInt(int columnIndex) throws SQLException | Devuelve el int en la fila actual en el índice de columna especificado. El índice de columna comienza en 1, lo que significa que la primera columna de una fila es 1, la segunda columna de una fila es 2, y así sucesivamente.

De manera similar, existen métodos get en la interfaz ResultSet para cada uno de los ocho tipos primitivos de Java, así como tipos comunes como java.lang.String, java.lang.Object y java.net.URL.

También existen métodos para obtener los tipos de datos SQL java.sql.Date, java.sql.Time, java.sql.TimeStamp, java.sql.Clob y java.sql.Blob. Consulte la documentación para obtener más información sobre el uso de estos tipos de datos SQL.

## Actualizar un conjunto de resultados

La interfaz ResultSet contiene una colección de métodos de actualización para actualizar los datos de un conjunto de resultados.

Al igual que con los métodos de obtención, hay dos métodos de actualización para cada tipo de datos:

Uno que tenga un nombre de columna.

Uno que incluye un índice de columna.

Por ejemplo, para actualizar una columna String de la fila actual de un conjunto de resultados, usaría uno de los siguientes métodos updateString():

S.N. | Métodos | Descripción
--- | --- | ---
1 | public void updateString(int columnIndex, String s) throws SQLException | Lanza SQLException Cambia la Cadena en la columna especificada al valor de s.
2 | public void updateString(String columnName, String s) throws SQLException | Similar al método anterior, excepto que la columna se especifica por su nombre en lugar de su índice.

Existen métodos de actualización para los ocho tipos de datos primitivos, así como para los tipos de datos String, Object, URL y SQL en el paquete java.sql.

La actualización de una fila en el conjunto de resultados cambia las columnas de la fila actual en el objeto ResultSet, pero no en la base de datos subyacente. Para actualizar sus cambios en la fila de la base de datos, debe invocar uno de los siguientes métodos.

S.N. | Métodos | Descripción
--- | --- | ---
1 | public void updateRow() | Actualiza la fila actual actualizando la fila correspondiente en la base de datos.
2 | public void deleteRow() | Elimina la fila actual de la base de datos.
3 | public void refreshRow() | Actualiza los datos del conjunto de resultados para reflejar cualquier cambio reciente en la base de datos.
4 | public void cancelRowUpdates() | Cancela cualquier actualización realizada en la fila actual.
5 | insertRow vacío público() | Inserta una fila en la base de datos. Este método solo se puede invocar cuando el cursor apunta a la fila de inserción.

---

:octocat: [Repositorio en Github](https://github.com/FernandoCalmet/JDBC)

[![ko-fi](https://www.ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/T6T41JKMI)
