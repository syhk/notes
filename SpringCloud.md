

### 服务拆分与远程调用
#### 服务拆分注意事项

- 不同微服务，不要重复开发相同业务（单一职责）
- 微服务数据独立，不要访问其它微服务的数据库
- 微服务将自己的部分业务暴露为接口，供其它微服务调用

远程调用方式分析

使用 Resttemplate 发起get 或 post 请求
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668069934420-61e6bffd-bdbe-4654-beeb-a9056c88d026.png#averageHue=%23dfd9c7&clientId=u630c4241-c187-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=37&id=u4c21b776&margin=%5Bobject%20Object%5D&name=image.png&originHeight=46&originWidth=580&originalType=binary&ratio=1&rotation=0&showTitle=false&size=27176&status=done&style=none&taskId=u980bcdfb-0ab6-46f7-9329-a9f67d83bc9&title=&width=464)
入参：请求地址，返回的类型

### 提供者与消费者

- 服务提供者：提供接口给其它微服务
- 服务消费者：调用其它微服务提供的接口

提供者和消费者角色是相对的。
一个服务可以同时是服务提供者也可以服务消费者
#### Eureka 注册中心
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668070579957-fdf84ccf-af69-4e90-8a02-d135cc15eb87.png#averageHue=%23ece4d2&clientId=u630c4241-c187-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=340&id=ued644391&margin=%5Bobject%20Object%5D&name=image.png&originHeight=425&originWidth=807&originalType=binary&ratio=1&rotation=0&showTitle=false&size=101568&status=done&style=none&taskId=udad21dee-0540-43b1-8615-31e1d70c722&title=&width=645.6)

- 消费者如何获取服务提供者具体信息？
   - 服务提供者启动时向 eureka注册自己的信息
   - eureka 保存这些信息
   - 消费者根据服务名称向 eureka拉取提供者信息’
- 有多个服务提供者，消费者该如何选择？
   - 服务消费者利用负载均衡算法，从服务列表中挑选一个
- 消费如何感知服务提供者健康状态？
   - 服务提供者会每隔 30 秒向 EurekaServer 发送心跳请求，报告健康状态
   - eureka 会更新记录服务列表信息，心跳不正常会被剔除
   - 消费者就可以接取到最新的信息
