---
layout:       post
title:        "Introduction To slf4j"
subtitle:     ""
date:         2019-06-02 12:03:00
author:       "Kuo"
header-img:   "img/post-bg-universe.jpg"
header-mask:  0.3
catalog:      false
multilingual: false
comments:     true
tags:
    - Spring-Boot
    - Java
---
## SLF4J
The Simple Logging Facade for Java (SLF4J) serves as a simple facade or abstraction for various logging frameworks, SLF4J allows the end-user to plug in the desired logging framework at deployment time.
| Supporting        | Require                      |
| -------------     |:----------------------------:| 
| java.util.logging |slf4j-jdk14-{version}.jar     |
| logback           | 
| log4j             |slf4j-log4j12-{version}.jar   |
| stder             |slf4j-simple-{version}.jar    |
![Binding](/img/SLF4J/binding.png)


## Example SLF4J code
```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class LoggingExample {
  public static void main(String[] args) {
    Logger logger = LoggerFactory.getLogger(LoggingExample.class);

    logger.info("userful information");
  }
}
```

## Default Logging Framework with Spring Boot
Spring boot provides a default starter for logging `spring-boot-starter-logging`. It is included by default in `spring-boot-starter` which is included in all other starters.

## Integrate with Spring Boot + Log4j
please checkout manual https://logging.apache.org/log4j/2.x/

### Maven Dependency
```xml
<dependencies>
  <dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-api</artifactId>
    <version>2.11.2</version>
  </dependency>
  <dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-core</artifactId>
    <version>2.11.2</version>
  </dependency>
</dependencies>
```
