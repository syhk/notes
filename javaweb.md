# javaweb

JSP：

B/S：浏览和服务器

C/S：客户端和服务器

可以解决三高问题：高并发、高可用、高性能

Tomcat是Apache软件基金会的akarta项目中的一个核心项目。

Tomcat启动和配置：

启动，关闭：



startup对应的是开启；shutdown对应的是关闭；

访问测试： [http://localhost:8080/](http://localhost:8080/)

可以下载 .exe 文件，下载安装 Tomcat



访问配置可以在这里更改

这里也要同时更改才可以：



#### 发布一个 web 网站

将自己写的网站，放到 Tomcat 指定的 web 应用的文件夹（webapps） 下，就可以访问了

#### HTTP

http(超文本传输协议) 是一个简单的请求、响应协议，它通常运行在 TCP 之上。 默认端口 80

https : 默认端口 443  安全的

> 响应状态码：


- 
200 响应成功

- 
404 找不到资源

- 
5xx 服务器代码错误


### Maven

目前用它来就方便导入 jar 包的！

核心思想：约定大于配置

- 有约束，不要违反

#### 在IDEA中使用 配置 Tomcat





解决警告问题：



上面路径不写，默认访问 localhsot:8080

如果写了 sy  就要访问 localhost:8080/sy



**pom.xml 是 Maven 的核心配置文件**

## Servlet

> 简介：

    	Servlet 就是 sun 公司开发动态 web 的一门技术

		Sun 在这些API 中提供了一个接口叫做： Servlet，如果你想开发一个 Servlet 程序，只需要完成两个小步骤： 编写一个类实现 Servlet 接口；把开发好的 java 类部署到 web 服务器中


**把实现了 Servlet 接口的 java 程序叫做 Servlet**

## HelloServlet

1. 创建一个 Maven 项目，删掉 src 目录，以后就用这个项目里面建立 Moudel；这个空的工程就是 Maven 的主目录

```java
//实现 Servlet 接口
public class helloservelt extends HttpServlet {

//    配置 Tomcat  的时候要注意项目发布的路径

//重写这两个方法 由于 get 或者 post 只是请求实现的不同的方式，可以相互调用，业务逻辑都一样

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        PrintWriter writer = resp.getWriter(); //响应流

        writer.println("Hello Servler!");

    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        super.doPost(req, resp);

    }
}
```

编写Servlet 的映射

```
为什么需要映射：我们写的是 JAVA 程序，但是要通过浏览器访问， 而浏览器需要连接 web 服务器，所以我们需要在 web 服务中注册我们写的 Servlet ，还需要给它一个能够访问的路径
```

web.xml 文件配置：

```xml
<web-app>
  <display-name>Archetype Created Web Application</display-name>






    <!--注册 Servlet-->
  <servlet>
         <!-- Servlet 的请求路径  访问： localhost:8080/hello-->
    <servlet-name>hello</servlet-name>
   
      <!--    一个 servlet-class 对应一个类 -->
    <servlet-class>com.sy.demoSer.HelloServlet</servlet-class>
      <!--如果这个路径不对，就右击标记为路径就可以了-->
  </servlet>
  <servlet-mapping>
      <!--请求映射的路径 也可以使用通配符 eg: *  /hello/*  -->
    <servlet-name>hello</servlet-name>
    <url-pattern>/hello</url-pattern>
  </servlet-mapping>
</web-app>
```

#### Servlet 原理

Servlet 是由 Web 服务器调用， web 服务器在收到浏览器请求后，



### Mapping问题

1. 
一个 Servlet 对应一个映射路径
```xml
  <servlet-mapping>
    <servlet-name>hello</servlet-name>
    <url-pattern>/hello</url-pattern>
  </servlet-mapping>
```


2. 
使用通配符
```xml
  <servlet-mapping>
    <servlet-name>hello</servlet-name>
    <url-pattern>/*</url-pattern>
  </servlet-mapping>
```


3. 
优先级问题：指定了固有的映射路径优先级最高，如果找不到就会走默认的处理请求。


![](image/image_rcEeFvOQ2y.png#alt=)

- 
http1.0

   - 客户端可以与 web 服务器连接后，只能获得一个 web 资源，断开连接，这是短连接
- 
http2.0

   - 客户端可以与 web 服务器连接后，可以获得多个 web 资源 ，这是长连接

![](image/image_vmoiR_0QQX.png#alt=)

- 一个Servlet 可以指定一个映射路径 : web.xml 中

```xml
  <!-- 注册 Servlet  -->
  <servlet>
  <servlet-name>hello</servlet-name>
  <servlet-class>com.syhk.HelloServlet</servlet-class>
  </servlet>
  
  <!-- Servlet 的请求路径 -->
  <servlet-mapping>
  <servlet-name>hello</servlet-name>
  <url-pattern>/hello</url-pattern>
  </servlet-mapping>
```

- 一个 servlet 可以指定多个映射路径

```xml
  <!-- 一个 Servlet 也可以指定多个 映射路径 -->
    <servlet-mapping>
  <servlet-name>hello</servlet-name> 注意上面这个不变
  <url-pattern>/hello1</url-pattern>
  </servlet-mapping>
      <servlet-mapping>
  <servlet-name>hello</servlet-name> 注意上面这个不变
  <url-pattern>/hello2</url-pattern>
  </servlet-mapping>
```

- 一个 servlet 可以配置一个通用映射路径

```xml
  <!-- 一个 servlet 可以指定通用映射路径 -->
       <servlet-mapping>
  <servlet-name>hello</servlet-name>
  <url-pattern>/hello2/*</url-pattern>
  </servlet-mapping>
  <!-- 访问 /hello2/sy 后面可以是任意的 -->
```

- 默认请求路径

```xml
 <!--  默认请求路径 -->
     <servlet-mapping>
  <servlet-name>hello</servlet-name>
  <url-pattern>/*</url-pattern>
  </servlet-mapping>
```

- 指定前缀或者后缀

```xml
<!-- 指定前缀或者后缀 -->
     <servlet-mapping>
  <servlet-name>hello</servlet-name>
  <url-pattern>*.syhk</url-pattern>
  </servlet-mapping>
```

优先级问题： 指定了固有的映射路径优先级最高，如果找不到就会走默认的处理请求。

配置也可以使用注解：

```java
@WebServlet(urlPatterns = "/login")
public class Login  extends HttpServlet{
  }
```

在类上面配置，就是访问地址。

网站是如何进行访问的：

输入一个域名后，先去的本机的 hosts 配置文件下有没有这个域名映射，有返回对应的 ip 地址，可以直接访问；没有去找 DNS 服务器，找到的话就返回，没有找到的话就返回找不到。

响应状态码：

- 
200 请求成功

- 
3xx 请求重定向（就是找新的位置去）

- 
4xx 客户端错误，资源不存在

- 
5xx 服务器错误， 502 网关错误


### 读取资源文件

Properties

- 
在 java 目录下新建 properties

- 
在 resources 目录下新建 properties


都被打包在  classpath: 

### HttpServletResponse

web 服务器针对客户端的 http 请求，分别创建一个代表请求的 HttpServletRequest 对象，和代表响应的一个 HttpServletResponse。

### 下载文件

```java
package com.syhk;

import java.io.FileInputStream;
import java.io.IOException;
import java.net.URLEncoder;

import jakarta.servlet.ServletException;
import jakarta.servlet.ServletOutputStream;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

public class downloadfiletest  extends HttpServlet{
  @Override
  protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
//  要下载文件的路径
    String realPathString = "D:\\日常杂项\\电脑美化\\电脑壁纸\\s10.png";
    System.out.println("下载的文件路径为："+realPathString);
//    下载的文件名是什么
    String fileName =realPathString.substring(realPathString.lastIndexOf("\\")+1);
//    让浏览器能够支持下载我们需要的东西，中文文件名 U  RLEncoder.encode 编码，否则可能会乱码
    resp.setHeader("Content-Dispostion", "attachment;filename="+URLEncoder.encode(fileName,"UTF-8"));
//    获取下载文件的输入流
    FileInputStream inputStream = new FileInputStream(realPathString);
//    创建缓冲区
    int len = 0;
    byte[] buffer = new byte[1024];
//    获取 outputstream 流对象
    ServletOutputStream outputStream = resp.getOutputStream();
//    将 FileOutputStream 流写入到 buffer 缓冲区， 使用 Outputstream 将缓冲区中的数据 输入出客户端;
    while ((len=inputStream.read(buffer))>0) {
      outputStream.write(buffer,0,len);
    }
    inputStream.close();
       outputStream.close();  
  }
  @Override
  protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    doGet(req, resp);
  }
}
```

#### 重定向和转发的区别？

相同点：页面都会实现跳转

不同点：请求转发的时候， url 不会产生变化 

重定向的时候， url 地址栏会发生变化 

```
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>Insert title here</title>
</head>
<body>
<!-- ${pageContext.request.contextPath} 代表当前的项目-->
<!-- 这个路径后面配置的是 servler-mapping 里面的参数 -->

<form action="${pageContext.request.contextPath}/login" method="get">
   uaername：<input type="text" name="username"> <br>
   password：   <input type="password" name="password"><br>
<input type="submit">
</form>

</body>
</html>
```

目录结构：

![](image/image_tBSBh9i0-P.png#alt=)

```java
import jakarta.servlet.ServletException;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

public class Login  extends HttpServlet{
  
  @Override
  protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
//    处理请求
    String username =req.getParameter("username");
    String password =req.getParameter("password");
    System.out.println(username+"  :"+password);
//    重定向
    resp.sendRedirect("pages/success.jsp");
  }
  @Override
  protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    doGet(req, resp);
  }

}
```

### HttpServletRequest

HttpServletRequest 代表客户端的请求，用户通过 Http 协议访问服务器，Http 请求中的所有信息会被封装到 HttpServletRequest ，通过这个它的方法，就可以获得客户端的所有信息

### jsp 中文乱码解决

原来的：

```
<%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    pageEncoding="ISO-8859-1"%>
```

发过后：中文就不会出现乱码了两个地方更改为 utf-8

```
<%@ page language="java" contentType="text/html; charset=utf-8"
    pageEncoding="utf-8"%>
```

```java
package com.syhk;

import java.io.IOException;
import java.util.Arrays;

import org.w3c.dom.ls.LSOutput;

import jakarta.servlet.ServletException;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

public class Login  extends HttpServlet{
  
  @Override
  protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    req.setCharacterEncoding("utf-8");
    resp.setCharacterEncoding("utf-8");
    
    //    处理请求
    System.out.println("========================");
    String username =req.getParameter("username");
    String password =req.getParameter("password");
    String[] hobbyStrings = req.getParameterValues("hobbys");
        
    System.out.println(username+"  :"+password);
    System.out.println(Arrays.toString(hobbyStrings));
    System.out.println("======================");
    
    
    System.out.println(req.getContextPath());
//    通过请求转发
    req.getRequestDispatcher("pages/success.jsp").forward(req, resp);    
    
//    重定向
//    resp.sendRedirect("pages/success.jsp");
  }
  @Override
  protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    doGet(req, resp);
  }

}
```

### Cookie,Session

> 会话： 用户打开一个浏览器，点击了很多网页，访问多个 web 资源，关闭浏览器，这个过程称为会话


> 有状态会话：一个同学来过教室，下次再来教室，我们会知道他，曾经来过，称为有状态会话


#### 保存会话有两种技术

- 
cookie

   - 客户端技术 （响应，请求）
- 
session

   - 服务器技术，利用这个技术，可以保存用户的会话信息，可以把信息或数据放在 session 中

一个 cookie  只能保存一个信息。大小限制  4kb。不设置有效期，关闭浏览器，自动失效。

session:

服务器会每一个用户创建一个 Session 对象

一个 Session 独占一个浏览器，只要浏览器没有关闭，它就存在

用户登录后，整个网站它都可以访问！

它们两个的区别：

Cookie 是把用户的数据写给用户的浏览器，浏览器保存（可以保存多个）

Session 把用户的数据写到用户独占 Session 中，服务器端保存

Session 对象由服务创建，可以保存任意数据

### JSP

> java Server Pages java 服务端页面，用于动态 web 技术


写 jsp 就像在写 html 

区别：

HTML 只给用户提供静态数据

jsp 页面中可以嵌入 java 代码，提供动态数据

jsp 最终会被转换成为一个 java类

JSP 本质就是一个 Servlet

### JSP 基础语法

```
<%@ page language="java" contentType="text/html; charset=utf-8"
    pageEncoding="utf-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="ISO-8859-1">
<title>JSP 语法</title>
</head>
<body>
<%--JSP 表达式  用来将程序的输出,输出到客户端
<%=变量或者表达式 %>
--%>

<%=new java.util.Date() %>

<%-- jsp 脚本片段 --%>
<%
int sum = 0;
for(int i = 1 ; i <=100; i++){
  sum+=i;
}
out.println("<h1>Sum="+sum+"</h1>");
%>

<%--脚本片段 --%>
<%
int x = 10;
out.println(x);
%>
<p>这是一个 JSP 文档</p>
<%
int y = 2;
out.println(y);
%>
<hr>

<%--在代码嵌入 HTML 元素 --%>
<%
for (int i = 0; i < 5 ; i++){
  %>
  <h1>Hello , World <%=i%></h1>
<%
}
%>
<%--JSP 声明 --%>
<%!    //注意这里有个感叹号
static {
  System.out.println("Loading Servlet!"); //输出到控制台
}
private int globalVar = 0;
public void syhk(){
  System.out.println("进入了方法 syhk!");
}
%>
<%-- jsp 声明,会被编译到 jsp 生成的 java 类中,其他的,就会被生成到 jspService 方法中 --%>

<%-- 在 JSP ,嵌入 java 代码即可 
<% %>
<%= %>
<%! %>
--%>
<%--注释 --%>

<%--JSP 指令
<%@page args=%>
<%@include file="" %>
 --%>
 
<%--@include 会将两个页面合二为一 --%>

<%--就是包含其他网页进来,也就点像组件化的利用 --%>
<%@include file="/pages/login.jsp"%>
<h1>网页主体</h1>

<%@include file="/pages/success.jsp"%>
<hr>

<%--jsp 标签
jsp:include :拼接页面,本质还是三个
 --%>
<jsp:include page="/pages/login.jsp"></jsp:include>
<h1>网页主体</h1>
<jsp:include page="/pages/login.jsp"/>

<%--9大内置对象 
PageContext 存东西
Request 存东西
Response
Session 存东西
Application 存东西
config
out
page , 不用了解
exception
pageContext.setAttribute("name1","秦疆1号"); //保存的数据只在一个页面中有效
request.setAttribute("name2","秦疆2号"); //保存的数据只在一次请求中有效，请求转发会携带这个数据
session.setAttribute("name3","秦疆3号"); //保存的数据只在一次会话中有效，从打开浏览器到关闭浏览器
application.setAttribute("name4","秦疆4号");  //保存的数据只在服务器中有效，从打开服务器到关闭服务器
--%>
</body>
</html>
```

### javaBean

有特定的写法：

- 
必须有一个无参构造

- 
属性必须私有化

- 
必须有对应的 get/set 方法


一般用来和数据库的字段做映射 ： ORM；

ORM 对象关系映射：

表 —>  类

字段 —> 属性

行记录 —>  对象

### MVC 三层架构 

Model  View  Controller   模型  视图 控制器

Model

- 
业务处理：业务逻辑（Service)

- 
数据持久层 （Dao)


View

- 
展示数据

- 
提供链接发起 Servlet 请求 (a,form,img..)


Controller (Servlet)

- 
接收用户的请求

- 
交给业务层处理对应的代码

- 
控制视图的跳转


### Filter 过滤器

用来过滤网站的数据；

![](image/image_VYRwOw55Ab.png#alt=)

java 代码：

```java
package com.syhk;

import java.io.IOException;

import jakarta.servlet.FilterChain;
import jakarta.servlet.FilterConfig;
import jakarta.servlet.ServletException;
import jakarta.servlet.ServletRequest;
import jakarta.servlet.ServletResponse;

//实现 Filter 接口
public class Filter implements jakarta.servlet.Filter{

//  初始化 web 服务器启动,就已经初始了,随时等待过滤对象出现 
  @Override
  public void init(FilterConfig filterConfig) throws ServletException {
    System.out.println("Filter 初始化");
  }
  /*
   * 过滤中的所有代码,在过滤特定请求的时候都会执行
   * 必须要让过滤器继续同行
   * */
  @Override
  public void doFilter(ServletRequest arg0, ServletResponse arg1, FilterChain arg2)
      throws IOException, ServletException {
    arg0.setCharacterEncoding("utf-8");
    arg1.setCharacterEncoding("utf-8");
    arg1.setContentType("text/html;charset=UTF-8");
    
    System.out.println("Filter 执行前.....");
    
//     让请求继续走,如果不写,程序到这里就被拦截停止
    arg2.doFilter(arg0, arg1);
    System.out.println("Filter 执行后");
  }
//  销毁:web 服务器关闭的时候,过滤会销毁
  @Override
    public void destroy() {
      System.out.println("Filter 销毁");
    }
//  写完后需要在 xml 中配置 Filter

}
```

xml 配置

```xml
  <!-- 配置 Filter -->
  <filter>
  <filter-name>Filter</filter-name>
  <filter-class>com.syhk.Filter</filter-class>
  </filter>
  <filter-mapping>
  <filter-name>Filter</filter-name>
  <!-- 只要是 /servlet 的任何请求,会经过这个过滤器 -->
  <url-pattern>/servlet/*</url-pattern>
<!-- 所有请求都会经过这个过滤器  <url-pattern>/*</url-pattern> -->
  </filter-mapping>
```

### 监听器

```java
package com.syhk;

import jakarta.servlet.ServletContext;
import jakarta.servlet.http.HttpSessionEvent;
import jakarta.servlet.http.HttpSessionListener;
// 统计网站在线人数: 统计 session
public class CountListener implements HttpSessionListener{

//  创建 Session 监听
//  一旦创建Session 就会触发一次事件
  @Override
  public void sessionCreated(HttpSessionEvent se) {
    ServletContext ctx =  se.getSession().getServletContext();
    
    System.out.println(se.getSession().getId());
    
    Integer onlineCount = (Integer) ctx.getAttribute("OnlineCount");
    
    if(onlineCount==null) {
      onlineCount=new Integer(1);
    }else {
      int count = onlineCount.intValue();
      onlineCount=new Integer(count+1);
    }
    ctx.setAttribute("onlineCount", onlineCount);
  }
  
//  销毁 session 监听
//  一旦销毁 session 就会触发一次这个事件
  @Override
    public void sessionDestroyed(HttpSessionEvent se) {
      ServletContext ctx = se.getSession().getServletContext();
      Integer onlineCount=(Integer) ctx.getAttribute("OnlineCount");
      if(onlineCount==null) {
        onlineCount=new Integer(0);
      }else {
        int count = onlineCount.intValue();
        onlineCount=new Integer(count-1);
      }
      ctx.setAttribute("onlineCount", onlineCount);
    }
  /*
   * Session 销毁
   * 1. 手动销毁 getSession().invalidate();
   * */
//  需要在 xml 注册监听器
  
}
```

xml 注册监听器

```xml
  <!-- 注册监听器 -->
  <listener>
  <listener-class>com.syhk.CountListener</listener-class>
  </listener>
```