#### eureka服务
pom.xml 导入依赖:
```xml
  <!--eureka服务端-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
        </dependency>
```
在springboot 启动类上加 `@EnableEurekaServer` 开启自动配置
编写配置文件：
```yaml
server:
  port: 10086 # 服务端口
# 为了做服务注册才配置的信息
spring:
  application:
    name: eurekaserver # eureka的服务名称
eureka:
  client:
    service-url:  # eureka的地址信息
      defaultZone: http://127.0.0.1:10086/eureka

```
[http://localhost:10086/](http://localhost:10086/) 进入地址查看详细信息
注册到 eureka 的服务（实例）：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668079852193-53c2adae-5f44-47fc-95c7-fa32c0691c2d.png#averageHue=%23a09283&clientId=u630c4241-c187-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=142&id=u302e4cdf&margin=%5Bobject%20Object%5D&name=image.png&originHeight=177&originWidth=923&originalType=binary&ratio=1&rotation=0&showTitle=false&size=37513&status=done&style=none&taskId=u53e22476-34ae-4583-aab1-0e8de7f23c9&title=&width=738.4)
#### 服务注册
步骤：

- 引入依赖
```xml
<!--eureka客户端依赖-->
<dependency>
  <groupId>org.springframework.cloud</groupId>
  <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```

- 加配置：配置 eureka 地址
```yaml
application:
	name: userservice # user 服务的名称
eureka:
  client:
    service-url:  # eureka的地址信息
      defaultZone: http://127.0.0.1:10086/eureka
```

![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668080604383-ce8694ce-7b06-40d9-af4d-6ecee3e85854.png#averageHue=%23aa9b8b&clientId=u630c4241-c187-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=151&id=u4706c757&margin=%5Bobject%20Object%5D&name=image.png&originHeight=189&originWidth=957&originalType=binary&ratio=1&rotation=0&showTitle=false&size=31787&status=done&style=none&taskId=u0bcf8c52-4caf-47ea-9215-47287d8324b&title=&width=765.6)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668080622801-d09a2f75-52ce-4c4f-9aa8-620268c37cef.png#averageHue=%23527c56&clientId=u630c4241-c187-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=118&id=u23d3dc39&margin=%5Bobject%20Object%5D&name=image.png&originHeight=147&originWidth=531&originalType=binary&ratio=1&rotation=0&showTitle=false&size=32925&status=done&style=none&taskId=uef743717-9468-4208-ad97-735dfde3bc7&title=&width=424.8)
三个服务已经全部成功注册！
一个服务启动多个实例：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668080683463-50357440-7006-4aba-9973-8cbcfc00b056.png#averageHue=%23e2cfbd&clientId=u630c4241-c187-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=264&id=u36a1a2ec&margin=%5Bobject%20Object%5D&name=image.png&originHeight=330&originWidth=866&originalType=binary&ratio=1&rotation=0&showTitle=false&size=96436&status=done&style=none&taskId=u9b647c16-4606-43fe-b6f5-adca1a847c4&title=&width=692.8)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668080818177-6ba21cd0-2c4e-4f99-be73-72edce717d27.png#averageHue=%23343637&clientId=u630c4241-c187-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=606&id=u675b43dd&margin=%5Bobject%20Object%5D&name=image.png&originHeight=757&originWidth=922&originalType=binary&ratio=1&rotation=0&showTitle=false&size=99812&status=done&style=none&taskId=ued8132a7-2c2a-45c0-9fff-7aac1dbf497&title=&width=737.6)![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668080828952-faf28f36-9171-42f4-be8f-2eb89c3299b4.png#averageHue=%233c4040&clientId=u630c4241-c187-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=114&id=ue6597a3c&margin=%5Bobject%20Object%5D&name=image.png&originHeight=142&originWidth=359&originalType=binary&ratio=1&rotation=0&showTitle=false&size=21481&status=done&style=none&taskId=uf7f0fed3-ddd5-41a0-8fda-e683c7ed476&title=&width=287.2)
成功注册了两个实例：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668080860549-c476e63f-8eed-423f-b604-bdd08b1222dd.png#averageHue=%23a99a8a&clientId=u630c4241-c187-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=188&id=ud08f5f73&margin=%5Bobject%20Object%5D&name=image.png&originHeight=235&originWidth=927&originalType=binary&ratio=1&rotation=0&showTitle=false&size=50355&status=done&style=none&taskId=u5ebc3f41-6124-440d-a012-f7c5d0368f0&title=&width=741.6)

#### 服务发现与拉取

- 修改Service 代码，修改访问的路径，用服务名代替 ip、端口：
```java
// url路径
String url = "http://userservice/user/" + order.getUserId();
```

- 在项目的启动类中的 `RestTemplate`添加负载均衡注解：
```java
@Beam
@LoadBalanced
public RestTemplate restTemplate(){
return new RestTemplate();
}
```

```java
@Autowired
    private RestTemplate restTemplate;

public Order queryOrderById(Long orderId) {
// 1.查询订单
Order order = orderMapper.findById(orderId);
// 2.利用RestTemplate发起http请求，查询用户
// 2.1.url路径
String url = "http://userservice/user/" + order.getUserId();
// 2.2.发送http请求，实现远程调用
User user = restTemplate.getForObject(url, User.class);//get请求
// 3.封装user到Order
order.setUser(user);
// 4.返回
return order;
}
```

### Ribbon负载均衡
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668081655820-ca43218d-6236-4cce-8fcc-96b7c6ea019a.png#averageHue=%23ead3c1&clientId=u630c4241-c187-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=302&id=u204b4f76&margin=%5Bobject%20Object%5D&name=image.png&originHeight=377&originWidth=833&originalType=binary&ratio=1&rotation=0&showTitle=false&size=47323&status=done&style=none&taskId=u555663a9-5d74-4c62-92cd-873e2891606&title=&width=666.4)
源码大致流程：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668082270354-edd40629-87e8-404b-94bc-d3b53df6e51d.png#averageHue=%23e8d2c0&clientId=u630c4241-c187-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=286&id=ub9ea2ace&margin=%5Bobject%20Object%5D&name=image.png&originHeight=357&originWidth=803&originalType=binary&ratio=1&rotation=0&showTitle=false&size=85355&status=done&style=none&taskId=ueb9845c5-63c4-467c-8e14-e6e3116588f&title=&width=642.4)
Ribbon 的负载均衡规则是叫 `IRule` 接口来定义的，每一个子接口都是一种规则：
eg: RoundRobinRule 轮询   RandomRule 随机
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668082393695-227fed41-82f4-4e95-8d7c-226d4b5ee81d.png#averageHue=%23ddcfb2&clientId=u630c4241-c187-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=207&id=uf4c73969&margin=%5Bobject%20Object%5D&name=image.png&originHeight=259&originWidth=705&originalType=binary&ratio=1&rotation=0&showTitle=false&size=72753&status=done&style=none&taskId=u3dfa54c6-5ade-4b93-a4ae-cd6b14ce9f9&title=&width=564)
常见策略：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668082473303-32b03ffb-037b-4f8a-bbcd-13f8ce279c57.png#averageHue=%23d5b6a7&clientId=u630c4241-c187-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=294&id=u9ffcedd5&margin=%5Bobject%20Object%5D&name=image.png&originHeight=367&originWidth=763&originalType=binary&ratio=1&rotation=0&showTitle=false&size=177908&status=done&style=none&taskId=u0d4051f3-1d78-44d9-8608-046c73d7d81&title=&width=610.4)

修改负载均衡策略：
第一种：单独写一个配置类或者是在启动类中注入一个 Bean （全局的）
```java
//    通过 IRule 修改负载均衡策略
    @Bean
    public IRule randomRule() {
        return new RandomRule();
    }
```
第二种：配置文件方式，针对某个微服务，而不是全局
```yaml
userservice:
ribbon:
NFLoadBalancerRuleClassName: com.alibaba.cloud.nacos.ribbon.NacosRule  # 负载均衡规则
```
### 饥饿加载
Ribbon 默认采用懒加载，即第一次访问时才会去创建 LoadBalanceClient ， 请求时间会很长。
而饥饿加载则会在项目启动时创建，降低第一次访问的耗时
```yaml
ribbon:
  eager-load:
    enabled: true # 开启饥饿加载
    clients: # 指定饥饿加载的服务名称 ( 是一个集合，只有一个可以直接写在后面，有多个需要换行写）
      - userservice
```

## Nacos 注册中心
> 是阿里巴巴的产品，是 springCloud 中的一个组件，功能比 Eureka 功能更加丰富


![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668083891788-01c1c635-8818-464b-b59f-aafa74f8885a.png#averageHue=%23faf9f8&clientId=u630c4241-c187-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=391&id=u23ede13d&margin=%5Bobject%20Object%5D&name=image.png&originHeight=489&originWidth=679&originalType=binary&ratio=1&rotation=0&showTitle=false&size=69976&status=done&style=none&taskId=ua3a4d00f-c64d-404d-a9db-a92373b18eb&title=&width=543.2)

`startup.cmd -m standalone`启动 （这里是单机启动）
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668083955005-6d49a40a-0faa-4446-8f2a-cb142c84c363.png#averageHue=%232c3038&clientId=u630c4241-c187-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=418&id=u173ebcd6&margin=%5Bobject%20Object%5D&name=image.png&originHeight=522&originWidth=1017&originalType=binary&ratio=1&rotation=0&showTitle=false&size=58196&status=done&style=none&taskId=u3880d290-d645-4bcf-956c-cf0d7bb41f3&title=&width=813.6)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668084077842-fa18b2f5-6cf8-43a6-afd0-448f1680c0e8.png#averageHue=%23353430&clientId=u630c4241-c187-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=430&id=u3ad5ca82&margin=%5Bobject%20Object%5D&name=image.png&originHeight=537&originWidth=1898&originalType=binary&ratio=1&rotation=0&showTitle=false&size=135771&status=done&style=none&taskId=ue0618812-a6a3-4d05-8c12-e01dec871ed&title=&width=1518.4)
需要登录：默认用户名和密码都是 `nacos`

spring alibaba 的管理依赖
在父工程的依赖中：
```xml
<dependency>
  <groupId>com.alibaba.cloud</groupId>
  <artifactId>spring-cloud-alibaba-dependencies</artifactId>
  <version>2.2.5.RELEASE</version>
  <type>pom</type>
  <scope>import</scope>
</dependency>
```
nacos 依赖
```xml
<!-- nacos客户端依赖包 -->
<dependency>
  <groupId>com.alibaba.cloud</groupId>
  <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
</dependency>
```
修改配置文件：
```yaml
 cloud:
    nacos:
      server-addr: nacos:8848 # nacos服务地址
      discovery:
        namespace: 4d6ce343-9e1b-44df-a90f-2cf2b6b3d177 # dev环境
        ephemeral: false # 是否是临时实例
```

### 服务分级存储模型
服务调用尽可能选择本地集群的服务，跨集群调用延迟较高
本地集群不可访问时，再去访问其它集群
#### 根据权重负载均衡
Nacos 控制台可以设置实例的权重值， 0 ~ 1 之间
同集群内的多个实例，权重越高被访问的频率越高
权重设置为 0 则完全不会被访问
#### 环境隔离
Nacos 中服务存储和数据存储的最外层都是一个名为 namespace  的东西，用来做最外层隔离
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668173890085-5d879324-c73c-4414-ad29-840504dbaead.png#averageHue=%23e6d1c0&clientId=ua1de23b4-71d9-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=273&id=u9568ed19&margin=%5Bobject%20Object%5D&name=image.png&originHeight=341&originWidth=622&originalType=binary&ratio=1&rotation=0&showTitle=false&size=48862&status=done&style=none&taskId=u710915f2-252e-4801-b85e-b74a0f1a24d&title=&width=497.6)
修改项目配置文件:
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668173943604-2ee5b777-5210-4afb-9bec-0ce862de86a0.png#averageHue=%232a2b32&clientId=ua1de23b4-71d9-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=68&id=ue6a1acd2&margin=%5Bobject%20Object%5D&name=image.png&originHeight=85&originWidth=749&originalType=binary&ratio=1&rotation=0&showTitle=false&size=14897&status=done&style=none&taskId=u380e19da-0380-4fcf-9b33-b4d1b1c1a75&title=&width=599.2)
每个 namespace 都有唯一的 id
不同 namespace 下的服务是不可见的

### Nacos 和 Eureka 的区别
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668174493831-c6e82c21-d696-48da-b8c7-8b6019ce60c1.png#averageHue=%23ead5c2&clientId=ua1de23b4-71d9-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=330&id=u94198ea4&margin=%5Bobject%20Object%5D&name=image.png&originHeight=412&originWidth=759&originalType=binary&ratio=1&rotation=0&showTitle=false&size=78028&status=done&style=none&taskId=u83e4b741-6cd1-47ee-ab44-9d8efa9e090&title=&width=607.2)
临时和非临时实例： `ephemeral: true/false` 来配置是否是临时实例
临时实例：服务不健康了或者是宕机了就会被直接删除
非临时实例：不会被删除，会等着服务恢复，除非手动删除
共同点：

- 都支持服务注册和服务拉取
- 都支持服务提供者心跳方式做健康检测

区别：

- Nacos 支持服务端主动检测提供者状态：**临时实例**采用心中模式，非临时实例采用主动检测模式
- 临时实例心跳不正常会被剔除，非临时实例则不会被剔除
- Nacos 支持服务列表变更的消息推送模式，服务列表更新更及时
- Nacos 集群默认采用 AP 方式，当集群中存在非临时实例时，采用 CP 模式；Eureka 采用 AP 方式

### Nacos 配置管理
统一配置管理：

- 配置更改热更新



- 配置获取的步骤：

引入 Nacos 的配置管理客户端依赖：
```xml
<!--nacos的配置管理依赖-->
<dependency>
  <groupId>com.alibaba.cloud</groupId>
  <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
</dependency>
```
在项目的配置目录下新建一个 bootstrap.yml 文件，这个文件就是引导文件，优先级高于 application.yml
配置自动刷新（微服务热更新）

- 方式一：在 @Value 注入的变量所在类上添加注解 `@RefreshScope`
   - ![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668254143034-1ff20eed-2bb6-49b6-a1e8-ceb4b5bd5a12.png#averageHue=%23dfceb6&clientId=ua58e7179-9ab0-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=182&id=uad9d1bd5&margin=%5Bobject%20Object%5D&name=image.png&originHeight=228&originWidth=636&originalType=binary&ratio=1&rotation=0&showTitle=false&size=66360&status=done&style=none&taskId=uc539ca05-706f-42a0-ad11-dbc1b2327fc&title=&width=508.8)
- 方式二：使用 `@ConfigurationProperties(prefix= "pattern")`
```java
@Data
    @Component
    @ConfigurationProperties(prefix = "pattern")
    public class PatternProperties {
        private String dateformat;
    }
```

多环境配置共享
多种配置的优先级： 服务名-profile.yaml ----> 服务名称.yaml ----> 本地配置
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668255106862-3e52e48f-a4f7-4230-aa12-deea60e23190.png#averageHue=%23e7c4aa&clientId=ua58e7179-9ab0-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=178&id=ufb75766b&margin=%5Bobject%20Object%5D&name=image.png&originHeight=222&originWidth=519&originalType=binary&ratio=1&rotation=0&showTitle=false&size=34878&status=done&style=none&taskId=ue95e4e5c-4c26-4299-a975-f6a8d6da320&title=&width=415.2)
nacos 集群搭建
 
> 启动 nacos 要用 cmd startup.cmd -m standalone 启动，直接打开脚本就会启动失败！！！
> 其实这里是设置单机启动，nacos 默认是集群启动





### Feign 远程调用
> Feign 替代 RestTemplate，帮我们优雅实现 http 请求的发送

1. 依赖
```xml
<dependency>
  <groupId>org.springframework.cloud</groupId>
  <artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
```

2. 启动类添加注解 `@EnableFeignClients`
3. 编写接口类
```java
@FeignClient("userservice")
    public interface UserClient {
        @GetMapping("/user/{id}")
        User findById(@PathVariable Long id);
    }

```
service 使用：
```java
@Service
    public class OrderService {
        @Autowired
        private UserClient userClient;
        @Autowired
        private OrderMapper orderMapper;

        public Order queryOrderById(Long orderId) {
            Order order = orderMapper.findById(orderId);
            //        用 Feign 发起远程调用
            User user= userClient.findById(order.getUserId());

            order.setUser(user);
            // 4.返回
            return order;
        }
```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668433245820-c52f8704-ae2e-42b2-829c-8241fae4fa3c.png#averageHue=%23373a3b&clientId=u6629c9de-2f4b-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=578&id=uff4db9e5&margin=%5Bobject%20Object%5D&name=image.png&originHeight=722&originWidth=298&originalType=binary&ratio=1&rotation=0&showTitle=false&size=53915&status=done&style=none&taskId=u64274539-7471-4b2d-bafc-0250554a0d0&title=&width=238.4)
自定义 feign 配置
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668434173817-6837ad54-e1cb-4194-bddf-dbc72b70d66f.png#averageHue=%23d6b8a9&clientId=u6629c9de-2f4b-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=291&id=u8c2d0649&margin=%5Bobject%20Object%5D&name=image.png&originHeight=364&originWidth=795&originalType=binary&ratio=1&rotation=0&showTitle=false&size=107863&status=done&style=none&taskId=uc5b0e973-a02b-447c-aaec-08c4f57226b&title=&width=636)
yaml 配置文件配置：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668434255504-e8202d07-a936-4a48-9544-03593d98df2a.png#averageHue=%23e5d4bd&clientId=u6629c9de-2f4b-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=322&id=u5b7eb922&margin=%5Bobject%20Object%5D&name=image.png&originHeight=402&originWidth=800&originalType=binary&ratio=1&rotation=0&showTitle=false&size=64054&status=done&style=none&taskId=uc4ecfaf6-638f-4e7b-ae11-11ac98401a9&title=&width=640)
java 配置类配置：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668434277644-ae7feeb6-c77d-48cf-9766-7fb98ac0f739.png#averageHue=%23e2d1bb&clientId=u6629c9de-2f4b-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=250&id=u78b4dc79&margin=%5Bobject%20Object%5D&name=image.png&originHeight=312&originWidth=787&originalType=binary&ratio=1&rotation=0&showTitle=false&size=84591&status=done&style=none&taskId=u8c88767b-9055-4002-bf5b-e80e17e00a2&title=&width=629.6)
Feign 的性能优化-连接池配置
添加依赖：
```xml
<!-- https://mvnrepository.com/artifact/org.apache.httpcomponents/httpclient -->
<dependency>
    <groupId>org.apache.httpcomponents</groupId>
    <artifactId>httpclient</artifactId>
    <version>4.5.13</version>
</dependency>

```
配置文件配置：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668434418322-bc91acdd-557c-42e7-9a79-962716cdb249.png#averageHue=%23e1d2ba&clientId=u6629c9de-2f4b-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=133&id=u24fd5fb3&margin=%5Bobject%20Object%5D&name=image.png&originHeight=166&originWidth=798&originalType=binary&ratio=1&rotation=0&showTitle=false&size=48092&status=done&style=none&taskId=uf9a7de91-ce69-4d60-ae22-7e2eab3fc5c&title=&width=638.4)
feign 最佳实践：

- 首先创建一个 module，可以命名为 feign-api ， 然后引入  feign 的依赖，将 feign 相关的全部存放 feign 模块中。

![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668435199837-16cd52fb-e73e-44ae-930d-8fcd5fb78c07.png#averageHue=%23e5d2be&clientId=u6629c9de-2f4b-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=210&id=uc803aca8&margin=%5Bobject%20Object%5D&name=image.png&originHeight=263&originWidth=820&originalType=binary&ratio=1&rotation=0&showTitle=false&size=53577&status=done&style=none&taskId=ud61392cc-e8da-4415-aa9c-9e0ba4f9eab&title=&width=656)
### Gateway服务网关（统一网关）
步骤：

1. 创建新的 module ， 引入 SpringCloudGateway 的依赖和 nacos 的服务发现依赖
```xml
<dependency>
  <groupId>org.springframework.cloud</groupId>
  <artifactId>spring-cloud-starter-gateway</artifactId>
</dependency>
```
2.编写路由配置及 nacos 地址
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668515188976-1431693c-9b6a-43fd-beaf-398f800148ae.png#averageHue=%23ead6c4&clientId=ua1014a65-e4f0-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=266&id=ubbcdd6ba&margin=%5Bobject%20Object%5D&name=image.png&originHeight=333&originWidth=503&originalType=binary&ratio=1&rotation=0&showTitle=false&size=72814&status=done&style=none&taskId=ud99eba42-4151-48ea-a3f8-3543b26cb8f&title=&width=402.4)
```yaml
server:
  port: 10010
spring:
  application:
    name: gateway
  cloud:
    gateway:
      routes:
        - id: user-service
          uri: lb://userservice
          predicates:
            - Path=/user/**
        - id: order-service
          uri: lb://orderservice
          predicates:
            - Path=/or
```
访问userservice 服务： http://localhost:10010/user/2 它会路由到 userservice 服务
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668515974846-44ebe290-8a4e-4bd5-8025-eaf819ab656a.png#averageHue=%23e8cfbd&clientId=ua1014a65-e4f0-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=315&id=u11e9177b&margin=%5Bobject%20Object%5D&name=image.png&originHeight=394&originWidth=712&originalType=binary&ratio=1&rotation=0&showTitle=false&size=65666&status=done&style=none&taskId=u302228ea-bc27-45d4-9ec5-acd4990b279&title=&width=569.6)
路由配置：

1. 路由id: 路由的唯一标示
2. 路由目标(uri) ：路由的目标地址， http 代表固定地址， lb 代表根据服务名负载均衡
3. 路由断言(predicates)：判断路由的规则 
4. 路由过滤器(filters)：对请求或响应做处理

##### 路由断言工厂 Route Predicate Factory
spring 提供了 11 种基本的 Predicate 工厂：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668516291033-0fe649e6-5fec-46d9-804e-825c294f4de0.png#averageHue=%23bfa699&clientId=ua1014a65-e4f0-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=243&id=uf000e606&margin=%5Bobject%20Object%5D&name=image.png&originHeight=304&originWidth=714&originalType=binary&ratio=1&rotation=0&showTitle=false&size=112627&status=done&style=none&taskId=ud9800a3a-4404-4112-8836-83b2de83455&title=&width=571.2)

spring 提供 30多种路由过滤器配置，用到的时候再去看官方文档。

全局过滤器（GlobalFilter) ：处理一切进入网关的请求和微服务响应，与 GatewayFilter 的作用一样。区别在于：GatewayFilter 通过配置定义，处理逻辑是固定的。而 GlobalFilter 的逻辑需要自己写代码实现。
步骤：

- 实现 GlobalFilter 接口
- 添加 @Order注解或实现 Ordered 接口
- 编写处理逻辑
```java
//@Order(-1) //当有多个过滤器，通过配置优先级，值越小优先级越高
@Component
public class MyGlobalFilter implements GlobalFilter, Ordered {
    @Override
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
//      1.  获取请求参数
        ServerHttpRequest request = exchange.getRequest();
        MultiValueMap<String,String> params = request.getQueryParams();
//       2. 获取参数中的 authorization 参数
        String auth = params.getFirst("authorization");
//       3. 判断参数值是否等于 admin
        if("admin".equals(auth))
        {
//      4.  是放行
            chain.filter(exchange);
        }
//        设置状态码  401 未登录
        exchange.getResponse().setStatusCode(HttpStatus.UNAUTHORIZED);

//      5.  否拦截
        return exchange.getResponse().setComplete();
    }
//    也可以实现接口，通过返回参数进行配置
    @Override
    public int getOrder() {
        return 0;
    }
}
```

请求路由后，会将当前路由过滤器和  DefaultFilter ，GlobalFilter 合并到一个过滤器链（集合）中
，排序依次执行每个过滤器
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668517667945-573cec4a-1b71-4fee-be5f-cd19a33d59b0.png#averageHue=%23e8c9b8&clientId=ua1014a65-e4f0-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=240&id=u2e472b41&margin=%5Bobject%20Object%5D&name=image.png&originHeight=300&originWidth=773&originalType=binary&ratio=1&rotation=0&showTitle=false&size=93376&status=done&style=none&taskId=u07269133-0292-4eed-8869-11048b176b1&title=&width=618.4)
执行顺序：每一个过滤器都必须指定一个 int 类型的 order 值，order 值越小，优先级越高，执行顺序越靠前
当 order 值一样时，顺序是 defaultFilter 最先，其次是局部的路由过滤器，最后是全局过滤器

#### 网关 CORS 的跨域配置
```yaml
 globalcors:
        cors-configurations:
          '[/**]':
            allowedOrigins: # 允许哪些网站的跨域请求
              - "http://localhost:8090"
              - "http://www.baidu.com"
            allowedMethods: # 允许的跨域 ajax 的请求方式
              - "GET"
              - "POST"
              - "DELETE"
              - "PUT"
              - "OPTIONS"
            allowedHeaders: "*" # 允许在请求中携带的头信息
            allowCredentials: true # 是否允许携带 cookie
            maxAge: 36000 #这次跨域检测的有效期 
```

### Docker
Docker 是一个CS架构的程序，由两部分组成：

- 服务端：Docker 守护进程，负责处理 Docker 指令，管理镜像、容器等
- 客户端：通过命令或 RestAPI 向 Docker 服务端发送指令。可以在本地或远程 向服务端发送指令

![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668519675860-eb6f4244-0c4f-44b8-b61c-534d9f198bb4.png#averageHue=%23ebd7c3&clientId=ua1014a65-e4f0-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=342&id=u67b6bf73&margin=%5Bobject%20Object%5D&name=image.png&originHeight=427&originWidth=861&originalType=binary&ratio=1&rotation=0&showTitle=false&size=88958&status=done&style=none&taskId=u430e01ce-9161-4e3e-899e-d886e60ab0c&title=&width=688.8)

docker save -o nginx.tar  nginx:latest 保存一个镜像
docker rmi  删除镜像
docker load  -i  读取一个镜像文件
docker 命令 --help 查看该命令的帮助文档

容器相关命令：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668520325311-0e87d963-f3a3-48e4-8d82-32ac8df7372c.png#averageHue=%23e4d2c0&clientId=ua1014a65-e4f0-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=286&id=ud14b3cf3&margin=%5Bobject%20Object%5D&name=image.png&originHeight=358&originWidth=767&originalType=binary&ratio=1&rotation=0&showTitle=false&size=56772&status=done&style=none&taskId=ue2390d22-d8e3-48d2-aa20-f3bf5f9372e&title=&width=613.6)
docker exec 进入容器执行命令
eg: `docker exec -it containername bash`
`docker run --name syhk -p 80:80 -d nginx` :

- --name 启一个名字
- -p 端口映射 
- -d 后台启动 
- nginx 容器名

`docker logs [-f]  name`:

- name 容器名 
- -f 参数持续跟踪日志

查看该容器的**日志**
**docker 删除镜像：**
**如果删除的时候提示：
是因为被删除的容器还在被引用着**
```dockerfile
Error response from daemon: conflict: unable to remove repository reference
"javaweb:1.0" (must force) - container 3499d7acf15e is using its referenced 
image c67c0599fe6d
```
**先执行命令: docker rm 容器id**
**再执行命令：docker rmi 容器id**

**数据卷命令**
> 数据卷是一个虚拟目录，指向宿主主机文件系统中的某个目录

`docker volume [COMMAND]`
`docker volume --help`查看相关命令帮助信息

- creat 创建一个 volume
- inspect 显示一个或多个 volume 的信息
- ls 列出所有的 volume
- prune 删除未使用的 volume

挂载数据卷：在创建容器时，通过 `-v` 参数来挂载一个数据卷到某个容器目录

**Dockerfile 自定义镜像**
镜像是分层结构，每一层称为一个 Layer

- BaseImage 层：包含基本的系统函数库、环境变量、文件系统 
- Entrypoin: 入口，是镜像中应用启动的命令
- 其它：在 BaseImage 基础上的添加依赖，安装程序，完成整个应用的安装和配置

![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668689428831-919ae3c4-4eb8-41cc-8066-51d3d49cc3b3.png#averageHue=%23caa99b&clientId=u1753c9ac-0025-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=248&id=u81726d83&margin=%5Bobject%20Object%5D&name=image.png&originHeight=310&originWidth=741&originalType=binary&ratio=1&rotation=0&showTitle=false&size=100344&status=done&style=shadow&taskId=uebae1ba1-a158-45f7-9d7a-b670b4cfd01&title=&width=592.8)

Dockerfile
```dockerfile

# 指定基础镜像
FROM ubuntu:16.04
# 配置环境变量，JDK的安装目录
ENV JAVA_DIR=/usr/local

# 拷贝jdk和java项目的包
COPY ./jdk8.tar.gz $JAVA_DIR/
COPY ./docker-demo.jar /tmp/app.jar

# 安装JDK
RUN cd $JAVA_DIR \
 && tar -xf ./jdk8.tar.gz \
 && mv ./jdk1.8.0_144 ./java8

# 配置环境变量
ENV JAVA_HOME=$JAVA_DIR/java8
ENV PATH=$PATH:$JAVA_HOME/bin

# 暴露端口
EXPOSE 8090
# 入口，java项目的启动命令
ENTRYPOINT java -jar /tmp/app.jar
```
打包命令
`docker build -t javaweb(名字):1.0(版本) .` 注意最后面有个点这个点代表 Dockerfile 所有在文件夹
构建完成就可以运行镜像了
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668690354218-d6a9a94a-900a-4c4a-81b0-5942143b1fa9.png#averageHue=%23292725&clientId=u1753c9ac-0025-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=685&id=ue4de3a5d&margin=%5Bobject%20Object%5D&name=image.png&originHeight=856&originWidth=917&originalType=binary&ratio=1&rotation=0&showTitle=false&size=86405&status=done&style=shadow&taskId=u7b356c6e-95bf-463b-a2b1-b4308c00fda&title=&width=733.6)

### DockerCompose
> 可以基于 Compose 文件帮我们快速的部署分布式应用，通过指令定义集群中的每个容器如何运行

![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668691664596-8033a460-8e80-4d31-a873-560f5387b897.png#averageHue=%231f1f1b&clientId=u1753c9ac-0025-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=u50088fc9&margin=%5Bobject%20Object%5D&name=image.png&originHeight=219&originWidth=200&originalType=url&ratio=1&rotation=0&showTitle=false&size=40088&status=done&style=shadow&taskId=uf70c1b08-93ca-4ec9-b4ee-34432198ad7&title=)
github 地址：[https://github.com/docker/compose](https://github.com/docker/compose)
官网地址：
安装 
```shell
sudo curl -L "https://github.com/docker/compose/releases/download/v2.6.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose
docker-compose --version
```
demo: 部署微服务集群
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668693613434-6fca260d-4265-4c1e-ad0b-78a5424bbd79.png#averageHue=%23300c25&clientId=u1753c9ac-0025-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=149&id=uaa2a5d4d&margin=%5Bobject%20Object%5D&name=image.png&originHeight=186&originWidth=674&originalType=binary&ratio=1&rotation=0&showTitle=false&size=65165&status=done&style=shadow&taskId=u3b87f425-5b78-4908-9967-86173a29edf&title=&width=539.2)
构建
`docker-compose up -d`
#### Docke 镜像仓库 docker Registry
> 有公有和私有两种形式

直接百度教程
## MQ
同步调用问题：

- 耦合度高
- 性能下降
- 资源浪费
- 级联失败

优点：时效性较强，可以立即得到结果 
异步调用：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668742711361-42900588-b2e7-4ab4-8004-d2b0c131eb6c.png#averageHue=%23f5e8dc&clientId=u85698476-b27d-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=271&id=u3297f9d4&margin=%5Bobject%20Object%5D&name=image.png&originHeight=339&originWidth=700&originalType=binary&ratio=1&rotation=0&showTitle=false&size=68065&status=done&style=shadow&taskId=u55912688-e694-4e6a-8541-27fc1c5f41a&title=&width=560)
以上支付服务只需要发布一个支付成功事件即可；
优点：耦合度低，吞吐量提升，故障隔离，流量削峰
缺点：
依赖于 Broker 的可靠性，安全性，吞吐能力
出了问题不好追踪管理

#### MQ 
> MQ message queue 消息队列

常见的 MQ：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668743535655-c697d735-56e1-46af-a312-3f48f86c50e3.png#averageHue=%23dbc5bf&clientId=u85698476-b27d-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=213&id=ue6d08d56&margin=%5Bobject%20Object%5D&name=image.png&originHeight=266&originWidth=759&originalType=binary&ratio=1&rotation=0&showTitle=false&size=58034&status=done&style=shadow&taskId=u101871ad-7cfc-40d6-898d-3efddb41640&title=&width=607.2)

### RabbitMQ 快速入门
> 基于 Erlang 语言开发的开源消息通信中间件

安装：使用 docker 镜像直接拉取使用
运行命令
```shell
docker run \
> -e RABBITMQ_DEFAULT_USER=itcast \
> -e RABBITMQ_DEFAULT_PASS=123321 \
> --name mq \
> --hostname mq1 \
> -p 15672:15672 \
> -p 5672:5672 \
> -d \
> rabbitmq:3-management
```
运行成功后浏览器访问：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668744126127-b8f21b1c-df3b-49f7-9466-4e435fc2de09.png#averageHue=%23dcd0c6&clientId=u85698476-b27d-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=212&id=u7878bfea&margin=%5Bobject%20Object%5D&name=image.png&originHeight=265&originWidth=949&originalType=binary&ratio=1&rotation=0&showTitle=false&size=21933&status=done&style=shadow&taskId=u3a4391d9-1f41-4ac5-9a4d-831516b9ad8&title=&width=759.2)
登录成功页面：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668744179091-0ffb0ca8-dea4-44ce-ac3e-0fc3ab6e02b0.png#averageHue=%23d4c9bf&clientId=u85698476-b27d-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=690&id=uc963c814&margin=%5Bobject%20Object%5D&name=image.png&originHeight=863&originWidth=1911&originalType=binary&ratio=1&rotation=0&showTitle=false&size=145360&status=done&style=shadow&taskId=u9f3c109f-fc56-4d49-a658-1afff589f0f&title=&width=1528.8)

![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668744442979-87adca6c-9329-4743-9265-4c7d959ab9ff.png#averageHue=%23f1e3d3&clientId=u85698476-b27d-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=270&id=u6e9a25db&margin=%5Bobject%20Object%5D&name=image.png&originHeight=337&originWidth=682&originalType=binary&ratio=1&rotation=0&showTitle=false&size=80565&status=done&style=shadow&taskId=u3a0428d1-6f85-408c-af97-29ea5529087&title=&width=545.6)

channel : 操作 MQ 的工具
exchange: 路由消息到队列中
queue : 缓存消息
virtual host ：虚拟主机，是对 queue,exchange 等资源的逻辑分组
官方 demo

- 基本消息队列 

![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668744833467-83c41fed-33ea-46f1-875a-50da337973e6.png#averageHue=%23d3bfb4&clientId=u85698476-b27d-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=145&id=u89434e61&margin=%5Bobject%20Object%5D&name=image.png&originHeight=181&originWidth=362&originalType=binary&ratio=1&rotation=0&showTitle=false&size=16988&status=done&style=shadow&taskId=uf2827cc2-f065-4d77-a04b-a2f10278873&title=&width=289.6)

- 工作消息队列 

![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668744842730-06f61fbb-bd50-4b75-a532-4f0ef4b04fce.png#averageHue=%23d3bfb3&clientId=u85698476-b27d-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=179&id=u8d520ee2&margin=%5Bobject%20Object%5D&name=image.png&originHeight=224&originWidth=335&originalType=binary&ratio=1&rotation=0&showTitle=false&size=22137&status=done&style=shadow&taskId=u8adbd099-2265-41c7-ae31-d5f6d416578&title=&width=268)

- 发布订阅
   - 根据交换机又分为三种：
      - 广播
      - ![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668744857638-d1849b3c-b6ee-49e3-84a1-e6cea7b75fd3.png#averageHue=%23d2bbaf&clientId=u85698476-b27d-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=143&id=ue2196d40&margin=%5Bobject%20Object%5D&name=image.png&originHeight=179&originWidth=340&originalType=binary&ratio=1&rotation=0&showTitle=false&size=19651&status=done&style=shadow&taskId=u0dcb2ea0-8489-4a05-807d-cac5cd16b74&title=&width=272)
      - 路由
      - ![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668744866897-9278f5af-932f-436e-9c38-4fea81fad76b.png#averageHue=%23d3c2b8&clientId=u85698476-b27d-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=178&id=ud7cede0e&margin=%5Bobject%20Object%5D&name=image.png&originHeight=222&originWidth=352&originalType=binary&ratio=1&rotation=0&showTitle=false&size=18891&status=done&style=shadow&taskId=u9d778bc6-ecd9-4328-94f5-45eadde9e23&title=&width=281.6)
      - 主题
      - ![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668744874544-6b337325-ad09-4d17-a2f0-e8abcca91a5e.png#averageHue=%23d3c3b8&clientId=u85698476-b27d-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=174&id=uc3c6821a&margin=%5Bobject%20Object%5D&name=image.png&originHeight=218&originWidth=367&originalType=binary&ratio=1&rotation=0&showTitle=false&size=22427&status=done&style=shadow&taskId=u9fbbe601-bc85-4f27-b829-6590f9a8f6b&title=&width=293.6)

RPC
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668744886787-6255c93c-6c2d-4f77-bdb5-63ed86a0491c.png#averageHue=%23d3c4ba&clientId=u85698476-b27d-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=191&id=u61882c09&margin=%5Bobject%20Object%5D&name=image.png&originHeight=239&originWidth=358&originalType=binary&ratio=1&rotation=0&showTitle=false&size=18311&status=done&style=shadow&taskId=u96fc1ea8-4afa-4252-86f4-b24dab02397&title=&width=286.4)

案例：

连接成功：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668745344421-d30f55a7-bbf5-4a28-bd86-7024a82ddcdc.png#averageHue=%23d1c6bd&clientId=u85698476-b27d-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=197&id=ue9c4f095&margin=%5Bobject%20Object%5D&name=image.png&originHeight=246&originWidth=958&originalType=binary&ratio=1&rotation=0&showTitle=false&size=46949&status=done&style=shadow&taskId=u1d601c92-16dc-4b2c-a14d-0d10fa131df&title=&width=766.4)
成功发送：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668745473899-ad67d268-0c5c-4f36-819b-1be008cb01c4.png#averageHue=%23d4c9bf&clientId=u85698476-b27d-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=637&id=u9540c2f3&margin=%5Bobject%20Object%5D&name=image.png&originHeight=796&originWidth=769&originalType=binary&ratio=1&rotation=0&showTitle=false&size=59186&status=done&style=shadow&taskId=u29382470-3bda-4f9a-b18f-4487bfdf71a&title=&width=615.2)
运行消费者(接收者)：
接收消息后，消息队列里面的消息就会被删除
Publisher:
```java
   @Test
    public void testSendMessage() throws IOException, TimeoutException {
        // 1.建立连接
        ConnectionFactory factory = new ConnectionFactory();
        // 1.1.设置连接参数，分别是：主机名、端口号、vhost、用户名、密码
        factory.setHost("192.168.96.133");
        factory.setPort(5672);
        factory.setVirtualHost("/");
        factory.setUsername("itcast");
        factory.setPassword("123321");
        // 1.2.建立连接
        Connection connection = factory.newConnection();

        // 2.创建通道Channel
        Channel channel = connection.createChannel();

        // 3.创建队列
        String queueName = "simple.queue";
        channel.queueDeclare(queueName, false, false, false, null);

        // 4.发送消息
        String message = "hello, rabbitmq!";
        channel.basicPublish("", queueName, null, message.getBytes());
        System.out.println("发送消息成功：【" + message + "】");

        // 5.关闭通道和连接
        channel.close();
        connection.close();

    }
}
```
Consumer
```java
    public static void main(String[] args) throws IOException, TimeoutException {
        // 1.建立连接
        ConnectionFactory factory = new ConnectionFactory();
        // 1.1.设置连接参数，分别是：主机名、端口号、vhost、用户名、密码
        factory.setHost("192.168.96.133");
        factory.setPort(5672);
        factory.setVirtualHost("/");
        factory.setUsername("itcast");
        factory.setPassword("123321");
        // 1.2.建立连接
        Connection connection = factory.newConnection();

        // 2.创建通道Channel
        Channel channel = connection.createChannel();

        // 3.创建队列
        String queueName = "simple.queue";
        channel.queueDeclare(queueName, false, false, false, null);

        // 4.订阅消息
        channel.basicConsume(queueName, true, new DefaultConsumer(channel){
            @Override
            public void handleDelivery(String consumerTag, Envelope envelope,
                                       AMQP.BasicProperties properties, byte[] body) throws IOException {
                // 5.处理消息
                String message = new String(body);
                System.out.println("接收到消息：【" + message + "】");
            }
        });
        System.out.println("等待接收消息。。。。");
    }
}

```

### SpringAMQP
> AMQP (Advanced Messgae Queuing Protocol) 是用于在应用程序或之间传递业务消息的开放标准，该协议与语言和平台无光
> Spring AMQP 是基于 AMQP 协议定义的一套 API 规范，提供了模板来发送和接收消息


案例1：一个发送者一个接收者
在父工程中引入依赖
```xml
<!--AMQP依赖，包含RabbitMQ-->
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-amqp</artifactId>
</dependency>
```
配置：
```yaml
spring:
  rabbitmq:
    host: 192.168.96.133 # 主机名
    port: 5672  # 端口
    virtual-host: /  # 虚拟主机
    username: itcast # 用户名
    password: 123321 # 密码
```
发送信息类：
```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class SpringAmqpTest {
    @Autowired
    private RabbitTemplate rabbitTemplate;

    @Test
    public void testSendMessage()
    {
        String queuename = "simple.queue";
        String message = "hello , syhk AMQP";
        rabbitTemplate.convertAndSend(queuename,message);
    }
}

```
接收者：
被 spring 托管，需要运行整个项目才能收到
```java
@Component
public class SpringRabbitListener {

    @RabbitListener(queues = "simple.queue")
    public void listenerQueue(String msg) //发的类型是什么就用什么
    {
        System.out.println("消费者接收到的 msg :" +msg);
    }
}

```
demo2:
 ![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668752545697-1abc73a7-7c8b-4230-8d29-0ade8c2b0ecb.png#averageHue=%23f3e8dc&clientId=u85698476-b27d-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=294&id=ude9fb57c&margin=%5Bobject%20Object%5D&name=image.png&originHeight=368&originWidth=673&originalType=binary&ratio=1&rotation=0&showTitle=false&size=31223&status=done&style=shadow&taskId=u98fc0498-b991-42cc-a0d8-18d9da44777&title=&width=538.4)

一个队列绑定多个消费者：
两个消费者
```java
@Component
public class SpringRabbitListener2 {
   @RabbitListener(queues = "simple.queue")
   public void listener(String msg) throws InterruptedException {
       System.out.println("消费者1接收到消息："+msg);
       Thread.sleep(20);
   }
    @RabbitListener(queues = "simple.queue")
    public void listener2(String msg) throws InterruptedException {
        System.out.println("消费者1接收到消息："+msg);
        Thread.sleep(20);
    }
}
```
一个发送者:
```java
 @Test
    public void testSendMessage() throws InterruptedException {
        String queuename = "simple.queue";
        String message = "hello , syhk AMQP__";
        for (int i = 1 ; i <= 50 ; i ++)
        rabbitTemplate.convertAndSend(queuename,message+i);
        Thread.sleep(20);
    }
```
这样写出来的并不是按处理能力来分配消息，添加下面配置来控制消费者预取的消息数量：
```yaml
   listener:
      simple:
        prefetch: 1
```
多个消费者绑定到一个队列 ，同一条消息只会被一个消费者处理
demo3: 发布(Publish)，订阅(Subscribe)
这个模式允许同一消息发送给多个消费者，实现方式加入了 exchage 交换机
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668753482922-6b07e484-423e-4811-ac72-937edcc6b8cf.png#averageHue=%23efe3d4&clientId=u85698476-b27d-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=210&id=ua957aea2&margin=%5Bobject%20Object%5D&name=image.png&originHeight=263&originWidth=737&originalType=binary&ratio=1&rotation=0&showTitle=false&size=49289&status=done&style=shadow&taskId=u6e65a22b-487d-45f9-8815-9f0e384c33a&title=&width=589.6)
绑定完成后查看绑定关系：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668754117014-2d324578-4d73-42d4-9c8c-3631f7a42a5b.png#averageHue=%23d5cac0&clientId=u85698476-b27d-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=511&id=u4d95fa23&margin=%5Bobject%20Object%5D&name=image.png&originHeight=639&originWidth=835&originalType=binary&ratio=1&rotation=0&showTitle=false&size=48485&status=done&style=shadow&taskId=u86727eeb-8602-481b-b0cd-9fea426b2c6&title=&width=668)
```java
@Configuration
public class FanoutConfig {
//    声明 FanoutExchange 交换机
    @Bean
    public FanoutExchange fanoutExchange(){
        return  new FanoutExchange("itcast.fanout");
    }

    @Bean
    public Queue fanoutQueue1(){
        return new Queue("fanout.queue1");
    }

//    绑定队列1到交换机
    @Bean
    public Binding fanoutBing1(Queue fanoutQueue1 /*按方法名注入*/ , FanoutExchange fanoutExchange){
        return BindingBuilder.bind(fanoutQueue1).to(fanoutExchange);
    }

    @Bean
    public Queue fanoutQueue2(){
        return new Queue("fanout.queue2");
    }
    //    绑定队列2到交换机
    @Bean
    public Binding fanoutBing2(Queue fanoutQueue2 /*按方法名注入*/ , FanoutExchange fanoutExchange){
        return BindingBuilder.bind(fanoutQueue2).to(fanoutExchange);
    }

}

```
发送消息
```java
//    发送消息到 exchange
    @Test
    public void TestSendFanouotExchange()
    {
//        交换机名称
        String exchangename = "itcast.fanout";
//       消息
        String msg = "hello every one";
//        发送消息
        rabbitTemplate.convertAndSend(exchangename,"",msg);
    }
```

DirecExchange : 会将接收到的消息根据规则路由到指定的 Queue ，路由模式（routes)
每一个 Queue 都与 Exchange 设置一个 BindingKey
发布者发送消息时，指定消息的 Routingkey
Exchange 将消息路由到 BindKey 与消息 RoutingKey 一致的队列 
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668754880081-8b8b1f0a-05a8-4ee8-8cba-330d017667ec.png#averageHue=%23f0e3d6&clientId=u85698476-b27d-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=202&id=u3494e2f8&margin=%5Bobject%20Object%5D&name=image.png&originHeight=253&originWidth=805&originalType=binary&ratio=1&rotation=0&showTitle=false&size=59960&status=done&style=shadow&taskId=u78105b04-cde4-4453-ab5d-49aadf7f4da&title=&width=644)
绑定关系图
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668755326282-0c14278f-d272-4268-92da-feaaff45e524.png#averageHue=%23d8cec4&clientId=u85698476-b27d-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=328&id=u475fff7c&margin=%5Bobject%20Object%5D&name=image.png&originHeight=410&originWidth=624&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23208&status=done&style=shadow&taskId=ud6c7535d-1c86-4a9f-be29-7eef18914ea&title=&width=499.2)
发送消息：
```java
    @Test
    public void TestSendFanouotExchange()
    {
//        交换机名称
        String exchangename = "itcast.direct";
//       消息
        String msg = "hello every blue";
//        发送消息
        rabbitTemplate.convertAndSend(exchangename,"yellow",msg);
    }

```
接收者：
```java
//   声明一个队列，一个交换机，一个绑定关系
@RabbitListener(bindings = @QueueBinding(
    value = @Queue(name = "direct.queue1"),
    exchange = @Exchange(name = "itcast.direct",type = ExchangeTypes.DIRECT),
    key = {"red","blue"}
))
    public void listenDirectQueue1(String msg)
    {
    System.out.println("消费者接收到 direct.queue1 的消息："+msg);
}

@RabbitListener(bindings = @QueueBinding(
    value = @Queue(name = "direct.queue2"),
    exchange = @Exchange(name = "itcast.direct",type = ExchangeTypes.DIRECT),
    key = {"red","yellow"}
))
    public void listenDirectQueue2(String msg)
    {
    System.out.println("消费者接收到 direct.queue2 的消息："+msg);
}
```

### TopicExchange 
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668755666959-13c7d262-b313-4a59-8865-269fe61f45fb.png#averageHue=%23e9dbcc&clientId=u85698476-b27d-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=285&id=u443c62f5&margin=%5Bobject%20Object%5D&name=image.png&originHeight=356&originWidth=711&originalType=binary&ratio=1&rotation=0&showTitle=false&size=123138&status=done&style=shadow&taskId=u873e017e-f9ad-448c-b1e1-69a5b7ee512&title=&width=568.8)

接收者：
```java
 @RabbitListener(bindings = @QueueBinding(
            value = @Queue(name = "topic.queue1"),
            exchange = @Exchange(name = "itcast.topic",type = ExchangeTypes.TOPIC),
            key = "china.#"
    ))
    public void listenTopicQueue1(String msg)
    {
        System.out.println("消费者接收到 topic.queue1 的消息："+msg);
    }
    @RabbitListener(bindings = @QueueBinding(
            value = @Queue(name = "topic.queue2"),
            exchange = @Exchange(name = "itcast.topic",type = ExchangeTypes.TOPIC),
            key = "china.#"
    ))
    public void listenTopicQueue2(String msg)
    {
        System.out.println("消费者接收到 topic.queue2 的消息："+msg);
    }
```
发送者
```java
    @Test
    public void TestSendTopicExchange()
    {
//        交换机名称
        String exchangename = "itcast.topic";
//       消息
        String msg = "syhk is one";
//        发送消息
        rabbitTemplate.convertAndSend(exchangename,"china.news",msg);
// 多个单词以 . 分隔
    }
```

#### SpringAMQP 消息转换器
修改消息转换器（修改为 json 转换）
引入依赖:
```xml
<dependency>
  <groupId>com.fasterxml.jackson.core</groupId>
  <artifactId>jackson-databind</artifactId>
</dependency>
```
配置：
```java
  @Bean
    public MessageConverter messageConverter()
    {
        return new Jackson2JsonMessageConverter();
    }
```
发送：
```java
  @Test
    public void send()
    {
        Map<String,Object> msg = new HashMap<>();
        msg.put("name","syhk");
        msg.put("age",21);
        rabbitTemplate.convertAndSend("object.queue",msg);
    }
```
成功：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668756900890-6288ae0d-4e72-40fc-96b9-6527a4ff13c4.png#averageHue=%23d5c9bf&clientId=u85698476-b27d-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=387&id=u62180a60&margin=%5Bobject%20Object%5D&name=image.png&originHeight=484&originWidth=772&originalType=binary&ratio=1&rotation=0&showTitle=false&size=30026&status=done&style=shadow&taskId=ue13a3db1-1131-4639-bfcc-6b501c25eb8&title=&width=617.6)
接收：
也需要进行消息转换器配置和上面配置代码一样

```java
 @RabbitListener(queues = "object.queue")
    public void listenerObjectQueue(Map<String,Object> msg)
    {
        System.out.println("接收到"+msg);

    }

```

## 分布式搜索 elasticsearch
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668760966146-bd5fc3a3-bcf3-46c9-97c5-9a4251ea179d.png#averageHue=%23d6d8d2&clientId=u85698476-b27d-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=270&id=u8b3ab5e1&margin=%5Bobject%20Object%5D&name=image.png&originHeight=338&originWidth=607&originalType=binary&ratio=1&rotation=0&showTitle=false&size=57080&status=done&style=shadow&taskId=ua420297d-528d-453f-aa3e-e67da5ee67f&title=&width=485.6)

Lucene：是 Apache 的开源搜索引擎类库，提供了搜索引擎的核心 API

##### 正向索引 和 倒排索引
文档：每一条数据就是一个文档
词条：对文档中的内容分词，得到的词语就是词条
正向索引：基于文档id创建索引。查询词条时必须先找到文档，而后判断是否包含词条
倒排索引：对文档内容分词，对词条创建索引，并记录词条所在文档的信息。查询时先根据词条查询到文档 id ， 而后获取到文档

#### ES 与 mysql 概念对比
ES 是面向文档存储的，文档数据会被序列化为 json 格式后存储在 es 中
索引：相同类型的文档的集合
映射：索引中文档的字段约束信息
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668762175885-f53ae393-a67f-437b-9e3d-ebd8831568ef.png#averageHue=%23cbb1a7&clientId=u85698476-b27d-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=272&id=u4767df03&margin=%5Bobject%20Object%5D&name=image.png&originHeight=340&originWidth=718&originalType=binary&ratio=1&rotation=0&showTitle=false&size=85493&status=done&style=shadow&taskId=ud05b7714-856a-4849-b014-42a8eaf7fda&title=&width=574.4)

mysql: 擅长事务类型操作，可以确保数据的安全和一致性
Elasticsearch：擅长海量数据的搜索，分析，计算
它们两个互补
es 安装搜索教程/或者看下面这个 docker 安装的
`docker network create syhk-net` 创建一个网络
[安装elasticsearch.md](https://www.yuque.com/attachments/yuque/0/2022/md/32483946/1668762879674-5e03ffe9-b0cc-4a37-9dab-9c6c7209a945.md)
部署单点es:
```shell
docker run -d \
	--name es \
    -e "ES_JAVA_OPTS=-Xms512m -Xmx512m" \
    -e "discovery.type=single-node" \
    -v es-data:/usr/share/elasticsearch/data \
    -v es-plugins:/usr/share/elasticsearch/plugins \
    --privileged \
    --network syhk-net \
    -p 9200:9200 \
    -p 9300:9300 \
elasticsearch:7.12.1
```
安装成功：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668763123531-c3103d3f-d74f-439f-a63e-da508c65854b.png#averageHue=%23ccbcaa&clientId=u85698476-b27d-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=473&id=ud40c6e7c&margin=%5Bobject%20Object%5D&name=image.png&originHeight=591&originWidth=947&originalType=binary&ratio=1&rotation=0&showTitle=false&size=114992&status=done&style=shadow&taskId=u7ab397b5-6ac6-495e-abeb-1f468434aec&title=&width=757.6)
Kibana 部署：提供一个 es 的可视化界面
```shell
docker run -d \
--name kibana \
-e ELASTICSEARCH_HOSTS=http://es:9200 \
--network=syhk-net \
-p 5601:5601  \
kibana:7.12.1
```

[http://192.168.96.133:5601/app/home#/](http://192.168.96.133:5601/app/home#/)  今天启动又没问题了
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668854618824-9a627a7a-def0-404a-90d5-320cbbf04809.png#averageHue=%23464743&clientId=uf02394b4-0268-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=725&id=u51ddfc23&margin=%5Bobject%20Object%5D&name=image.png&originHeight=906&originWidth=931&originalType=binary&ratio=1&rotation=0&showTitle=false&size=145624&status=done&style=shadow&taskId=ufc1e61bd-eb8e-42b9-8a1b-bdc93150ccf&title=&width=744.8)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668854803368-0888e9e4-0cee-4f3e-9ecc-33413c9531f6.png#averageHue=%23c8b5a3&clientId=uf02394b4-0268-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=405&id=ud034d7bd&margin=%5Bobject%20Object%5D&name=image.png&originHeight=506&originWidth=943&originalType=binary&ratio=1&rotation=0&showTitle=false&size=100689&status=done&style=shadow&taskId=ub0e62a75-a0e6-4da6-ab8f-4f33fa7ccb6&title=&width=754.4)

### 安装 IK 分词器
默认的分词规则对中文处理并不友好。
因此处理中文分词器，一般会使用 IK
安装教程百度：
IK 分词器包含两种模式： 

- ik_smart: 最少切分
- ik_max_word 最细切分 
```java
POST /_analyze
{
  "text": "黑马程序员java", 
  "analyzer": "ik_max_word"  (ik_smart)
}
```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668855536332-4753ffaf-2d35-4b29-9e0e-679e4f87fe89.png#averageHue=%23c7b4a2&clientId=uf02394b4-0268-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=614&id=ua2890a82&margin=%5Bobject%20Object%5D&name=image.png&originHeight=767&originWidth=916&originalType=binary&ratio=1&rotation=0&showTitle=false&size=101546&status=done&style=shadow&taskId=uaa73c471-e8cb-4f9e-9bb5-56ee9e14384&title=&width=732.8)

### IK 分词器的拓展和停用词典
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668855911131-e73cd3ab-d5bd-4c77-ac78-b199f23965e9.png#averageHue=%23dfceb8&clientId=uf02394b4-0268-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=167&id=u7de4f0bb&margin=%5Bobject%20Object%5D&name=image.png&originHeight=209&originWidth=719&originalType=binary&ratio=1&rotation=0&showTitle=false&size=93070&status=done&style=shadow&taskId=ufab3e5c6-78ab-46b7-b270-2a9551198d1&title=&width=575.2)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668856190808-f938508b-b573-4243-a21b-d8a845b82d55.png#averageHue=%233a3862&clientId=uf02394b4-0268-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=268&id=u9294ab5e&margin=%5Bobject%20Object%5D&name=image.png&originHeight=335&originWidth=757&originalType=binary&ratio=1&rotation=0&showTitle=false&size=105626&status=done&style=shadow&taskId=u31a112e5-f099-4a34-8a53-0410b73864c&title=&width=605.6)

#### 索引库操作
mapping 映射：对索引库中文档的约束
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668856824139-4e193a98-734e-4920-9eed-84a77a8cb473.png#averageHue=%23e5d3bf&clientId=uf02394b4-0268-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=261&id=ub7d07199&margin=%5Bobject%20Object%5D&name=image.png&originHeight=326&originWidth=744&originalType=binary&ratio=1&rotation=0&showTitle=false&size=89479&status=done&style=shadow&taskId=u72435478-3970-45b1-91d8-3b54fd9e0de&title=&width=595.2)
```json
# 创建索引库
PUT /syhk
{
  "mappings": {
    "properties": {
      "info": {
        "type": "text",
        "analyzer": "ik_smart"
      },
      "email": {
        "type": "keyword",
        "index": false
      },
      "name": {
        "type": "object",
        "properties": {
          "first_name": {
            "type": "keyword"
          },
          "last_name":{
            "type":"keyword"
          }
        }
      }
    }
  }
}

```

#### 查看、删除索引库
```json
# 查询 
GET /syhk

# 删除
DELETE /syhk

```

禁止修改索引库，但是可以添加新的字段 
```json
# 修改
PUT /syhk/_mapping
{
  "properties":{
    "age":{
      "type":"integer"
    }
  }
}
```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32483946/1668857462222-f1cd6408-0f36-46da-b3ae-d4cb418b65d3.png#averageHue=%23c7b4a2&clientId=uf02394b4-0268-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=526&id=u7022d61f&margin=%5Bobject%20Object%5D&name=image.png&originHeight=657&originWidth=951&originalType=binary&ratio=1&rotation=0&showTitle=false&size=74901&status=done&style=shadow&taskId=udac25a85-e92d-439e-ba3e-f1895a05763&title=&width=760.8)

- 创建： PUT /name
- 查询   GET /name
- 删除   DELETE /name
- 添加 PUT /name

#### 文档操作 增删改查
```json
# 查询文档
GET /syhk/_doc/1

# 删除文档
DELETE /syhk/_doc/1

# 修改
PUT /syhk/_doc/1
```

- 全量修改，会删除旧文档，添加新文档
```json
# 修改（/添加）文档 , 先删除再新增，可做添加操作
PUT /syhk/_doc/1
{
  "info":"syhk",
  "email":"itcast.cn",
  "name":{
    "first-name":"2",
    "last_name":"3"
  }
}
```

- 增量修改，修改指定字段值
```json
# 局部修改
POST /syhk/_update/1  # 注意这里是 _update
{
  "doc":{
    "email":"725482520"
  }
}

```

### RestCilent 操作索引库
案例：
```json
# 酒店的 mapping
PUT /hotel
{
  "mappings": {
    "properties": {
      "id":{
        "type": "keyword"
      },
      "name":{
        "type": "text",
        "analyzer": "ik_max_word",
        "copy_to": "all"
      },
      "address":{
        "type": "keyword",
        "index": false
      },
      "price": {
        "type": "integer"
      },
      "brand":{
        "type": "keyword"
      },
      "city":{
        "type": "keyword",
        "copy_to": "all"
      },
      "starName":{
        "type": "keyword"
      },
      "business":{
        "type": "keyword",
        "copy_to": "all"
      },
      "loaction":
      {
        "type": "geo_point"
      },
      "pic":{
        "type": "keyword",
        "index": false
      },
      "all":{
        "type": "text",
        "analyzer": "ik_max_word"
      }
    }
  }
}
```
字段拷贝可以使用 copy_to 属性将当前字段拷贝到指定字段。上面定义是 all
使用 `copyt_to`
初始化 javaRestClient
引入依赖
```xml
<dependency>
  <groupId>org.elasticsearch.client</groupId>
  <artifactId>elasticsearch-rest-high-level-client</artifactId>
  <version>7.12.1</version>
</dependency>
```
pom 定义版本：
```xml
 <properties>
        <java.version>1.8</java.version>
        <elasticsearch.version>7.12.1</elasticsearch.version>
    </properties>
```
创建：
```java
 private RestHighLevelClient restHighLevelClient;

    @BeforeEach
    void setUp(){
        this.restHighLevelClient=new RestHighLevelClient(RestClient.builder(
                HttpHost.create("http://192.168.96.133:9200")
        ));
    }
```
```java
   @Test
    void testCreateHotelIndex() throws IOException{
//        创建 Request 对象
        CreateIndexRequest request = new CreateIndexRequest("hotel");
//        请求参数
//        json 字符串太大，可以单独创建一个类管理它们，把它们定义为常量
        request.source("{\n" +
                "  \"mappings\": {\n" +
                "    \"properties\": {\n" +
                "      \"id\":{\n" +
                "        \"type\": \"keyword\"\n" +
                "      },\n" +
                "      \"name\":{\n" +
                "        \"type\": \"text\",\n" +
                "        \"analyzer\": \"ik_max_word\",\n" +
                "        \"copy_to\": \"all\"\n" +
                "      },\n" +
                "      \"address\":{\n" +
                "        \"type\": \"keyword\",\n" +
                "        \"index\": false\n" +
                "      },\n" +
                "      \"price\": {\n" +
                "        \"type\": \"integer\"\n" +
                "      },\n" +
                "      \"brand\":{\n" +
                "        \"type\": \"keyword\"\n" +
                "      },\n" +
                "      \"city\":{\n" +
                "        \"type\": \"keyword\",\n" +
                "        \"copy_to\": \"all\"\n" +
                "      },\n" +
                "      \"starName\":{\n" +
                "        \"type\": \"keyword\"\n" +
                "      },\n" +
                "      \"business\":{\n" +
                "        \"type\": \"keyword\",\n" +
                "        \"copy_to\": \"all\"\n" +
                "      },\n" +
                "      \"loaction\":\n" +
                "      {\n" +
                "        \"type\": \"geo_point\"\n" +
                "      },\n" +
                "      \"pic\":{\n" +
                "        \"type\": \"keyword\",\n" +
                "        \"index\": false\n" +
                "      },\n" +
                "      \"all\":{\n" +
                "        \"type\": \"text\",\n" +
                "        \"analyzer\": \"ik_max_word\"\n" +
                "      }\n" +
                "    }\n" +
                "  }\n" +
                "}", XContentType.JSON);


//        发送请求
        restHighLevelClient.indices().create(request, RequestOptions.DEFAULT);

    }
```
删除，查询是否存在
```java
//    删除索引库
    @Test
    void testDelete() throws IOException {
//        创建 Request 对象
        DeleteIndexRequest request = new DeleteIndexRequest("hotel");
//        请求参数
//        发送请求
        restHighLevelClient.indices().delete(request, RequestOptions.DEFAULT);
    }

    //    删除索引库
    @Test
    void testExists() throws IOException {
//        创建 Request 对象
        GetIndexRequest request = new GetIndexRequest("hotel");
//        发送请求
    boolean exists=  restHighLevelClient.indices().exists(request, RequestOptions.DEFAULT);
    if(exists)
    {
        System.out.println("exists");
    }
    }
```
总结： XxxIndexRequest , xxx 是 CREATE GET  Delete

#### RestClient 操作文档
```java
//    操作文档
    @Test
    void testIndexDocument() throws  IOException {
//        根据 id 查询数据
        Hotel hotel = iHotelService.getById(61003L);
//        转换为文档类型
        HotelDoc hotelDoc = new HotelDoc(hotel);
//        创建 Request 对象
        IndexRequest request = new IndexRequest("hotel").id(hotel.getId().toString());
//        准备 json 文档
       request.source(JSON.toJSONString(hotelDoc),XContentType.JSON);
//        发送请求
        restHighLevelClient.index(request,RequestOptions.DEFAULT);
    }
```
查询
```java
//    查询文档
    @Test
    void TestGetdocumentById() throws IOException
    {
//        准备 request
        GetRequest request = new GetRequest("hotel","61003");
// 响应结果
    GetResponse response= restHighLevelClient.get(request,RequestOptions.DEFAULT);
        String json = response.getSourceAsString();
        HotelDoc hotelDoc = JSON.parseObject(json,HotelDoc.class);
        System.out.println(hotelDoc);
    }
```
修改/更新
```java
//    修改
    @Test
    void updateDocumentById() throws  IOException
    {
//        Request 对象
        UpdateRequest request = new UpdateRequest("hotel","61003");
//        请求参数
        request.doc(
                "price","992",
                "starName","四占"
        );
//        发送请求
        restHighLevelClient.update(request,RequestOptions.DEFAULT);
    }
```
删除：
```java
 private RestHighLevelClient restHighLevelClient;
@Test
    void testDocument() throws IOException {
        DeleteRequest request = new DeleteRequest("hotel","61003");
        restHighLevelClient.delete(request,RequestOptions.DEFAULT);
    }

```
批量：
```java
//    批量新增文档
    @Test
    void testBulk() throws IOException {
//        批量查询数据
        List<Hotel> hotelList= iHotelService.list();
//        创建 Bulk 请求
        BulkRequest request = new BulkRequest();
//        准备参数
        for( Hotel hotel : hotelList)
        {
            HotelDoc hotelDoc = new HotelDoc(hotel);
            request.add(new IndexRequest("hotel").id("61038").source(JSON.toJSONString(hotelDoc),XContentType.JSON));
        }
        request.add(new IndexRequest("hotel").id("61038").source("json",XContentType.JSON));
        request.add(new IndexRequest("hotel").id("61038").source("json",XContentType.JSON));
//        发送请求
        restHighLevelClient.bulk(request,RequestOptions.DEFAULT);
    }
```

P101
















