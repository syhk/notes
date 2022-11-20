# Spring Security

[https://www.baeldung.com/spring-security-cas-sso](https://www.baeldung.com/spring-security-cas-sso)

> Spring Security 是 Spring 家族中心的一个安全管理框架。还有另外一个安全框架 Shiro


中大型项目有  Spring Security ，小项目一般使用 Shiro

### 认证和授权

**认证**：验证当前访问系统的是不是本系统的用户，并要确认具体是哪个用户

**授权**：经过认证后判断当前用户是否有权限进行某个操作

环境准备：

创建一个 springboot 项目：

依赖选择 web 和 lombok　就行了

引入 SpringSecurity

```xml
 <dependency>
   <groupId>org.springframework.boot</groupId>
 <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

再次启动访问：会自动跳转到必须要登录才可以访问

![](image/image_B4InMaKZEK.png#alt=)

整合进来的时候默认的用户名和密码：

用户名： user

密码：

![](image/image_vBn5PTxwSp.png#alt=)

它默认有个 logout 登出

[http://localhost:8080/logout](http://localhost:8080/logout)

### 前后端分离校验流程：

![](image/image_550BugUd-o.png#alt=)

SpringSecurity 的原理就是一个过滤器链，内部包含了各种功能的过滤器。

![](image/image_5j8NV_yslu.png#alt=)

调试查看有哪些过滤器：

![](image/image_MehPkZP6n9.png#alt=)

![](image/image_Cc4LHYXFaE.png#alt=)

输入方法查看：

![](image/image_Yv2PYFVVtA.png#alt=)

![](image/image_sX4Y7bUOsy.png#alt=)

思路分析：

登录 ：

1. 
自定义登录接口，调用 ProviderManager 的方法进行认证如果认证通过生成 jwt，把用户信息存放 redis 中

2. 
自定义 UserDetailsService 在这个实现类中去查询数据库


校验：

1. 定义 JWT 认证过滤器，获取 token ，解析 token 获取其中的 userid ，从 redis 中获取用户信息，存储 SecurityContextHolder

#### 准备工作：

添加依赖：

```xml
        <!--redis依赖-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
        </dependency>
        <!--fastjson依赖-->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>fastjson</artifactId>
            <version>1.2.33</version>
        </dependency>
        <!--jwt依赖-->
        <dependency>
            <groupId>io.jsonwebtoken</groupId>
            <artifactId>jjwt</artifactId>
            <version>0.9.0</version>
        </dependency>
```

类准备：

项目结构 ：

![](image/image_bwEwieWnMC.png#alt=)

RedisConfig:

```java
package com.example.springsecurity.config;
import com.example.springsecurity.utils.FastJsonRedisSerializer;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.redis.connection.RedisConnectionFactory;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.serializer.StringRedisSerializer;

@Configuration
public class RedisConfig {

    @Bean
    @SuppressWarnings(value = { "unchecked", "rawtypes" })
    public RedisTemplate<Object, Object> redisTemplate(RedisConnectionFactory connectionFactory)
    {
        RedisTemplate<Object, Object> template = new RedisTemplate<>();
        template.setConnectionFactory(connectionFactory);

        FastJsonRedisSerializer serializer = new FastJsonRedisSerializer(Object.class);

        // 使用StringRedisSerializer来序列化和反序列化redis的key值
        template.setKeySerializer(new StringRedisSerializer());
        template.setValueSerializer(serializer);

        // Hash的key也采用StringRedisSerializer的序列化方式
        template.setHashKeySerializer(new StringRedisSerializer());
        template.setHashValueSerializer(serializer);

        template.afterPropertiesSet();
        return template;
    }
}
```

HelloController:

```java
package com.example.springsecurity.controller;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloController {

    @GetMapping("/hello")
    public String hello(){
        return "Hello";
    }

}
```

ResponseResult：响应类

```java
package com.example.springsecurity.domain;

import com.fasterxml.jackson.annotation.JsonInclude;

@JsonInclude(JsonInclude.Include.NON_NULL)
public class ResponseResult<T> {
    /**
     * 状态码
     */
    private Integer code;
    /**
     * 提示信息，如果有错误时，前端可以获取该字段进行提示
     */
    private String msg;
    /**
     * 查询到的结果数据，
     */
    private T data;

    public ResponseResult(Integer code, String msg) {
        this.code = code;
        this.msg = msg;
    }

    public ResponseResult(Integer code, T data) {
        this.code = code;
        this.data = data;
    }

    public Integer getCode() {
        return code;
    }

    public void setCode(Integer code) {
        this.code = code;
    }

    public String getMsg() {
        return msg;
    }

    public void setMsg(String msg) {
        this.msg = msg;
    }

    public T getData() {
        return data;
    }

    public void setData(T data) {
        this.data = data;
    }

    public ResponseResult(Integer code, String msg, T data) {
        this.code = code;
        this.msg = msg;
        this.data = data;
    }
}
```

User:

```java
package com.example.springsecurity.enity;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

import java.io.Serializable;
import java.util.Date;

@Data
@AllArgsConstructor
@NoArgsConstructor
public class User implements Serializable {
    private static final long serialVersionUID = -40356785423868312L;

    /**
     * 主键
     */
    private Long id;
    /**
     * 用户名
     */
    private String userName;
    /**
     * 昵称
     */
    private String nickName;
    /**
     * 密码
     */
    private String password;
    /**
     * 账号状态（0正常 1停用）
     */
    private String status;
    /**
     * 邮箱
     */
    private String email;
    /**
     * 手机号
     */
    private String phonenumber;
    /**
     * 用户性别（0男，1女，2未知）
     */
    private String sex;
    /**
     * 头像
     */
    private String avatar;
    /**
     * 用户类型（0管理员，1普通用户）
     */
    private String userType;
    /**
     * 创建人的用户id
     */
    private Long createBy;
    /**
     * 创建时间
     */
    private Date createTime;
    /**
     * 更新人
     */
    private Long updateBy;
    /**
     * 更新时间
     */
    private Date updateTime;
    /**
     * 删除标志（0代表未删除，1代表已删除）
     */
    private Integer delFlag;
}
```

FastJsonRedisSeriailzer:

```java
package com.example.springsecurity.utils;
import com.alibaba.fastjson.JSON;
import com.alibaba.fastjson.serializer.SerializerFeature;
import com.fasterxml.jackson.databind.JavaType;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.databind.type.TypeFactory;
import org.springframework.data.redis.serializer.RedisSerializer;
import org.springframework.data.redis.serializer.SerializationException;
import com.alibaba.fastjson.parser.ParserConfig;
import org.springframework.util.Assert;
import java.nio.charset.Charset;

/**
 * Redis使用FastJson序列化
 *
 * @author sg
 */
public class FastJsonRedisSerializer<T> implements RedisSerializer<T>
{

    public static final Charset DEFAULT_CHARSET = Charset.forName("UTF-8");

    private Class<T> clazz;

    static
    {
        ParserConfig.getGlobalInstance().setAutoTypeSupport(true);
    }

    public FastJsonRedisSerializer(Class<T> clazz)
    {
        super();
        this.clazz = clazz;
    }

    @Override
    public byte[] serialize(T t) throws SerializationException
    {
        if (t == null)
        {
            return new byte[0];
        }
        return JSON.toJSONString(t, SerializerFeature.WriteClassName).getBytes(DEFAULT_CHARSET);
    }

    @Override
    public T deserialize(byte[] bytes) throws SerializationException
    {
        if (bytes == null || bytes.length <= 0)
        {
            return null;
        }
        String str = new String(bytes, DEFAULT_CHARSET);

        return JSON.parseObject(str, clazz);
    }


    protected JavaType getJavaType(Class<?> clazz)
    {
        return TypeFactory.defaultInstance().constructType(clazz);
    }
}
```

JwtUtil:

```java
package com.example.springsecurity.utils;

import io.jsonwebtoken.Claims;
import io.jsonwebtoken.JwtBuilder;
import io.jsonwebtoken.Jwts;
import io.jsonwebtoken.SignatureAlgorithm;

import javax.crypto.SecretKey;
import javax.crypto.spec.SecretKeySpec;
import java.util.Base64;
import java.util.Date;
import java.util.UUID;

/**
 * JWT工具类
 */
public class JwtUtil {

    //有效期为
    public static final Long JWT_TTL = 60 * 60 *1000L;// 60 * 60 *1000  一个小时
    //设置秘钥明文
    public static final String JWT_KEY = "sangeng";

    public static String getUUID(){
        String token = UUID.randomUUID().toString().replaceAll("-", "");
        return token;
    }

    /**
     * 生成jtw
     * @param subject token中要存放的数据（json格式）
     * @return
     */
    public static String createJWT(String subject) {
        JwtBuilder builder = getJwtBuilder(subject, null, getUUID());// 设置过期时间
        return builder.compact();
    }

    /**
     * 生成jtw
     * @param subject token中要存放的数据（json格式）
     * @param ttlMillis token超时时间
     * @return
     */
    public static String createJWT(String subject, Long ttlMillis) {
        JwtBuilder builder = getJwtBuilder(subject, ttlMillis, getUUID());// 设置过期时间
        return builder.compact();
    }

    private static JwtBuilder getJwtBuilder(String subject, Long ttlMillis, String uuid) {
        SignatureAlgorithm signatureAlgorithm = SignatureAlgorithm.HS256;
        SecretKey secretKey = generalKey();
        long nowMillis = System.currentTimeMillis();
        Date now = new Date(nowMillis);
        if(ttlMillis==null){
            ttlMillis=JwtUtil.JWT_TTL;
        }
        long expMillis = nowMillis + ttlMillis;
        Date expDate = new Date(expMillis);
        return Jwts.builder()
                .setId(uuid)              //唯一的ID
                .setSubject(subject)   // 主题  可以是JSON数据
                .setIssuer("sg")     // 签发者
                .setIssuedAt(now)      // 签发时间
                .signWith(signatureAlgorithm, secretKey) //使用HS256对称加密算法签名, 第二个参数为秘钥
                .setExpiration(expDate);
    }

    /**
     * 创建token
     * @param id
     * @param subject
     * @param ttlMillis
     * @return
     */
    public static String createJWT(String id, String subject, Long ttlMillis) {
        JwtBuilder builder = getJwtBuilder(subject, ttlMillis, id);// 设置过期时间
        return builder.compact();
    }

    public static void main(String[] args) throws Exception {
        String token = "eyJhbGciOiJIUzI1NiJ9.eyJqdGkiOiJjYWM2ZDVhZi1mNjVlLTQ0MDAtYjcxMi0zYWEwOGIyOTIwYjQiLCJzdWIiOiJzZyIsImlzcyI6InNnIiwiaWF0IjoxNjM4MTA2NzEyLCJleHAiOjE2MzgxMTAzMTJ9.JVsSbkP94wuczb4QryQbAke3ysBDIL5ou8fWsbt_ebg";
        Claims claims = parseJWT(token);
        System.out.println(claims);
    }

    /**
     * 生成加密后的秘钥 secretKey
     * @return
     */
    public static SecretKey generalKey() {
        byte[] encodedKey = Base64.getDecoder().decode(JwtUtil.JWT_KEY);
        SecretKey key = new SecretKeySpec(encodedKey, 0, encodedKey.length, "AES");
        return key;
    }

    /**
     * 解析
     *
     * @param jwt
     * @return
     * @throws Exception
     */
    public static Claims parseJWT(String jwt) throws Exception {
        SecretKey secretKey = generalKey();
        return Jwts.parser()
                .setSigningKey(secretKey)
                .parseClaimsJws(jwt)
                .getBody();
    }


}
```

RedisCache:

```java
package com.example.springsecurity.utils;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.redis.core.BoundSetOperations;
import org.springframework.data.redis.core.HashOperations;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.core.ValueOperations;
import org.springframework.stereotype.Component;

import java.util.*;
import java.util.concurrent.TimeUnit;

@SuppressWarnings(value = { "unchecked", "rawtypes" })
@Component
public class RedisCache
{
    @Autowired
    public RedisTemplate redisTemplate;

    /**
     * 缓存基本的对象，Integer、String、实体类等
     *
     * @param key 缓存的键值
     * @param value 缓存的值
     */
    public <T> void setCacheObject(final String key, final T value)
    {
        redisTemplate.opsForValue().set(key, value);
    }

    /**
     * 缓存基本的对象，Integer、String、实体类等
     *
     * @param key 缓存的键值
     * @param value 缓存的值
     * @param timeout 时间
     * @param timeUnit 时间颗粒度
     */
    public <T> void setCacheObject(final String key, final T value, final Integer timeout, final TimeUnit timeUnit)
    {
        redisTemplate.opsForValue().set(key, value, timeout, timeUnit);
    }

    /**
     * 设置有效时间
     *
     * @param key Redis键
     * @param timeout 超时时间
     * @return true=设置成功；false=设置失败
     */
    public boolean expire(final String key, final long timeout)
    {
        return expire(key, timeout, TimeUnit.SECONDS);
    }

    /**
     * 设置有效时间
     *
     * @param key Redis键
     * @param timeout 超时时间
     * @param unit 时间单位
     * @return true=设置成功；false=设置失败
     */
    public boolean expire(final String key, final long timeout, final TimeUnit unit)
    {
        return redisTemplate.expire(key, timeout, unit);
    }

    /**
     * 获得缓存的基本对象。
     *
     * @param key 缓存键值
     * @return 缓存键值对应的数据
     */
    public <T> T getCacheObject(final String key)
    {
        ValueOperations<String, T> operation = redisTemplate.opsForValue();
        return operation.get(key);
    }

    /**
     * 删除单个对象
     *
     * @param key
     */
    public boolean deleteObject(final String key)
    {
        return redisTemplate.delete(key);
    }

    /**
     * 删除集合对象
     *
     * @param collection 多个对象
     * @return
     */
    public long deleteObject(final Collection collection)
    {
        return redisTemplate.delete(collection);
    }

    /**
     * 缓存List数据
     *
     * @param key 缓存的键值
     * @param dataList 待缓存的List数据
     * @return 缓存的对象
     */
    public <T> long setCacheList(final String key, final List<T> dataList)
    {
        Long count = redisTemplate.opsForList().rightPushAll(key, dataList);
        return count == null ? 0 : count;
    }

    /**
     * 获得缓存的list对象
     *
     * @param key 缓存的键值
     * @return 缓存键值对应的数据
     */
    public <T> List<T> getCacheList(final String key)
    {
        return redisTemplate.opsForList().range(key, 0, -1);
    }

    /**
     * 缓存Set
     *
     * @param key 缓存键值
     * @param dataSet 缓存的数据
     * @return 缓存数据的对象
     */
    public <T> BoundSetOperations<String, T> setCacheSet(final String key, final Set<T> dataSet)
    {
        BoundSetOperations<String, T> setOperation = redisTemplate.boundSetOps(key);
        Iterator<T> it = dataSet.iterator();
        while (it.hasNext())
        {
            setOperation.add(it.next());
        }
        return setOperation;
    }

    /**
     * 获得缓存的set
     *
     * @param key
     * @return
     */
    public <T> Set<T> getCacheSet(final String key)
    {
        return redisTemplate.opsForSet().members(key);
    }

    /**
     * 缓存Map
     *
     * @param key
     * @param dataMap
     */
    public <T> void setCacheMap(final String key, final Map<String, T> dataMap)
    {
        if (dataMap != null) {
            redisTemplate.opsForHash().putAll(key, dataMap);
        }
    }

    /**
     * 获得缓存的Map
     *
     * @param key
     * @return
     */
    public <T> Map<String, T> getCacheMap(final String key)
    {
        return redisTemplate.opsForHash().entries(key);
    }

    /**
     * 往Hash中存入数据
     *
     * @param key Redis键
     * @param hKey Hash键
     * @param value 值
     */
    public <T> void setCacheMapValue(final String key, final String hKey, final T value)
    {
        redisTemplate.opsForHash().put(key, hKey, value);
    }

    /**
     * 获取Hash中的数据
     *
     * @param key Redis键
     * @param hKey Hash键
     * @return Hash中的对象
     */
    public <T> T getCacheMapValue(final String key, final String hKey)
    {
        HashOperations<String, String, T> opsForHash = redisTemplate.opsForHash();
        return opsForHash.get(key, hKey);
    }

    /**
     * 删除Hash中的数据
     *
     * @param key
     * @param hkey
     */
    public void delCacheMapValue(final String key, final String hkey)
    {
        HashOperations hashOperations = redisTemplate.opsForHash();
        hashOperations.delete(key, hkey);
    }

    /**
     * 获取多个Hash中的数据
     *
     * @param key Redis键
     * @param hKeys Hash键集合
     * @return Hash对象集合
     */
    public <T> List<T> getMultiCacheMapValue(final String key, final Collection<Object> hKeys)
    {
        return redisTemplate.opsForHash().multiGet(key, hKeys);
    }

    /**
     * 获得缓存的基本对象列表
     *
     * @param pattern 字符串前缀
     * @return 对象列表
     */
    public Collection<String> keys(final String pattern)
    {
        return redisTemplate.keys(pattern);
    }
}
```

WebUtils:

```java
package com.example.springsecurity.utils;

import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class WebUtils
{
    /**
     * 将字符串渲染到客户端
     *
     * @param response 渲染对象
     * @param string 待渲染的字符串
     * @return null
     */
    public static String renderString(HttpServletResponse response, String string) {
        try
        {
            response.setStatus(200);
            response.setContentType("application/json");
            response.setCharacterEncoding("utf-8");
            response.getWriter().print(string);
        }
        catch (IOException e)
        {
            e.printStackTrace();
        }
        return null;
    }
}
```

#### 数据库校验用户：

数据库新建一个用户表：

```sql
use svue;
CREATE TABLE `sys_user` (
  `id` BIGINT(20) NOT NULL AUTO_INCREMENT COMMENT '主键',
  `user_name` VARCHAR(64) NOT NULL DEFAULT 'NULL' COMMENT '用户名',
  `nick_name` VARCHAR(64) NOT NULL DEFAULT 'NULL' COMMENT '昵称',
  `password` VARCHAR(64) NOT NULL DEFAULT 'NULL' COMMENT '密码',
  `status` CHAR(1) DEFAULT '0' COMMENT '账号状态（0正常 1停用）',
  `email` VARCHAR(64) DEFAULT NULL COMMENT '邮箱',
  `phonenumber` VARCHAR(32) DEFAULT NULL COMMENT '手机号',
  `sex` CHAR(1) DEFAULT NULL COMMENT '用户性别（0男，1女，2未知）',
  `avatar` VARCHAR(128) DEFAULT NULL COMMENT '头像',
  `user_type` CHAR(1) NOT NULL DEFAULT '1' COMMENT '用户类型（0管理员，1普通用户）',
  `create_by` BIGINT(20) DEFAULT NULL COMMENT '创建人的用户id',
  `create_time` DATETIME DEFAULT NULL COMMENT '创建时间',
  `update_by` BIGINT(20) DEFAULT NULL COMMENT '更新人',
  `update_time` DATETIME DEFAULT NULL COMMENT '更新时间',
  `del_flag` INT(11) DEFAULT '0' COMMENT '删除标志（0代表未删除，1代表已删除）',
  PRIMARY KEY (`id`)
) ENGINE=INNODB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8mb4 COMMENT='用户表'
```

引入 mysql 和mybatis-plus

```xml
        <!--        引入 mysql 和 mybatis-plus-->
 
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>

        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.5.2</version>
        </dependency>
```

![](image/image_HZ2nLumnxG.png#alt=)

UserMapper:

```java
package com.example.springsecurity.mapper;

import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import com.example.springsecurity.enity.User;
import org.apache.ibatis.annotations.Mapper;


//这里要指定实体类
@Mapper
public interface UserMapper extends BaseMapper<User>{
}
```

User有更改：

```java
package com.example.springsecurity.enity;
import com.baomidou.mybatisplus.annotation.TableId;
import com.baomidou.mybatisplus.annotation.TableName;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;
import org.springframework.data.annotation.Id;

import java.io.Serializable;
import java.util.Date;

@Data
@AllArgsConstructor
@NoArgsConstructor
@TableName("sys_user")
public class User implements Serializable {
    private static final long serialVersionUID = -40356785423868312L;

    /**
     * 主键
     */
    @TableId
    private Long id;
    /**
     * 用户名
     */
    private String userName;
    /**
     * 昵称
     */
    private String nickName;
    /**
     * 密码
     */
    private String password;
    /**
     * 账号状态（0正常 1停用）
     */
    private String status;
    /**
     * 邮箱
     */
    private String email;
    /**
     * 手机号
     */
    private String phonenumber;
    /**
     * 用户性别（0男，1女，2未知）
     */
    private String sex;
    /**
     * 头像
     */
    private String avatar;
    /**
     * 用户类型（0管理员，1普通用户）
     */
    private String userType;
    /**
     * 创建人的用户id
     */
    private Long createBy;
    /**
     * 创建时间
     */
    private Date createTime;
    /**
     * 更新人
     */
    private Long updateBy;
    /**
     * 更新时间
     */
    private Date updateTime;
    /**
     * 删除标志（0代表未删除，1代表已删除）
     */
    private Integer delFlag;
}
```

主类：

```java
@SpringBootApplication
@MapperScan("com.example.springsecurity.mapper")
public class SpringSecurityApplication {

    public static void main(String[] args) {
        ConfigurableApplicationContext run = SpringApplication.run(SpringSecurityApplication.class, args);
//        返回的是 Spring 容器
        System.out.println(11);
    }

}
```

yml配置：

```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/svue?characterEncoding=utf-8&serverTimezone=UTC
    username: root
    password: 725482520
    driver-class-name: com.mysql.cj.jdbc.Driver
```

测试：

```java
@SpringBootTest
class SpringSecurityApplicationTests {

    @Autowired
    private UserMapper userMapper;

    @Test
    void testUserMapper() {
        List list = userMapper.selectList(null);
        System.out.println(list);

    }

}
```

### 数据库校验核心代码实现

添加一个用户：

```sql
insert into sys_user values(1,'syhk','小天','{noop}123456','0','725482520@qq.com','15356042885','0',null,'0',null,null,null,null,0);
默认密码需要在前面加 {noop}
```

service包下：

```java
package com.example.springsecurity.service;

import com.baomidou.mybatisplus.core.conditions.query.LambdaQueryWrapper;
import com.example.springsecurity.domain.LoginUser;
import com.example.springsecurity.enity.User;
import com.example.springsecurity.mapper.UserMapper;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.authentication.dao.DaoAuthenticationProvider;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.stereotype.Service;

import java.util.Objects;

@Service
public class UserDetailsServiceImpl implements UserDetailsService {

    @Autowired
    private UserMapper userMapper;

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
//        查询用户信息
        LambdaQueryWrapper<User> queryWrapper=new LambdaQueryWrapper<>();
        queryWrapper.eq(User::getUserName,username);
        User user = userMapper.selectOne(queryWrapper);
//    如果没有查询到用户就抛出异常
        if(Objects.isNull(user)){
            throw new RuntimeException("用户名或者密码错误");
        }
//        把数据封闭成 UserDetails 返回

//        LoginUser 实现 UserDetails
        return  new LoginUser(user);
    }
}
```

domain 包下：

```java
package com.example.springsecurity.domain;


