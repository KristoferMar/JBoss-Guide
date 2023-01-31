# Configuring JBoss EAP as a Standalone Server

The general structure of standalone.xml looks like this

<pre>
    <'server xmlns="urn:jboss:domain:4.1">
        <'extensions>
            ...list of extensions here
        <'/extensions>

        <'system-properties>
            ...system properties defined here
        <'/system-properties>

        <'management>
            ...management interfaces defined here
        <'/management>

        <'profile>
            ...list of subsystems and their configurations
        <'/profile>

        <'interfaces>
            ...interface definitions
        <'/interfaces>

        <'socket-binding-group>
            ...socket binding definitions
        <'/socket-binding-group>

        <'deployments>
            ...deployed applications go here
        <'/deployments>
    <'/server>
</pre>

## Extensions
Extensions are modules that extend the core capabilities of the server. A module is a bundle composed of a library developed in Java and an XML configuration file that provides Java EE compliant functionalities

<pre>
    <'extensions>
        <!-- list all extensions that you want made available to this server -->
        <'extension module="org.jboss.as.clustering.infinispan"/>
        <'extension module="org.jboss.as.deployment-scanner"/>
        <'extension module="org.jboss.as.ejb3"/>
        <'extension module="org.jboss.as.jpa"/>
    <'/extensions>
</pre>


## Management Interfaces
After the <'extensions> section in standalone.xml is the <'management> section, which is used for defining the management interfaces. The management interfaces allow remote clients to connect to the EAP instance for the purpose of managing the instance. EAP exposes two management interfaces:

- HTTP Interface: provides access to the management console.

- Native Interface: the native interface allows for management operations to be executed over a proprietary binary protocol, which is what the Command Line Interface (CLI) tool uses.

<pre>
<'management>
  ...
  <'management-interfaces>
            <'native-interface security-realm="ManagementRealm">
                <'socket-binding native="management-native"/>
            <'/native-interface>
            <'http-interface security-realm="ManagementRealm">
                <'socket-binding http="management-http"/>
            <'/http-interface>
  <'/management-interfaces>
  ...
<'/management>
</pre>

## Profiles and Subsystems
- If a subsystem appears within a profile, then that subsystem will be made available to the server using that profile.
- The subsystem allows for configuring the extension to suit the user's specific needs.

<pre>
    <'profile>
        <'subsystem xmlns="urn:jboss:domain:jsf:1.0"/>
        <'subsystem xmlns="urn:jboss:domain:jaxrs:1.0"/>

        ...other subsystem definitions
    <'/profile>
</pre>

Some subsystems have lots of configuration settings, like the datasources subsystem:

<pre>
    <'profile>
        <'subsystem xmlns="urn:jboss:domain:datasources:4.0">
            <'datasources>
                <'datasource jndi-name="java:jboss/datasources/ExampleDS" pool-name="ExampleDS" enabled="true" use-java-context="true">
                    <'connection-url>jdbc:h2:mem:test;DB_CLOSE_DELAY=-1;DB_CLOSE_ON_EXIT=FALSE<'/connection-url>
                    <'driver>h2<'/driver>
                    <'security>
                        <'user-name>sa<'/user-name>
                        <'password>sa<'/password>
                    <'/security>
                <'/datasource>
                <'drivers>
                    <'driver name="h2" module="com.h2database.h2">
                        <'xa-datasource-class>org.h2.jdbcx.JdbcDataSource</'xa-datasource-class>
                    <'/driver>
                <'/drivers>
            <'/datasources>
        <'/subsystem>
    <'/profile>
</pre>

