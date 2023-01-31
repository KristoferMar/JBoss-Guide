# Configuring Datasources

In EAP 7, a data source is configured in the server's configuration file (domain.xml or standalone.xml) within the datasource subsystem.

The structure of the data source subsystem is as follows:

```
<subsystem xmlns="urn:jboss:domain:datasources:4.0">
<datasources>
    <datasource jndi-name="..."1 jta="true or false"2 pool-name="..."3 enabled="true or false"4 use-java-context="true or false"5>
        <connection-url> connection URL </connection-url>6
        <driver> the driver module </driver>
        <pool>7
            Connection pool settings
        </pool>
        <security>8
            Username and password for connecting to the database
        </security>
        <validation>validation settings</validation>9
        <timeout>timeouts</timeout>10
        <statement>SQL statements</statement>11
    </datasource>
    <drivers>
        <driver name="..." module="module_name"/>12
    </drivers>
</datasources>
</subsystem>
```

1. The JNDI name used for looking up the data source.

2. Dictates whether JTA integration is enabled or disabled.

3. The name of the pool referenced in the <'pool> child element.

4. Dictates whether the data source is enabled or disabled.

5. Dictates whether to bind the data source to global JNDI.

6. The JDBC driver connection URL.

7. The connection pool settings.

8. Security credentials for connecting to the database.

9. The validation strategies for the data source connections.

10. Contains child elements which are timeout settings.

11. Describes the configuration for the behavior of prepared statements.

12. The driver definition.


## Database connection validation
EAP 7 provides the capability to manage outages, network problems, database maintenance, or any other event that can cause the server to lose connection to the database. Using one of several validation methods, users can configure and customize when and how to validate the database connection and how to handle the loss of connection.

The first step for enabling validation is selecting one of the following validation methods and setting the value to true within the <'datasource> section of the server configuration file in the <'validate> section:

### Validate-on-match
If the validate-on-match value is set to true, every time a connection is pulled from the connection pool, it will be validated. 

The following is an example of using validate-on-match: 

```
<datasources>
  <datasource ...>
    <validation>
      <validate-on-match>true or false</validate-on-match>1
      <exception-sorter class-name="org.jboss.jca.adapters.jdbc.extensions.mysql.MySQLExceptionSorter"/>2
    </validation>
  </datasource>
</datasources>
```

1. Can be true or false to enable or disable the validate on match.

2. The class name for the exception sorter, specific to the database vendor.


### Background-validation
If the background-validation value is set to true, the connection will be validated based on the value set in the <'background-validation-millis>. 

The following is an example of using background-validation: 

```
<datasources>
  <datasource ...>
    <validation>
      <background-validation>true or false</background-validation>1
      <exception-sorter class-name="org.jboss.jca.adapters.jdbc.extensions.mysql.MySQLExceptionSorter"/>2
    </validation>
  </datasource>
</datasources>
```

1. Can be true or false to enable or disable the background validation.

2. The class name for the exception sorter, specific to the database vendor.

After selecting the validation method, users need to select a validation mechanism to describe how the connection will be validated. Select one of the following mechanisms for validating the connection:

#### valid-connection-checker
EAP 7 provides several classes that can be used to validate a database connection that is specific to the user's relational database management system. For example, for a MySQL validation class checker, appears as follows: 

```
<valid-connection-checker
class-name="org.jboss.jca.adapters.jdbc.extensions.mysql.MySQLValidConnectionChecker"/>
```


#### check-valid-connection-sql
This validation mechanism requires the user to provide a simple SQL statement that will be executed to validate the connection. The following is an example for a MySQL database connection: 

```
<check-valid-connection-sql>select 1</check-valid-connection-sql>
```

#### exception-sorter
Using the exception-sorter allows users to provide a class to properly detect and clean up after fatal connection exceptions. EAP 7 provides several exception sorter classes based on the relational database management system being used. The following is an example for a MySQL exception sorter:

```
<exception-sorter class-name="org.jboss.jca.adapters.jdbc.extensions.mysql.MySQLExceptionSorter"/>
```

# Creating a data source with the management console

The EAP Management Console provides an easy way to create a data source using templates that are preconfigured for different database vendors. The following steps can be used to create a Non-XA data source with the management console in a standalone server. 

1. Select Configuration from the top of the management console and select Subsystems and then Datasources to access the data source subsystem.

2. Click Non-XA and then click Add in order to open the Create Datasource wizard.

3. Select the database that should be used by the Java application and click Next. If none of them are valid, then select the Custom option and look for the JDBC driver documentation to complete the following steps.

4. After entering the Datasource Attributes, click Next to select the driver.

5. Click the Detected Driver option and select the mysql driver that was installed as a module and select Next.

6. On the final step, notice that the connection URL is formatted for the correct MySQL syntax. Click Done to finish creating the data source.

# Testing connection pool
The management console provides a Test Connection button to check that the connections from a connection pool can access the database. It can be accessed clicking the data source name and accessing the Test Connection button. 

Likewise, the CLI can be used to test if the data source was correctly configured. To check it use the following command:

<pre>
[standalone@localhost /] /subsystem=datasources/data-source=datasource_name:test-connection-in-pool
</pre>

# Data source example
The following data source configurations is for a MySQL database named MySqlDS

```
<datasources>
  <datasource jndi-name="java:jboss/MySqlDS"1 pool-name="MySqlDS">
    <connection-url>jdbc:mysql://localhost:3306/jbossdb</connection-url>
    <driver>mysql</driver>2
    <security>3
      <user-name>admin</user-name>
      <password>admin</password>
    </security>
    <validation>
      <valid-connection-checker class-name="org.jboss.jca.adapters.jdbc.extensions.mysql.MySQLValidConnectionChecker"/>
      <validate-on-match>true</validate-on-match>4
      <background-validation>false</background-validation>
      <exception-sorter class-name="org.jboss.jca.adapters.jdbc.extensions.mysql.MySQLExceptionSorter"/>
    </validation>
  </datasource>
</datasources>
```

1. The JNDI name that would be used to look up the data source by a component on the EAP server.

2. The name that refers to the driver that is defined in the <'drivers> section below the data source.

3. The credentials used to access the MySQL database.

4. Both validate on match and background validation are defined, but only one of them can be enabled. In this case, the data source will validate each connection as it is pulled from the connection pool using the class org.jboss.jca.adapters.jdbc.extensions.mysql.MySQLValidConnectionChecker.

The corresponding CLI command to define this data source is

```
[standalone@localhost] data-source add --name=MySqlDS --jndi-name=java:jboss/MySqlDS \
--driver-name=mysql \
--connection-url=jdbc:mysql://localhost:3306/jbossdb \
--user-name=admin --password=admin \
--validate-on-match=true --background-validation=false \
--valid-connection-checker-class-name=\
org.jboss.jca.adapters.jdbc.extensions.mysql.MySQLValidConnectionChecker \
--exception-sorter-class-name=\
org.jboss.jca.adapters.jdbc.extensions.mysql.MySQLExceptionSorter
```

A similar command can be used when in domain mode in the CLI to add a data source to a specific profile:

```
[domain@localhost] data-source add --name=MySqlDS --profile=full-ha \
--jndi-name=java:jboss/MySqlDS --driver-name=mysql \
--connection-url=jdbc:mysql://localhost:3306/jbossdb \
--user-name=admin --password=admin \
--validate-on-match=true --background-validation=false \
--valid-connection-checker-class-name=\
org.jboss.jca.adapters.jdbc.extensions.mysql.MySQLValidConnectionChecker \
--exception-sorter-class-name=\
org.jboss.jca.adapters.jdbc.extensions.mysql.MySQLExceptionSorter
```