import com.example.springsecurity.enity.User;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;
import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.userdetails.UserDetails;

import java.util.Collection;

@Data
@AllArgsConstructor
@NoArgsConstructor
public class LoginUser implements UserDetails {

    private User user;


    @Override
    public Collection<? extends GrantedAuthority> getAuthorities() {
        return null;
    }

    @Override
    public String getPassword() {
        return user.getPassword();
    }

    @Override
    public String getUsername() {
        return user.getUserName();
    }

//    是否没过期
    @Override
    public boolean isAccountNonExpired() {
        return true;
    }

    @Override
    public boolean isAccountNonLocked() {
        return true;
    }

    @Override
    public boolean isCredentialsNonExpired() {
        return true;
    }

    @Override
    public boolean isEnabled() {
        return true;
    }
}
```

{noop}123456 这样写的原因：

实际项目中我们不会把密码明文存储在数据库中。

默认使用的PasswordEncoder要求数据库中的密码格式为：{id}password 。它会根据id去判断密码的加密方式。但是我们一般不会采用这种方式。所以就需要替换PasswordEncoder。

我们一般使用SpringSecurity为我们提供的BCryptPasswordEncoder。

我们只需要使用把BCryptPasswordEncoder对象注入Spring容器中，SpringSecurity就会使用该PasswordEncoder来进行密码校验。

我们可以定义一个SpringSecurity的配置类，SpringSecurity要求这个配置类要继承WebSecurityConfigurerAdapter。

到这里的文件:

[SpringSecurity.zip](file/SpringSecurity_S96DrNH_Ts.zip)

![](image/image_EhORFxuNPU.png#alt=)

```java
package com.kmair.exam.config;

