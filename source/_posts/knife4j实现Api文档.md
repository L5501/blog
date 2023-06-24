---
title: knife4j制作api文档
description: spring boot整合knife4j
data: 2023-6-23
updata: 2023-6-23
tags: 
  - java
categories:
  - java
---

### 引入knife4j

|   版本    |                         说明                         |
| :-------: | :--------------------------------------------------: |
|   1.9.6   |       蓝色皮肤风格，开始更名，增加更多后端模块       |
| 2.0~2.0.5 |      Ui重写，底层依赖的springfox框架版本是2.9.2      |
|  2.0.6~   |    springfox框架版本升级至2.10.5，OpenAPI规范是V2    |
|   3.0~    | 底层依赖spingfox框架版本升级至3.0.3，OpenAPI规范是V3 |

以下为2.0.9依赖，相对比较稳定，3.X坑有点多

```java
<dependency>
            <groupId>com.github.xiaoymin</groupId>
            <artifactId>knife4j-spring-boot-starter</artifactId>
            <version>2.0.9</version>
        </dependency>
```

### 创建Swagger配置类

```java
import io.swagger.annotations.ApiOperation;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import springfox.documentation.builders.ApiInfoBuilder;
import springfox.documentation.builders.RequestHandlerSelectors;
import springfox.documentation.service.ApiInfo;
import springfox.documentation.service.Contact;
import springfox.documentation.spi.DocumentationType;
import springfox.documentation.spring.web.plugins.Docket;
import springfox.documentation.swagger2.annotations.EnableSwagger2WebMvc;

@Configuration
@EnableSwagger2WebMvc
public class SwaggerConfig {
    // 创建Docket存入容器，Docket代表一个接口文档
    @Bean
    public Docket webApiConfig(){
        return new Docket(DocumentationType.SWAGGER_2)
                // 创建接口文档的具体信息
                .apiInfo(webApiInfo())
                // 创建选择器，控制哪些接口被加入文档
                .select()
                // 指定@ApiOperation标注的接口被加入文档
                .apis(RequestHandlerSelectors.withMethodAnnotation(ApiOperation.class))
                .build();
    }

    // 创建接口文档的具体信息，会显示在接口文档页面中
    private ApiInfo webApiInfo(){
        return new ApiInfoBuilder()
                // 文档标题
                .title("标题：系统接口文档")
                // 文档描述
                .description("描述：本文档描述了系统的接口定义")
                // 版本
                .version("1.0")
                // 联系人信息
                .contact(new Contact("龙场悟道中", "https://mentaltest.run", "mentaltest1225@gmail.com"))
                // 版权
                .license("龙场悟道中")
                // 版权地址
                .licenseUrl("https://mentaltest.run")
                .build();
    }
```

### 实体类测试

如果只是做了这些配置，并不会在接口文档中记录，如果需要被记录，则必须在加了swagger注解的controller中使用

```java
import io.swagger.annotations.ApiModel;
import io.swagger.annotations.ApiModelProperty;
import lombok.Data;

@Data
@ApiModel("手机实体类")
public class Phone {

    @ApiModelProperty("id")
    private String id;

    @ApiModelProperty("手机名称")
    private String name;

    @ApiModelProperty("手机价格")
    private Double price;
}
```

### 控制类测试

```java
import com.ll.knife4j_demo.entity.Phone;
import com.ll.knife4j_demo.entity.User;
import io.swagger.annotations.Api;
import io.swagger.annotations.ApiOperation;
import io.swagger.annotations.ApiParam;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("controller1")
@Api(tags = "这是一个应用")
public class TestController {

    @GetMapping("api1/{id}")
    @ApiOperation("api1")
    public void api1(@PathVariable("id") @ApiParam("用户id") String id){
        // 伪代码
    }

    @PostMapping("api2")
    @ApiOperation("api2")
    // 如果这里没有引入User类，则不会在文档中出现User
    public void api2(@RequestBody User user) {
        // 伪代码
    }
}

```

### YML文件相关配置

```yml
server:
  port: 1225
spring:
  application:
    name: demo

knife4j:
  # 开启增强配置
  enable: true
  # 开启认证功能，默认为false
  basic:
    enable: true
    # 用户名
    username: luke
    # 密码
    password: 123456
```

