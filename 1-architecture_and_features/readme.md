# Red Hat JBoss Enterprise Application Platform: Architecture and Features

## Understanding Extensions, Subsystems, and Profiles

## 

##

Installation link
https://access.redhat.com/jbossnetwork/restricted/listSoftware.html?downloadType=distributions&product=appplatform&version=7.4

 ## Starting and stopping EAP

 To start EAP with out-of-the-box configurations, run the startup script specified for the desired operating mode.

 For standalone server:
<pre>
$ ${JBOSS_HOME}/bin/standalone.sh
</pre>
 For managed domain host controller (which will be the master)
 <pre>
 $ ${JBOSS_HOME}/bin/domain.sh
 </pre>

 To stop EAP, there are three choices
- Interrupt the server instance (or host controller) process using Ctrl+C
- Kill the server instance (or host controller) process using the kill command (Unix and OS X) or a GUI task manager.
- Use the management CLI (shown later in this student guide.)

# 

# Managing JBoss EAP

link here