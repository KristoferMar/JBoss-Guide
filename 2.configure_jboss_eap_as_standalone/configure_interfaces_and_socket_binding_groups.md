# Configuring Interfaces and Socket Binding Groups

## Interfaces
An interface is a logical name for a network interface, IP address, or host name to which a socket can be bound. The <'interfaces> element is used to define an interface using the <'interface> child element. For example, the <'interfaces> section defined in the default standalone.xml configuration file looks like the following:

<pre>
    <'interfaces>
            <'interface name="management">
                <'inet-address value="${jboss.bind.address.management:127.0.0.1>
            <'/interface>
            <'interface name="public">
                <'inet-address value="${jboss.bind.address:127.0.0.1}"/>
            <'/interface>
    <'/interfaces>
</pre>


Users can configure the IP address for accessing the Management Console, when starting EAP, using the command line variable -bmanagement. For example:
<pre>
$ ./standalone.sh -bmanagement 127.0.0.1
</pre>

The name of the interface can be any value and there are various options for defining interfaces. If a host has multiple Network Interface Cards (NICs) with multiple IP addresses and only a certain NIC should be exposed, then use the first of the following interface configurations. If all the network interfaces need to be exposed, use the second interface configuration.
<pre>
    <'interfaces>
        <'interface name="my_ip_address">
            <'inet-address value="128.164.0.15"/>
        <'/interface>
    <'/interfaces>
</pre>

The following interface uses the wildcard any-address to bind to any and all IP addresses available on the server:
<pre>
    <'interfaces>
        <'interface name="global">
            <'any-address/>
        <'/interface>
    <'/interfaces>
</pre>


The interface above demonstrates the use of an interface criteria, which is used at runtime to determine what IP address to use for an interface. The following interface also uses a criteria that will only bind to an IPv4 address:
<pre>
    <'interfaces>
        <'interface name="ipv4-global">
            <'any-ipv4-address/>
        <'/interface>
    <'/interfaces>
</pre>


Criteria can be combined to be very specific about an interface definition. For example, the following interface defines a specific subnet that supports multicast. The address must be up, and cannot be point-to-point:
<pre>
    <'interfaces>
        <'interface name="my_specific_interface">
            <'subnet-match value="192.168.0.0/16"/>
            <'up/>
            <'multicast/>
            <'not>
                <'point-to-point/>
            <'/not>
        <'/interface>
    <'/interfaces>
</pre>

On a Linux™ environment, users can define an interface for a specific network card:
<pre>
    <'interface name="internal">
        <'nic name="eth1"/>
    <'/interface>
</pre>

# Socket Binding Groups
A socket binding group is a named collection of socket bindings, which allows users to define all of the ports needed for an EAP instance. A socket binding group is defined using the <'socket-binding-group> element, which consists of a collection of <'socket-binding> child elements. For example, here is the default socket binding group defined in standalone.xml:

```
    <socket-binding-group name="standard-sockets" default-interface="public" port-offset="${jboss.socket.binding.port-offset:0}">
            <socket-binding name="management-http" interface="management" port="${jboss.management.http.port:9990}"/>
            <socket-binding name="management-https" interface="management" port="${jboss.management.https.port:9993}"/>
            <socket-binding name="ajp" port="${jboss.ajp.port:8009}"/>
            <socket-binding name="http" port="${jboss.http.port:8080}"/>
            <socket-binding name="https" port="${jboss.https.port:8443}"/>
            <socket-binding name="txn-recovery-environment" port="4712"/>
            <socket-binding name="txn-status-manager" port="4713"/>
            <outbound-socket-binding name="mail-smtp">
                <remote-destination host="localhost" port="25"/>
            </outbound-socket-binding>
    </socket-binding-group>
```


## Start a jboss instance on a different socket bind

```
[kris@workstation standalone]$ sudo -u jboss \
/opt/jboss-eap-7.0/bin/standalone.sh \
-Djboss.server.base.dir=/opt/jboss/standalone2/ \
-Djboss.socket.binding.port-offset=10000
```