# Deploying Applications to a Standalone Server

The <'deployments> section of standalone.xml lists the deployed applications on the standalone server. The <'deployments> element contains a single child element named <'deployment>. 

Here is an example of a <'deployments> section:

```
<deployments>
  <deployment name="bookstore.war" runtime-name="bookstore.war">
    <content sha1="e1e57cb8b89371794d6c7e80baeb8bf0e3da4fcf"/>
  </deployment>
  <deployment name="example.war" runtime-name="example.war" enabled="false">
    <content sha1="0a07b224819ce516b231b1afba0eadc45b272298"/>
  </deployment>
  <deployment name="version.war" runtime-name="version">
    <fs-exploded path="deploying-filesystem/version.war" relative-to="labs"/>
   </deployment>
</deployments>
```

The <'content> element represents a secure hash algorithm (SHA) value of the deployment file, and is used internally as a unique identifier for the deployment. 

## Deploying applications with management tools

There are three ways to deploy an application using a standalone server:

### The management console
Here you choose "Deployments in the browser and press "add" and then upload your application.

The next step on the wizard has three options to be defined:

- Name: Identifies the deployment and must be a unique value across all deployments.

- Runtime Name: Defines the context of the application. The context is the name of the application in the runtime environment. If a deployment has a runtime name defined as myapp, it will be available on http://server:port/myapp.

- Enabled: Defines if the deployment should start immediately. If it is not checked, it is possible to enable it later. 

You can "Disable", "enable" and "replace" deployment applications in the browser aswell.


### Deploy using a CLI
Connect to the CLI with controller
<pre>
[student@workstation ~]$ cd /opt/jboss-eap-7.0/bin
[student@workstation bin]$ ./jboss-cli.sh --connect --controller=localhost:19990
</pre>
<br><br>

The CLI provides the deploy command to start a new deployment. This command has the following arguments.

    file_path: Path to the application to deploy.

    --url: URL at which the deployment content is available for upload to the deployment content repository.

    --name: The unique name of the deployment. if no name is provided then the file name is used.

    --runtime-name: Optional, defines the runtime name for the deployment.

    --force: If the deployment with the specified name already exists, by default, the deployment is aborted and the corresponding message is printed. The --force (-f) forces the replacement of the existing deployment with the one specified in the command arguments.

    --disabled : Indicates that the deployment has to be added to the repository in a disabled state.

To deploy an application that is located at /home/student/myapp.war use the following command:
<pre>
[standalone@localhost:19990 /] deploy /home/student/myapp.war --name=myapp.war
</pre>

#### Undeploying using the CLI tool 
The CLI provides the undeploy command to remove a deployment. This command has several arguments and the most common are:

    name: The name of the application to undeploy.

    --keep-content: Disable the deployment but do not remove its content from the repository.

[standalone@localhost:19990 /] undeploy myapp.war
<br><br>


### The file system deployer
The file system deployer is a subsystem from EAP that will scan for JEE compliant app at a certain directory.

The standalone server supports deployment of a new application manually. To start a manually deployment, it is only required to copy the application to the JBOSS_HOME/deployments folder. 

The deployment process is managed using marker files. A marker file is an empty file that uses the same name as the application, and adds the objective of the marker file to the end of the file name. The marker file should be created in the deployments folder. The following marker files extensions can be used:

    .dodeploy: It is created by the user. It triggers a new deployment of the application.

    .deployed: It is created by the deployment scanner. It indicates that the application has been deployed.

    .isdeploying: It is created by the deployment scanner. It indicates that the application is deploying.

    .failed: It is created by the deployment scanner. It indicates that the application has failed.

    .isundeploying: It is created by the deployment scanner. It indicates that the application is undeploying.

    .undeployed: It is created by the deployment scanner. It indicates that the application has been undeployed.

    .skipdeploy: It is created by the user. It indicates that the application should not be deployed.

    .pending: It is created by the deployment scanner. It indicates that it has noticed the need to deploy content but has not yet instructed the server to deploy it. 

To deploy an application named myapp.war, create the myapp.war.dodeploy marker file.