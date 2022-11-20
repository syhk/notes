# SpringMVC

开发步骤：

- 
导入 springMVC 相关坐标

- 
配置 SpringMVC 核心控制器 DIspathcerServlet

- 
创建 Controller 类和视图页面

- 
使用注解配置 Controller 类中业务方法的映射地址

- 
配置 SpringMVC核心文件 spring-mvc.xml

- 
客户端发起请求测试


导入SpingMVC依赖：

```xml
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.2.5.RELEASE</version>
    </dependency>

    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>3.1.0</version>
    </dependency>

    <dependency>
      <groupId>javax.servlet.jsp</groupId>
      <artifactId>javax.servlet.jsp-api</artifactId>
      <version>2.3.3</version>
    </dependency>
```

```xml
   <!-- https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api -->
<!-- 添加 servlet 配置-->
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>3.1.0</version>
</dependency>

<!-- 添加 spingmvc 配置-->


<!-- https://mvnrepository.com/artifact/org.springframework/spring-web -->
<!-- spring-web 依赖-->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-web</artifactId>
    <version>5.3.22</version>
</dependency>

    
    
    
 <!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
<!-- spring-webmvc 依赖-->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.3.22</version>
</dependency>


<!-- https://mvnrepository.com/artifact/org.springframework/spring-core -->
<!-- spring-core 依赖-->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-core</artifactId>
    <version>5.3.22</version>
</dependency>
```

> 😄IDEA 每个项目的 maven 都是独立的，需要分别 配置，每次开始都回到 c 盘下面，要配置到 D 盘下面


springmvc 的配置文件：开始模板官方

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">

    
</beans>
```

[@Component ](/Component )   组件 

[@Service ](/Service )       service 

[@Controller ](/Controller )     controller 

[@Repository ](/Repository )     dao 

前端发送数据过来：

如果参数名和传递过来的名字一样可以不用写 @RequestParm("")

```java
@RequestMapping("/hello")
public String hello(String name)
{
return "hello";
}
发起的请求：
http://localhost:8080/hello?name=syhk
```

如果不一样的话需要用 @RequestParm("name") 来指定请求的参数名。

### 数据显示到前端 

- 通过 ModelAndView

```java
/返回一个模型视图对象
        ModelAndView mv = new ModelAndView();
        mv.addObject("msg","ControllerTest1");
        mv.setViewName("test");
        return mv;
```

- 
ModelMap

- 
Model

