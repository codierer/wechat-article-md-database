# 1 IoC容器概述
IoC（Inversion of Control）容器,翻译为控制反转。IoC也称为依赖注入（Dependency injection：DI）。对象通过构造函数参数，工厂方法的参数或从工厂方法返回后的对象实例设置的属性来定义依赖项。容器在创建Bean时将依赖项注入。根本上是通过类的构造方法或服务定位器模式来控制依赖项的实例化和位置的逆过程。

包`org.springframework.beans` 和 `org.springframework.context`是提供IoC容器的基础包`org.springframework.beans.factory.BeanFactory`提供任何类型对象的高级管理机制接口。`org.springframework.context.ApplicationContext` 是 `BeanFactory` 的子接口。增加了与Spring的AOP功能集成，消息资源处理，活动发布和应用层特定的上下文等功能。

`org.springframework.context.ApplicationContext`接口代表IoC容器，负责实例化、配置和组装`Bean`。容器通过读取配置元数据获取相关实例化、配置和组装对象。配置元数据主要有以下种类：

1. 基于XML的配置方式（同Groovy方式）
2. 基于注解的配置方式（Spring2.5引入）
3. 基于Java的配置方式（Spring3.0引入，目前最为流行的方式）


![IoC构造模块](https://imgkr.cn-bj.ufileos.com/862ac84c-5fb5-4206-8e91-1f6bbae7d4f9.jpg)

## 1.1 配置元数据

本文配置元数据主要介绍XML（Groovy）方式，其它两种方式将在后续环节进行介绍。

### 1.1.1 XML配置方式
XML方式
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="..." class="...">  
        <!-- collaborators and configuration for this bean go here -->
    </bean>

    <bean id="..." class="...">
        <!-- collaborators and configuration for this bean go here -->
    </bean>

    <!-- more bean definitions go here -->

</beans>
```
groovy方式
```groovy
beans {
    dataSource(BasicDataSource) {
        driverClassName = "org.hsqldb.jdbcDriver"
        url = "jdbc:hsqldb:mem:grailsDB"
        username = "sa"
        password = ""
        settings = [mynew:"setting"]
    }
    sessionFactory(SessionFactory) {
        dataSource = dataSource
    }
    myService(MyService) {
        nestedBean = { AnotherBean bean ->
            dataSource = dataSource
        }
    }
}
```

### 1.1.2 基于注释配置方式

`@Required` 注释适用于bean属性setter方法，从`@Required` Spring Framework 5.1后弃用了该注释，以支持所需的设置使用构造函数注入设置使用构造函数注入，或自定义一个`InitializingBean.afterPropertiesSet（）`实现bean属性设置器方法。

```java
public class SimpleMovieLister {

    private MovieFinder movieFinder;

    @Required
    public void setMovieFinder(MovieFinder movieFinder) {
        this.movieFinder = movieFinder;
    }

    // ...
}
```

`@Autowired`注释应用于构造函数，以指示容器使用哪个构造函数。

```java
public class MovieRecommender {

    private final CustomerPreferenceDao customerPreferenceDao;

    @Autowired
    public MovieRecommender(CustomerPreferenceDao customerPreferenceDao) {
        this.customerPreferenceDao = customerPreferenceDao;
    }

    // ...
}
```

### 1.1.3 基于Java配置方式
主要提供`@Bean`和`@Configuration`两种注释类，`@Configuration`表示作为`Bean`定义的来源。`@Configuration`类允许在其他方法中使用`@Bean`来定义`Bean`之间的依赖关系。`@Bean`注释被用于指示一个方法实例配置。
```java
@Configuration
public class AppConfig {

    @Bean
    public MyService myService() {
        return new MyServiceImpl();
    }
}
```

等价于

```xml
<beans>
    <bean id="myService" class="com.acme.services.MyServiceImpl"/>
</beans>
```
## 1.2 实例化容器

### 1.2.1 基于XML实例化

XML配置方式
```java
GenericApplicationContext context = new GenericApplicationContext();
new XmlBeanDefinitionReader(context).loadBeanDefinitions("services.xml", "daos.xml");
context.refresh();
```
使`bean`定义跨越多个XML文件，每个单独的XML配置文件一般代表体系结构中的逻辑层或模块。

Groovy配置方式

```groovy
GenericApplicationContext context = new GenericApplicationContext();
new GroovyBeanDefinitionReader(context).loadBeanDefinitions("services.groovy", "daos.groovy");
context.refresh();
```

### 1.2.2 基于注释方式

使用`CustomAutowireConfigurer`
```xml
<bean id="customAutowireConfigurer"
        class="org.springframework.beans.factory.annotation.CustomAutowireConfigurer">
    <property name="customQualifierTypes">
        <set>
            <value>example.CustomQualifier</value>
        </set>
    </property>
</bean>
```

### 1.2.3 基于Java配置方式
使用`AnnotationConfigApplicationContext`,不仅接受`@Configuration`类作为输入，还接受普通`@Component`类和使用JSR-330元数据注释的类。

```java
public static void main(String[] args) {
    ApplicationContext ctx = new AnnotationConfigApplicationContext(AppConfig.class);
    MyService myService = ctx.getBean(MyService.class);
    myService.doStuff();
}
```
