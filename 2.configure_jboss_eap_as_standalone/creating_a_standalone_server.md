# Creating a Standalone Server
A method to customize the EAP server but still leaving the original installation untouched is to copy the directories to a new location.

<pre>
[kris@workstation ~]$ cd /opt/jboss-eap-7.0/standalone
[kris@workstation standalone]$ cp -r configuration deployments lib \
~/JB248/labs/standalone-instance
</pre>

You can then modify the Port Offset for the Standalone Server
- open /home/kris/JB248/labs/standalone-instance/configuration/standalone.xml
<pre>
...
<'socket-binding-group name="standard-sockets" default-interface="public" port-offset="${jboss.socket.binding.port-offset:10000}">
...
</pre>

### Run server with the new modifications 
<pre>
[kris@workstation standalone]$ cd /opt/jboss-eap-7.0/bin
[kris@workstation bin]$ ./standalone.sh \
-Djboss.server.base.dir=/home/kris/JB248/labs/standalone-instance/
</pre>

Now you can 
- Point the browser to http://localhost:18080 to see the EAP welcome page with the new port offset.

- Point the browser to http://localhost:19990 to see the EAP Management Console that is running with the same port offset.