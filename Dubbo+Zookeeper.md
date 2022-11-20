# Dubbo+Zookeeper

[https://dubbo.apache.org/zh/index.html](https://dubbo.apache.org/zh/index.html)

zeekeeper 安装：

[Apache Download Mirrors](https://www.apache.org/dyn/closer.lua/zookeeper/zookeeper-3.4.14/zookeeper-3.4.14.tar.gz)

sudo apt-get install zookeeper

在 bin 目录下启动：

 ./zkServer.sh start

![](image/image_JpB2nldlRH.png#alt=)

启动成功！！！

查看状态

![](image/image_qtppF1O1zY.png#alt=)

配置文件：

![](image/image_3PXxF2u36j.png#alt=)

实现步骤：

- 
创建服务提供者 Prowvider 模块

- 
创建服务消费者 Consumer 模块

- 
在服务提供者模块编写 UserServiceImpl 提供服务

- 
在服务消费者中的 UserController 远程调用 UserServiceImpl 提供的服务

- 
分别启动两个服务，测试


分布式系统：是若干独立计算机的集合，这些计算机对用户来说就像单个相关系统。

![](image/image_Q475OnxEZR.png#alt=)

invoke 调用     subscribe 订阅      async 异步    sync 同步

- 
Provider 服务提供者：暴露服务的服务提供方，服务提供者在启动时，向注册中心注册自己提供的服务

- 
Consumer 服务消费者：调用远程服务的服务消费方，服务消费方在启动时，向注册中心订阅自己所需要的服务，服务消费者，从提供者地址列表中，基于软负载均衡算法，选一台提供者进行调用，如果调用失败，再选另一台调用

- 
Registyr 注册中心：注册中心返回服务提供者地址列表给消费者，如果有变更，注册中心将基于长连接推送变更数据给消费者

- 
Monitor 监控中心：服务消费者和提供者，在内存中累计调用次数和调用时间，定时第分钟发送一次统计数据到监控中心


**dubbo 推荐使用 zookeeper 做为注册中心**

zookeeper官方网：

[https://zookeeper.apache.org/](https://zookeeper.apache.org/)

windows 安装 zookeeper:

![](image/image_SyFOdBRP5i.png#alt=)

解压后运行 /bin/zkServer.cmd ，初次运行可能有闪退或报错问题 **没有zoo.cfg配置文件**

解决：在 zkServer.cmd 文件末尾添加`pause` ，保留窗口，查看错误信息

![](image/image_kvTFwi1Vct.png#alt=)

修改 zoo.cfg 配置文件：

将 conf 文件夹下面的 zoo_sample.cfg 复制一份改名为 zoo.cfg 

![](image/image_JmgUcAoxvT.png#alt=)

打开配置文件：

![](image/image_PzXpv3wFid.png#alt=)

如果需要更改的话，更改完以后重新启动。

遇到问题总结：

出现 找不出类 javaclassnotfound 之类的错误，是由于文件的不完整，并且下载的文件要下载带有 bin 的目录，然后原来用解压缩软件  band 解压出来的不行，不完整，我又用了 winrar 软件进行解压后就没错了。

如果出现 Zookeeper audit is disabled ：

解决打开 zkServer.cmd 脚本：添加红线的命令

[https://blog.csdn.net/u011702673/article/details/109963726](https://blog.csdn.net/u011702673/article/details/109963726)

![](image/image_n590eDYIEw.png#alt=)

启动成功：

![](image/image_y6H6ERdYVt.png#alt=)

使用 zkCli.cmd 测试

`ls /` 列出 zookeeper 根下保存的所有节点

![](image/image_gzlGg8oPu6.png#alt=)

`help` 查看帮助

创建一个 syhk 节点，值为 123

![](image/image_bPzto5yrWG.png#alt=)

获取节点的值

![](image/image_iuU0NYuXvL.png#alt=)

再次查看节点就有一个节点了：

![](image/image_783tp7xkUG.png#alt=)

### windows 下安装 dubbo-admin

下载：

[https://www.apache.org/dyn/closer.lua/dubbo/dubbo-admin/0.4.0/apache-dubbo-admin-0.4.0-source-release.zip](https://www.apache.org/dyn/closer.lua/dubbo/dubbo-admin/0.4.0/apache-dubbo-admin-0.4.0-source-release.zip)

dubbo 本身并不是一个服务软件。它就是一个 jar 包，能帮助我们的 java 程序连接到 zookeeper，并利用 zookeeper 消费，提供服务

dubbo-admin 是官方提供的一个可视化的监控程序.

解压进入目录：

D:\myapp\apache-dubbo-admin-0.4.0-source-release\dubbo-admin-server\src\main\resources

修改指定 zookeeper 地址

![](image/image_cwoU_-1-bA.png#alt=)

配置

```
admin.registry.address=zookeeper://127.0.0.1:2181


#==========================配置==========================
server.port=7001
spring.velocity.cache=false
spring.velocity.charse=UTF-8
spring.velocity.layout-url=/templates/default.vm
spring.messages.fallback-to-system-locale=false
spring.messages.basename=i18n/message
spring.root.password=root
spring.guest.password=guest
```

在项目目录下打包 dubbo-admin 

![](image/image_r9LZwsSCS_.png#alt=)

这个目录下执行：`mvn clean package`

第一次打包时间可能有点长，亲自验证时间确实很长

![](image/image_vTUyPb7hPT.png#alt=)

成功！！！

执行  dubbo-admin-server\target 目录下的dubbo-admin-server-0.4.0.jar

`java -jar dubbo-admin-server-0.4.0.jar`

**zookeeper的服务一定要打开**

![](image/image_7qlctbWfYj.png#alt=)

启动好后访问 ： 

localhost:7001   我的到这里有错误，无法访问那个页面

### SpringBoot+Dubbo+zookeeper

框架搭建：

- 
启动 zookeeper

- 
IDEA 创建一个空项目

- 
创建一个模块，实现服务提供者：provider-server，选择web 依赖

- 
项目创建完毕，写一个服务


#### 服务提供者

dubbo 依赖：

```xml
<!-- https://mvnrepository.com/artifact/org.apache.dubbo/dubbo-spring-boot-starter -->
<dependency>
    <groupId>org.apache.dubbo</groupId>
    <artifactId>dubbo-spring-boot-starter</artifactId>
    <version>3.1.0</version>
</dependency>
```

zkclient依赖：

```xml
   <!-- https://mvnrepository.com/artifact/com.github.sgroschupf/zkclient -->
        <dependency>
            <groupId>com.github.sgroschupf</groupId>
            <artifactId>zkclient</artifactId>
            <version>0.1</version>
        </dependency>
```

zookeeper依赖：

```xml
<!--        引入 zookeeper-->
        <dependency>
            <groupId>org.apache.curator</groupId>
            <artifactId>curator-framework</artifactId>
            <version>5.3.0</version>
        </dependency>
        <dependency>
            <groupId>org.apache.flink</groupId>
            <artifactId>flink-shaded-curator-recipes</artifactId>
            <version>1.3.3</version>
        </dependency>


        <!-- https://mvnrepository.com/artifact/org.apache.zookeeper/zookeeper -->
        <dependency>
            <groupId>org.apache.zookeeper</groupId>
            <artifactId>zookeeper</artifactId>
            <version>3.7.1</version>
<!--            排除这个 slf4j-log4j12-->
            <exclusions>
                <exclusion>
                    <groupId>org.slf4j</groupId>
                    <artifactId>slf4j-log4j12</artifactId>
                </exclusion>

            </exclusions>
        </dependency>
```

服务提供者的配置文件里面添加

```xml

#配置 dubbo 相关属性
#当前应用名字
dubbo.application.name=dubbotest

#注册中心地址
dubbo.registry.address=zookeeper://127.0.0.1:2181

#扫描指定包下服务
dubbo.scan.base-packages=com.example.dubbotest.service
```

实现类中配置配置服务注解：发布服务

```java
package com.example.dubbotest.service;
import org.springframework.stereotype.Component;
import org.springframework.stereotype.Service;
@Service  //将服务发布出去
@Component  //放在容器中
public class TicketServiceImpl implements TickService{
    @Override
    public String getTicket() {
       return "SYHK";
    }
}
```

应用起动起来，dubbo 就会自动扫描指定的包下带有 [@Component ](/Component ) 注解的服务，将它发布在指定的注册中心 

#### 消费者

导入依赖，和之前的依赖一样：

```xml
    <!-- https://mvnrepository.com/artifact/org.apache.dubbo/dubbo-spring-boot-starter -->
        <dependency>
            <groupId>org.apache.dubbo</groupId>
            <artifactId>dubbo-spring-boot-starter</artifactId>
            <version>3.1.0</version>
        </dependency>



        <!-- https://mvnrepository.com/artifact/com.github.sgroschupf/zkclient -->
        <dependency>
            <groupId>com.github.sgroschupf</groupId>
            <artifactId>zkclient</artifactId>
            <version>0.1</version>
        </dependency>


        <!--        引入 zookeeper-->
        <dependency>
            <groupId>org.apache.curator</groupId>
            <artifactId>curator-framework</artifactId>
            <version>5.3.0</version>
        </dependency>
        <dependency>
            <groupId>org.apache.flink</groupId>
            <artifactId>flink-shaded-curator-recipes</artifactId>
            <version>1.3.3</version>
        </dependency>


        <!-- https://mvnrepository.com/artifact/org.apache.zookeeper/zookeeper -->
        <dependency>
            <groupId>org.apache.zookeeper</groupId>
            <artifactId>zookeeper</artifactId>
            <version>3.7.1</version>
            <!--            排除这个 slf4j-log4j12-->
            <exclusions>
                <exclusion>
                    <groupId>org.slf4j</groupId>
                    <artifactId>slf4j-log4j12</artifactId>
                </exclusion>

            </exclusions>
        </dependency>
```

配置

```xml
# 消费者配置
dubbo.application.name=consumer-server

#注册中心地址
dubbo.registry.address=zookeeper://127.0.0.1:2181
```

正常的步骤是需要将服务提供者的接口打包，然后用 pom 文件导入，这里使用简单的方式，直接将服务的接口拿过来，路径必须保证正确，就是和服务提供者相同

官方网教程地址：

[https://dubbo.apache.org/zh/docs3-v2/java-sdk/quick-start/spring-boot/](https://dubbo.apache.org/zh/docs3-v2/java-sdk/quick-start/spring-boot/)

[https://dubbo.apache.org/zh/docs3-v2/java-sdk/reference-manual/config/annotation/](https://dubbo.apache.org/zh/docs3-v2/java-sdk/reference-manual/config/annotation/)

```java
@DubboService 注解：@Service 注解从 3.0 开始已经废弃，改用这个，用以区别于
 Spring 的 @Service
定义好 Dubbo 服务接口后，提供服务接口的实现逻辑，并且 @DubboService 注解标记，
就可以达到 dubbo 的服务暴露

@DubboReference 注解： @Reference从 3.0 开始弃用，改用这个，以区别 Spring 的
@Reference 注解；这个注解自动注入为 Dubbo 服务代理实例

@EnableDubbo 注解：这个注解必须配置，否则无法加载Dubbo 注解定义的服务，
它可以定义在主类上
@SpringBootApplication
@EnableDubbo
public class ProviderApplication {
    public static void main(String[] args) throws Exception {
        SpringApplication.run(ProviderApplication.class, args);
    }
}
Spring Boot 注解默认只会扫描 main 类所有的 package 如果服务定义在其它 package中，
需要增加配置 EnableDubbo(scanBasePackages ={"...路径"})
```

### RPC

[https://en.wikipedia.org/wiki/Remote_procedure_call](https://en.wikipedia.org/wiki/Remote_procedure_call)

> 远程过程调用，是一种进程间通信方式。它允许程序调用另一个地址空间（通常是共享网络的另一台机器上）的过程或函数。而不用程序员显式编码这个远程调用的细节。

