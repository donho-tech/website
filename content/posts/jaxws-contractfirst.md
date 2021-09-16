+++
date = 2020-08-19T19:55:18+02:00
title = "Contract-first webservice with Spring Boot and JAX-WS RI"
description = "This tutorial shows you how to create a contract-first SOAP web service with JAX-WS RI and Spring Boot. You will define the service contract first and then implement it."
slug = ""
authors = []
tags = []
categories = []
externalLink = ""
series = []
+++

[Download source code](https://github.com/herrho/hellojaxws)

At first, set up your pom.xml file. Besides stuff for Spring Boot, you'll include `jaxws-rt` (the runtime)
and `jaxws-spring` (a helper library for integrating jaxws-rt with Spring). Exclude the Spring dependencies from
jaxws-spring to avoid conflicts. You will also need the `jaxws-maven-plugin` to help you generate the service classes

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.example</groupId>
    <artifactId>jaxws-contract-first</artifactId>
    <version>1.0-SNAPSHOT</version>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>1.4.3.RELEASE</version>
    </parent>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <!-- JAX-WS -->
        <dependency>
            <groupId>com.sun.xml.ws</groupId>
            <artifactId>jaxws-rt</artifactId>
            <version>2.2.10</version>
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
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>

            <plugin>
                <groupId>org.codehaus.mojo</groupId>
                <artifactId>jaxws-maven-plugin</artifactId>
                <version>1.12</version>
                <configuration>
                    <wsdlDirectory>src/main/resources</wsdlDirectory>
                    <packageName>com.example.greetingservice</packageName>
                    <keep>true</keep>
                    <sourceDestDir>target/generated-sources/wsimport</sourceDestDir>
                </configuration>
                <executions>
                    <execution>
                        <goals>
                            <goal>wsimport</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>

</project>
```

Next, you want to create the service contract by defining the Web Service (WSDL) and Type Definitions (XSD). If you
don't know how to read WSDL documents, here is an [excellent introduction](https://www.predic8.com/wsdl-reading.htm).

hello.wsdl

```xml

<definitions xmlns:soap="http://schemas.xmlsoap.org/wsdl/soap/" xmlns:tns="http://hellojaxws.example.com/"
             xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.xmlsoap.org/wsdl/"
             targetNamespace="http://hellojaxws.example.com/" name="GreetingService">
    <types>
        <xsd:schema>
            <xsd:import namespace="http://hellojaxws.example.com/" schemaLocation="hello.xsd"/>
        </xsd:schema>
    </types>

    <message name="greeting">
        <part name="parameters" element="tns:greeting"/>
    </message>
    <message name="greetingResponse">
        <part name="parameters" element="tns:greetingResponse"/>
    </message>

    <portType name="GreetingService">
        <operation name="greeting">
            <input message="tns:greeting"/>
            <output message="tns:greetingResponse"/>
        </operation>
    </portType>

    <binding name="GreetingServicePortBinding" type="tns:GreetingService">
        <soap:binding transport="http://schemas.xmlsoap.org/soap/http" style="document"/>
        <operation name="greeting">
            <soap:operation soapAction=""/>
            <input>
                <soap:body use="literal"/>
            </input>
            <output>
                <soap:body use="literal"/>
            </output>
        </operation>
    </binding>

    <service name="GreetingService">
        <port name="GreetingServicePort" binding="tns:GreetingServicePortBinding">
            <soap:address location="http://localhost:8080/hello"/>
        </port>
    </service>
</definitions>
```

hello.xsd

```xml

<xs:schema xmlns:tns="http://hellojaxws.example.com/" xmlns:xs="http://www.w3.org/2001/XMLSchema" version="1.0"
           targetNamespace="http://hellojaxws.example.com/">

    <xs:element name="greeting" type="tns:greeting"/>
    <xs:element name="greetingResponse" type="tns:greetingResponse"/>

    <xs:complexType name="greeting"/>

    <xs:complexType name="greetingResponse">
        <xs:sequence>
            <xs:element name="message" type="xs:string" minOccurs="0"/>
        </xs:sequence>
    </xs:complexType>
</xs:schema>
```

Run `mvn clean compile` to generate the Service API. You should see in the `target/generated-sources/wsimport` folder
the service interface and type classes.

The `Application` class is the main entry to your application. As you'll see, you will import an xml config file (
jaxwsconfig.xml) which contains bean wiring for JAX-WS RI. Also, you need to register `WSSpringServlet` to receive
incoming requests.

Application.java

```java
package com.example;

import com.sun.xml.ws.transport.http.servlet.WSSpringServlet;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.web.servlet.ServletRegistrationBean;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ImportResource;

@SpringBootApplication
@ImportResource(locations = "jaxwsconfig.xml")
public class Application {

    public static void main(String[] args) throws Exception {
        SpringApplication.run(Application.class, args);
    }

    @Bean
    public ServletRegistrationBean servletRegistrationBean() {
        return new ServletRegistrationBean(new WSSpringServlet(), "/hello");
    }
}
```

The `GreetingServiceImpl` class implements the web service. It references the service endpoint interface which tells the
JAX-WS runtime to use it as an explicit
interface (<a href="https://docs.oracle.com/javaee/7/tutorial/jaxws001.htm#BNAYN">read more about SEIs here</a>).
Through the @Component annotation you can make sure it gets picked up, added to the Spring context and will be given a
name. You will reference the service by the name later to let it handle the web service requests.

GreetingServiceImpl.java

```java
package com.example.greetingservice;

import org.springframework.stereotype.Component;

import javax.jws.WebService;

@Component(value = "greetingService")
@WebService(endpointInterface = "com.example.greetingservice.GreetingService")
public class GreetingServiceImpl implements GreetingService {

    @Override
    public String greeting() {
        return "Hello World!";
    }
}
```

Lastly, you have to add an xml file for wiring the service on the endpoint:

jaxwsconfig.xml

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

## Build and run it<

Build it: `mvn clean package`
Run it: `java -jar helloJaxws-0.0.1-SNAPSHOT.jar`

Now open <http:/localhost:8080/hello?wsdl> to see the generated wsdl file. Your webservice has been deployed, and you
could now create a client for it.

## What to do next?

Here are some hints what topics you could explore next.

* Test your webservice with SoapUI
* See what other alternatives to RI are available
