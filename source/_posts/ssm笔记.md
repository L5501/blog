---
title: ssm构建笔记
description: ssm构建笔记
data: 2022-11-26 19:50:00
updata: 2022-11-26 19:50:00
tags: 
  - java
categories:
  - java笔记
---
# ssm笔记

### 原始版构建方式

1. alt+insert添加依赖，引入spring-webmvc依赖

2. open moudle setting 配置web中的web Resource Directories路径和Deployment Descriptos路径

3. 创建任意controller

4. 于resources文件夹中创建applicationContext.xml和spring-servlet.xml

5. applicationContext.xml配置，先配置需要扫描的包

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/c"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

       <!-- base-package 表示要扫描的包；use-default-filters表示使用默认的过滤器，true表示全扫-->
       <context:component-scan base-package="org.ll" use-default-filters="true">
           <!--除去controller，其他的都扫-->
           <context:exclude-filter type="annotation" experssion="org.springframework.stereotype.Controller"/>
       </context:component-scan>

       <!--下面可配mybatis-->
   </beans>
   ```

6. spring-servlet.xml的配置

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/c"
          xmlns:mvc="http://www.springframework.org/schema/mvc"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">

       <!--不使用默认的过滤器，需要指定扫描的包-->
       <context:component-scan base-package="org.ll" user-default-filters="false">
           <!--包括哪个，即指定哪个包-->
           <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
       </context:component-scan>

       <mvc:annotation-driven/>
       <!--视图解析器等在下面配-->
   </beans>
   ```

7. web.xml文件配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <!--加载spring的配置文件-->
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:applicationContext.xml</param-value>
    </context-param>
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>
    <!--spring mvc的配置文件-->
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:spring-servlet.xml</param-value>
        </init-param>
    </servlet>
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
</web-app>
```