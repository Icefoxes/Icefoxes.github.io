---
layout:       post
title:        "Deploy Spring Boot Application On Tomcat And Set SSL"
subtitle:     "Deploy Spring Boot Application On Tomcat And Set SSL"
date:         2019-06-09 19:45:00
author:       "Kuo"
header-img:   "img/post-bg-miui6.jpg"
header-mask:  0.3
catalog:      false
multilingual: false
comments:     true
tags:
    - AOP
---

### 1. Package project as War
There are just two items you need to change to your configuration
- how to package project: `<packaging>war</packaging>`
- export name package name `<finalName>finalName</finalName>`
then 


```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <packaging>war</packaging>

    ...

    <build>
        <finalName>finalName</finalName>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
</project>
```
Then package project as war
```bash
$ mvn package
```
Upload your war to production server with `pscp`
### 2. Install and config Tomcat
```bash
$ Wget https://mirrors.tuna.tsinghua.edu.cn/apache/tomcat/tomcat-8/v8.5.41/bin/apache-tomcat-8.5.41.zip
$ unzip apache-tomcat-8.5.41.zip
```
copy `war` file into `tomcat/webapps/` the application will be unziped automatacally, open `tomcat/conf/service.xml`, change two configurations

- Add a https connector
```xml
<Connector port="8443"
protocol="HTTP/1.1"
SSLEnabled="true"
scheme="https"
secure="true"
keystoreFile="/var/tomcat/certs/xx.pfx"
keystoreType="PKCS12"
keystorePass="password"
clientAuth="false"
SSLProtocol="TLSv1+TLSv1.1+TLSv1.2"
ciphers="TLS_RSA_WITH_AES_128_CBC_SHA,TLS_RSA_WITH_AES_256_CBC_SHA,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256,TLS_RSA_WITH_AES_128_CBC_SHA256,TLS_RSA_WITH_AES_256_CBC_SHA256" />

```
- Change host to host app
```xml
     <Host name="localhost"  appBase="webapps"
        unpackWARs="true" autoDeploy="true">
        <Context path="" docBase="/var/tomcat/webapps/apps" debug="0" reloadable="true"/>
```