import com.fasterxml.jackson.databind.ObjectMapper;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.authentication.*;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.core.Authentication;
import org.springframework.security.core.AuthenticationException;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.NoOpPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.security.web.RedirectStrategy;
import org.springframework.security.web.authentication.AuthenticationFailureHandler;
import org.springframework.security.web.authentication.AuthenticationSuccessHandler;
import org.springframework.security.web.authentication.logout.LogoutHandler;
import org.springframework.security.web.authentication.logout.LogoutSuccessHandler;

import javax.servlet.FilterChain;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;
import java.util.HashMap;
import java.util.Map;
import java.util.Objects;

@Configuration
public class MyWevSecurityConfig extends WebSecurityConfigurerAdapter {

    @Bean
    PasswordEncoder passwordEncoder(){
//        return NoOpPasswordEncoder.getInstance(); // 不对密码加密，开发者应该指定一种加密方式
//        设置密码加密
        return new BCryptPasswordEncoder(10);//默认值为 10 ；密钥的迭代次数
//        下方存放的密码要变成加密后的密码
    }

    @Override
    protected void configure(AuthenticationManagerBuilder aurh) throws Exception {
//        配置三个用户
      aurh.inMemoryAuthentication()
              .withUser("syhk").password("123456").roles("ADMIN","USER")
              .and()
              .withUser("sheng").password("123456").roles("USER") //sheng 具有 USER 角色权限
              .and()
              .withUser("root").password("123456").roles("ADMIN","DBA"); //root 具有 ADMIN 和 DBA 角色权限
    }

//    自定义受保护资源
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        //   测试连接    /admin/hello   /user/hello  /db/hello    /hello
        http.authorizeRequests() //开启 HttpSecurity 的配置
                .antMatchers("/admin/**") //表示要访问 /admin/** 路径下的资源必须具有 ADMIN 的角色才可以进行访问
                .hasRole("ADMIN")
                .antMatchers("/user/**")// 表示 要访问  /user/** 路径下的资源必须具有 ADMIN 或 USER 角色的权限   注意是这的 或
                .access("hasAnyRole('ADMIN','USER')")
                .antMatchers("/db/**")//表示要访问访问 /db/** 路径下的资源必须要具有 ADMIN 和 DBA 角色的权限  注意这里是和
                .access("hasRole('ADMIN') and hasRole('DBA')")
                .anyRequest()//这行和下面一行表示除了前面定义的 URL 模式之外，用户访问其他的 URL 资源都必须认证后访问也就是登录后访问
                .authenticated()
                .and()//==================
                .formLogin()
                .loginPage("/login_page") //配置登录自定义登录页面
//                =============================================================
                .loginProcessingUrl("/login")// 登录请求处理接口
                .usernameParameter("name")
                .passwordParameter("passwd")//上面这两行定义了认证所需要的用户名和密码，默认是username 和 password ，在这里可以自定义
//                =====================================
                .successHandler(new AuthenticationSuccessHandler() {
                    @Override
                    public void onAuthenticationSuccess(HttpServletRequest req, HttpServletResponse resp, Authentication auth) throws IOException, ServletException {
//                  auth 参数用来获取当前登录用户的信息
                        Object principal=auth.getPrincipal();
                        resp.setContentType("application/json;charet=utf-8");
                        PrintWriter out = resp.getWriter();
                        resp.setStatus(200);
                        Map<String,Object> map = new HashMap<>();
                        map.put("status",200);
                        map.put("msg",principal);
                        ObjectMapper om = new ObjectMapper();
                        out.write(om.writeValueAsString(map));
                        out.flush();
                        out.close();
                    }
//                    以上这个方法中定义了登录成功的处理逻辑。
                })
                .failureHandler(new AuthenticationFailureHandler() {
                    @Override
                    public void onAuthenticationFailure(HttpServletRequest req, HttpServletResponse resp, AuthenticationException e) throws IOException, ServletException {
                        resp.setContentType("application/json;charset=utf-8");
                        PrintWriter out=resp.getWriter();
                        resp.setStatus(401);
                        Map<String,Object> map=new HashMap<>();
                        map.put("status",401);
                        if(e instanceof LockedException){
                            map.put("msg","账户被锁定，登录失败！");
                        }else if(e instanceof BadCredentialsException){
                            map.put("msg","账户名或密码输入错误，登录失败！");
                        }else if(e instanceof DisabledException){
                            map.put("msg","账户被禁用，登录失败");
                        }else if (e instanceof AccountExpiredException){
                            map.put("msg","账户已过期，登录失败");
                        }else  if(e instanceof CredentialsExpiredException){
                            map.put("msg","密码已过期，登录失败");
                        }else {
                            map.put("msg","登录失败");
                        }
                    ObjectMapper om = new ObjectMapper();
                        out.write(om.writeValueAsString(map));
                        out.flush();
                        out.close();
                    }
//                    上面定义了登录失败的处理逻辑
                })
//                permitAll 表示和登录相关的接口都不需要谁即可访问
                .permitAll()//======================上面这四行表示开启单登录，同时配置了登录接口为 ”/login“ ,post 请求中 用户名必须为 username 密码必须命名为 password
                .and()
                //                ====================注销登录配置===========================
                .logout() //开启注销登录的配置
                .logoutUrl("/logout")//注销登录请求URL 为 /logout，默认也是这个
                .clearAuthentication(true)//是否清除身份认证信息，默认为 true
                .invalidateHttpSession(true)//是否使 Session 失效，默认为 true
                .addLogoutHandler(new LogoutHandler() {
                    @Override
                    public void logout(HttpServletRequest req, HttpServletResponse resp, Authentication auth) {
//                        这里面可以完成一些数据清除工作
                    }
                })
                .logoutSuccessHandler(new LogoutSuccessHandler() {
                    @Override
                    public void onLogoutSuccess(HttpServletRequest req, HttpServletResponse resp, Authentication auth) throws IOException, ServletException {
                    resp.sendRedirect("/login_page");//可以在这里返回 JSON 数据或跳转到登录页面
                    }
                })
                .and()
                .csrf()//这行和上面那行表示关闭 csrf
                .disable();

    }

}
```

```java
package com.kmair.exam.config;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.core.annotation.Order;
import org.springframework.security.access.hierarchicalroles.RoleHierarchy;
import org.springframework.security.access.hierarchicalroles.RoleHierarchyImpl;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.crypto.password.NoOpPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;

