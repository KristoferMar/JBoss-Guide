# Configuring JDBC Drivers

We start by accessing our eap server
<pre>
[student@workstation ~]$ cd
/opt/jboss-eap-7.0/bin
[student@workstation bin]$ sudo -u jboss ./jboss-cli.sh
</pre>

Located in the /usr/share/java directory from the workstation VM is the MySQL JDBC JAR file. It is a driver provided by Red Hat's yum repository and it was installed during the VM installation by running yum -y install mysql-connector-java as root.

By installing it as a module, it becomes available as a driver in any EAP instances based on this installation in order to connect to MySQL databases. 

The following command in EAP CLI will create the module by pointing to the JBCD JAR file, listing the JAR's dependencies and the MySQL driver vendor ID as the name

<pre>
[disconnected /] module add --name=com.mysql \
--resources=/usr/share/java/mysql-connector-java.jar \
--dependencies=javax.api,javax.transaction.api
</pre>

Now i Evaluate if the driver was correctly installed as a module by checking if a directory was created with module.xml and mysql-connector-java.jar using the following command from the terminal window:

<pre>
[student@workstation ~]$ ls /opt/jboss-eap-7.0/modules/com/mysql/main
module.xml  mysql-connector-java.jar
</pre>

Evaluate the module.xml file

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

## Define the MySQL Driver
We start my connecting to the server through managed.
<pre>
[disconnected /] connect localhost:9990
</pre>

Now we define the MySQL driver by specifying the EAP module installed.
<pre>
[standalone@localhost:9990 /]
/subsystem=datasources\
/jdbc-driver=mysql:add(driver-name=mysql,driver-module-name=com.mysql)
</pre>


## Verify the driver
We can inspect the MySQL JDBC driver in the following way.

<pre>
[standalone@localhost:9990 /] /subsystem=datasources/jdbc-driver=mysql:read-resource
</pre>


# Extras

## Scripted JDBC driver installation
The following is an example of how to install a JDBC driver through scripting 
<pre>
./jboss-cli.sh -c --command="module add --name=org.mysql --resources=/home/user/mysql-connector-java-5.1.18-bin.jar  --dependencies=javax.api,javax.transaction.api"
</pre>