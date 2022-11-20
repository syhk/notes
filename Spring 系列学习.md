# Spring 系列学习

### bean 的作用域

1. 
单例模式（默认机制） single
```java
@Test
public void test(){

     ApplicationContext context=new ClassPathXmlApplicationContext("Userbeans.xml");
//        单例情况下
     User user=(User) context.getBean("us");

     User user1=(User) context.getBean("us");

     System.out.println(user.equals(user1));
     System.out.println(user.hashCode()==user1.hashCode());
     }
     xml:
<bean id="us" class="pojo.demo.User"  scope="singleton" >
<property name="name" value="ss"/>
<property name="age"  value="85"/>
</bean>
scope 改变为 prototype后全部返回false
```



返回 true , true

1. 
原型板式 prototype 每次从容器中 get 的时候，都会产生一个新对象

2. 
其他 request、session、application、这些只能在 web 开发中使用到！


### bean 的自动装配

- 
自动装配是 spring 满足 bean 依赖的一种方式！

- 
spring 会在上下文中自动寻找，并自动给 bean 装配属性


有三种装配的方式：

1. 
在 xml 中显示配置

2. 
在 java 中显示配置

3. 
隐式的自动装配 bean


byName: 会自动在容器上下文中查找，和自己对象 set 方法对应的 beanid

byType: 会自动在容器上下文中查找，和自己对象属性相同的 beanid （这个可以不用写id，

因为它是根据类型

)

小结： byname 的时候，需要保证所有 bean 的id 唯一，并且这个 bean 需要和自动注入的属性的set方法的值一致！

bytype 的时候，需要保证所有 bean 的 class 唯一，并且这个 bean 需要和自动注入的属性的类型一致!

-->

<bean id="people" class="pojo.demo.People" autowire="byType">

<property name="name" value="shenyang"/>

</bean>

### 自动装配

自动装配的 xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd"
>

    <context:annotation-config/>

    <!-->引入注解的配置<别忘记这个
        @Autowired 直接在属性上使用,也可以在 set 方法上使用
        使用它可以不用再编写 set 方法，前提是   -->

    <bean id="cat" class="pojo.demo.Cat"/>
    <bean id="dog" class="pojo.demo.Dog"/>
    <bean id="people" class="pojo.demo.People"/>

</beans>
```

==[context:annotation-config/]() 要配置这个==

```java
package pojo.demo;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.ImportResource;
import org.springframework.lang.Nullable;

public class People {
//    如何显示定义了 required 为 false 说明这个对象可以空
//     @Autowired(required = false)
    private Cat cat;
    @Autowired
    private Dog dog;

//    自动装配的环境比较复杂，自动装配无法通过一个注解完成的时候，可以使用
//     @Qualifier (value = "id") 指定 bean 对象里面唯一的对象

//@Nullable 这个注解也是标记了这个字体说明可以为空
    public void setCat(@Nullable Cat  cat) {
        this.cat = cat;
    }
    @Override
    public String toString() {
        return "People{" +
                "cat="  +
                ", dog="  +
                '}';
    }
}
```

**[@Nullable ](/Nullable ) 这个注解也是标记了这个注解说明可以为空 **

### 使用注解开发

使用注解需要导入 context 约束，添加注解支持

也要保证 aop 包的导入

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd"
>
<!--    指定要扫描的包，这个包下的注解就会生效-->
    <context:component-scan base-package="pojo.demo1"/>
    <context:annotation-config></context:annotation-config>

<!--    <bean id="user" class="pojo.demo1.dao.User">-->
<!--        <property name="name" value="shenyang"/>-->
<!--    </bean>-->

    <context:annotation-config/>

</beans>
```

类使用注解

```java
package pojo.demo1.dao;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;
//在这里
@Component
public class User {
//     @Value("shenyang") 相当于<bean id="user" class="pojo.demo1.dao.User">
//        <property name="name" value="shenyang"/>
//    </bean>

    @Value("shenyang")
    public String name;
//    @Value("shenyang") 这个是属性注入

}
```

衍生的注解

[@Component ](/Component ) 有几个衍生的注解，我们 web 开发中，会按照 mvc 三层架构分层！ 

- 
dao   [@Repository ](/Repository ) 

- 
service  [@Service ](/Service ) 

- 
controller  [@Controller ](/Controller ) 


这四个注解功能都是一样的，都是代表将某个类注册到 spring 容器串，装配！

@Scope("singleton") //标注单例，作用域也可以标注原型，可以 xml 中看有哪些选项

singleton：默认的，Spring会采用单例模式创建这个对象。关闭工厂 ，所有的对象都会销毁。

prototype：多例模式。关闭工厂 ，所有的对象不会销毁。内部的垃圾回收机制会回收

小结： xml 更加万能，适用于任何场合，维护简单方便

注解不是自己类用不了，维护相对复杂

最佳实践：xml 用来管理 bean

注解只负责完成属性的注入

在使用的过程中，必须让注解生效，需要开启注解的支持

