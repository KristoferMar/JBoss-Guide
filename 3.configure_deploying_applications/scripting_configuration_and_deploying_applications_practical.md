# Scripting Configuration and Deploying Applications

We start an EAP standalone server using standalone2 and then verify it's running
<pre>
[student@workstation ~]$ cd /opt/jboss-eap-7.0/bin
[student@workstation bin]$ sudo -u jboss ./standalone.sh \
-Djboss.server.base.dir=/opt/jboss/standalone2/
</pre>

We open a new terminal window and connect to it
<pre>
[student@workstation ~]$ cd /opt/jboss-eap-7.0/bin
[student@workstation bin]$ sudo -u jboss ./jboss-cli.sh -c --controller=localhost:9990
</pre>

We navigate to socket-binding 
<pre>
[standalone@localhost:9990 /] cd /socket-binding-group=standard-sockets
</pre>

We navigate to socket binding called "http"
<pre>
[standalone@localhost:9990 socket-binding-group=standard-sockets] cd \
socket-binding=http
</pre>

We change the port attribute to 8081
<pre>
[standalone@localhost:9990 socket-binding=http] :write-attribute\
(name=port,value=8081)
</pre>

We read the resource properties to the socket binding.
<pre>
[standalone@localhost:9990 socket-binding=http] :read-resource
</pre>

We reload the standalone server to start listening on port 8081
<pre>
[standalone@localhost:9990 socket-binding=http] cd /
[standalone@localhost:9990 /] :reload
</pre>
<br><br>


## Change the log level

<pre>
[standalone@localhost:9990 /] cd /subsystem=logging/console-handler=CONSOLE
</pre>

Change level to INFO
<pre>
[standalone@localhost:9990 console-handler=CONSOLE] :write-attribute\
(name=level,value=INFO)
</pre>

Read the resource properties of the CONSOLE console handler
<pre>
[standalone@localhost:9990 console-handler=CONSOLE] :read-resource
</pre>
<br><br>


## Deploy the application
We use the deploy command
<pre>
[standalone@localhost:9990 console-handler=CONSOLE] cd /
[standalone@localhost:9990 /] deploy \
/tmp/bookstore.war 
</pre>