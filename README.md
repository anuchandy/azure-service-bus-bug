# azure-service-bus-bug

This sample uses the Session Processor based on the upcoming Service Bus (/Event Hubs) V2 stack.

The upcoming V2 stack is under testing in the [PR](https://github.com/Azure/azure-sdk-for-java/pull/34854) and is not released. So, to test the Session Processor based on V2 stack, it is required to build the Service Bus version `7.15.0-beta.4` from this PR source.

### The steps to build from the source.

1. Clone the fork hosting the V2 work.
> git clone git@github.com:anuchandy/azure-sdk-for-java.git
2. Change directory to root of the clone
> cd azure-sdk-for-java
3. Checkout the V2 working branch
> git checkout -b build-v2 origin/messaging-integrate-new-core-types-1
4. Build the tools
> mvn clean install -f eng/code-quality-reports/pom.xml
5. Build the azure-messaging-servicebus:7.15.0-beta.4 (also builds azure-amqp-core: 2.9.0-beta.6 transitively)
> mvn clean install --settings ./eng/settings.xml -Dcheckstyle.skip=false -Dgpg.skip -Dmaven.javadoc.skip -Drevapi.skip -DskipSpringITs -DskipTests -Dspotbugs.skip -Djacoco.skip -Denforcer.skip=true -pl com.azure:azure-messaging-servicebus -am

At this point you will have azure-messaging-servicebus:7.15.0-beta.4 built and installed to local `.m2` directory. 

### Run this project using azure-messaging-servicebus:7.15.0-beta.4

The project is already updated with the following steps, but listing the steps here just as documentation. 

1. Update the POM to use 7.15.0-beta.4

```xml
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-messaging-servicebus</artifactId>
    <version>7.15.0-beta.4</version> 
</dependency>
```

2. When building the Session Processor opt-in for V2 stack (by default it’s V1, exactly same as binaries in maven)

```java

.configuration(new com.azure.core.util.ConfigurationBuilder()
    .putProperty("com.azure.messaging.servicebus.session.processor.asyncReceive.v2", "true")
    .build())
```


3. Rebuild the project
> mvn clean install 