# 1.初始

## 入手

applicationContext.xml
Test.java
HelloWorld.java

```java
///Test.java

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Test {
    public static void main(String[] args) {
        //1、创建Spring的IOC容器的对象
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
        //2、从IOC的容器中获取Bean的实例
        HelloWorld helloWorld = (HelloWorld) applicationContext.getBean("hello");
        //3、调用方法
        helloWorld.papi();
    }
}
```

```java
///applicationContext.xml

<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="hello" class="HelloWorld"></bean>
</beans>
```

```java
///HelloWorld

public class HelloWorld {
    public void papi(){
        System.out.println("Hello Spring");
    }
}
```

## Spring常用类和方法

![预览大图](Spring.assets/330628)

两个实现类：

- `ClassPathXmlApplicationContext`：加载类路径下`Spring`的配置文件

```java
ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
```

- `FileSystemXmlApplicationContext`：加载本地磁盘下`Spring`的配置文件

```java
String path="文件绝对路径";
ApplicationContext applicationContext = new FileSystemXmlApplicationContext(path);
```

判断容器是否包含id为XXX的bean实例

```java
ApplicationContext ap=new ClassPathXmlApplicationContext("applicationContext.xml");
System.out.println(ap.containsBean("Student"));  //return true or false
```

使用三种方法获取`bean`实例

- ​	`HelloWorld t=(HelloWorld) ap.getBean("helloworld");`
- ​    `HelloWorld t2=ap.getBean("helloworld",HelloWorld.class);`
- ​    `HelloWorld t3=(HelloWorld) ap.getBean(HelloWorld.class);`

## Bean管理

Spring 的核心 IOC（ Inverse of Control 反转控制 ）所做的事就是将对象的创建权，交由 Spring 完成。

```java
Peron p1 = new Person();    //自己手动完成Person p2 = spring容器.get***();    //由spring容器创建
```

### Bean标签的常用属性

- `id`属性：用于指定配置对象的名称，不能包含特殊符号。

- `class`属性：创建对象所在类的全路径。

- `name`属性：功能同`id`属性一致，但是`name`属性值中可以包含特殊符号，若`bean`标签上没有`id`属性，那么`name`可以作为`id`使用。

- `scope`属性：

    -`singleton`：默认值，单例，该模式下程序只有一个实例。

    -`prototype`：多例。每次从容器中调用`bean`时，都返回一个新的实例，即每次调用`ap.getBean()`时，相当于执行`new xxxBean()`：不会在容器启动时创建对象。

    -`request`：`web`开发中，创建了一个对象，将这个对象存入`request`范围，`request.setAttribute()`。

    -`session`：`web`开发中，创建了一个对象，将这个对象存入`session`范围，`session.setAttribute()`。

    -`globalSession`：一般用于 Porlet 应用环境，指的是分布式开发；非 Porlet 环境，`globalSession`等同于`session`。

    实际开发中主要使用`singleton`，`prototype`。

`applicationContext.xml`配置文件：

![预览大图](https://www.educoder.net/api/attachments/332721)



### Bean实例化

接下来我们就可以通过`Spring`的配置文件来创建对象了，**即在配置文件中配置好一次`bean`后，我们可以在项目中任意位置通过容器的`getBean`方法进行访问**，`bean`的实例化有**三**种实现方式：

- 构造方法实例化（默认为创建无参构造）

```java
package educoder;
public class Bean1 {    
    String message;
    public Bean1(){ 
    }
    public Bean1(String s){
        message=s;
    }
}

<!-- 等同于 bean1 = new educoder.Bean1(); class属性值为类的全路径-->
<bean id="bean1" class="educoder.Bean1"></bean>
```

- 静态工厂实例化方法（了解）

如果一个`bean`不能通过`new`直接实例化，而是通过工厂类的某个静态方法创建的，需要把`<bean>`的`class`属性配置为工厂类。

```java
package educoder;
public class Bean2 {}
public class Bean2Factory {    
    public static Bean2 getbean2(){        
        return new Bean2();    
    }
}

<!-- 等同于 bean2 = educoder.Bean2Factory.getbean2(); -->
<bean id="bean2" class="educoder.Bean2Factory" factory-method="getbean2"></bean>
```

- 实例工厂实例化方法（了解）

如果一个`bean`不能通过`new`直接实例化，而是通过工厂类的某个实例方法创建的，需要先配置工厂的`<bean>`标签，然后在需要创建的对象的`bean`标签的`factory-bean`属性配置为工厂类对象，`factory-method`属性配置为产生实例的方法。

```java
package educoder;
public class Bean3 {}
public class Bean3Factory {    
    public Bean3 getbean3(){        
        return new Bean3();    
    }
}

<!-- 等同于 beanfactory = new educoder.Bean3Factory(); -->
<bean id="beanfactory" class="educoder.Bean3Factory"></bean>
<!-- 等同于 bean3 = beanfactory.getbean3(); -->
<bean id="bean3" factory-bean="beanfactory" factory-method="getbean3"></bean>
```

### Bean的作用域

Spring中可以为Bean指定作用域，通过设置bean的scope

|     描述      |                          作用域名称                          |
| :-----------: | :----------------------------------------------------------: |
| **singleton** | 默认的作用域，使用singleton定义的Bean在Spring容器中只有一个Bean实例 |
| **prototype** | Spring容器每次获取prototype定义的Bean，容器都将创建一个新的Bean实例 |
|    request    | 在一次HTTP请求中容器将返回一个Bean实例，不同的HTTP请求返回不同的Bean实例。<br />仅在web Sprig应用程序上下文中使用 |
|    session    | 在一个HTTP Session中，容器将返回同一个Bean实例。仅在Web Spring应用程序上下文中使用 |
|  application  | 为每个ServletContext对象创建一个实例，即同一个应用共享一个Bean实例。仅在Web spring应用程序上下文中使用 |
|   websocket   | 为每个WebSocket对象创建一个Bean实例。仅在Web Spring应用程序上下文中使用 |

#### **singleton**作用域

singleton是scope的默认值；
测试一下：

```java
//2、从IOC的容器中获取Bean的实例
HelloWorld helloWorld1 = (HelloWorld) ap.getBean("hello");
HelloWorld helloWorld2 = (HelloWorld) ap.getBean("hello");
//3、调用方法
System.out.println(helloWorld1);
System.out.println(helloWorld2);

输出：
    HelloWorld@12d3a4e9
    HelloWorld@12d3a4e9	
即是同一个Bean实例
```

#### prototype作用域

不言而喻，每一次创建都是新的实例

```java
//2、从IOC的容器中获取Bean的实例
HelloWorld helloWorld1 = (HelloWorld) ap.getBean("hello");
HelloWorld helloWorld2 = (HelloWorld) ap.getBean("hello");
//3、调用方法
System.out.println(helloWorld1);
System.out.println(helloWorld2);

输出：
    HelloWorld@1f1c7bf6
    HelloWorld@25b485ba	
即是两个Bean实例
```

### Bean的装配方式

即Bean依赖注入的方式





# Maven配置

https://www.cnblogs.com/lizm166/p/10748867.html	Apache Maven 3.6.1配置安装

idea配置maven  https://blog.csdn.net/u012538609/article/details/104901047