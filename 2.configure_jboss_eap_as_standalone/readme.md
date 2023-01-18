# Configure standalone 
Location for standalone configuration
<pre>
JBOSS_HOME/standalone/configuration/standalone.xml
</pre>


### JBOSS_HOME/standalone/configuration/configuration
- configuration: contains the configuration files for a standalone server, along with a standalone_xml_history subfolder that maintains a version history of the configuration files. This folder also contains the properties file logging.properties for configuring loggers, a users file mgmt-users.properties, for defining login credentials for securing the management interfaces and a groups file mgmt-groups.properties, for defining login roles.

### JBOSS_HOME/standalone/configuration/deployments
- This folder holds all deployed applications on the server.
- Any jar, war or ear file you copy to this directory will be automatically deployed to the server an avaiable application.

### JBOSS_HOME/standalone/configuration/lib
- is used for deploying common JAR files. 

## JBOSS_HOME/standalone/data
- data: a location available to subsystems that store content in the file system, such as message queue or an in-memory database.

### JBOSS_HOME/standalone/configuration/log
- log: the default location for the server log files, including gc.log.0.current and server.log, which contains startup logs from EAP.

### JBOSS_HOME/standalone/configuration/tmp
- tmp: for temporary files, such as the shared-key mechanism used by the command line interface (CLI), to authenticate local users to the server.


# Creating standalone server

# Configure standalone server
