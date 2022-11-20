# Swagger2

> 它是一个开源框架，可以帮助开发人员设计，构建，记录和使用 RESTful Web 服务，它将代码和文档融为一体。


添加依赖：

```xml
<!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger2 -->
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>3.0.0</version>
</dependency>
```

配置类：

```java
package com.kmair.exam.config;

import org.springframework.context.annotation.Configuration;
import springfox.documentation.builders.ApiInfoBuilder;
import springfox.documentation.builders.PathSelectors;
import springfox.documentation.builders.RequestHandlerSelectors;
import springfox.documentation.service.Contact;
import springfox.documentation.spi.DocumentationType;
import springfox.documentation.spring.web.plugins.Docket;
import springfox.documentation.swagger2.annotations.EnableSwagger2;

@Configuration
@EnableSwagger2
//通过这个注解开启 Swagger2，最主要是配置一个 Docket
public class SwaggerConfig {

    Docket docket(){
        return new Docket(DocumentationType.SWAGGER_2)
                .select()
                .apis(RequestHandlerSelectors.basePackage("org.syhk.controller"))
//                apis 方法配置要扫描的 controller 位置，通过 paths 配置路径
                .paths(PathSelectors.any())
                .build().apiInfo(new ApiInfoBuilder()
//                        apiInfo 中构建文档的基本的信息。
                        .description("项目接口测试文档")
                        .contact(new Contact("syhk","https://github.com/syhk","725482520@qq.com"))
                        .version("v1.0")
                        .title("API 测试文档")
                        .license("Apache2.0")
                        .licenseUrl("http://www.apache.org/licenses/LICENSE-2.0")
                        .build());

    }

}
```

Controller 类：

```java
package com.kmair.exam.controller;

import io.swagger.annotations.*;
import org.apache.catalina.User;
import org.springframework.web.bind.annotation.*;
import springfox.documentation.annotations.ApiIgnore;

//Swagger2 测试
@RestController
@Api(tags = "用户数据接口")
//Api 用在类上描述整个 Controller 的信息
public class TestSwaggercontroller {
    /*@ApiOperation 用在开发方法上，描述一个方法的基本信息， value 是对方法一个简短描述， notes 来备注这个方法详细作用
    @ApiImplicitParam用在方法上，描述方法的参数， paramType 是指方法参数的类型， 可选值有：path 参数获取方式 @PathVarible query 参数获取方式 @ResquestParam  header 参数获取方式 @RequestHeader , body 及 form
    name 表示参数名称和参数变量对应 , value 参数的描述信息 , required 这个参数是否必填, defaultValue 这个参数的默认值注意这些只是描述，并不能真正约束到
    *@ApiResponse  是对响应结果的描述 code 表示响应码， message 表示相应的描述信息,有多个放在 @ApiResponses 中

    * */

    @ApiOperation(value = "查询用户",notes = "根据id查询用户")
    @ApiImplicitParam(paramType = "path",name = "id",value = "用户 id",required = true)
    @GetMapping("/user/{id}")
   public String getUserById(@PathVariable Integer id){
       return "/user/"+id;
   }

   @ApiResponses({
           @ApiResponse(code = 200, message = "删除成功"),
           @ApiResponse(code = 500, message = "删除失败") })
   @ApiOperation(value = "删除用户",notes = "通过id删除用户")

   @DeleteMapping("/user/{id}")
   public Integer deleteUserById(@PathVariable Integer id){
        return id;
   }

   @ApiOperation(value = "添加用户", notes = "添加一个用户，传入用户名和地址")
   @ApiImplicitParams({
           @ApiImplicitParam(paramType = "query",name = "username",value = "用户名",required = true,defaultValue = "syhk"),
           @ApiImplicitParam(paramType = "query",name = "address",value = "用户地址",required = true,defaultValue = "云南 ")
   })
   @PostMapping("/user")
   public String addUser(@RequestParam String username,@RequestParam String address){
        return username+":  "+address;
   }

   @ApiOperation(value = "修改用户",nickname = "修改用户，传入用户信息")
   @PutMapping("/user")
   public String updateUser(@RequestBody String user){
        return user;
   }

   @GetMapping("/ignore")
   @ApiIgnore
//   这个表示不对某个接口生成文档
   public void ingoreMethod(){

   }

}
```