public class MyWevSecurityConfig1 {

    PasswordEncoder passwordEncoder(){
        return NoOpPasswordEncoder.getInstance();
    }

    //    也可以配置多个 HttpSecurity 实现对 WebSecurityConfigurerAdapter 的多次扩展
    @Autowired
    protected void configure(AuthenticationManagerBuilder auth) throws Exception{
        auth.inMemoryAuthentication()
                .withUser("admin").password("123").roles("ADMIN","USER")
                .and()
                .withUser("sheng").password("123").roles("USER");
    }

//    不需要继承 WebSecurityConfigurerAdapter ，创建静态内部类继承即可，静态内部类上添加 @Configuration 注解和 @Order 注解
    @Configuration
    @Order(1)
//    Order 注解表示该配置的优先级，数字越小优先级越高，未加 Order 注解的配置优先级最小
    public static class AdminSecurityConfig extends WebSecurityConfigurerAdapter{
        @Override
        protected void configure(HttpSecurity http) throws Exception {
            http.antMatcher("/admin/**").authorizeRequests()
                    .anyRequest().hasRole("ADMIN");
        }
    }

    @Configuration
    public static class OtherSecurityConfig extends WebSecurityConfigurerAdapter{
        @Override
        protected void configure(HttpSecurity http) throws Exception {
            http.authorizeRequests()
                    .anyRequest().authenticated()
                    .and()
                    .formLogin()
                    .loginProcessingUrl("/login")
                    .permitAll()
                    .and()
                    .csrf()
                    .disable();
        }
    }


//    角色继承： 就是像 admin 应该拥有 admin 的角色和 user 的角色
//    只需要我们提供一个RoleHierarchy就行
    @Bean
    RoleHierarchy roleHierarchy(){
        RoleHierarchyImpl roleHierarchy=new RoleHierarchyImpl();
        String hierarchy = "ROLE_dba > ROLE_admin   ROLE_admin > ROLE_user";
        roleHierarchy.setHierarchy(hierarchy);
        return roleHierarchy;
    }



}
```

```java
package com.kmair.exam.config;


