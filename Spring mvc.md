##Configuration
Let's take a look at a Java-based configuration fIle, the com.apress.prospringmvc.moneytransfer.annotation.ApplicationContextConfiguration class .There are two annotations used in the class: org.springframework.context annotation.Configurationand org. springframework. context. annotation. Bean. The first stereotypes our class as a configuration fIle,while the second indicates that the result of the method is to be used as a factory to create a bean. The name of the bean is, by default, the method name. In Listing 2-6, we have three beans. They are named
accountReposi tory, transactionReposi tory, and moneyTransferService. We could also explicitly specify a bean name by setting the name attribute on the Bean annotation.

```java
@Configuration
public class ApplicationContextConfiguration {
    @Bean
	public AccountRepository accountRepository() {
		return new MapBasedAccountRepositorY()j
	}
	@Bean
	public TransactionRepository transactionRepository() {
		return new MapBasedTransactionRepositorY()j
	}
	@Bean
	public MoneyTransferService moneyTransferService() {
		return new MoneyTransferServicelmpl()j
	}
}
```

##Component-Scanning
Spring also has something called component-scanning. In short, this feature enables Spring to scan your
classpath for classes that are annotated with org. springframework. stereotype. Component (or one of the
specialized annotations like org. springframework. stereotype. Service,
org.springframework.stereotype.Repository,org.springframework.stereotype.Controller,or
org. springframework. context. annotation. Configuration). Ifwe want to enable component-scanning,
we need to instruct the application context to do so. The
org. springframework. context. annotation. ComponentScan annotation enables us to accomplish that.
This annotation needs to be put on our configuration class to enable component -scanning. Listing 2-9
shows the modified configuration class.

```java
import org.springframework.context.annotation.ComponentScanj
import org.springframework.context.annotation.Configurationj
@Configuration
@ComponentScan(basePackages = {
    "com.apress.prospringmvc.moneytransfer.scanning",
    "com.apress.prospringmvc.moneytransfer.repository" })

public class ApplicationContextConfiguration {}
```

A look at Listing 2-9 reveals that the class has no more content. There are just two annotations. One
annotation indicates that this class is used for configuration, while the other enables component-
scanning. The component -scan annotation is configured with a package to scan.

**Caution** 

It is considered bad practice to scan the whole classpath by not specifying a package or to use too
broad a package (like com.apress). This can lead to scanning a large part or even all classes, which will severely
impact the startup time of your application.

##Scopes
By default, all beans in a Spring application context are singletons. As the name implies, there is a single
instance of a bean, and it is used for the whole application. This doesn't typically present a problem
because our services and repositories don't hold state; they simply execute a certain operation and
(optionally) return a value.

However, a singleton would be problematic if we wanted to keep state inside our bean. We are
developing a web application that we hope will attract thousands of users. If we have a single instance of
a bean and all those users operate on the same instance, then the users will see and modify each other's
data or data from several users combined. Obviously, this is not something we want. Fortunately, Spring
provides several scopes for beans that we can use to our advantage (see Table 2-5).
Table 2-5. An Overview of Scopes

Prefix Description

- **singleton** The default scope. A single instance of a bean is created and shared throughout the application.
- **prototype** Each time a certain bean is needed, a fresh instance of the bean is returned.
- **thread** The bean is created when needed and bound to the current executing thread. If the thread dies, the bean is destroyed.
- **request** The bean is created when needed and bound to the lifetime of the incoming javax. servlet. ServletRequest. If the request is over, the bean instance is destroyed.
- **session** The bean is created when needed and stored in javax. servlet. HttpSession. When the session is destroyed, so is the bean instance.
- **globalSession** The bean is created when needed and stored in the globally available session (which is available in Portlet environments). If no such session is available, the scope reverts to the session scope functionality.
- **application** This scope is very similar to the singleton scope; however, beans with this scope are also registered in javax. serv let. Serv letContext.

##Profiles
总的来说就是可以设置不同的环境参数，在加载bean的时候没有设置profile的默认会被加载，加了profile的只有在这个profile里面的才会被加载

Spring introduced the concept of profIles in version 3.1. ProfIles make it easy to create different
configurations of our application for different environments. For instance, we can create separate
profIles for our local environment, for testing, and for our deployment to CloudF oundry. Each of these
environments requires some environment -specific configuration or beans. One can think of database
configuration; messaging solutions; and for testing environments, stubs of certain beans.

