---
layout:     post
title:      "Specify JDK for Maven project"
subtitle:   "Maven项目指定JDK版本"
date:       2018-04-11 16:21:00
author:     "Yuanbo"
header-img: "img/home-bg-o.jpg"
catalog:    true

tags:
    - 技术
    - JAVA
    - Maven
    
---


## Specify JDK for maven based Project


Many times, we encounter the jdk version problems in maven based project. 
For convenient, I took down this note and facilitate the following dev/test works. 

Before we set the jdk version, make sure you have installed JAVA&MAVEN and configured the JAVA_HOME 
M2_HOME successfully. 

In maven pom.xml, add pugin configuration as shown below. Then It will ensure that your particular project uses that version of jdk.
                                                               
 

```xml

<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <configuration>
                <source>1.7</source>
                <target>1.7</target>
            </configuration>
        </plugin>
    </plugins>
</build>

```


---

### END