```xml
<!--    指定要扫描的包，这个包下的注解就会生效-->
    <context:component-scan base-package="pojo.demo1"/>
    <context:annotation-config></context:annotation-config>
```

#### 完全使用 java 的方式配置 Spring

现在要完全使用 java ，不使用 xml 配置

javaConfig 是 spring 的一个子项目，在 spring4 之后成为了一个核心功能！

config 配置类

```java
package pojo.demo1.dao.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import pojo.demo1.dao.Userconfig;

//配置类
@Configuration
@Import(User.class) //导入其他配置
public class config {//它也会被 spring 容器托管，注册到容器中，因为他本来就是一个 @Component
//   @Configuration 代表是一个配置类，和前面的 bean.xml 一样

//    注册一个 bean 就相当于之前写的一个 bean 标签，
//    这个方法的名字就相当于 bean 标签中的 id 属性
//    方法的返回值，就想相当于 bean 标签中的 class 属性
    @Bean
    public Userconfig getUserconfig(){
        return new Userconfig();
    }

}
```

普通类：

```java
package pojo.demo1.dao;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Controller;

@Controller
public class Userconfig {
    private String name;

    public String getName() {
        return name;
    }
    @Value("张三")
    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Userconfig{" +
                "name='" + name + '\'' +
                '}';
    }
}
```

测试类

```
@Test
public void test3(){
//        如何完全使用了配置类去做，我们就只能通过 AnnotationConfig 上下文来获取容器，通过配置类的 class 对象加载！
    
  ApplicationContext context=new AnnotationConfigApplicationContext(config.class);
    Userconfig userconfig=(Userconfig) context.getBean("getUserconfig");
    System.out.println(userconfig.toString());
}
```

导入其他配置：[@Import(User.class) ](/User.class) 

## 代理模式

因为它是 SpringAOP 的底层！

代理模式的分类：

- 
静态代理

- 
动态代理


### 静态代理

角色分析：

- 
抽象角色：一般会使用接口或者抽象类来解决

- 
真实角色：被代理的角色

- 
代理角色：代理真实角色，代理真实角色后，一般会做一些附属操作

- 
客户：访问代理对象的人！


好处：

- 
可以使真实角色的操作更加纯粹！不用去关注一些公共的业务

- 
公共业务就交给了代理角色！实现了业务的分工

- 
公共业务发生扩展的时候，方便管理


1. 接口

```java
package pojo.demo1.dao.agent;
//租房接口
public interface Rent {

    public void rent();
}
```

1. 真实角色

```java
package pojo.demo1.dao.agent;

//房东
public class Host implements Rent{

    @Override
    public void rent() {
        System.out.println("房东要出租房子");
    }
}
```

1. 代理角色

```java
package pojo.demo1.dao.agent;

import pojo.demo1.dao.demo.Host;

public class Proxy implements Rent {
    private Host host;

    public Proxy() {

    }

    public Proxy(Host host) {
        this.host = host;
    }

    @Override
    public void rent() {
        this.seeHouse();
        host.rent();
        this.fare();

    }

    //    看房
    public void seeHouse() {
        System.out.println("中介带看房");
    }

    //    收中介费
    public void fare() {
        System.out.println("收中介费");
    }

}
```

1. 客户端访问角色

```java
package pojo.demo1.dao.agent;

import pojo.demo1.dao.demo.Host;

public class client {

    public static void main(String[] args) {
        Host host = new Host();

//        代理一般会有一些附属操作
        Proxy proxy = new Proxy(host);
//       不用面对房东，直接找中介租房子
        proxy.rent();
    }
}
```

缺点：

- 一个真实角色就会产生一个代理角色；代码量会翻倍，开发效率低

### AOP （面向切面编程）

### 动态代理 

动态代理和静态代理的角色一样

动态代理的类是动态生成的，不是我们直接写好的 ！

动态代理分为两大类：

- 
基于接口的动态代理

   - jdk 的动态代理
- 
基于类的动态代理

   - 
cglib 

   - 
java 字节码实现 javasist


两个类： Proxy：代理、InvocationHandler：调用处理程序

一个动态代理的是一个接口，一般就对应一类业务

一个动态代理可以代理多个类，代理的是接口

AOp在spring 中的作用:提供声明式事务，允许用户自定义切面

使用 spring 实现 aop 需要导入一个包：

```xml
<!--        使用 aop 需要导入这个依赖包-->

        <!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.9.9.1</version>
        </dependency>
```

### 整合 Mybatis

步骤：

- 
导入相关 jar 包

   - 
junit

   - 
mybatis

   - 
mysql

   - 
spring

   - 
aop织入

   - 
mybatis-spring (新包)

- 
编写配置文件

- 
测试


# 声明式事务

- 
要么成功，要么都失败

- 
事务在项目开发中，十分重要，涉及数据的一致性问题；

- 
确保完整性和一致性


ACID原则：

- 
原子性

- 
一致性

- 
隔离性

- 
持久性

   - 事务一旦提交，无论系统发生什么问题，结果都不会被影响
