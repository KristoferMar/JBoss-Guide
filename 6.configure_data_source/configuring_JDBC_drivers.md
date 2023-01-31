# Configuring JDBC Drivers

A JDBC driver is a component that Java applications use to communicate with a database. A JDBC driver is packaged in a JAR (Java Archive) file, and contains a class file that contains the driver definition. JDBC drivers are available from database vendors, such as from MySQL or PostgreSQL. In order to use a JDBC driver in EAP 7, it first needs to be installed as a module on the server. 

Adding the JDBC driver module can be done either manually or much more simply with the EAP CLI. The module add command from CLI requires the user to provide:

- Name: A name based on a Java package name that will be used by EAP to store the JAR file and a configuration file called module. It should be unique and cannot conflict with existing libraries available at $JBOSS_HOME/modules directory.

- Resources: Refers to the location that stores the JAR file.

- Dependencies: Identify which Java libraries are used by the JDBC driver and where they are located in EAP.

The syntax for the module add command is:

<pre>
[disconnected /] module add
--name=<module_name>
--resources=<JDBC_Driver>
--dependencies=<library1>,<
library2>,...
</pre>

For example, the following command in the EAP CLI will create the JDBC MySQL 5.5 module by using the file mysql-connector-java.jar that was downloaded from the database vendor:

<pre>
[disconnected /] module add --name=com.mysql \
--resources=/opt/jboss-eap-7.0/bin/mysql-connector-java.jar \
--dependencies=javax.api,javax.transaction.api
</pre>

After executing this command, a new directory is created at /opt/jboss-eap-7.0/modules/com/mysql/main. The new directory contains the provided JAR file as well as a module.xml that is generated based on the driver's given dependencies.

The following module.xml file defines a module for the MySQL JDBC driver:

```
<?xml version="1.0" ?>

<module xmlns="urn:jboss:module:1.1" name="com.mysql">

    <resources>
        <resource-root path="mysql-connector-java.jar"/>
    </resources>

    <dependencies>
        <module name="javax.api"/>
        <module name="javax.transaction.api"/>
    </dependencies>
</module>
```

To remove a module from EAP, the server must be stopped and the following command can be used in the CLI:

```
[disconnected /] module remove --name=<module_name>
```

## Defining drivers
After adding the driver as a module, the next step is to create a <'driver> definition in the <'drivers> section of the data source subsystem in the EAP configuration file.

A CLI operation can be used to create it easily, without touching the XML file. The following command can be executed on a stand-alone server:

```
/subsystem=datasources/jdbc-driver=<driver_name>:add\
(driver-module-name=<module_name>,driver-name=<unique_driver_name>)
```

In the driver definition command, the following fields are required:

```
    driver-name: A unique name for the driver.

    driver-module-name: The unique name from the module installed at $JBOSS_HOME/modules directory.
```

For the managed domain mode, the syntax is as follows: 

```
/profile=<profile_name>/subsystem=datasources/jdbc-driver=<
driver_name>:add\
(driver-module-name=<module_name>,driver-name=<unique_driver_name>)
```

For example, the CLI command to define the MySQL driver in the default profile in a managed domain looks like the following:
<pre>
[domain@172.25.250.254 /] /profile=default/subsystem=datasources/jdbc-​driver=mysql:add(\
           driver-module-name=com.mysql,\
           driver-name=mysql\
)
</pre>
<br>

The domain.xml/standalone.xml configuration file will have an additional driver tag similar to the following after the driver is defined using the CLI:

```
<drivers>
    <driver name="mysql" module="com.mysql"/>
</drivers>
```

After defining the <'driver> within the <'drivers> section of the data source subsystem, the driver is referenced by its name attribute. After completing this step, users can create data sources that utilize the driver. 