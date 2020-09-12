##### 1 POJO

`POJO`(`Plain Old Java Object`) 普通的`Java`对象，除了`Java`语言规范要求外，没有任何特殊限制要求，并且不依赖任何除`Object`类。`POJO`用于提高程序的可读性和可重用性。

`POJOs`类不符合以下条件：

1. 继承预先指定的类；
2. 实现特定的接口；
3. 包含预先指定的注释。

`POJOs`通常用来定义实体类，用于封装业务逻辑对象。

##### 2 Java Bean
`Java Bean`做为一种特殊的`POJO`类。相对于`POJO`存在一定限制。[对比](https://www.geeksforgeeks.org/pojo-vs-java-beans/ "对比")

- 所有的`Java Beans`都属于`POJO`，但不是所有的`POJO`都是`Java Bean`；
- `Java Beans`需实现`Serializable`接口，仍有`POJO`可不实现`Serializable`接口，因为`Serializable`做为标记接口不会带来太多负担；
- 字段必须设置为私有访问；
- 字段须设置`getters` 或 `setters`；
- 应该有一个无参构造器；
- 字段只能通过`getters, setters`或构造方法访问。

getters 和 setters 方法须满足以下规范
```java
public "returnType" getSomeProperty()
{
   return someProperty;
} 
```
```java
public void setSomeProperty(someProperty)
{
   this.someProperty=someProperty;
}
```

##### 3 VO

`VO`(`Value Object`) 值对象，用于做为存储数据的基本对象。如：`java.lang.Integer`


##### 4 DTO

`DTO`(`Data transfer object`)数据传输对象，以前称为值对象(`VO`)，是一种设计模式，用于在软件应用程序子系统之间传输数据。 `DTO`通常与数据访问对象结合使用，以从数据库中检索数据。

