# Springboot

[11、多环境配置及配置文件位置_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1PE411i7CV?p=11&spm_id_from=pageDriver&vd_source=bef5db7795169cc985279d9e8fbc422c)

- 
jdk

- 
maven

- 
springboot

- 
idea


官方的快速网站：

[Spring Initializr](https://start.spring.io/)

![](image/image_OobpKsydT-.png#alt=)

双击Package可以将程序打包为一个可执行的 jar 包，在target 目录下。

springbanner在线生成：

[Spring Boot banner在线生成工具，制作下载banner.txt，修改替换banner.txt文字实现自定义，个性化启动banner-bootschool.net](https://www.bootschool.net/ascii/)

![](image/image_d4Vh7roibL.png#alt=)

在 resources 目录下新建一个文本文件，将内容放进去就可以自定义：

启动项目的开关图案

![](image/image_ulFpcBchvv.png#alt=)

修改访问端口号：

![](image/image_x68RE8xDxg.png#alt=)

### Springboot 自动装配原理

pom.xml

- 
spring-boot-dependencies : 核心依赖在父工程中。

- 
在写或者引入一些Springboot 依赖的时候，不需要指定版本，因为有这些版本仓库

- 
启动器

   - 
```xml
<dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-web</artifactId>
      eg:这个会自动导入web环境所有的依赖
    </dependency>
```

要使用什么功能，只要找到对应的启动器就可以了。

```java
//@SpringBootApplication : 标注这个类是一个 springboot的应用
@SpringBootApplication
public class DemoApplication {
  public static void main(String[] args) {
//    将  springboot 应用启动
    SpringApplication.run(DemoApplication.class, args);
  }
```

注解：

```java
@SpringBootConfiguration : springboot 的配置
@Configuration spring配置类
@Component：说明也是一个spring的组件

@EnableAutoConfiguration ： 自动配置
```

spring boot 所有自动配置都是在启动的时候扫描并加载： spring.factories 所有的自动配置类都在这里面，但是不一定生效，要判断条件是否成立，只要导入了对应的 start ，就有对应的启动器了，有了启动器，自动装配就会生效，就可以配置成功。

### yaml 语法详解

官方的配置文件可以删除了，用这个后缀名的文件来代替，不过文件名字要一样

![](image/image_5i65OKXYKV.png#alt=)

写成这样：

![](image/image_7IB5GQhxh8.png#alt=)

基础语法：

对空格要求高

可以注入到我们的配置类中。

```yaml
k: (空格) v 
#=====================================================
#  普通的 key-value
  name: syhk
  
#对象
student:
  name: syhk
  age: 21
  
#  行内写法
studnet: {name: syhk,age: 21} 
#都有空格分隔

#数组 
pets:
  - cat
  - dog
  - pig
pets: [cat,dog,pig]
```

yaml 可以直接给实体类赋值

![](image/image_xopbgzeVmB.png#alt=)

yaml中：

![](image/image_o6XsS7F5X1.png#alt=)

赋值成功。

```java
@ConfigurationProperties(prefix = "xxx")
将yaml配置文件绑定到 java 类上
```

#### jsr303数据校验（验证数据是否合法） 和 松散绑定

#### springboot 的多环境配置可以指定激活哪个配置环境。

![](image/image_Zf2vDwlOvv.png#alt=)

用 yaml 可以一个文件编写多个配置环境。用三条横杆分隔，active : 文件名 激活哪个配置。 profiles : 文件名 为配置文件起名字

---

#### 控制反转

> 传统程序设计中，我们需要使用某个对象的方法，需要先通过new创建一个该对象，我们这时是主动行为；而IoC是我们将创建对象的控制权交给IoC容器，这时是由容器帮忙创建及注入依赖对象，我们的`程序被动的接受IoC容器创建的对象`，控制权反转，所以叫控制反转.。


> DI（依赖注入：Dependency Injection）的概念，即让第三方来实现注入，以移除我们类与需要使用的类之间的依赖关系。总的来说，IoC是目的，DI是手段，创建对象的过程往往意味着依赖的注入。我们为了实现IoC，让生成对象的方式由传统方式(new)反转过来，需要创建相关对象时由IoC容器帮我们注入(DI)。


自动装配原理：

- 
Springboot 启动会加载大量的自动配置类

- 
需要看我们需要的功能有没有在Springboot 默认写好的自动配置类中

- 
再看这个自动配置类中到底配置哪些组件。要用到的组件存在其中就不需要手动配置

- 
给容器中自动配置类添加组件的时候，会从 properties 类中获取某些属性，只需要配置文件中指定这些属性即可。

- 
xxxAutoConfiguration : 自动配置类；给窗口中添加组件

- 
xxxProperties 封闭配置文件中相关属性


 

可以通过这个在配置文件中知道哪些配置生效哪些没有生效

```yaml
debug: true
```

**1.Spring的自动装配原理**：Spring Boot启动的时候会通过@EnableAutoConfiguration注解找到META-INF/spring.factories配置文件中的所有自动配置类，并对其进行加载，这些自动配置类都是以AutoConfiguration结尾来命名的，它实际上就是一个JavaConfig形式的Spring容器配置类，通过@Bean导入到Spring容器中，以Properties结尾命名的类是和配置文件进行绑定的。它能通过这些以Properties结尾命名的类中取得在全局配置文件中配置的属性，我们可以通过修改配置文件对应的属性来修改自动配置的默认值，来完成自定义配置

**2.run方法的作用**

1、推断应用的类型是普通的项目还是Web项目

2、查找并加载所有可用初始化器 ， 设置到initializers属性中

3、找出所有的应用程序监听器，设置到listeners属性中

4、推断并设置main方法的定义类，找到运行的主类

![](image/image_iB667C0qYs.png#alt=)

### SpringBoot Web开发

 

> webjars


[https://www.webjars.org/documentation](https://www.webjars.org/documentation)

![](image/image_MubmxSyjKF.png#alt=)

resources 的优先级最高，其次是 public  ; templates 是不可以放的

在Springboot 中我们使用以下方式处理静态资源：

- 
webjars   localhost:8080/webjars/**

- 
public,static,/ **,resources   localhost:8080/


Thymeleaf

[百里香叶 (thymeleaf.org)](https://www.thymeleaf.org/)

导入对应的依赖就可以使用了。将 html 放在 templates 下即可。

依赖在 github 上找：

[thymeleaf/pom.xml at 3.1-master · thymeleaf/thymeleaf (github.com)](https://github.com/thymeleaf/thymeleaf/blob/3.1-master/pom.xml)

所有的Html元素都可以被  thymeleaf 接管。

```html
<div th:text="${msg}"></div>
```

扩展SringMvc 

扩展视图解析器

```java
package com.example.demo.config;
// 全面扩展 SpringMvc
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.View;
import org.springframework.web.servlet.ViewResolver;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

import java.util.Locale;

@Configuration
public class MyMvcConfig implements WebMvcConfigurer {

//    public interface ViewResolver 实现了视图解析器接口的类，就可以把它看作是一个视图解析器
    public static class MyViewResolver implements ViewResolver{
//自定义了一个视图解析器
    @Bean
    public ViewResolver myViewResolver(){
        return new MyViewResolver();
    }
    @Override
    public View resolveViewName(String viewName, Locale locale) throws Exception {
        return null;
    }
}
}
```

配置类一般写 config 下

`./mvnw spring-boot:run ./mvnw clean package`

运行上面这两个命令也可以把程序打包成 jar

用命令运行打包好的 jar 包：

```java
java -jar target/gs-serving-web-content-0.1.0.jar
```

[@RestController ](/RestController ) ：这个注解会将返回的对象数据转换为 JSON 格式。 

[@RequestMapping ](/RequestMapping ) 注解负责 URL 的路由映射。 

- 
常用属性：

   - 
value ： 请求URL 的路径，支持通配符匹配 URL

   - 
method ： HTTP 请求方法

   - 
consumes : 请求的媒体类型（Content-Type)

   - 
produces ：响应的媒体类型

   - 
params,headers : 请求的参数及请求头的值 


[@RequestParam ](/RequestParam ) ： 参数映射，也要求这个参数必须要传递 

### RESTful

### Swagger

> 用于生成 描述  调用 和可视化 RESTful 风格的 Web 服务。


[适用于团队的 API 文档和设计工具|斯瓦格 (swagger.io)](https://swagger.io/)

### ORM

> ORM 对象关系映射 ： 是为了解决面向对象与关系数据库存在的互不匹配现象


ORM 通过使用描述对象和数据库之间映射的元数据将程序中的对象自动持久化到关系数据库

本质：简化操作数据库的编码

### MyBatis-Plus

[www.mybatis-plus.com](https://www.mybatis-plus.com/en/guide/)

[MyBatis-Plus (baomidou.com)](https://baomidou.com/)

```xml
  <dependency>
      <groupId>com.baomidou</groupId>
      <artifactId>mybatis-plus-boot-starter</artifactId>
      <version>3.5.2</version>
    </dependency>
    
        <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis-spring</artifactId>
      <version>2.0.2</version>
    </dependency>
```

---


---


---


---


---


---


---

【【尚硅谷】SpringBoot2零基础入门教程（spring boot2干货满满）】 [https://www.bilibili.com/video/BV19K4y1L7MT?p=6&share_source=copy_web&vd_source=b64f1de0086ccff3506b0fb6d8e13d4f](https://www.bilibili.com/video/BV19K4y1L7MT?p=6&share_source=copy_web&vd_source=b64f1de0086ccff3506b0fb6d8e13d4f)

spingboot 可以打包成 jar包在服务器上运行

```xml
   <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.7.3</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    
    它的父项目：
     <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-dependencies</artifactId>
    <version>2.7.3</version>
  </parent>
  它几乎包含了平常开发中常用的包
  
  spring-boot-starter-* : 只要引入  starter 
  *-spring-boot-starter 第三方提供的场景启动器
```

默认引入依赖可以不写版本号，但如何引入的没有要自动配置列表中，就必须需要写版本号

springboot 帮我们配置好了所有 web 开发的常见场景

主程序所在的包及其下面的所有子包里面的组件都会被默认扫描出来

如果想要改变扫描路径：

```java
@ComponentScan("路径")
或者
@SpringBootApplication(scanBasePackages = "路径")
两个注解不能同时出现在一个地方
```

配置是按需加载：引入了哪些场景配置才会开启

```java
@SpringBootApplication(scanBasePackages = "com.example") //这一个相当于下面三个合成的
//@SpringBootConfiguration
//@EnableAutoConfiguration
//@ComponentScan(excludeFilters = { @ComponentScan.Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),
//        @ComponentScan.Filter(type = FilterType.CUSTOM, classes = AutoConfigurationExcludeFilter.class) })

//============================================================
```

- 
@SpringBootConfiguration 

   - [@Configuration ](/Configuration ) 代表当前是一个配置类 
- 
[@ComponentScan ](/ComponentScan ) 

   - 指定扫描 哪些 Spring 注解
- 
@EnableAutoConfiguration  开启什么自动配置
```java
@AutoConfigurationPackage
@Import(AutoConfigurationImportSelector.class)
public @interface EnableAutoConfiguration {
```


- 
@AutoConfigurationPackage 

   - 
自动配置包
```java
@Import(AutoConfigurationPackages.Registrar.class)//给容器中导入组件 
public @interface AutoConfigurationPackage {

//利用 Registrar 给容器中导入一系列组件 
//将指定一个包下的所有组件导入进来 MainApplication 所在包下，主类下
```


- 
@Import(AutoConfigurationImportSelector.class)

   - getAutoConfigurationEntry(annotationMetadata)
```java
getAutoConfigurationEntry(annotationMetadata); 
利用这个方法组容器中批量导入组件
List<String> configurations = getCandidateConfigurations(annotationMetadata, attributes);
    


spring-boot-autoconfigure-2.7.3.jar
```

![](image/image_3Nl8_UBwdE.png#alt=)

![](image/image_6F2dgccyJ4.png#alt=)

![](image/image__68L4BKpXW.png#alt=)

```java
  @Bean
    @ConditionalOnBean(MultipartResolver.class)//容器中有这个类型组件
    @ConditionalOnMissingBean(name = DispatcherServlet.MULTIPART_RESOLVER_BEAN_NAME)
    //容器中没有这个名字的组件 multipartResolver
    public MultipartResolver multipartResolver(MultipartResolver resolver) {
      // Detect if the user has created a MultipartResolver but named it incorrectly
     //给 @Bean 标注的方法传入了对象参数，这个参数的值就会从容器中找
     //SpringMVC  MultipartResolver  防止用户配置的文件上传功能不符合规范
      return resolver;
    }
    给容器中加入文件上传解析器
```

Springboot 默认会在底层配好所有组件，如果用户自己配置了以用户配置的优先。

总结：

- 
SpringBoot 先加载所有的自动配置类  xxxAutoConfiguration

- 
每个自动，配置类按照条件进行生效，默认都会绑定配置文件指定的值  xxxProperties 里面拿，跟配置文件进行了绑定

- 
生效的配置类就会给容器中装配很多组件

- 
只要容器中有这些组件，相当于这些功能就有了

- 
只有用户自己配置了，就会以用户配置的优先


定制化配置：

- 
用户直接自己[@Bean ](/Bean ) 替换 

- 
用户去看这个组件是获取的配置文件什么值去修改


配置文件中：

#开启自动配置报告

debug=true

Positive matches:生效的

Nogative 不生效的

自定义加入或替换组件：

[@Bean ](/Bean ) , [@Component ](/Component ) 

### 使用 Lombok 

简化 javaBean 开发

```xml
 <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
```

引入依赖

```java
@Data
@ToString  
@AllArgsConstructor  //有参构造器
@NoArgsConstructor //无参构造器
@Getter
@Setter //引入 lombok 后简化开发
@EqualsAndHashCode
@Slf4j  使用：  log.info("请求进来了");  日志
```

需要定制的话可以自己在下面写一个

### dev-tools

```xml
  <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
```

修改代码后直接 ：ctrl+F9 就可以了

如果是静态页面就可以不用重启，其他的需要重启，手动点也行

目录结构：

![](image/image_tuKTQqSytf.png#alt=)

```java
package com.example.demo1;

import com.example.demo1.bean.Pet;
import com.example.demo1.bean.User;
import com.example.demo1.config.MyConfig;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.SpringBootConfiguration;
import org.springframework.boot.autoconfigure.AutoConfigurationExcludeFilter;
import org.springframework.boot.autoconfigure.EnableAutoConfiguration;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.context.TypeExcludeFilter;
import org.springframework.cache.interceptor.CacheAspectSupport;
import org.springframework.context.ConfigurableApplicationContext;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.FilterType;

//这是一个 springboot 应用

/*
*
*
*
*
* */

//@SpringBootApplication(scanBasePackages = "com.example") //这一个相当于下面三个合成的
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(excludeFilters = { @ComponentScan.Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),
        @ComponentScan.Filter(type = FilterType.CUSTOM, classes = AutoConfigurationExcludeFilter.class) })

public class Demo1Application {

    public static void main(String[] args) {
//        返回我们的 IOC 容器
        ConfigurableApplicationContext run = SpringApplication.run(Demo1Application.class, args);
//查看容器里面的组件
        String[] names = run.getBeanDefinitionNames();
        for (String name : names){
            System.out.println(name);
        }
//
////        从容器中获取组件，注册的组件默认是单实例的
////      run.getBean("pet", Pet.class);
////        run.getBean("pet", Pet.class);
//        System.out.println(  run.getBean("syhk", Pet.class)==  run.getBean("pet", Pet.class));
////结果为 true
//
//        MyConfig config=run.getBean(MyConfig.class);
//        System.out.println(run.getBean(MyConfig.class));
////@Configuration(proxyBeanMethods=true) 代理对象调用方法， springboot 总会检查这个组件 是否在容器中
////        也主是保持着 组件的单实例 proxyBeanMethods 它为 true 的情况下为 false 不保持
//    Pet pet=config.pet();
//    Pet pet1=config.pet();
//        System.out.println(pet1==pet);
//
////用户中的宠物和容器中的宠物是同一个
//        User user=run.getBean("user01",User.class);
//        System.out.println(user.getPet()==pet);
//
//        System.out.println("=============================================");
////
//        String[] beanNamesForType = run.getBeanNamesForType(User.class);
//        for(String s : beanNamesForType){
//            System.out.println(s);
//        }
//检查某个组件是否在容器中
        System.out.println(run.containsBean("syhk"));
        System.out.println(run.containsBean("user01"));
        System.out.println(run.containsBean("pet"));

        System.out.println(run.getBeanDefinitionCount());//获取容器中的组件数量

    }

}
```

HelloController:

```java
package com.example.demo1.controller;


import com.example.demo1.bean.Cat;
import lombok.extern.java.Log;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.ResponseBody;
import org.springframework.web.bind.annotation.RestController;

//@Controller
//@ResponseBody
@RestController  //相当于上面两个注解
@Slf4j
public class HelloController {


    @RequestMapping("/")
    public String handle01(){
        return "Hello springboot";
    }


    @Autowired
    Cat cat;

    @RequestMapping("/cat")
    public String cat(@RequestParam("name") String name){
        log.info("请求进来了");
        return cat+name;
    }



}
```

Myconfig:

```java
package com.example.demo1.config;


import com.example.demo1.bean.Cat;
import com.example.demo1.bean.Pet;
import com.example.demo1.bean.User;
import org.springframework.boot.autoconfigure.condition.ConditionalOnBean;
import org.springframework.boot.autoconfigure.condition.ConditionalOnMissingBean;
import org.springframework.boot.context.properties.EnableConfigurationProperties;
import org.springframework.context.annotation.*;

//配置类里面使用 @Bean 标注在方法上给容器注册组件，默认也是单实例的
//配置类本身也是一个组件
//proxyBeanMethods ：代理 bean 的方法 默认为 true
//Full 模式和 Lite 模式
/**
 * @Import({User.class}) 参数是一个数组，容器中会自动创建出这个组件
 *默认组件的名字就是全类名
 *
 */


//@ConditionalOnBean(name="pet")
//加在类上，就说明只有这个成立了，这个类里面的配置才会生效
@EnableConfigurationProperties(Cat.class)
//开启 Cat 配置绑定功能； 把这个组件 Cat 自动注册到容器中
@Import({User.class})
@ImportResource("classpath:beans.xml")
//可以使用这个注解导入以前 spring 用 xml 来配置
@ConditionalOnMissingBean(name="pet")
//和上面那个是相反的，没有则生效
@Configuration(proxyBeanMethods=true) 
//告诉 springboot 这是一个配置类
public class MyConfig {

//    外部无论对配置类中的各个组件注册方法调用多少次获取的都是之前 
//注册容器中的单实例对象
//    @Bean //给容器中添加组件，以方法名作为组件的 id ，
//返回类型就是组件类型，返回的值，就是组件在容器中的实例
    public Pet pet(){
       return new Pet("syhk",21);
    }

//    @Bean("syhk")
    public Pet tomcatPet(){
        return new Pet("tomcat");
    }

//    @ConditionalOnBean(name="pet")
//容器中有 pet 组件的时候才注册 User
    @Bean
    public User user01(){
        User zhangsan=new User("zhangsan",22);
        zhangsan.setPet(pet());
        return zhangsan;
    }



}
```

bean目录下相关：

```java
package com.example.demo1.bean;

import lombok.*;

@Data
@AllArgsConstructor
@NoArgsConstructor
@ToString
@Setter
@Getter
@EqualsAndHashCode
public class User {
    private String name;
    private int age;
    private Pet pet;

    /*也可以自己写*/
    public User(String name, int age) {
        this.name = name;
        this.age = age;
    }
    //
//    @Override
//    public String toString() {
//        return "User{" +
//                "name='" + name + '\'' +
//                ", age=" + age +
//                ", pet=" + pet +
//                '}';
//    }
//
//    public Pet getPet() {
//        return pet;
//    }
//
//    public void setPet(Pet pet) {
//        this.pet = pet;
//    }
//
//    public String getName() {
//        return name;
//    }
//
//    public User() {
//    }
//
//    public void setName(String name) {
//        this.name = name;
//    }
//
//    public int getAge() {
//        return age;
//    }
//
//    public void setAge(int age) {
//        this.age = age;
//    }
//
//    public User(String name, int age) {
//        this.name = name;
//        this.age = age;
//    }
}
//========================================================
package com.example.demo1.bean;




public class Pet {
    private String name;
    private Integer age;

    public Pet(String name) {
        this.name = name;
    }

    public Pet() {
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    public Pet(String name, Integer age) {
        this.name = name;
        this.age = age;
    }
}

//===================================================
package com.example.demo1.bean;

import lombok.*;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.boot.context.properties.EnableConfigurationProperties;
import org.springframework.stereotype.Component;

// 第一种方式：
/*
* //@Component  //把类给容器托管
@ConfigurationProperties(prefix = "mycat") 
//配置绑定,根配置文件中的值进行绑定
*
* */
//只有在容器中的组件都会拥有 springboot 提供的强大功能
//@Component  //把类给容器托管
//@EnableConfigurationProperties(Cat.class) 
//这个要加上配置类上，不能放在实体类上
@ConfigurationProperties(prefix = "mycat") 
//配置绑定,根配置文件中的值进行绑定
@Data
@ToString
@AllArgsConstructor  //有参构造器
@NoArgsConstructor //无参构造器
@Getter
@Setter //引入 lombok 后简化开发
public class Cat {
    private String name;
    private Integer age;
//    public String getName() {
//        return name;
//    }
//
//    public Cat() {
//    }
//
//    public void setName(String name) {
//        this.name = name;
//    }
//
//    public Integer getAge() {
//        return age;
//    }
//
//    public void setAge(Integer age) {
//        this.age = age;
//    }
//
//    public Cat(String name, Integer age) {
//        this.name = name;
//        this.age = age;
//    }
}
```

### yaml

基本语法： k: v

kv 之间有空格，用缩进来表示层级关系

缩进不允许使用  tab ，只允许空格

#表示注释

### Web 

只要静态资源

请求进来，先去找 Controller 能不能处理，不能处理的所有请求又都交给静态资源处理器，静态资源也找不到就报 404

```yaml
resources:
  static-locations: classpath:/syhk
```

自定义静态资源路径。

静态资源访问前缀会影响网站图标

### 静态资源管理

配置类只有一个有参构造器

WebMvcAutoConfiguration 类中：

资源处理的规则：

```java
@Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
      if (!this.resourceProperties.isAddMappings()) {
        logger.debug("Default resource handling disabled");
        return;
      }
      addResourceHandler(registry, "/webjars/**", "classpath:/META-INF/resources/webjars/");
      addResourceHandler(registry, this.mvcProperties.getStaticPathPattern(), (registration) -> {
        registration.addResourceLocations(this.resourceProperties.getStaticLocations());
        if (this.servletContext != null) {
          ServletContextResource resource = new ServletContextResource(this.servletContext, SERVLET_LOCATION);
          registration.addResourceLocations(resource);
        }
      });
    }
```

```yaml
add-mappings: false 对应上面的方法!this.resourceProperties.isAddMappings()
```

欢迎页处理规则：

```java
    HandlerMapping: 处理器映射，保存了第一个 Handler 能处理哪些请求。
    @Bean
    public WelcomePageHandlerMapping welcomePageHandlerMapping(ApplicationContext applicationContext,
        FormattingConversionService mvcConversionService, ResourceUrlProvider mvcResourceUrlProvider) {
      WelcomePageHandlerMapping welcomePageHandlerMapping = new WelcomePageHandlerMapping(
          new TemplateAvailabilityProviders(applicationContext), applicationContext, getWelcomePage(),
          this.mvcProperties.getStaticPathPattern());
      welcomePageHandlerMapping.setInterceptors(getInterceptors(mvcConversionService, mvcResourceUrlProvider));
      welcomePageHandlerMapping.setCorsConfigurations(getCorsConfigurations());
      return welcomePageHandlerMapping;
    }
    
    
      WelcomePageHandlerMapping(TemplateAvailabilityProviders templateAvailabilityProviders,
      ApplicationContext applicationContext, Resource welcomePage, String staticPathPattern) {
    if (welcomePage != null && "/**".equals(staticPathPattern)) {
      logger.info("Adding welcome page: " + welcomePage);
      setRootViewName("forward:index.html");
    }
    else if (welcomeTemplateExists(templateAvailabilityProviders, applicationContext)) {
      logger.info("Adding welcome page template: index");
      setRootViewName("index");
    }
  }
```

### 请求参数处理

Rest风格支持

/user  GET-获取用户 DELETE-删除用户  PUT-修改用户 POST-保存用户

核心Filter: HiddenHttpMethodFilter

用法： 表单 method=post ,隐藏域 _method=put

springboot 中手动开启

```xml
mvc:
  hiddenmethod:
    filter:
      enabled: true
      或者：
      server.port=8888
spring.mvc.hiddenmethod.filter.enabled=true
```

```java
    @RequestMapping(value = "/user",method = RequestMethod.GET)
    public String getUser(){
        return "GET-张三";
    }


    @RequestMapping(value = "/user",method =RequestMethod.POST)
    public String saveUser(){
        return "POST-张三";
    }

@RequestMapping(value = "/user",method = RequestMethod.PUT)
    public String puttUser(){
        return "PUT-张三";
    }


@RequestMapping(value = "/user",method = RequestMethod.DELETE)
    public String deleteUser(){
        return "DELETE-张三";
    }
```

![](image/image_QAr1n7faeR.png#alt=)

![](image/image_4Il1d-MGqi.png#alt=)

```html
<form action="/user" method="get">
    <input value="REST-GET 提交 " type="submit">
</form>

<form action="/user" method="post">
    <input value="REST-POST 提交 " type="submit">
</form>

<form action="/user" method="post">
    <input name="_method" type="hidden" value="DELETE"/>
    <input value="REST-DELETE 提交 " type="submit">
</form>

<form action="/user" method="post">
    <input name="_method" type="hidden" value="PUT"/>
    <input value="REST-PUT 提交 " type="submit">
</form>
```

    <input name="_method" type="hidden" value="DELETE"/> 要加这个 请求必须是 POST 方式

成功；

REST原理：

- 
表单提交会带上 method=PUT

- 
请求过来被  HiddenHttpMethodFilter 拦截

- 
获取到 _method 的值

- 
兼容以下请求：PUT,DELETE,PATCH

- 
原生 request (post) ,包装模式 requestWrapper 重写了 getMedthod 方法，返回的是传入的值

- 
过滤器链放行的时候用 wrapper, 以后的方法调用 getMethod 是调用 requestWrapper 的


Rest 使用客户端工具：

- 如 postman 直接发送 put,delete 等方式请求，无需 Filter

```java
    @GetMapping("/user")
    @RequestMapping(value = "/user",method = RequestMethod.GET)
    public String getUser(){
        return "GET-张三";
    }


    @PostMapping("/user")
    @RequestMapping(value = "/user",method =RequestMethod.POST)
    public String saveUser(){
        return "POST-张三";
    }

    @PutMapping("/user")
//@RequestMapping(value = "/user",method = RequestMethod.PUT)
    public String puttUser(){
        return "PUT-张三";
    }


    @DeleteMapping("/user")
//@RequestMapping(value = "/user",method = RequestMethod.DELETE)
    public String deleteUser(){
        return "DELETE-张三";
    }
```

效果是一样的！！！

```java
@Configuration(proxyBeanMethods = false)
public class WebConfig {

    public HiddenHttpMethodFilter hiddenHttpMethodFilter(){
        HiddenHttpMethodFilter methodFilter=new HiddenHttpMethodFilter();
        methodFilter.setMethodParam("_m");//修改默认的 _Method
        return methodFilter;
    }

}
```

所有请求映射都在 HandlerMapping 中

SpringBoot 自动配置了默认的 RequestMappingHandlerMapping

请求进来，挨个尝试所有的 HandlerMapping 看是否有请求信息

如果有就找到这个请求对应的 Handler

如果没有就是下一个 HandlerMapping

需要自定义映射处理，可以给容器中添加自定义的 HandlerMapping

### 常用参数注解

- 注解

![](image/image_IQZiYtw6KE.png#alt=)

![](image/image_Jil2KcsPEj.png#alt=)

```java
    @GetMapping("/car/{id}/owner/{username}")
public Map<String,Object> gteCat(@PathVariable("id") Integer id
,@PathVariable("username") String name
,@PathVariable Map<String,String> pv
    ){
     Map<String,Object> map=new HashMap<>();
     map.put("id",id);
     map.put("name",name);
     map.put("pv",pv);
        return  map;
    }
    
       @GetMapping("/car/{id}/owner/{username}")
    public Map<String,Object> gteCat(@PathVariable("id") Integer id
                                     , @PathVariable("username") String name
                                    , @PathVariable Map<String,String> pv
                                     , @RequestHeader("user-Agent") String userAgent //获取请求头
                                     ,@RequestHeader Map<String,String> header
                                     ){
     Map<String,Object> map=new HashMap<>();
     map.put("id",id);
     map.put("name",name);
     map.put("pv",pv);
     map.put("userAgent",userAgent);
     map.put("headers",header);
        return  map;
    }
```

![](image/image_1lXRnvVRIR.png#alt=)

```java
    <a href="car/3/owner/lisi?age=18&inters=basketball&inters=game">car/{id}/owner/{username}</a>
 //{..} 表示是变量
    @GetMapping("/car/{id}/owner/{username}")
    public Map<String,Object> gteCat(@PathVariable("id") Integer id
   , @PathVariable("username") String name
  , @PathVariable Map<String,String> pv
  , @RequestHeader("user-Agent") String userAgent //获取请求头
   , @RequestHeader Map<String,String> header
   , @RequestParam("age") Integer age
   , @RequestParam("inters") List<String> inters
  ,@RequestParam  Map<String,String> params
                                     ){
     Map<String,Object> map=new HashMap<>();
//     map.put("id",id);
//     map.put("name",name);
//     map.put("pv",pv);
//     map.put("userAgent",userAgent);
//     map.put("headers",header);
        map.put("age",age);
        map.put("inters",inters);
        map.put("parms",params);
        return  map;
    }
```

@RequestAttribute:

```java
    @GetMapping("/goto")
    public String goToPage(HttpServletRequest request){
        request.setAttribute("msg","成功了...");
        request.setAttribute("code",200);
        return "forward:/success";// 转发到 /success 请求
    }

    @ResponseBody
    @GetMapping("/success")
    public Map success(@RequestAttribute("msg") String msg,
                          @RequestAttribute("code") Integer code,
                          HttpServletRequest request
                          ){
        Object msg1=request.getAttribute("msg");
        Map<String,Object> map=new HashMap<>();
        map.put("reqMethod_msg",msg1);//成功了...
        map.put("annotation_msg",msg);//成功了...
        return map;
    }
    
    
    //springboot 默认是禁用了矩阵变量的功能  

//    手动开启 urlPathHelper 进行解析。  

@GetMapping("/cats/{path}")  

public String carsSell(@MatrixVariable("low") Integer low,  

  @MatrixVariable("brand") List<String> brand){  

  return low+brand.toArray().toString();  

}
```

参数处理原理：

- 
HandlerMapping 中找到能处理请求的 Handler

- 
为当前 Handler 找一个适配器 HandlerAdapter


![](image/image_Ixi7LKZRhM.png#alt=)

  0. 支持方法上标注 [@RequestMapping ](/RequestMapping ) 

参数解析器：

目标方法能写多少种参数类型，取决于参数解析器。

返回值处理器

笔记地址。

[https://www.yuque.com/atguigu/springboot](https://www.yuque.com/atguigu/springboot)

整合 mybatis-plus

```xml
<!--        mybatis-plus　-->
        <!-- https://mvnrepository.com/artifact/com.baomidou/mybatis-plus -->
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.2.0</version>
        </dependency>
```

只需要写 继承 BaseMapper 类就行了，传入泛型参数

```java
@Mapper
public interface BookDao extends BaseMapper<Book> {
    
}
测试类中：
         System.out.println(bookDao.selectById(2));
//查询所有的
        System.out.println(bookDao.selectList(null));
```

```xml
#配置 Mp 相关的配置
#mybatis-plus:
#  global-config:
#    db-config:
#      table-prefix: book
这个就是配置表的名字的前缀，如果找不到的话就配，book 表的话，
加上上面前缀的话就是 bookbook ，能找到就不用配了
```

整合 Druid

```xml
<!-- https://mvnrepository.com/artifact/com.alibaba/druid -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.2.11</version>
</dependency>
```

```yaml
spring:
  datasource:
    druid:  #就加了这里
    driver-class-name: com.mysql.cj.jdbc.Driver
    username: root
    password: 725482520
    url: jdbc:mysql://localhost:3306/svue?serverTimezone=UTC
```

整合第三方技术步骤：

- 
导入对应的 starter

- 
根据提供的配置格式，配置非默认值对应的配置项


```java
import lombok.*;
//简化 POJO 实体类开发
@Data //这里面包含了 equals Getter 和 Setter toString hashCode
@AllArgsConstructor
@NoArgsConstructor
public class Book {
    private Integer id;
    private String name;
    private String author;
}
```

增删改查

```java
    @Autowired
    BookDao bookDao;

    @Test
    void testgetById(){
        System.out.println(bookDao.selectById(3));


    }
    @Test
    void testsave(){
        Book book = new Book();
        book.setId(16);
        book.setName("ruby");
        book.setAuthor("syhk13");
        bookDao.insert(book);
    }

    @Test
    void testUpdate(){
        Book book = new Book();
        book.setId(16);
        book.setName("gogog");
        book.setAuthor("syhk13");
        bookDao.updateById(book);
    }

    @Test
    void testGetAll(){
        System.out.println(bookDao.selectList(null));
    }

    @Test
    void testDelete(){
        bookDao.deleteById(16);
    }
```

开启MP 日志：比如查看 sql 语句

```xml
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
    开启日志
```

```java
 分页功能
    @Test
    void testGetPage(){
//        需要自己手工开启，使用拦截器做的

        IPage page=new Page(1,5);
        IPage page1 = bookDao.selectPage(page, null);
        System.out.println(page1.getCurrent());
        System.out.println(page1.getSize());
        System.out.println(page1.getPages());//总页数
        System.out.println(page1.getRecords());
        System.out.println(page1.getTotal());
    }
    //拦截器 配置类
    //MP 拦截器
@Configuration
public class MPconfig {

    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor(){
        MybatisPlusInterceptor interceptor=new MybatisPlusInterceptor();
        interceptor.addInnerInterceptor(new PaginationInnerInterceptor());//分页的
        return interceptor;
    }
}
```

根据条件查询：

```java
    @Test
//    根据条件查询
    void testGetBy(){
        QueryWrapper<Book> qw=new QueryWrapper<>();
        qw.like("author","syhk2");//条件
        bookDao.selectList(qw);
    }
    @Test
//    根据条件查询
    void testGetBy2(){
        String name="syhk3";
        LambdaQueryWrapper<Book> qw=new LambdaQueryWrapper<>();
        qw.like(name!=null,Book::getAuthor,name);//条件   %syhk4%(String) 加了条件防止传递过来的数据值为 null
        bookDao.selectList(qw);
    }
```

两个对象都行推荐使用 LambdaQueryWrapper

目录结构 ：

![](image/image_Mqe4C0NEaY.png#alt=)

MPconfig类

```java
package com.example.bootdemo.config;

import com.baomidou.mybatisplus.extension.plugins.MybatisPlusInterceptor;
import com.baomidou.mybatisplus.extension.plugins.inner.PaginationInnerInterceptor;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.stereotype.Component;

//MP 拦截器
@Configuration
public class MPconfig {

    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor(){
        MybatisPlusInterceptor interceptor=new MybatisPlusInterceptor();
        interceptor.addInnerInterceptor(new PaginationInnerInterceptor());//分页的
        return interceptor;
    }
}
```

BookDao 接口

```java
package com.example.bootdemo.dao;

import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import com.example.bootdemo.enity.Book;
import org.apache.ibatis.annotations.Mapper;

//Dao 使用 Mapper 也行
@Mapper
public interface BookDao  extends BaseMapper<Book> {
//    @Select("select * from book where id = #{id}")
//    Book getById(Integer id);
//    MP做法 国人开发中文注释
}
```

Book 实体类

```java
package com.example.bootdemo.enity;

import lombok.*;
//简化 POJO 实体类开发
@Data //这里面包含了 equals Getter 和 Setter toString hashCode
@AllArgsConstructor
@NoArgsConstructor
public class Book {
    private Integer id;
    private String name;
    private String author;
}
```

BookServerImpl类

```java
package com.example.bootdemo.impl;

import com.baomidou.mybatisplus.core.metadata.IPage;
import com.baomidou.mybatisplus.extension.plugins.pagination.Page;
import com.example.bootdemo.Service.BookService;
import com.example.bootdemo.dao.BookDao;
import com.example.bootdemo.enity.Book;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.time.temporal.ChronoUnit;
import java.util.List;

@Service
public class BookServerImpl  implements BookService {

    @Autowired
    private BookDao bookDao;
    @Override
    public int save(Book book) {
      return bookDao.insert(book);
    }

    @Override
    public int update(Book book) {
        return bookDao.updateById(book);
    }

    @Override
    public int delete(Integer id) {
        return bookDao.deleteById(id);
    }

    @Override
    public Book getById(Integer id) {
        return bookDao.selectById(id);
    }

    @Override
    public List<Book> getAll() {
        return bookDao.selectList(null);
    }

    @Override
    public IPage<Book> getPage(int currentPage, int pageSize) {
     IPage page=new Page(currentPage,pageSize);
        return bookDao.selectPage(page,null);
    }
}
```

BookServiec 接口

```java
package com.example.bootdemo.Service;

import com.baomidou.mybatisplus.core.metadata.IPage;
import com.example.bootdemo.enity.Book;
import org.springframework.context.annotation.Bean;
import org.springframework.stereotype.Component;
import org.springframework.stereotype.Service;

import java.util.List;
@Component
@Service
public interface BookService {
    int save(Book book);
    int update(Book book);
    int delete(Integer id);
    Book getById(Integer id);
    List<Book> getAll();

    IPage<Book> getPage(int currentPage,int pageSize);

}
```

测试类：

```java
package com.example.bootdemo;

import com.baomidou.mybatisplus.core.conditions.query.LambdaQueryWrapper;
import com.baomidou.mybatisplus.core.conditions.query.QueryWrapper;
import com.baomidou.mybatisplus.core.metadata.IPage;
import com.baomidou.mybatisplus.extension.plugins.pagination.Page;
import com.example.bootdemo.Service.BookService;
import com.example.bootdemo.dao.BookDao;
import com.example.bootdemo.enity.Book;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

@SpringBootTest
class BootdemoApplicationTests {


    @Test
    void contextLoads() {


    }
    @Autowired
    BookDao bookDao;

    @Test
    void testgetById(){
        System.out.println(bookDao.selectById(3));


    }
    @Test
    void testsave(){
        Book book = new Book();
        book.setId(16);
        book.setName("ruby");
        book.setAuthor("syhk13");
        bookDao.insert(book);
    }

    @Test
    void testUpdate(){
        Book book = new Book();
        book.setId(16);
        book.setName("gogog");
        book.setAuthor("syhk13");
        bookDao.updateById(book);
    }

    @Test
    void testGetAll(){
        System.out.println(bookDao.selectList(null));
    }

    @Test
    void testDelete(){
        bookDao.deleteById(16);
    }

//    分页
    @Test
    void testGetPage(){
//        需要自己手工开启，使用拦截器做的
        IPage page=new Page(1,5);
        IPage page1 = bookDao.selectPage(page, null);
        System.out.println(page1.getCurrent());
        System.out.println(page1.getSize());
        System.out.println(page1.getPages());//总页数
        System.out.println(page1.getRecords());
        System.out.println(page1.getTotal());
    }

    @Test
//    根据条件查询
    void testGetBy(){
        QueryWrapper<Book> qw=new QueryWrapper<>();
        qw.like("author","syhk2");//条件
        bookDao.selectList(qw);
    }
    @Test
//    根据条件查询
    void testGetBy2(){
        String name="syhk3";
        LambdaQueryWrapper<Book> qw=new LambdaQueryWrapper<>();
        qw.like(name!=null,Book::getAuthor,name);//条件   %syhk4%(String) 加了条件防止传递过来的数据值为 null
        bookDao.selectList(qw);
    }

    @Autowired
    private BookService bookService;

    @Test
    void testService(){

    bookService.getPage(2,3);
    Book book=new Book(18,"ssy","sss");
    bookService.save(book);
        bookService.getAll();
        bookService.delete(16);
    }

}
```

快速开发

![](image/image_ZaRgU4NdUS.png#alt=)

IBookService

```java
package com.example.bootdemo.Service;

import com.baomidou.mybatisplus.extension.service.IService;
import com.example.bootdemo.enity.Book;
import org.springframework.stereotype.Component;
import org.springframework.stereotype.Service;

@Service
@Component
public interface IBookService  extends IService<Book> {

//    想要自己一些定制化，就可平常写的一样
    int saveBook(Book book);

    int modify(Book book);

    int delete(Integer id);

}
```

```java
BookServiceImplMP 类
package com.example.bootdemo.impl;

import com.baomidou.mybatisplus.extension.service.impl.ServiceImpl;
import com.example.bootdemo.Service.IBookService;
import com.example.bootdemo.dao.BookDao;
import com.example.bootdemo.enity.Book;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;
import org.springframework.stereotype.Service;

//泛型参数第一个是 用的实现类 第二个是对应的模型类，实体类
@Service
public class BookServiceImplMP  extends ServiceImpl<BookDao, Book> implements IBookService {

//    下面是自己想定制化的像这样写了
    @Autowired
    private BookDao bookDao;
    @Override
    public int saveBook(Book book) {
        return bookDao.insert(book);
    }

    @Override
    public int modify(Book book) {
        return bookDao.updateById(book);
    }

    @Override
    public int delete(Integer id) {
        return bookDao.deleteById(id);
    }
}
```

如果没有特殊需求，上面两个类中可以什么都不写，直接用MP 自带的。

MP 提供有业务层通用接口 IService<T> 与业务层通用实现类 ServiceImpl<M,T>

需要定制化：在通用类的基础上做功能重载或功能追加，上面就是功能追加

注意重载时不要覆盖原始操作，避免原始提供的功能丢失

### 表现层

接收参数：

- 
实体数据： [@RequestBody ](/RequestBody ) 

- 
路径变量：[@PathVariable ](/PathVariable ) 


//    删除更新一般是传递 JSON 数据过来，是通过请求体传递过来的

//    查询操作一般是用 路径参数传递的     [@PathVariable ](/PathVariable ) 

设计表现层返回结果的模型类，用于后端与前端进行数据格式统一，也称为 前后端数据协议

@RequestMapping("/books") //这个放在类上面的意思 是： 这里面的路径是它方法路径的前缀路径 例如:要访问save 需要写 /books/save

前后端数据一致性处理：也就是表现层数据一致性处理

![](image/image_Z0JW4t5OFu.png#alt=)

设计一个模型类：

util 包下

```java
package com.example.bootdemo.utils;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@NoArgsConstructor
@AllArgsConstructor
public class R {
    private Boolean flag; //成功还是失败
    private Object data; //数据
    public R(boolean flag){
        this.flag=flag;
    }
    public R(Object data){
        this.data=data;
    }
}
```

controller层

```java
package com.example.bootdemo.controller;


import com.example.bootdemo.Service.IBookService;
import com.example.bootdemo.enity.Book;
import com.example.bootdemo.utils.R;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;
import java.util.List;

@RestController
@RequestMapping("/books") //这个放在类上面的意思 是： 这里面的路径是它方法路径的前缀路径 eg:要访问save 需要写 /books/save
public class Bookcontroller {
//    @Qualifier("IBookService") //用来区分相同类型的 bean

    R r = new R();
    @Autowired
    private IBookService bookService; //这里为什么一起红线暂时不知道
    @GetMapping("/getall")
    public R getAll(){
        return new R(bookService.list());
    };
//    删除更新一般是传递 JSON 数据过来，是通过请求体传递过来的
//    查询操作一般是用 路径参数传递的     @PathVariable

    @PutMapping("/save")
    public R save(@RequestBody Book book){
        return  new R(bookService.save(book));
    }

    @DeleteMapping("/delete")
    public R delete(@RequestParam("id") Integer id){
      return    new R(bookService.delete(id));
    }

//    @DeleteMapping("{id}")
//    public R delete(@PathVariable("id") Integer id){
//        return    new R(bookService.delete(id));
//    }


    //    修改更新
    @PutMapping("/update")
    public R modify(@RequestBody Book book){
     return new R(bookService.modify(book));
    }


}
```

### json 转换器

常见的 json 转换器除了 jackson-databind ，还有 Gson 和 fastjson

- Gson 是 google 的开源解析框架

导入依赖

```xml
<!-- https://mvnrepository.com/artifact/com.google.code.gson/gson -->
<dependency>
    <groupId>com.google.code.gson</groupId>
    <artifactId>gson</artifactId>
    <version>2.9.0</version>
</dependency>
```

如果使用 gson 格式化日期类型，需要我们自定义 HttpMessageConverter

```java
@Configuration
public class GsonConfig {
//    如果使用 gson 格式化日期类型，需要我们自定义 HttpMessageConverter
    @Bean
    GsonHttpMessageConverter gsonHttpMessageConverter(){
        GsonHttpMessageConverter converter= new GsonHttpMessageConverter();
        GsonBuilder builder= new GsonBuilder();
        builder.setDateFormat("yyyy-MM-dd");
        builder.excludeFieldsWithModifiers(Modifier.PROTECTED);
        Gson gson = builder.create();
        converter.setGson(gson);
        return  converter;
    }

}
```

- fastjson  是阿里巴巴的开源 json 解析框架。

```xml
   <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>fastjson</artifactId>
            <version>2.0.12</version>
        </dependency>
```

```java
@Configuration
public class MyFastJsonConfig {
@Bean
    FastJsonHttpMessageConverter fastJsonHttpMessageConverter(){

    FastJsonHttpMessageConverter converter= new FastJsonHttpMessageConverter();
    FastJsonConfig config= new FastJsonConfig();
    config.setDateFormat("yyyy-MM-dd");
    config.setCharset(Charset.forName("UTF-8"));
    config.setSerializerFeatures(
            SerializerFeature.WriteClassName,
            SerializerFeature.WriteMapNullValue,
            SerializerFeature.PrettyFormat,
            SerializerFeature.WriteNullListAsEmpty,
            SerializerFeature.WriteNullStringAsEmpty
    );
    converter.setFastJsonConfig(config);
    return converter;
}
}
```

配置类

还需要在配置文件中添加配置：

```xml
使用时已经过期了，得使用 fastjson2
```

### 实现单文件上传功能

```java
@RestController
public class FileUploadController {
    SimpleDateFormat sdf = new SimpleDateFormat("yyyy/MM/dd");
// 有上传文件的最大限制，可以在配置文件中更改大小
    @PostMapping("/upload")
    public String upload(MultipartFile uploadFile, HttpServletRequest req){
        String realPath = req.getSession().getServletContext().getRealPath("/uploadFile/");
        String format = sdf.format(new Date());
        File folder = new File(realPath+format);
        if(!folder.isDirectory()){
            folder.mkdirs();
        }
        String oldName = uploadFile.getOriginalFilename();
        String newName = UUID.randomUUID().toString()+oldName.substring(oldName.lastIndexOf("."),oldName.length());
        try{
            uploadFile.transferTo(new File(folder,newName));
            String filePath = req.getScheme()+"://"+req.getServerName()+":"+req.getServerPort()+"/uploadFile/"+format+newName;

        }catch (IOException e ){
            e.printStackTrace();
        }
        return "上传失败;";
    }

//    上传多个文件
    @PostMapping("/uploads")
    public String upload(MultipartFile[] uploadFiles, HttpServletRequest req){
//       遍历  uploadFiles  数组分别存储
        return "上传失败";
    }
}
```

### [@ControllerAdvice ](/ControllerAdvice ) 

它是 [@Controller ](/Controller ) 的增强版。主要用来处理全局数据。 

定义捕获全局异常处理。比如上传文件的功能中，用户上传的文件太大。

```java
@ControllerAdvice
public class CustomExceptionHandler {
//定义类，然后添加 @ControllerAdvice 这个注解就可以了，当启动时，它会被自动扫描到 Spring 容器中，然后在方法添加  @ExceptionHandler 注解
//注解中定义的    MaxUploadSizeExceededException.class 表明这个就去用来处理 MaxUploadSizeExceededException 类型的异常
//如果想让这个方法处理所有类型的异常，只需要将参数改为  Exception 就可以了。
    @ExceptionHandler(MaxUploadSizeExceededException.class)
    public void uploadException(MaxUploadSizeExceededException e, HttpServletResponse resp) throws IOException {
        resp.setContentType("text/html;charset=utf-8");
        PrintWriter out = resp.getWriter();
        out.write("上传文件大小超出限制 ");
        out.flush();
        out.close();
    }
//测试成功
}
```

### 添加全局数据 [@ModelAttribute ](/ModelAttribute ) 

```java
//@ControllerAdvice 是一个全局处理组件
@ControllerAdvice
public class GlobalConfig {
//    value  属性表示返回数据的 key , 而方法的返回值是返回数据的 value
//    在任意的请求的 Controller 中，通过方法参数中的 Model 都可以获取 info 的数据
    @ModelAttribute(value = "info")
    public Map<String, String> userInfo(){
        HashMap<String,String> map = new HashMap<>();
        map.put("username","沈扬");
        map.put("gender","男");
        return  map;
    }
}
```

获取数据

```java
@RestController
public class GetGlobalData {
    @GetMapping("/getglobal")
    @ResponseBody
    public void hello(Model model){
        Map<String, Object> map=model.asMap();
        Set<String> keySet = map.keySet();
        Iterator<String> iterator=keySet.iterator();
        while (iterator.hasNext()){
            String key = iterator.next();
            Object  value= map.get(key);
            System.out.println(key+"     "+value);
        }
    }
}
```

### 自定义错误页面

springboot 中的错误默认是由 BasicErrorController

自定义错误页面很简单：提供 4xx 和 5xx页面即可。

![](image/image_FDV8kMwN9W.png#alt=)

测试成功：

![](image/image_RtlHzgKYFR.png#alt=)

Thymeleaf 页面模板默认处于 classpath:/templates/目录下。

![](image/image_j7rEYswPzg.png#alt=)

我们定义了多个错误页面，响应码.html 的优先级高于  4xx.html 页面的优先级。当前是一个  404 错误，会优先展示 404.html 而不是 4xx.html 更具体的优先级高，动态页面的优先级高于静态页面，也就是  templates 和static 下同时定义了 4xx.html ，优先展示   templates 下面的。

### 自定义 Error 数据

> SpringBoot 默认返回的 Error 信息一共有 5 条：  timestamp,status,error,message,path


### CORS 支持

> w3c 制定的为了解决前端的跨域请求.


配置跨域：一个是直接在相应的请求方法上加注解：

```java
@PostMapping("/")
    @CrossOrigin(value = "http://loaclhost:8080",maxAge = 1800,allowCredentials = "*")
    public String addBook(String name){
        return "receive:"+name;
    }  
    @DeleteMapping("/{id}")
    @CrossOrigin(value = "http://loaclhost:8080",maxAge = 1800,allowCredentials = "*")
    public String deleteBookById(@PathVariable Long id){
        return String.valueOf(id);
    }
//    value 表示支持的域，maxAge 表示探测请求的有效期， allowedHeaders 表示允许的请求头
```

二种是采用全局配置：

```java
public class MyWebMvcConfig implements WebMvcConfigurer {
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/book/**")//对哪种格式的请求路径进行跨域处理
                .allowedHeaders("*")//允许的请求头，默认允许所有
                .allowedMethods("*")//允许的请求方法，默认是 GET  POST HEAD
                .maxAge(1800)//探测请求的有效期
                .allowedOrigins("http://localhost:8080");//支持的域
    }
}
```

自定义类实现 WebMvcConfigurer 接口，实现接口中的 addCorsMappings 方法

#### [@RequestParam ](/RequestParam ) 

这个注解可以将请求参数传递给方法，通过它的 value 属性指定参数名，required 属性指定参数是否必需，默认为 true。如果为 true  请求参数中不包含对应的参数就会抛出异常

```java
public String requestParam(
  @RequestParam(value="loginName") String loginName,
    @RequestParam(value="loginPwd") String loginPwd
){
....
}
```

请求链接：

....?loginName=syhk&loginPwd=123

    

#### [@RequestParam ](/RequestParam ) 和 [@RequestBody ](/RequestBody ) 

[@RequestParam ](/RequestParam ) 它接收的参数是来自 requestHeader 即请求头，通常使用GET 请求 

```java
    public void test1(@RequestParam(value = "name" ,required = true , defaultValue = "syhk") String name){
    //。。。
```

required 表示是否必须默认为 true 必须

defaultValue 设置请求码数的默认值

value 为接收 url 的参数名  

[@RequestBody ](/RequestBody ) 接收的参数是来自 requestBody 中，即请求体。 

#### 数据校验 validator

导入依赖：

```xml
<!--     添加  validator 依赖，做数据校验的   -->
        <dependency>
            <groupId>org.hibernate</groupId>
            <artifactId>hibernate-validator</artifactId>
            <version>8.0.0.Final</version>
        </dependency>
```

使用：

```java
//    数据校验
    @NotEmpty //不能为空
    @Size(min=6,max=20)  //这个代表指定范围最小为 6 最大为 20
    private String name;
    @Range(min = 18, max = 45) //这个 age 元素必须  18 到 45 范围内
    private int age;
    @Email  //被注解的元素必须是电子邮件格式
    @URL //被注解的元素必须是 URL 形式
    @NotBlank //检查被注解的元素是不是 NULL ,以及被去掉前面空格的长度是否大于 0
    @Length(min=10,max=34)  //被注解的元素的字符串大小必须在指定的范围内
    @Null //被注解的元素必须为 NULL
    @NotNull //被注解的元素必须为 非 NULL
    @AssertFalse //必须为 False
    @AssertTrue//必须为 True
    @Min(1) //被注解的元素必须是一个数字，值必须大小等于最小值 @Max
    @Digits(integer = 23, fraction = 0) //被注解的元素必须是一个数字，值必须在可接受范围内
    @Past //被注解的元素必须是一个过去的日期
    @Future //被注解的元素必须是一个将来的日期
//    @Pattern("df")//被注解的元素必须符合指定的正则表达式
    private String email;
```
