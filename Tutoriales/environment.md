# JDBC - Environment Setup

Para comenzar a desarrollar con JDBC, debe configurar su entorno JDBC siguiendo los pasos que se muestran a continuación. Suponemos que está trabajando en una plataforma Windows.

## Instalar Java

Instale J2SE Development Kit 5.0 (JDK 5.0) desde el sitio oficial de Java.

Asegúrese de que las siguientes variables de entorno estén configuradas como se describe a continuación:

- **JAVA_HOME**: esta variable de entorno debe apuntar al directorio donde instaló el JDK, p. Ej. C: \ Archivos de programa \ Java \ jdk1.5.0.

- **CLASSPATH**: Esta variable de entorno debe tener configuradas las rutas adecuadas, p. Ej. C: \ Archivos de programa \ Java \ jdk1.5.0_20 \ jre \ lib.

- **RUTA**: Esta variable de entorno debe apuntar al contenedor JRE apropiado, p. Ej. C: \ Archivos de programa \ Java \ jre1.5.0_20 \ bin.

Es posible que ya tenga estas variables configuradas, pero solo para asegurarse de que aquí se explica cómo verificar.

- Vaya al panel de control y haga doble clic en Sistema. Si es un usuario de Windows XP, es posible que tenga que abrir Rendimiento y mantenimiento antes de ver el icono Sistema.

- Vaya a la pestaña Avanzado y haga clic en Variables de entorno.

- Ahora verifique si todas las variables mencionadas anteriormente están configuradas correctamente.

Obtiene automáticamente los paquetes JDBC java.sql y javax.sql cuando instala J2SE Development Kit 5.0 (JDK 5.0).

## Instalar base de datos

Lo más importante que necesitará, por supuesto, es una base de datos en ejecución real con una tabla que puede consultar y modificar.

Instale una base de datos que sea más adecuada para usted. Puede tener muchas opciones y las más comunes son:

- **MySQL DB**: MySQL es una base de datos de código abierto. Puede descargarlo del sitio oficial de MySQL. Recomendamos descargar la instalación completa de Windows.

Además, descargue e instale MySQL Administrator y MySQL Query Browser. Estas son herramientas basadas en GUI que facilitarán mucho su desarrollo.

Finalmente, descargue y descomprima MySQL Connector / J (el controlador MySQL JDBC) en un directorio conveniente. Para el propósito de este tutorial, asumiremos que ha instalado el controlador en C: \ Archivos de programa \ MySQL \ mysql-connector-java-5.1.8.

En consecuencia, establezca la variable CLASSPATH en C: \ Archivos de programa \ MySQL \ mysql-connector-java-5.1.8 \ mysql-connector-java-5.1.8-bin.jar. La versión de su controlador puede variar según su instalación.

- **PostgreSQL DB**: PostgreSQL es una base de datos de código abierto. Puede descargarlo del sitio oficial de PostgreSQL.

La instalación de Postgres contiene una herramienta administrativa basada en GUI llamada pgAdmin III. Los controladores JDBC también se incluyen como parte de la instalación.

- **Oracle DB**: Oracle DB es una base de datos comercial vendida por Oracle. Asumimos que tiene los medios de distribución necesarios para instalarlo.

La instalación de Oracle incluye una herramienta administrativa basada en GUI llamada Enterprise Manager. Los controladores JDBC también se incluyen como parte de la instalación.

## Instalar controladores de base de datos

El último JDK incluye un controlador JDBC-ODBC Bridge que hace que la mayoría de los controladores Open Database Connectivity (ODBC) estén disponibles para los programadores que utilizan la API JDBC.

Hoy en día, la mayoría de los proveedores de bases de datos suministran los controladores JDBC adecuados junto con la instalación de la base de datos. Entonces, no debes preocuparte por esta parte.

## Establecer credencial de base de datos

Para este tutorial usaremos la base de datos MySQL. Cuando instala cualquiera de las bases de datos anteriores, su ID de administrador se establece en **root** y proporciona la posibilidad de establecer una contraseña de su elección.

Con el ID de root y la contraseña puede crear otro ID de **usuario** y **contraseña**, o puede utilizar el ID de root y la contraseña para su aplicación JDBC.

Hay varias operaciones de base de datos, como la creación y eliminación de bases de datos, que necesitarían una identificación de administrador y una contraseña.

Para el resto del tutorial de JDBC, usaríamos la base de datos MySQL con el nombre de usuario como ID y la contraseña como contraseña.

Si no tiene privilegios suficientes para crear nuevos usuarios, puede pedirle a su administrador de base de datos (DBA) que cree una identificación de usuario y una contraseña para usted.

## Crear base de datos

Para crear la base de datos EMP, siga los siguientes pasos:

> Paso 1

Abra un símbolo del sistema y cambie al directorio de instalación de la siguiente manera:

```bash
C:\>
C:\>cd Program Files\MySQL\bin
C:\Program Files\MySQL\bin>
```

```note
**Nota**: La ruta a mysqld.exe puede variar según la ubicación de instalación de MySQL en su sistema. También puede consultar la documentación sobre cómo iniciar y detener su servidor de base de datos.
```

> Paso 2

Inicie el servidor de la base de datos ejecutando el siguiente comando, si ya no se está ejecutando.

```bash
C:\Program Files\MySQL\bin>mysqld
C:\Program Files\MySQL\bin>
```

> Paso 3

Cree la base de datos EMP ejecutando el siguiente comando:

```bash
C:\Program Files\MySQL\bin> mysqladmin create EMP -u root -p
Enter password: ********
C:\Program Files\MySQL\bin>
```

## Crear tabla

Para crear la tabla Empleados en la base de datos de EMP, siga los siguientes pasos:

> Paso 1

Abra un símbolo del sistema y cambie al directorio de instalación de la siguiente manera:

```bash
C:\>
C:\>cd Program Files\MySQL\bin
C:\Program Files\MySQL\bin>
```

> Paso 2

Inicie sesión en la base de datos de la siguiente manera:

```bash
C:\Program Files\MySQL\bin>mysql -u root -p
Enter password: ********
mysql>
```

> Paso 3

Cree la tabla Empleado de la siguiente manera:

```sql
mysql> use EMP;
mysql> create table Employees
    -> (
    -> id int not null,
    -> age int not null,
    -> first varchar (255),
    -> last varchar (255)
    -> );
Query OK, 0 rows affected (0.08 sec)
mysql>
```

## Crear registros de datos

Finalmente, crea algunos registros en la tabla Empleado de la siguiente manera:

```sql
mysql> INSERT INTO Employees VALUES (100, 18, 'Zara', 'Ali');
Query OK, 1 row affected (0.05 sec)

mysql> INSERT INTO Employees VALUES (101, 25, 'Mahnaz', 'Fatma');
Query OK, 1 row affected (0.00 sec)

mysql> INSERT INTO Employees VALUES (102, 30, 'Zaid', 'Khan');
Query OK, 1 row affected (0.00 sec)

mysql> INSERT INTO Employees VALUES (103, 28, 'Sumit', 'Mittal');
Query OK, 1 row affected (0.00 sec)

mysql>
```

Para una comprensión completa de la base de datos MySQL, estudie el Tutorial de MySQL. Ahora está listo para comenzar a experimentar con JDBC. El siguiente capítulo le ofrece un ejemplo de programación JDBC.

---

:octocat: [Repositorio en Github](https://github.com/FernandoCalmet/JDBC)

[![ko-fi](https://www.ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/T6T41JKMI)
