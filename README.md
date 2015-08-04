WELCOME
=======
The goal of this module to enable the request of a resource at any point in a flow. This resource can be a file, a message (from VM, JMS, AMQP, etc.), an e-mail, etc. It's intended for resources that originally can only requested by message sources.

Some of its common use cases are:

- Load a file in the middle of a flow for processing.
- Consume messages (one, N, all) from a queue in the middle of a flow.
- Pull messages from a mail server on demand, to use its data in an enricher for example.

Usage
-----

### Single resource

```xml
<mulerequester:request config-ref="" resource="" timeout="" returnClass="" throwExceptionOnTimeout="" />
```

Request a resource from an address or endpoint. 
To make the request using the address, use the format "protocol://address". E.g.: "file://path/to/file". 
Otherwise, you can use a global endpoint name. E.g.: "fileEndpoint". 

Parameters:
- resource The address of the resource or the global endpoint name
- timeout The timeout to wait for when requesting the resource (optional - default 1000 ms)
- returnClass The return class to which this processor will transform the payload from the requested resource (optional)
- throwExceptionOnTimeout Whether to throw an exception or not if no message is received in the configured timeout (optional - default false)

Returns:
- A MuleMessage containing the requested resource as the payload

### Collection of resources

```xml
<mulerequester:request-collection config-ref="" resource="" timeout="" returnClass="" throwExceptionOnTimeout="" count="" />
```

Request a collection of resources from an address or endpoint. 
To make the request using the address, use the format "protocol://address". E.g.: "file://path/to/file". 
Otherwise, you can use a global endpoint name. E.g.: "fileEndpoint". 

Parameters:
- resource The address of the resource or the global endpoint name
- timeout The timeout to wait for when requesting the resource (optional - default 1000 ms)
- returnClass The return class to which this processor will transform the payload from the requested resource (optional)
- throwExceptionOnTimeout Whether to throw an exception or not if no message is received in the configured timeout (optional - default false)
- count Number of resources to retrieve. Default is -1 (all resources available).

Returns:
- A MuleMessageCollection with the requested resources as part of MuleMessages


TESTING
=======

This  project also contains test classes that can be run as part of a test
suite.

ADDITIONAL RESOURCES
====================
Everything you need to know about getting started with Mule can be found here:
http://www.mulesoft.org/documentation/display/MULE3INTRO/Home

There further useful information about extending Mule here:
http://www.mulesoft.org/documentation/display/DEVKIT/Home

Remember if you get stuck you can try getting help on the Mule user list:
http://www.mulesoft.org/email-lists

Also, MuleSoft, the company behind Mule, offers 24x7 support options:
http://www.mulesoft.com/enterprise-subscriptions-and-support

INSTALLATION
============
For MuleStudio
--------------
1. Download the update site zip file from the following location:
https://repository-master.mulesoft.org/nexus/content/repositories/releases/org/mule/modules/mule-module-requester/1.2/mule-module-requester-1.2-studio-plugin.zip
2. Install it in MuleStudio as a regular update site from a file. The module will appear under the Components tab.

For Maven
---------
```xml
<dependency>
    <groupId>org.mule.modules</groupId>
    <artifactId>mule-module-requester</artifactId>
    <version>1.2</version>        
</dependency>
```  

Modify (or include if you don't have it) your configuration for maven-mule-plugin as follows:
```xml
<plugin>
    <groupId>org.mule.tools</groupId>
    <artifactId>maven-mule-plugin</artifactId>
    <version>1.9</version>
    <extensions>true</extensions>
    <configuration>
        <excludeMuleDependencies>false</excludeMuleDependencies>
        <copyToAppsDirectory>true</copyToAppsDirectory>
        <inclusions>
            <inclusion>
                <groupId>org.mule.modules</groupId>
                <artifactId>mule-module-requester</artifactId>
            </inclusion>
        </inclusions>          
    </configuration>
</plugin>
```

Enjoy your Mule ride!

The Mule Team
