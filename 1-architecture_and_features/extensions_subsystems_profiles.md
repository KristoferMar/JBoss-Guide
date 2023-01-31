# Understanding Extensions, Subsystems, and Profiles
## JBoss modules 
JBoss Modules is designed to work with any existing library or application without changes, and its simple naming and resolution strategy is what makes that possible. 

- The JBOSS_HOME/modules/system/layers/base folder contains the modules provided by the EAP 7 product.

- Third-party products that add modules to EAP 7 are expected to create their own folders inside JBOSS_HOME/modules/system/layers.

- The system administrator is expected to add local modules as folders, directly below JBOSS_HOME/modules.

- Navigating down the path of modules you will eventually find the .jar file which gets loaded by the server system/layers/base/org/woldfly/extension/undertow/main/wildfly-undertow-7.0.0.0.GA-redhat-2.jar

## JBoss extensions
Extensions are added to an EAP instance using the '<'extension'>' element, which appears at the beginning of the EAP main configuration file (standalone.xml or domain.xml). The following XML listing shows some of the extensions declared in the out-of-the-box standalone.xml configuration file:

<pre>
    <'?xml version="1.0" ?>
    server xmlns="urn:jboss:domain:4.1">
        <'extensions>
            <'extension module="org.jboss.as.clustering.infinispan"/>
            <'extension module="org.jboss.as.clustering.jgroups"/>
            <'extension module="org.jboss.as.connector"/>
            <'extension module="org.jboss.as.ee"/>
            <'extension module="org.jboss.as.ejb3"/>
    ... remainder of configuration file ...
    <'/server>
</pre>

### Subsystems
Some Extensions make use of subsystems and these are also configured within the standalone.xml file. 

For example, the org.jboss.as.jpa extension provides the jpa subsystem. This subsystem configuration, in the out-of-the-box standalone.xml configuration file, looks like:
<pre>
<'subsystem xmlns="urn:jboss:domain:jpa:1.1">
    <'jpa default-datasource="" default-extended-persistence-inheritance="DEEP"/>
<'/subsystem>
</pre>

To see all subsystems available you can do
<pre>
ls docs/schema
</pre>


# Subsystems and Profiles
A profile keeps a collection of subsystems which are configured for your Jboss server in various ways.

A subsystem configuration does not stand by itself in the EAP 7 configuration files. A set of subsystem configurations is grouped inside a <'profile> element.

1. default: Is the most commonly used subsystems, including logging, security, datasources, infinispan, weld, webservices, and ejb3. The default implements not only the JEE Web Profile, but also most of the JEE Full Profile.

2. ha: Contains the exact same subsystems as the default profile, with the addition of clustering capabilities, provided mainly by the jgroups subsystem.

3. full: Is similar to the default profile, but notably adds the messaging (messaging-activemq) and a few other less used subsystems.

4. full-ha: Is same as the full profile, but with the addition of clustering capabilities. 

Look in the JBOSS_HOME/standalone/configuration folder. Notice there are four standalone configuration files:

    standalone.xml: Which compares to the default profile in domain.xml.

    standalone-ha.xml: Which compares to the ha profile in domain.xml.

    standalone-full.xml: Which compares to the full profile in domain.xml.

    standalone-full-ha.xml: Which compares to the full-ha profile in domain.xml.