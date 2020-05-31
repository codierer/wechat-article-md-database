# 1 Bean概述

`Spring IoC`容器管理一个或多个`Bean`，`Bean`通过配置元数据进行创建的。使用`BeanDefinition`创建`Bean`对象。其中包含以下元数据：

- `Bean`限定的类名,使用`class`属性定义;
- 行为属性配置，声明`Bean`在容器中的作用域，生命周期等属性;
- 引用属性配置，声明`Bean`之间的相互协作或依赖关系;
- 其他属性配置，声明`Bean`的其他属性，如池大小，连接数等。

下表显示了这些属性的描述：

| 属性名称   | 说明 |
| :----- | :--: |
| class |  标识类，实例化类对象，创建`Bean`  |
| name |  `Bean`名称  |
| scope |  `Bean`作用范围  |
| Constructor arguments |  依赖注入构造方法参数  |
| Properties |  依赖注入成员属性值  |
| Autowiring mode |  自动装配方式  |
| Lazy initalization mode |  `Lazy` 模式初始化`Bean`  |
| Initialization method |  初始回调  |
| Destruction method |  销毁回调  |

# 2创建Bean

实例化`Bean`本质就是创建一个或多个类对象。创建`Bean`有两种方式：

- 通过调用`class`的构造方法;
- 通过`static`工厂方法来创建`Bean`;
- 使用实例工厂方法创建`Bean`;
- 通过继承创建`Bean`.

## 2.1使用类构造方法创建

`IoC`容器可以管理任何类，使用默认的构造函数，或者通过依赖注入的方式创建。
```xml
<bean id="simpleMovieLister" class="examples.SimpleMovieLister"/>
```

```java
public class SimpleMovieLister {
    // the SimpleMovieLister has a dependency on a MovieFinder
    private MovieFinder movieFinder;
    // a constructor so that the Spring container can inject a MovieFinder
    public SimpleMovieLister(MovieFinder movieFinder) {
        this.movieFinder = movieFinder;
    }
    // business logic that actually uses the injected MovieFinder is omitted...
}
```

## 2.2使用静态工厂方法创建

```xml
<bean id="clientService"
    class="examples.ClientService"
    factory-method="createInstance"/>
```

```java
public class ClientService {
    private static ClientService clientService = new ClientService();
    private ClientService() {}
    public static ClientService createInstance() {
        return clientService;
    }
}
```

## 2.3使用实例工厂方法创建

```xml
<!-- the factory bean, which contains a method called createInstance() -->
<bean id="serviceLocator" class="examples.DefaultServiceLocator">
    <!-- inject any dependencies required by this locator bean -->
</bean>
<!-- the bean to be created via the factory bean -->
<bean id="clientService"
    factory-bean="serviceLocator"
    factory-method="createClientServiceInstance"/>
```

```java
public class DefaultServiceLocator {
    private static ClientService clientService = new ClientServiceImpl();
    public ClientService createClientServiceInstance() {
        return clientService;
    }
}
```

## 2.4通过继承创建

```xml
<bean id="inheritedTestBean" abstract="true"
        class="org.springframework.beans.TestBean">
    <property name="name" value="parent"/>
    <property name="age" value="1"/>
</bean>

<bean id="inheritsWithDifferentClass"
        class="org.springframework.beans.DerivedTestBean"
        parent="inheritedTestBean" init-method="initialize">  
    <property name="name" value="override"/>
    <!-- the age property value of 1 will be inherited from parent -->
</bean>
```
出现循环依赖，Spring IoC容器将会检测到循环引用，并抛出`BeanCurrentlyInCreationException`。

# 3Bean命名
每个`bean`具有一个或多个标识符,这些标识符在承载`Bean`的容器内必须是唯一的。一个`bean`通常只有一个标识符。但是，如果需要多个，则可以将多余的别名视为别名。需要为`bean` 提供`name`或`id`,如果不提供 `name`或`id`显式提供，容器将为该`bean`生成一个唯一的名称。`bean`名称以小写字母开头，用驼峰式大小写。

需要指定别名通过`<alias>`进行指定，实例如下所示：
```xml
<alias name="fromName" alias="toName"/>
```
这种情况下，`fromName`定义了该`Bean`，也可以称为`toName`。

# 4Bean作用[域](https://docs.spring.io/spring/docs/5.3.0-SNAPSHOT/spring-framework-reference/core.html#beans-factory-scopes "域")

| 范围|描述|
| :-- | :-- |
| `singleton`|(默认)单个对象实例|
| `prototype`|任意数量的对象实例|
| `request`|单个`HTTP`请求的生命周期|
| `session`|`Session`的生命周期|
| `application`|`ServletContext`的生命周期|
| `websocket`|`WebSocket`的生命周期|

## 4.1Singleton
Spring IoC容器将为Bean定义创建一个对象实例。`Spring`的`singleton bean`的概念与设计模式(`Design Patterns Elements of Reusable Object-Oriented Software`)[`GoF`]一书中定义的singleton模式不同。GoF单例对对象的范围进行硬编码，`Spring`单例的范围描述为每个容器和每个`bean`。
```xml
<bean id="accountService" class="com.something.DefaultAccountService" scope="singleton"/>
```

![Singleton作用域](https://imgkr.cn-bj.ufileos.com/22074ef3-38ce-4893-a605-2b85124c623c.png)

## 4.2Prototype
每次对特定`bean`提出请求时，`bean`部署的非单一原型范围都会导致创建一个新`bean`实例。`Spring`容器在原型作用域`bean`方面的角色是`Java` `new`运算符的替代。
```xml
<bean id="accountService" class="com.something.DefaultAccountService" scope="prototype"/>
```

![Prototype作用域](https://imgkr.cn-bj.ufileos.com/1708d87b-f032-4499-8889-0e2e1e9e116e.png)

## 4.3Prototype 依赖 Singleton
当使用对`prototype Bean`依赖`singleton Bean`时，如果依赖范围的`prototype bean`注入到`singleton bean`中，则将实例化`prototype bean`，然后将依赖项注入到该`singleton Bean`中。

## 4.4其他
在`request`，`session`，`application`，和`websocket`范围只有当使用基于web的Spring`ApplicationContext`实现.

初始化Web配置如下：
```xml
<web-app>
    ...
    <listener>
        <listener-class>
            org.springframework.web.context.request.RequestContextListener
        </listener-class>
    </listener>
    ...
</web-app>
```
### 4.4.1Request
`Spring`容器通过每个`HTTP`请求使用`bean`定义来创建`bean`的新实例。

```xml
<bean id="loginAction" class="com.something.LoginAction" scope="request"/>
```
使用注释驱动的组件或`Java`配置时，`@RequestScope`可以使用注释将组件分配给合并`request`范围。
```java
@RequestScope
@Component
public class LoginAction {
    // ...
}
```
### 4.4.2Session
`Spring`容器通过对单个`HttpSession`的生存期内使用新实例。每一个`HttpSession`下都会拥有一个单独的实例.
在`Portlet`环境下，每个`globalSession`都会拥有一个单独的实例。
### 4.4.3Application
`Spring`容器通过对整个`Web`应用程序使用一次来创建新实例。
### 4.4.4[WebSocket](https://blog.csdn.net/elim168/article/details/75581612 "WebSocket")
在`ServletContext`生命周期内会拥有一个单独的实例，即在整个`ServletContext`环境下只会拥有一个实例。


