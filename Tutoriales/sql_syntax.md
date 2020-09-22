# SQL Syntax

Structured Query Language (SQL) es un lenguaje estandarizado que le permite realizar operaciones en una base de datos, como crear entradas, leer contenido, actualizar contenido y eliminar entradas.

SQL es compatible con casi cualquier base de datos que probablemente usará y le permite escribir código de base de datos independientemente de la base de datos subyacente.

Este capítulo ofrece una descripción general de SQL, que es un requisito previo para comprender los conceptos de JDBC. Después de leer este capítulo, podrá crear, crear, leer, actualizar y eliminar (a menudo denominados operaciones CRUD) datos de una base de datos.

Para una comprensión detallada de SQL, puede leer nuestro Tutorial de MySQL.

## Crear base de datos

La sentencia CREATE DATABASE se utiliza para crear una nueva base de datos. La sintaxis es -

```sql
SQL> CREATE DATABASE DATABASE_NAME;
```

Ejemplo La siguiente declaración SQL crea una base de datos denominada EMP:

```sql
SQL> CREATE DATABASE EMP;
```

## Eliminar base de datos

La instrucción DROP DATABASE se utiliza para eliminar una base de datos existente. La sintaxis es -

```sql
SQL> DROP DATABASE DATABASE_NAME;
```

```note
**Nota**: Para crear o eliminar una base de datos, debe tener privilegios de administrador en su servidor de base de datos. Tenga cuidado, eliminar una base de datos perdería todos los datos almacenados en la base de datos.
```

## Crear tabla

La sentencia CREATE TABLE se utiliza para crear una nueva tabla. La sintaxis es -

```sql
SQL> CREATE TABLE table_name
(
   column_name column_data_type,
   column_name column_data_type,
   column_name column_data_type
   ...
);
```

**Ejemplo**: La siguiente instrucción SQL crea una tabla llamada Empleados con cuatro columnas:

```sql
SQL> CREATE TABLE Employees
(
   id INT NOT NULL,
   age INT NOT NULL,
   first VARCHAR(255),
   last VARCHAR(255),
   PRIMARY KEY ( id )
);
```

## Eliminar tabla

La instrucción DROP TABLE se utiliza para eliminar una tabla existente. La sintaxis es -

```sql
SQL> DROP TABLE table_name;
```

## INSERTAR Datos

La sintaxis de INSERT es similar a la siguiente, donde column1, column2, etc.representan los nuevos datos que aparecerán en las respectivas columnas:

```sql
SQL> INSERT INTO table_name VALUES (column1, column2, ...);
```

**Ejemplo**: La siguiente instrucción SQL INSERT inserta una nueva fila en la base de datos de empleados creada anteriormente:

```sql
SQL> INSERT INTO Employees VALUES (100, 18, 'Zara', 'Ali');
```

## SELECCIONAR Datos

La instrucción SELECT se utiliza para recuperar datos de una base de datos. La sintaxis de SELECT es:

```sql
SQL> SELECT column_name, column_name, ...
     FROM table_name
     WHERE conditions;
```

La cláusula WHERE puede utilizar los operadores de comparación como =,! =, <,>, <= Y> =, así como los operadores BETWEEN y LIKE.

**Ejemplo**: La siguiente declaración SQL selecciona la edad, la primera y la última columna de la tabla Empleados, donde la columna de identificación es 100 -

```sql
SQL> SELECT first, last, age
     FROM Employees
     WHERE id = 100;
```

La siguiente declaración SQL selecciona la edad, la primera y la última columna de la tabla Empleados donde la primera columna contiene Zara -

```sql
SQL> SELECT first, last, age
     FROM Employees
     WHERE first LIKE '%Zara%';
```

## Actualizar datos

La instrucción UPDATE se usa para actualizar datos. La sintaxis de ACTUALIZAR es:

```sql
SQL> UPDATE Employees SET age=20 WHERE id=100;
```

## Borrar datos

La instrucción DELETE se utiliza para eliminar datos de tablas. La sintaxis de DELETE es -

```sql
SQL> DELETE FROM table_name WHERE conditions;
```

La cláusula WHERE puede utilizar los operadores de comparación como =,! =, <,>, <= Y> =, así como los operadores BETWEEN y LIKE.

Ejemplo La siguiente declaración SQL DELETE elimina el registro del empleado cuya identificación es 100 -

```sql
SQL> DELETE FROM Employees WHERE id=100;
```

---

:octocat: [Repositorio en Github](https://github.com/FernandoCalmet/JDBC)

[![ko-fi](https://www.ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/T6T41JKMI)