import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.method.configuration.EnableGlobalMethodSecurity;

//通过注解配置
@Configuration
//prePostEnabled = true 会解锁 @PreAuthorize（注解会在方法执行前进行验证） 和 @PostAuthorize（会在方法执行后进行验证） 两个注解
//securedEnabled = true 会解锁 @Secured 注解
@EnableGlobalMethodSecurity(prePostEnabled = true,securedEnabled = true)
public class MyWebSecurityConfig2 {
//    接下来在 MethodService 中配置
}
```

MethodService

```java
package com.kmair.exam.service;

import org.springframework.security.access.annotation.Secured;
import org.springframework.security.access.prepost.PreAuthorize;
import org.springframework.stereotype.Service;

@Service
public class MethodService {

    @Secured("ROLE_ADMIN")
//    @Secured("ROLE_ADMIN") 表示访问该方法需要 ADMIN 角色，这里要角色前面加一个 ROLE_ 前缀
    public String admin(){
        return "Hello admin";
    }

    @PreAuthorize("hasRole('ADMIN') and hasRole('DBA')")
//     这个表示访问该方法既需要 ADMIN 又需要 DBA 角色
    public String dba(){
        return "Hello dba";
    }

    @PreAuthorize("hasAnyAuthority('ADMIN','DBA','USER')")
//    这个表示上面三个中的一个其中一个就可以访问
    public String user(){
        return "user";
    }
}
```

# WebSocket

[https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API)

服务端向客户端推送消息。

是一种单个TCP 连接上进行全双工通信的协议。它允许服务端主动向客户端推送数据。

```xml
<!--        WebSocket 依赖-->
        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-websocket -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-websocket</artifactId>
            <version>2.7.3</version>
        </dependency>