To enable a profIle, we need to tell the application context which profIles are active. To activate
certain profIles, we need to set a system property called spring. profiles. acti ve (in a web environment,
this can be a servlet initialization parameter or web context parameter). This is a comma-separated
string containing the names of the active profIles. If we now add some (in this case static inner) classes
(see Listing 2-10) with the org. springframework. context. annotation. Configuration and
org. springframework. context. annotation. Profile annotations, then only the classes that match one of
the active profIles will be processed. All other classes will be ignored.

```java
@Configuration
public class ApplicationContextConfiguration {
@Bean
public AccountRepository accountRepository() {
return new MapBasedAccountRepositorY()j
}
@Bean
public MoneyTransferService moneyTransferService() {
return new MoneyTransferServicelmpl()j
}
@Configuration
@Profile(value = "test")
public static class TestContextConfiguration {
@Bean
public TransactionRepository transactionRepository() {
return new StubTransactionRepository();
}
}
@Configuration
@Profile(value = "local")
public static class LocalContextConfiguration {
@Bean
public TransactionRepository transactionRepository() {
}
}
}
return new MapBasedTransactionRepository();

```
shows some example bootstrap code. In general, we will not be setting the active
profIles from our bootstrap code. Instead, we will set up our environment using a combination of system
variables. This enables us to leave our application unchanged, but still have the flexibility to change our
runtime configuration.

```java
public class MoneyTransferSpring {
private static final Logger logger = LoggerFactory.getLogger(MoneyTransferSpring.class)j
public static void main(String[] args) {
System.setProperty("spring.profiles.active", "test");
AnnotationConfigApplicationContext ctxl = new AnnotationConfigApplicationContext(
ApplicationContextConfiguration.class)j
transfer(ctxl)j
ApplicationContextLogger.log(ctXl)j
ApplicationContext ctx2 = new ClassPathXmlApplicationContext(
"/com/apress/prospringmvc/moneytransfer/annotation/profiles~
lapplication-context.xml")j
transfer(ctx2)j
ApplicationContextLogger.log(ctX2)j
System.setProperty("spring.profiles.active", "local");
AnnotationConfigApplicationContext ctx3 = new AnnotationConfigApplicationContext(
ApplicationContextConfiguration.class)j
transfer(ctx3)j
ApplicationContextLogger.log(ctX3)j
ApplicationContext ctx4 = new ClassPathXmlApplicationContext(
"/com/apress/prospringmvc/moneytransfer/annotation/profiles~
lapplication-context.xml");
transfer(ctx4);
ApplicationContextLogger.log(ctX4);
}
private static void transfer(ApplicationContext ctx) {
MoneyTransferService service = ctx.getBean("moneyTransferService",~
MoneyTransferService.class);
Transaction transaction = service.transfer("123456", "654321", new~
BigDecimal("250.00"));
logger.info("Money Transfered: {}", transaction);
}
}
```
You might wonder why we should use profIles, anyway. One reason is that it allows for flexible
configurations. This means that our entire configuration is under version control and in the same source
code, instead of being spread out over different servers, workstations, and so on. Of course, we can still
load additional fIles containing some properties (like usemames and passwords). This can prove useful
if a company's security policy won't allow us to put these properties into version control. We are going to
use profIles extensively when we cover testing and deploying to the cloud because the two tasks require
different configurations for the datasource.

##DispatcherServlet

**Using web.xml**

The web. xml fIle has been around since the inception of the servlet specification. It is an XML fIle that
contains all the configuration you need to bootstrap the servlet, listeners, and/ or fIlters. Listing 4-1
shows the minimal web.xml required to bootstrap the DispatcherServlet. The web.xml fIle must be in the
WEB- INF directory of the web application (this is dictated by the servlet specification).
Listing 4-1. The web.xml configuration (Servlet 3.0)

```java
<web-app version="3.0"
xmlns="http://java.sun.com/xml/ns/javaee"
xmlns:xsi="hUp:llwww.w3.orgI2001/XMLSchema-instance"
xsi:schemaLocation=http://java.sun.com/xml/ns/javaee
http://java.sun.com/xml/ns/javaee/web-app_3_o.xsd
metadata-complete="true">
<servleb
<servlet-name>bookstore</servlet-name>
<servlet-class>
org.springframework.web.servlet.DispatcherServlet
</servlet-class>
<load-on-startup>l</load-on-startup>
</servleb
<servlet-mapping>
<servlet-name>bookstore</servlet-name>
<url-pattern>I*</url-pattern>
</servlet-mapping>
</web-app>
```
**Note**

> By default, the dispatcher servlet loads a file named [servletname]- servlet. xml from the WEB- INF
directory.
