# ¿Qué es JDBC`?`

JDBC son las siglas de Java Database Connectivity, que es una API de Java estándar para la conectividad independiente de la base de datos entre el lenguaje de programación Java y una amplia gama de bases de datos.

La biblioteca JDBC incluye API para cada una de las tareas mencionadas a continuación que se asocian comúnmente con el uso de la base de datos.

- Hacer una conexión a una base de datos.

- Creación de declaraciones SQL o MySQL.

- Ejecución de consultas SQL o MySQL en la base de datos.

- Ver y modificar los registros resultantes.

Básicamente, JDBC es una especificación que proporciona un conjunto completo de interfaces que permite el acceso portátil a una base de datos subyacente. Java se puede utilizar para escribir diferentes tipos de ejecutables, como:

- Aplicaciones Java

- Applets de Java

- Servlets de Java

- Páginas de servidor Java (JSP)

- Enterprise JavaBeans (EJB).

Todos estos diferentes ejecutables pueden usar un controlador JDBC para acceder a una base de datos y aprovechar los datos almacenados.

JDBC proporciona las mismas capacidades que ODBC, lo que permite que los programas Java contengan código independiente de la base de datos.

## Requisito previo

Antes de seguir adelante, debe tener una buena comprensión de los dos temas siguientes:

- Programación Core JAVA

- Base de datos SQL o MySQL

## Arquitectura JDBC

La API de JDBC admite modelos de procesamiento de dos y tres niveles para el acceso a la base de datos, pero en general, la arquitectura JDBC consta de dos capas:

- API de JDBC: proporciona la conexión de aplicación a JDBC Manager.

- API del controlador JDBC: admite la conexión de administrador a controlador JDBC.

La API de JDBC utiliza un administrador de controladores y controladores específicos de la base de datos para proporcionar conectividad transparente a bases de datos heterogéneas.

El administrador de controladores JDBC garantiza que se utilice el controlador correcto para acceder a cada fuente de datos. El administrador de controladores es capaz de admitir varios controladores simultáneos conectados a varias bases de datos heterogéneas.

A continuación se muestra el diagrama de arquitectura, que muestra la ubicación del administrador de controladores con respecto a los controladores JDBC y la aplicación Java:

![Arquitectura JDBC](/extra/img/jdbc-architecture.jpg)

## Componentes comunes de JDBC

La API de JDBC proporciona las siguientes interfaces y clases:

- **DriverManager**: esta clase administra una lista de controladores de bases de datos. Hace coincidir las solicitudes de conexión de la aplicación Java con el controlador de base de datos adecuado mediante el subprotocolo de comunicación. El primer controlador que reconoce cierto subprotocolo bajo JDBC se utilizará para establecer una conexión de base de datos.

- **Controlador**: esta interfaz maneja las comunicaciones con el servidor de la base de datos. Muy raramente interactuará directamente con los objetos Driver. En su lugar, utiliza los objetos DriverManager, que administra objetos de este tipo. También abstrae los detalles asociados con el trabajo con objetos Driver.

- **Conexión**: esta interfaz con todos los métodos para contactar una base de datos. El objeto de conexión representa el contexto de comunicación, es decir, toda la comunicación con la base de datos se realiza únicamente a través del objeto de conexión.

- **Declaración**: utiliza objetos creados a partir de esta interfaz para enviar las declaraciones SQL a la base de datos. Algunas interfaces derivadas aceptan parámetros además de ejecutar procedimientos almacenados.

- **ResultSet**: estos objetos contienen datos recuperados de una base de datos después de ejecutar una consulta SQL utilizando objetos Statement. Actúa como un iterador para permitirle moverse a través de sus datos.

- **SQLException**: esta clase maneja cualquier error que ocurra en una aplicación de base de datos.

## Los paquetes JDBC 4.0

Java.sql y javax.sql son los paquetes principales para JDBC 4.0. Esta es la última versión de JDBC en el momento de redactar este tutorial. Ofrece las clases principales para interactuar con sus fuentes de datos.

- Las nuevas funciones de estos paquetes incluyen cambios en las siguientes áreas:

- Carga automática del controlador de la base de datos.

- Mejoras en el manejo de excepciones.

- Funcionalidad BLOB / CLOB mejorada.

- Mejoras en la interfaz de conexión y declaración.

- Soporte de juego de caracteres nacionales.

- Acceso SQL ROWID.

- Soporte de tipo de datos XML de SQL 2003.

- Anotaciones.

---

:octocat: [Repositorio en Github](https://github.com/FernandoCalmet/JDBC)

[![ko-fi](https://www.ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/T6T41JKMI)
