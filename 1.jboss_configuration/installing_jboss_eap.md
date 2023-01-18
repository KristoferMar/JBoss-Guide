
If you have the jar file available directly. 
<pre>
[kris@workstation installs]$ sudo java -jar \
 jboss-eap-7.0.0-installer.jar
</pre>
<br><br>

Use the following as installation path 
<pre>
/opt/jboss-eap-7.0
</pre>
<br><br>

- Create an administrativ user
- choose Perform default configuration

- Press Generate installation script and properties file
    - Store this xml file(s) in the jboss main directory

To configure jboss on your machine you configure your .bashrc with the following.
<pre>
JBOSS_HOME=/opt/jboss-eap-7.0
PATH=$PATH:$JBOSS_HOME/bin
export JBOSS_HOME PATH   
</pre>

Add the jboss user
<pre>
[kris@workstation ~]$ sudo useradd -r jboss

[kris@workstation installs]$ id jboss
</pre>

Configure accessrights for JBOSS_HOME
<pre>
[kris@workstation installs]$ sudo chown -R jboss:jboss /opt/jboss-eap-7.0
</pre>

Configure the admin password through the "myinstall.xml.variables" by accessing the file and setting the admin password. 

Start JBoss eap with 
<pre>
[kris@workstation ~]$ sudo -u jboss /opt/jboss-eap-7.0/bin/standalone.sh
</pre>


# Setup EAP as a service 

Inspect the init script configuration file available at /opt/jboss-eap-7.0/bin/init.d/jboss-eap.conf with your favorite text editor as the jboss user using sudo -u jboss vim, to evaluate its contents.

Configure the following variables in that file.
- JAVA_HOME: /etc/alternatives/java_sdk (The directory where Java is installed)
- JBOSS_HOME:/opt/jboss-eap-7.0(The directory where EAP is installed)
- JBOSS_USER: jboss (The owner of the EAP process)
- JBOSS_MODE:standalone (The mode to start EAP (standalone or domain))
- JBOSS_CONFIG:standalone.xml (The configuration file that should be used by the process)
- JBOSS_CONSOLE_LOG: /var/log/jboss-eap/console.log (The file where all the logs will be stored)
<br><br>

Copy the file "jboss-eap.conf" to /etc/default
<pre>
[kris@workstation ~]$ sudo cp \
/opt/jboss-eap-7.0/bin/init.d/jboss-eap.conf \
/etc/default/jboss-eap.con
</pre>

To make the init script visible for systemctl, the script must be stored at /etc/init.d with execution permission
<pre>
[kris@workstation ~]$ sudo cp \
/opt/jboss-eap-7.0/bin/init.d/jboss-eap-rhel.sh \
/etc/init.d/jboss-eap
[kris@workstation ~]$ sudo chmod 755 /etc/init.d/jboss-eap
</pre>

Now we can can work with jboss-eap using systemctl

<pre>
[kris@workstation ~]$ sudo systemctl daemon-reload

[kris@workstation ~]$ sudo systemctl enable jboss-eap

[kris@workstation ~]$ sudo systemctl start jboss-eap

[kris@workstation ~]$ sudo systemctl disable jboss-eap

[kris@workstation ~]$ sudo systemctl stop jboss-eap
</pre>