# Driver Types

## ¿Qué es el controlador JDBC`?`

Los controladores JDBC implementan las interfaces definidas en la API JDBC, para interactuar con su servidor de base de datos.

Por ejemplo, el uso de controladores JDBC le permite abrir conexiones de bases de datos e interactuar con ellas enviando comandos SQL o de base de datos y luego recibiendo resultados con Java.

El paquete Java.sql que se envía con JDK, contiene varias clases con sus comportamientos definidos y sus implementaciones reales se realizan en controladores de terceros. Los proveedores externos implementan la interfaz java.sql.Driver en su controlador de base de datos.

## Tipos de controladores JDBC

Las implementaciones del controlador JDBC varían debido a la amplia variedad de sistemas operativos y plataformas de hardware en las que opera Java. Sun ha dividido los tipos de implementación en cuatro categorías, Tipos 1, 2, 3 y 4, que se explican a continuación:

## Tipo 1: controlador de puente JDBC-ODBC

En un controlador de Tipo 1, se utiliza un puente JDBC para acceder a los controladores ODBC instalados en cada máquina cliente. El uso de ODBC requiere configurar en su sistema un Nombre de fuente de datos (DSN) que represente la base de datos de destino.

Cuando apareció Java por primera vez, este era un controlador útil porque la mayoría de las bases de datos solo admitían el acceso ODBC, pero ahora este tipo de controlador se recomienda solo para uso experimental o cuando no hay otra alternativa disponible.

![tipo 1](/extra/img/driver-type1.jpg)

El puente JDBC-ODBC que viene con JDK 1.2 es un buen ejemplo de este tipo de controlador.

## Tipo 2: API nativa de JDBC

En un controlador de tipo 2, las llamadas a la API de JDBC se convierten en llamadas nativas de la API C / C ++, que son exclusivas de la base de datos. Estos controladores normalmente los proporcionan los proveedores de bases de datos y se utilizan de la misma manera que el puente JDBC-ODBC. El controlador específico del proveedor debe instalarse en cada máquina cliente.

Si cambiamos la base de datos, tenemos que cambiar la API nativa, ya que es específica de una base de datos y ahora están en su mayoría obsoletas, pero puede darse cuenta de un aumento de velocidad con un controlador de tipo 2, porque elimina la sobrecarga de ODBC.

![tipo 2](/extra/img/driver-type2.jpg)

El controlador Oracle Call Interface (OCI) es un ejemplo de controlador de tipo 2.

## Tipo 3: Java puro JDBC-Net

En un controlador de tipo 3, se utiliza un enfoque de tres niveles para acceder a las bases de datos. Los clientes JDBC utilizan sockets de red estándar para comunicarse con un servidor de aplicaciones de middleware. A continuación, el servidor de aplicaciones de middleware traduce la información del socket al formato de llamada requerido por el DBMS y la reenvía al servidor de la base de datos.

Este tipo de controlador es extremadamente flexible, ya que no requiere ningún código instalado en el cliente y un solo controlador puede proporcionar acceso a múltiples bases de datos.

![tipo 3](/extra/img/driver-type3.jpg)

Puede pensar en el servidor de aplicaciones como un "proxy" JDBC, lo que significa que realiza llamadas para la aplicación cliente. Como resultado, necesita algún conocimiento de la configuración del servidor de aplicaciones para poder utilizar de forma eficaz este tipo de controlador.

Su servidor de aplicaciones puede usar un controlador de Tipo 1, 2 o 4 para comunicarse con la base de datos, comprender los matices será útil.

## Tipo 4: Java 100% puro

En un controlador de tipo 4, un controlador puro basado en Java se comunica directamente con la base de datos del proveedor a través de una conexión de socket. Este es el controlador de mayor rendimiento disponible para la base de datos y generalmente lo proporciona el propio proveedor.

Este tipo de controlador es extremadamente flexible, no es necesario instalar software especial en el cliente o servidor. Además, estos controladores se pueden descargar de forma dinámica.

![tipo 4](/extra/img/driver-type4.jpg)

El controlador Connector / J de MySQL es un controlador de tipo 4. Debido a la naturaleza propietaria de sus protocolos de red, los proveedores de bases de datos suelen suministrar controladores de tipo 4.

## ¿Qué controlador se debe utilizar`?`

Si accede a un tipo de base de datos, como Oracle, Sybase o IBM, el tipo de controlador preferido es 4.

Si su aplicación Java accede a varios tipos de bases de datos al mismo tiempo, el tipo 3 es el controlador preferido.

Los controladores de tipo 2 son útiles en situaciones en las que un controlador de tipo 3 o tipo 4 aún no está disponible para su base de datos.

El controlador de tipo 1 no se considera un controlador de nivel de implementación y, por lo general, se usa solo con fines de desarrollo y prueba.

---

:octocat: [Repositorio en Github](https://github.com/FernandoCalmet/JDBC)

[![ko-fi](https://www.ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/T6T41JKMI)
