_This page provides guidance on upgrading to Spring Framework 5.x. See also the [[Spring-Framework-5-FAQ]] and [[What's New in Spring Framework 5.x]]._

# Upgrading to Version 5.0

## Baseline update

Spring Framework 5.0 requires JDK 8 (Java SE 8) or above, since its entire codebase is now based on Java 8 source code level, and provides full compatibility with JDK 9 on the classpath as well as the module path (Jigsaw).

The Java EE 7 API level is required in Spring's corresponding modules now, with runtime support for the EE 8 level:

* Servlet 3.1 / 4.0
* JPA 2.1 / 2.2
* Bean Validation 1.1 / 2.0
* JMS 2.0
* JSON Binding API 1.0 (as an alternative to Jackson / Gson)

## Web Servers

* Tomcat 8.5+
* Jetty 9.4+
* WildFly 10+
* WebSphere 9+
* with the addition of Netty 4.1 and Undertow 1.4 for Spring WebFlux

## Libraries

* Jackson 2.9+
* EhCache 2.10+
* Hibernate 5.0+
* OkHttp 3.0+
* XmlUnit 2.0+

## Removed Packages, Classes and Methods

* Package `mock.staticmock` removed from `spring-aspects` module.
  * No support for `AnnotationDrivenStaticEntityMockingControl` anymore.
* Packages `web.view.tiles2` and `orm.hibernate3/hibernate4` dropped.
  * Minimum requirement: Tiles 3 and Hibernate 5 now.
* Many deprecated classes and methods removed across the codebase.
  * A few compromises made for commonly used methods in the ecosystem.
* Note that several deprecated methods have been removed from the JSP tag library as well.
  * e.g. FormTag's "commandName" attribute, superseded by "modelAttribute" years ago.

## Dropped support

The Spring Framework no longer supports: Portlet, Velocity, JasperReports, XMLBeans, JDO, Guava (replaced by the Caffeine support). If those are critical to your project, you should stay on Spring Framework 4.3.x (supported until 2020).
Alternatively, you may create custom adapter classes in your own project (possibly derived from Spring Framework 4.x).

## Commons Logging setup

Spring Framework 5.0 comes with its own Commons Logging bridge in the form of the 'spring-jcl' module that 'spring-core' depends on. This replaces the former dependency on the 'commons-logging' artifact which required an exclude declaration for switching to 'jcl-over-slf4j' (SLF4J / Logback) and an extra bridge declaration for 'log4j-jcl' (Log4j 2.x).

Now, 'spring-jcl' itself is a very capable Commons Logging bridge with first-class support for Log4j 2, SLF4J and JUL (java.util.logging), working out of the box without any special excludes or bridge declarations for all three scenarios.

You may still exclude 'spring-jcl' from 'spring-core' and bring in 'jcl-over-slf4j' as your choice, in particular for upgrading an existing project. However, please note that 'spring-jcl' can easily supersede 'jcl-over-slf4j' by default for a streamlined Maven dependency setup, reacting to the plain presence of the Log4j 2.x / Logback core jars at runtime. 

Please note: For a clean classpath arrangement (without several variants of Commons Logging on the classpath), you might have to declare explicit excludes for 'commons-logging' and/or 'jcl-over-slf4j' in other libraries that you're using.