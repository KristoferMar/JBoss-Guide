# Exploring the CLI Tool

You can start the cli in the following way
<pre>
[student@workstation ~]$
sudo -u jboss
/opt/jboss-eap-7.0/bin/jboss-cli.sh
</pre>

To access the server in "offline mode" you can start an embedded server using the "exploring-cli.xml"
<pre>
[disconnected /] embed-server --server-config=exploring-cli.xml
</pre>
<br><br>

Verify the settings at the top level 
<pre>
[standalone@embedded /] /:read-resource
</pre>
<br><br>


Now we can verify the configuration of the default thread pool in the ejb3 subsystem using the absolute path.
<pre>
[standalone@embedded /] /subsystem=ejb3/thread-pool=default:read-resource
</pre>
<br><br>


Now you can navigate to the logging subsystem and verify the configuration recursively. 
<pre>
[standalone@embedded /] cd /subsystem=logging
[standalone@embedded subsystem=logging] :read-resource(recursive=true)
</pre>
<br><br>


A data source is a resource in the server that create connections responsible for accessing the database from a Java application. The data source specifies the nu,ber of connections that should be created to minimize the amount of time spent waiting for the connection to be available. 

Navigate to a data source named ExampleDS in the datasource subsystem and view the description of the test-connection-in-pool operation.
<pre>
[standalone@embedded subsystem=logging] cd \
/subsystem=datasources/data-source=ExampleDS
[standalone@embedded data-source=ExampleDS] :read-operation-description\
(name=test-connection-in-pool)
</pre>
<br><br>

Description of all attributes from the ExampleDS data source.
<pre>
[standalone@embedded data-source=ExampleDS] :read-resource-description
</pre>
<br><br>


### Modifying resources
Example of modifying data source to be at least 5 connections and up to ten connections within the pool.
<pre>
[standalone@embedded data-source=ExampleDS] :write-attribute\
(name=min-pool-size,value=5)
[standalone@embedded data-source=ExampleDS] :write-attribute\
(name=max-pool-size,value=10)
</pre>

Read the values
<pre>
[standalone@embedded data-source=ExampleDS] :read-attribute(name=min-pool-size)
[standalone@embedded data-source=ExampleDS] :read-attribute(name=max-pool-size)
</pre>

The above changes will be made dynamically directly within the standalone.xml file.
<br><br>


Create a new subsystem property named env whose value is production.
<pre>
[standalone@embedded data-source=ExampleDS] /system-property=\
env:add(value=production)
</pre>

Remember that when you want to create a new resource, you need to specify a name after the = and call the :add operation, passing the required attributes. You can discover the available attributes using the Tab key for auto completion.

To remove the path named user.home you can use use the /path.
<pre>
[standalone@embedded data-source=ExampleDS] /path=user.home:remove
</pre>
It was not possible to remove the user.home path because it is read-only.

remove subsystem
<pre>
[standalone@embedded data-source=ExampleDS] /subsystem=jsf:remove
</pre>

Now after some testing we can remove the exploring configuration.
<pre>
[student@workstation ~]$ sudo \
rm /opt/jboss-eap-7.0/standalone/configuration/exploring-cli.xml
</pre>