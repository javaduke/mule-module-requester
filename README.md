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

If you have more than one connector in your configuration you will need to link the resource request to it. This can be done by appending '?connector=<CONNECTOR NAME>' at the end of the URL for the resource. More info at:
https://docs.mulesoft.com/mule-user-guide/v/3.7/mule-endpoint-uris

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

If you have more than one connector in your configuration you will need to link the resource request to it. This can be done by appending '?connector=<CONNECTOR NAME>' at the end of the URL for the resource. More info at:
https://docs.mulesoft.com/mule-user-guide/v/3.7/mule-endpoint-uris

Parameters:
- resource The address of the resource or the global endpoint name
- timeout The timeout to wait for when requesting the resource (optional - default 1000 ms)
- returnClass The return class to which this processor will transform the payload from the requested resource (optional)
- throwExceptionOnTimeout Whether to throw an exception or not if no message is received in the configured timeout (optional - default false)
- count Number of resources to retrieve. Default is -1 (all resources available).

Returns:
- A MuleMessageCollection with the requested resources as part of MuleMessages

INSTALLATION
============
For MuleStudio
--------------
1. Download the update site zip file from the following location:
https://repository-master.mulesoft.org/nexus/content/repositories/releases/org/mule/modules/mule-module-requester/1.3/mule-module-requester-1.3-studio-plugin.zip
2. Install it in MuleStudio as a regular update site from a file. The module will appear under the Components tab.

For Maven
---------
```xml
<dependency>
    <groupId>org.mule.modules</groupId>
    <artifactId>mule-module-requester</artifactId>
    <version>1.3</version>        
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

TECHNICAL DETAILS
=================
Overview
--------
The Mule Requester is an abstraction on top of the actual transport (JMS, File, etc.), which provides the following extra functionality:
- Reusability 
- Transformation
- Timeout
The module relies on a call to MuleClient.request() which in turn delegates on each of the transport's requester classes.
 
Limitations
-----------
- It's responsibility of each underlying transport how resources are retrieved and if it has any logic in place to delete/move/copy the requested resource. To analyze this you need to check the actual requester for the transport, which is usually <transport name>MessageRequester, e.g.: FileMessageRequester.
- The same applies for timeouts: if the underlying requester of the transport doesn't use them, then the Mule Requester won't use them either, as expected.
- Filters and any transport specific configuration fall under the same considerations.
- It's also important to highlight that some attributes that are configured at the endpoint or connector level may be used by the MessageReceiver of the transport, but ignored or not used in the MessageRequester of the same module. This can be established by looking at the source code of the latter.
- The request-collection operation makes use of sequential calls to the specified resource, so if the underlying transport doesn't delete it, the same resource will be requested again. This can be prevented by (provided the underlying transport allows it):
a. Setting a transformer in the operation so the resource is consumed
b. Setting the auto delete flag of the transport to true
This way the resource will be consumed, transformed and deleted. Otherwise this operation will fall into an infinite loop, requesting always the same resource. 


