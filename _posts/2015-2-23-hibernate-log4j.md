---
layout: post
title: Using Log4J2 with Hibernate 4.3.8 
category: 
  - Tricks
comments: true
tags: hibernate log4j2
---

[The offical document](http://docs.jboss.org/hibernate/orm/4.3/topical/html/logging/Logging.html) said since version 4.0, Hiberante has used the JBoss Logging library for its logging needs. And log4j2 is one of jboss-logging supported back-ends. But in fact it does not work.
[The problem](http://stackoverflow.com/questions/27088083/log4j2-jpa-hibernate-logging-is-not-working) has been asked on stackoverflow. The reason why log4j2 doesn't work is jboss-logging-3.1.3.GA along with Hibernate-4.3.8.Final doesn't support log4j2. You can upgrade to jboss-logging-3.2.0.Final  to fix this problem.

<!--more-->

### Maven

```xml
<dependency>
    <groupId>org.hibernate</groupId>
    <artifactId>hibernate-core</artifactId>
    <version>4.3.8.Final</version>
    <exclusions>
        <exclusion>
            <artifactId>jboss-logging</artifactId>
            <groupId>org.jboss.logging</groupId>
        </exclusion>
    </exclusions>
</dependency>
<dependency>
    <groupId>org.jboss.logging</groupId>
    <artifactId>jboss-logging</artifactId>
    <version>3.2.0.Final</version>
</dependency>
```

### Gradle

```groovy
compile ('org.hibernate:hibernate-core:4.3.8.Final') {
    exclude module: 'jboss-logging'
}
compile 'org.jboss.logging:jboss-logging:3.2.0.Final'
```

