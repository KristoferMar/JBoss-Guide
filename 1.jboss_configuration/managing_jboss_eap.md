## 
The cli and web interface use the same mechanisms and both go through the managementport 9990.

How to open the JBoss CLI
<pre>
sudo -u jboss /opt/jboss-eap-7.0/bin/jboss-cli.sh --connect
</pre>

To manage the jboss server we first start it
<pre>
[kris@workstation ~]$ sudo -u jboss /opt/jboss-eap-7.0/bin/standalone.sh
</pre>

We create users in the following way
<pre>
[kris@workstation ~]$ sudo -u jboss /opt/jboss-eap-7.0/bin/add-user.sh
</pre>

After creating a user you can insepct the mgmt-users.properties file which is for defining users who need access to the EAP management console, either through the web interface, or using the CLI.

The path is
<pre>
JBOSS_HOME/standalone/configuration/mgmt-users.properties.
</pre>

You can then access the mangement console through the web browser using the following url
<pre>
http://localhost:9990/
</pre>


### Verify configuration changes
All configuration changes are made directly into the standalone.xml file. You can therfore verify the changes directly in the standalone.xml file. 
<pre>
JBOSS_HOME/standalone/configuration/standalone.xml
</pre>

We can then also launch the cli and make changes directly through the cli. 

<pre>
[kris@workstation ~]$ sudo -u jboss /opt/jboss-eap-7.0/bin/jboss-cli.sh \
 --connect
[standalone@localhost:9990 /] /subsystem=deployment-scanner/scanner=default:read-resource
{
    "outcome" => "success",
    "result" => {
        "auto-deploy-exploded" => false,
        "auto-deploy-xml" => true,
        "auto-deploy-zipped" => true,
        "deployment-timeout" => 600,
        "path" => "deployments",
        "relative-to" => "jboss.server.base.dir",
        "scan-enabled" => true,
        "scan-interval" => 8000
    }
}
</pre>


# Uninstall jboss-eap
That is done by using the native uninstaller jar file
<pre>
[student@workstation ~]$ sudo \
java -jar /opt/jboss-eap-7.0/uninstaller/uninstaller.jar
</pre>

# Install jboss-eap from a installation file
If an installation xml file is present, then you can create your jboss-eap server by pointing towards the installation file. 
<pre>
[student@workstation ~]$ cd /home/student/JB248/installs/
[student@workstation ~]$ sudo java -jar jboss-eap-7.0.0-installer.jar \
 ../labs/features-eap/myinstall.xml
</pre>


# Chaning ownership of the server to jboss
<pre>
[student@workstation ~]$ sudo chown -R jboss:jboss /opt/jboss-eap-7.0
</pre>