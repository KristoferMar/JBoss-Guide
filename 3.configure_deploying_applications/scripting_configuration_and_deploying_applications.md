# Scripting Configuration and Deploying Applications

## Executing commands

### Operations
The following operations are very common
• :read-resource: Reads a model resource's attribute values and either basic or complete
information about any child resources.
• :read-operation-names: Gets the names of all the operations for the given resource.
• :read-operation-description: Gets the description of given operation.
• :reload: Reloads the server by shutting down all its services and starting them again. The JVM
itself is not restarted.
• :read-attribute: Gets the value of an attribute for the selected resource.
• :write-attribute: Sets the value of an attribute for the selected resource.
• :remove: Removes the node.

### Commands
Commands contains a user friendly syntax and most of them translate into operation requests.
The following commands are the most basic supported commands for the CLI
• cn or cd: Change the current node path to the argument.
• connect: Connect to the server or domain controller.
• data-source: Used to manage resources of type /subsystem=datasources/data-source.
• deploy: Deploy an application.
• help: Display the help page.
• history: Print or disable,enable, or clear the history expansion;
• ls: list the contents of the node path.
• pwn or pwd: Prints the current working node.
• exit or quit: Quit the command line interface.
• undeploy: Undeploy an application.
• version: Prints the version and environment information.
