---
title: SpringBootApplication 注解
description: SpringBootApplication 注解
data: 2022-12-18 20:42:00
updata: 2022-12-18 20:42:00
tags: 
  - java
categories:
  - java笔记
---
## @SpringBootApplication 注解

@SpringBootApplication是一个组合注解，主要由@SpringBootConfiguration、@EnableAutoConfiguration和@ComponentScan组成

### @SpringBootConfiguration详解

@SpringBootConfiguration实际上就是@Configuration，功能上没有太大区别

#### @Configuration 注解

表示该类为配置类，在里面注入Bean

##### @Configuration和@Component 的区别

1. spring容器在启动时会默认加载一些后置处理器，其中有个处理器就叫ConfigurationClassPostProcessor，这个处理器会专门处理带有@Configuration注解的类。它会将所有带有@Conguration的类存进指定容器，然后通过cglib代理进行增强，对于已经被创建的对象不会进行重新创建。而@Component则没有被代理，会重复创建对象。

### @EnableAutoConfiguration注解

开启自动化配置，主要依靠@Import和@AutoConfigurationPackage,实际上就是导入了两个配置类AutoConfigurationImportSelector和Registrar

#### 首先我们解析@import的作用：

在原生的spring framework中，组件装配经历了三个阶段：

1. spring 2.5+开始，可以使用@component等注解来装配bean

2. spring 3.0+开始，使用@Configuration + @bean的方式

3. spring 3.1+ 开始，使用模块装配，比如@EnableXXX + @Import，下为演示

   1. 首先创建一个实体类

      ```java
      public class Apple {

          private String name;

          public String getName() {
              return name;
          }

          public void setName(String name) {
              this.name = name;
          }
      }
      ```

   2. 然后自定义一个注解

      ```java
      @Retention(RetentionPolicy.RUNTIME)
      @Target(ElementType.TYPE)
      @Import({Apple.class})
      public @interface EnableFruit {
      }
      ```

   3. 在启动类中加入注解

      ```java
      @SpringBootApplication
      @EnableFruit
      public class DemoApplication {

          public static void main(String[] args) {
              SpringApplication.run(DemoApplication.class, args);
          }

      }
      ```

   4. 在测试类中进行测试

      ```java
      @SpringBootTest
      class DemoApplicationTests {

          @Autowired
          Apple apple;
          
          @Test
          void contextLoads() {
              System.out.println("apple = " + apple);
          }

      }
      ```

   5. 如果在控制台看到下列结果，就可证明注入成功

      ```java
      apple = com.example.demo.module.Apple@75798d03
      ```

### @ComponentScan注解

包扫描，**默认情况下扫描的是当前这个类所在的包下面的所有类**，所以建议放在根包下面，否则需要进行重新指定