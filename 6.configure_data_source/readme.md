# Configuring Data Sources
A data source is a facility provided by an application server that is responsible for accessing databases without providing sensitive information, such as credentials and the database location, to an application. To connect to a database, the application uses a name bound to the data source via the application server. For a JEE application server, the name is defined by the Java Naming and Directory Interface (JNDI) specification.

Using a Java Database Connectivity (JDBC) API specific to a user's relational database management system, users can fine tune a data source to maximize efficiency and manage load on the database.

Data sources are configured through the Datasource Subsystem. Declaring a new data source consists of two separate steps:

- Installing the JDBC driver for a particular database server.

- Define a data source within the datasource subsystem of the configuration file. 

## Database connection pools
One obstacle to overcome when managing a data source is being able to handle traffic and concurrent connections to the database. It can be a potential bottleneck and a performance concern during peak loads for the deployed applications.

To improve the performance of connecting to a database, EAP creates a database connection pool for each database connection that is used by the applications. The pool contains open connections to the database, so that when the application needs to access the database, an open connection is already available, avoiding the overhead of opening and closing a connection for every single request. The size of the connection pool can be configured for each data source.