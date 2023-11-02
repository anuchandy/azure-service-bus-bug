# azure-service-bus-bug

This project uses the Session Processor based on the upcoming Service Bus library built on V2 stack. Currently, the V2 stack based libraries is being released as beta and undergoing stress testing (As of November 2023, the latest beta is 7.15.0-beta.4).

### Running the project using azure-messaging-servicebus:7.15.0-beta.4

The project is already updated with the following steps, but listing the steps here just as documentation. 

1. Update the POM to use 7.15.0-beta.4

```xml
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-messaging-servicebus</artifactId>
    <version>7.15.0-beta.4</version> 
</dependency>
```

2. When building the Session Processor opt-in for V2 stack (by default it’s V1)

```java

.configuration(new com.azure.core.util.ConfigurationBuilder()
    .putProperty("com.azure.messaging.servicebus.session.processor.asyncReceive.v2", "true")
    .build())
```


3. Rebuild the project
> mvn clean install 

The dependency tree (mvn dependency:tree) should show the project is resolving to azure-messaging-servicebus:7.15.0-beta.4 and azure-core-amqp:2.9.0-beta.6.
```
[INFO] com.azure:azure-service-bus-bug:jar:1.0-SNAPSHOT
[INFO] +- com.azure:azure-messaging-servicebus:jar:7.15.0-beta.4:compile <=====
[INFO] |  +- com.azure:azure-core:jar:1.44.1:compile
[INFO] |  |  +- com.azure:azure-json:jar:1.1.0:compile
[INFO] |  |  +- com.fasterxml.jackson.core:jackson-annotations:jar:2.13.5:compile
[INFO] |  |  +- com.fasterxml.jackson.core:jackson-core:jar:2.13.5:compile
[INFO] |  |  +- com.fasterxml.jackson.core:jackson-databind:jar:2.13.5:compile
[INFO] |  |  +- com.fasterxml.jackson.datatype:jackson-datatype-jsr310:jar:2.13.5:compile
[INFO] |  |  \- io.projectreactor:reactor-core:jar:3.4.33:compile
[INFO] |  |     \- org.reactivestreams:reactive-streams:jar:1.0.4:compile
[INFO] |  +- com.azure:azure-core-amqp:jar:2.9.0-beta.6:compile          <=====
[INFO] |  |  +- com.microsoft.azure:qpid-proton-j-extensions:jar:1.2.4:compile
[INFO] |  |  \- org.apache.qpid:proton-j:jar:0.33.8:compile
[INFO] |  +- ...
[INFO] |  |  +- ...
[INFO] |  |  |  +- ...
[INFO] |  |  |  \- ...

``` 
