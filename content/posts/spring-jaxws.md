+++
date = 2021-09-24
title = "Create a webservice with Spring Boot and JAX-WS RI"
description = ""
slug = ""
authors = []
tags = []
categories = []
externalLink = ""
series = []
+++

Learn how to create a SOAP webservice with JAX-WS RI and Spring Boot.

What do you need to complete this tutorial?
* Maven
* Java 11

[Download source code](https://github.com/donho-tech/tutorial-java-jaxws)

Use [spring initializr](https://start.spring.io/#!type=maven-project&language=java&packaging=jar&dependencies=web) to create a Spring Boot application with Spring Web dependency.

Open your pom.xml and include `jaxws-rt` (the runtime) and `jaxws-spring` (a helper library for
integrating jaxws-rt with Spring) in the `dependencies` section. Exclude the Spring dependencies from
jaxws-spring to avoid conflicts.

```xml

<dependencies>
    <!-- JAX-WS -->
    <dependency>
        <groupId>com.sun.xml.ws</groupId>
        <artifactId>jaxws-rt</artifactId>
        <version>2.3.3</version>
    </dependency>

    <!-- Spring JAX-WS Integration -->
    <dependency>
        <groupId>org.jvnet.jax-ws-commons.spring</groupId>
        <artifactId>jaxws-spring</artifactId>
        <version>1.9</version>
        <exclusions>
            <exclusion>
                <groupId>org.springframework</groupId>
                <artifactId>spring-core</artifactId>
            </exclusion>
            <exclusion>
                <groupId>org.springframework</groupId>
                <artifactId>spring</artifactId>
            </exclusion>
            <exclusion>
                <groupId>org.springframework</groupId>
                <artifactId>spring-context</artifactId>
            </exclusion>
        </exclusions>
    </dependency>
    ...
</dependencies>
```

Create a `TutorialJavaJaxwsApplication.java` class, by importing an xml config file (jaxwsconfig.xml) which
contains bean wiring for JAX-WS RI and register `WSSpringServlet` to receive incoming requests.

```java

@SpringBootApplication
@ImportResource(locations = "jaxwsconfig.xml")
public class TutorialJavaJaxwsApplication {

    public static void main(String[] args) {
        SpringApplication.run(TutorialJavaJaxwsApplication.class, args);
    }

    @Bean
    public ServletRegistrationBean servletRegistrationBean() {
        return new ServletRegistrationBean(new WSSpringServlet(), "/hello");
    }
}
```

Create a `GreetingService.java` class which greets the client upon being called. You will reference the service by the
name later to let it handle the web service requests.

```java

@Component(value = "greetingService")
@WebService
public class GreetingService {

    @WebMethod
    public String greeting() {
        return "Hello World!";
    }
}
```

Lastly, you have to add file `jaxwsconfig.xml` for wiring the service on the endpoint:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:ws="http://jax-ws.dev.java.net/spring/core"
       xmlns:wss="http://jax-ws.dev.java.net/spring/servlet"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans-4.3.xsd
       http://jax-ws.dev.java.net/spring/core
        http://jax-ws.dev.java.net/spring/core.xsd
        http://jax-ws.dev.java.net/spring/servlet
        http://jax-ws.dev.java.net/spring/servlet.xsd"
>

    <wss:binding url="/hello">
        <wss:service>
            <ws:service bean="#greetingService"/>
        </wss:service>
    </wss:binding>

</beans>
```

Now open <http://localhost:8080/hello?wsdl> to see the generated wsdl file. Your webservice has been deployed, and you
could now create a client for it.

## What to do next?

Here are some hints what topics you could explore next.
* Test your webservice with SoapUI
* Find out, why you should write your service contract (wsdl) first and not the code
* Learn how to develop a service "contract-first" with JAX-WS RI
* See what other alternatives to RI are available