```

```java
package com.kmair.exam.config;

import org.springframework.context.annotation.Configuration;
import org.springframework.messaging.simp.config.MessageBrokerRegistry;
import org.springframework.web.socket.config.annotation.StompEndpointRegistry;
import org.springframework.web.socket.config.annotation.WebSocketMessageBrokerConfigurer;

@Configuration
public class WebSocketConfig implements WebSocketMessageBrokerConfigurer {
//继承 WebSocketMessageBrokerConfigurer 类进行 WebSocket　配置
    @Override
    public void configureMessageBroker(MessageBrokerRegistry registry) {
//        设置消息代理的前缀，如果消息前缀的 ”/topic", 就会将消息转发给消息代理(broker),再由消息代理将消息广播给当前连接的客户端
        registry.enableSimpleBroker("/topic");
//        配置一个或多个前缀，通过前缀过滤出需要被注解方法处理的消息；eg: “/app” 的消息可以通过 @MessageMapping 注解的方法处理，而其他将被直接交给 broker 处理
        registry.setApplicationDestinationPrefixes("/app");
    }

    /*
    它在使用时需要先创建连接，它是一种有状态的协议

    * 使用这个协议，当消息达到的时候，客户端和服务端都可以知道
    * WebSocket  在 端口 80 和 443 上创建，与 HTTP 使用的端口相同，基本上所有的防火墙都不会阻止它的连接
      心跳消息（ping 和 pong) 将被反复发送，让 WebSocket 一起保持活跃状态
      它在关闭时将发送一个特殊的关闭消息

    * 它支持跨域
    * 它支持协议扩展，可以定制自己的子协议
    * */
    @Override
    public void registerStompEndpoints(StompEndpointRegistry registry) {
        registry.addEndpoint("/chat").withSockJS();
    }
//    表示定义一个前缀为 "/chat" 的endPoint ，并开启 sockjs 支持可以解决浏览器对 WebSocket 的兼容性问题，客户端将通过这里的配置的 URL 来建立连接
    
}
```

windows 安装 redis

[https://github.com/MicrosoftArchive/redis/releases](https://github.com/MicrosoftArchive/redis/releases)
