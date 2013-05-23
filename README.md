Leightweigth Camel CDI Integration
==================================

Camel supports since version 2.10 CDI with "camel-cdi".
This implementation relies on http://incubator.apache.org/deltaspike and uses a common pattern (Singleton/Startup) to start an CamelCDIContext.
I also have a few ideas from this project used. 

But here, the main actor is a CDI-Extension: 
- creates the CamelContext and integrates the CDI-BeanManager with them
- discovers and registers all available RouteBuilders
- discovers and registers all beans with camel annotations: @Consume, @Produce
- starts the context.

That's all. Nothing more to do ...

How to 
- create a consumer ? Write a bean and use @Consume (see SampleFileConsumer)
- create a producer ? Write a bean and use @Produce (see SampleJmsProducer)
- create a route ? Write a bean an implement RouteBuilder (see SampleFileRoute, SampleJmsRoute)
- create a componet ? Write a bean/component and use @Named (see JmsComponent/HelloWorldComponent).

The rest is done by CDI magic. 

I have only WELD and JBoss-AS 7 for development and tests used.

Projects:
--------
Build the project as usual with 'mvn install'.
Run maven with the profile 'local' and parameter 'jboss.as.home' copies a test application and camel modules in the corresponding JBoss directories.
(mvn install -Plocal -Djboss.as.home=JBOSS_AS_HOME_PATH) 

- cci:
  Implementation of a litte bit CDI magic, JUnit tests using WELD
  
- cci-modile: 
  JBoss 7 module with some required camel components. 
  Unzip the artefact in the jboss-as-xxx/modules directory.
  
  
- cci-testapp:
  Simple REST-application, copy the artefact to jboss-as-xxx/standalone/deployments directory.
  The JMS-samples require an running activemq-broker on the same host (with default port 61616).
  Test the application with 
  curl localhost:8080/cci
  curl localhost:8080/cci/jms/hallo - routes "hallo" to the topic 'data'
  curl localhost:8080/cci/file/hallo - routes "hallo" to the jboss-as-xxx/standalone/tmp/end     
 


 
 