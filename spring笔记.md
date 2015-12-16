##spring是什么
struts是web框架（jsp/action/actionfrom）
spring是一种容器框架，用于配置bean并且维护bean之间关系的框架。
bean(是java中的一种对象，javabean，service，action，数据源，dao，ioc（控制反转inverse of control），di（依赖注入dependency of injection）


##开发spring项目
1. 引入spring的开发包（最小配置spring.jar，该包把常用的jar都包括），另外引入一个写日志包common-logging.jar。
2. 创建spring的核心文件applicationContext.xml文件（相当于hibernate的核心文件hibernate.cfg.xml)，该文件一般放在src目录下。该文件可以从spring中的示例demo中拷贝一份。
3. 使用maven，在maven中的pom.xml文件中配置依赖:

```java
<!--  add the spring framework -->
    <dependency>
        <groupId>org.springframework</groupId>
    	<artifactId>spring-context</artifactId>
    	<version>3.1.1.RELEASE</version>
    </dependency>
```

###配置bean
bean元素的作用是，当我们的spring框架加载的时候，spring会自动创建一个bean对象，并放入内存。
下面是一个applicationContext.xml文件：
```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xmlns:context="http://www.springframework.org/schema/context"
xmlns:util="http://www.springframework.org/schema/util"
xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-2.5.xsd
http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-3.0.xsd
http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-2.5.xsd">

<!-- bean -->
<!-- bean元素的作用是，当我们的spring框架加载的时候，spring会自动创建一个bean对象，并放入内存。
     UserService user = new UserService();
	 user.setName("zale");
-->
<bean id = "userService" class = "spring.UserService">
<property name="name">
	<value>zale</value>
</property>
</bean>

</beans>
```
其中我配置了一个bean，对应的在java文件中新建了一个UserService.java文件：
```java
public class UserService {
    private String name;

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}
	
	public void sayHello()
	{
		System.out.println("hello" + name);
	}
}
```

然后写一个测试类test.java
```java
import org.springframework.context.*;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class test {
    public static void main(String[] args)
	{
		 ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
		 UserService user = (UserService) context.getBean("userService");
		 user.sayHello();
	}
}
```

##spring和传统方法的区别
1. 使用spring，我们把new对象的任务交给了spring框架了。
2. 

##bean实例化时机
1. 如果使用beanfactory去获取bean时，如果是singleton只是实例化了该容器，那么容器中的bean不会被实例化，在使用getbean时才会实例化，一般用于移动开发。在applicationContext上下文时一创建就能加载bean容器，在内存不紧张90%以上都是用ApplicationContext,这样消耗内存，但是快。小规范：在项目中一般来没有特殊要求用上下文方式去获取。

2. 如果bean的scope不是singleton的话就不是获取容器就实例化bean，如果不是singleton的话也有延迟加载的效果，在singleton范围下可尝试获取两个相同id的bean，比较他们的地址一样不一样，在singleton模式下取出的两个bean是一样的。

3. bean的作用范围singleton类似于global session，而prototype类似于requset，只不过一个是在web开发中的范围。

###三种获取 ApplicationContext对象引用的方法
1. ClassPathXmlApplicationContext 通过类路径
2. FileSystemXmlApplicationContext 通过文件路径（全路径）
3. XmlWebApplicationContext

###bean生命周期
1. 实例化（当我们的程序加载beans.xml时），把我们的bean(前提是scope = singleton)实例化到内存。
2. 调用set方法设置属性。
3. 如果你实现了bean名字关注接口（BeanNameAware）则可以通过setBeanName获取id号。
4. 如果你实现了bean工厂关注接口，setBeanFactoryAware则可以获取beanFactory
5. 如果实现了ApplicationContext接口，则调用方法setApplicationContextAware
6. 如果bean和一个后置处理器关联，则会自动去调用Object postProcessBeforeInitialization方法。
7. 如果你实现了initialingBean接口，则会调用它的afterProperiteSet方法。
8. 如果自己在<bean init-method= "init"/> 则可以在bean中定义自己的初始化方法。
9. 调用postProcessAfterInitialization方法方法
10. bean可以使用了。
11. 容器关闭。
12. 可以定制自己的销毁方法，（也可以实现DisposableBean接口可以在destory中关闭数据连接，socket，文件流，释放bean资源。）
13. 可以在bean中定制<bean destory-method = "fun1"/>调用定制的销毁方法。

小结：在实际开发中没有用到这么多方法，常见的过程为

1->2->6->9->10->11

###对AOP的理解
现在假如说有这么一个需求，要求记录每一个对象被实例化的时间，过滤每个调用对象的ip。面向切面编程就好比一个模具，以前对实例化的对象进行属性添加之类的操作的话就要一个一个来，现在的面向切面编程的意思就是造了一个模具一次性都从这个模具做出来。

###bean的生命周期不同
在从上下文获取的bean的生命周期比工厂获得的生命周期长。

###装配bean
使用原型bean会对性能产生影响，即尽量不要用prototype，尽量使用scope=singleton。

###如何给集合类型注入值
java中的主要集合：map,set,list(set,list的父接口为Collection<E>，父接口的方法没有继承的多),数组。

###内部bean
<bean id = "foo" class = "..">
    <property name = "bar">
    <!--第一种方法引用>
    <ref bean = "bean对象名"/>
    <!--内部bean>
    <bean>
        <propety>
        </propety>
    </bean>
    </propety>
</bean>
这种方式的缺点是你无法在其他地方重用这个bar实例，原因是它专门为foo而用，是个内部bean。有点向内部类，一般很少用。