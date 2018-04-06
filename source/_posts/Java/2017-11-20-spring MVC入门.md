---
title: spring MVC 入门
date: 2017-11-20 8:10:09
category: Java
tags: Java
keywords: Java
---
Spring Web MVC是一种基于Java的实现了Web MVC设计模式的请求驱动类型的轻量级Web框架.
<!--more-->
## Spring MVC请求处理的整体流程如图：
![](http://okjl482qy.bkt.clouddn.com/springMVC_1_01.png)
## 创建一个hello world程序
打开idea应用，点击create new project 
![](http://okjl482qy.bkt.clouddn.com/idea_1_01.png)
进入如图界面，首先选择左边栏Maven，再配置JDK(如果之前添加了JDK的话会自动填充，如未添加点击旁边的New将JDK目录导入即可)。勾选"Create from archetype"，然后选中maven-archetype-webapp，点Next，进入如下界面：
![](http://okjl482qy.bkt.clouddn.com/idea_1_02.png)
这里需要填写GroupId和ArtifactId,Version默认即可，这三个属性可以唯一标识你的项目。
![](http://okjl482qy.bkt.clouddn.com/idea_1_03.png)
我自己的maven配置
![](http://okjl482qy.bkt.clouddn.com/idea_1_04.png)
填写项目名，选择项目保存路径，点击Finish：
![](http://okjl482qy.bkt.clouddn.com/idea_1_05.png)
maven会在后台生成web项目，这需要等待一定的时间，视网络环境而定.
下图展示了该项目的文件结构。可以发现，它在src/main下创建了一个recources文件夹，该文件夹一般用来存放一些资源文件，还有一个webapp文件夹，用来存放web配置文件以及jsp页面等，这已经组成了一个原始的web应用。选择右边红框的Enable-Auto- Import，可以在每次修改pom.xml后，自动的下载并导入jar包。
我们可以看到，目录结构并不是严格的maven格式,因为少了java源码文件夹
首先在main文件夹下创建一个文件夹，名称为Java,然后将Java文件夹标识为Source Root
![](http://okjl482qy.bkt.clouddn.com/idea_1_06.png)

## Maven自动导入jar包
既然我们要用Spring MVC开发，那肯定少不了Spring MVC的相关jar包。如果不使用Maven的话，那就需要去官网下载相关的jar包，然后导入到项目中。现在使用maven的话，就不需要上网找jar包了。
Maven所做的工作其实很简单，就是自动把你需要的jar包下载到本地，然后关联到项目中来。maven的所有jar包都是保存在几个中央仓库里面的，其中一个最常用的是Maven Repository，即，你需要什么jar包，它就会从仓库中拿给你。那么如何告诉maven需要什么jar包呢？我们看看工程目录，能找到一个pom.xml文件 ，maven就是靠它来定义需求的，代码如下：
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.zou</groupId>
  <artifactId>helloworld</artifactId>
  <packaging>war</packaging>
  <version>1.0-SNAPSHOT</version>
  <name>helloworld Maven Webapp</name>
  <url>http://maven.apache.org</url>
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>


    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-web</artifactId>
      <version>4.3.6.RELEASE</version>
    </dependency>

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>4.3.6.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-tx</artifactId>
      <version>4.3.6.RELEASE</version>
    </dependency>

    <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-core</artifactId>
      <version>2.8.6</version>
    </dependency>

    <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-annotations</artifactId>
      <version>2.8.6</version>
    </dependency>

    <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-databind</artifactId>
      <version>2.8.6</version>
    </dependency>
  </dependencies>
  <build>
    <finalName>helloworld</finalName>
  </build>
</project>
```
## SpringMVC框架配置
进行完上面的配置，那就说明现在基本的开发环境已经搭建好了，现在要开始进行Spring MVC的网站开发。
1、web.xml配置
打开src\main\webapp\WEB-INF\下的web.xml文件,修改约束文件，如下：
```xml
<!DOCTYPE web-app PUBLIC
 "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
 "http://java.sun.com/dtd/web-app_2_3.dtd" >

<web-app>
  <display-name>Archetype Created Web Application</display-name>
  <servlet>
    <servlet-name>dispatcher</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <load-on-startup>1</load-on-startup>
  </servlet>

  <servlet-mapping>
    <servlet-name>dispatcher</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>

  <servlet-mapping>
    <servlet-name>default</servlet-name>
    <url-pattern>*.jpg</url-pattern>
  </servlet-mapping>

  <servlet-mapping>
    <servlet-name>default</servlet-name>
    <url-pattern>*.js</url-pattern>
  </servlet-mapping>

  <servlet-mapping>
    <servlet-name>default</servlet-name>
    <url-pattern>*.css</url-pattern>
  </servlet-mapping>
</web-app>
```
2.dispatcher-servlet.xml配置
在配置完web.xml后，需在WEB-INF目录下新建 dispatcher-servlet.xml（[servlet-name]-servlet.xml是固定规则，前面是在servlet里面定义的servlet名）：
```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/mvc
        http://www.springframework.org/schema/mvc/spring-mvc-4.0.xsd"
>

    <!-- 自动注册DefaultAnnotationHandlerMapping与AnnotationMethodHandlerAdapter 两个bean,是spring MVC为@Controllers分发请求所必须的。-->
    <mvc:annotation-driven/>
    <!-- 标签是告诉Spring 来扫描指定包下的类，并注册被@Component，@Controller，@Service，@Repository等注解标记的组件。-->
    <context:component-scan base-package="com.zou.controller"/>

    <!-- mvc view 对应文件的前缀与后缀  action 返回值 为  "index"  => "/index.jsp"  -->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/"/>
        <property name="suffix" value=".jsp"/>
    </bean>

    <!-- @ResponseBody 返回 json 格式数据 -->
    <bean id="messageAdapter" class="org.springframework.web.servlet.mvc.annotation.AnnotationMethodHandlerAdapter">
        <property name="messageConverters">
            <list>
                <!-- Support JSON -->
                <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter"/>
            </list>
        </property>
    </bean>
    <bean id="exceptionMessageAdapter" class="org.springframework.web.servlet.mvc.annotation.AnnotationMethodHandlerExceptionResolver">
        <property name="messageConverters">
            <list>
                <!-- Support JSON -->
                <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter"/>
            </list>
        </property>
    </bean>
</beans>
```
## java代码
     MVC框架有model、view、controller三部分组成。model一般为一些基本的Java Bean，view用于进行相应的页面显示，controller用于处理网站的请求。
在src\java中新建一个用于保存controller的package：在controller包中新建java类testpage（名称并不固定，可任意取），并修改如下：
```java
package com.zou.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.ResponseBody;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.servlet.ModelAndView;

@Controller

public class testPage {
    @RequestMapping("/helloworld")
    public String hello(){
        System.out.println("hello world dddddd");
        return "success";
    }

    @ResponseBody
    @RequestMapping(value="/body/{x}", method = RequestMethod.GET)
    public bodytest getBody(@PathVariable("x") String x){
        System.out.println("URI Part 1 : " + x);
        bodytest bt = new bodytest();
        bt.a = x;
        bt.b = "123";
        bt.c = "dfdfdk";
        return bt;
    }

    public class  bodytest
    {
        public  String a;
        public String b;
        public  String c;
    }

    /*
    *
    @RequestMapping(value = "/user/{userId}/roles/{roleId}", method = RequestMethod.GET)
    public String getLogin(@PathVariable("userId") String userId,
                           @PathVariable("roleId") String roleId) {

        System.out.println("User Id : " + userId);
        System.out.println("Role Id : " + roleId);
        return "success";
    }
    @RequestMapping(value="/product/{productId}",method = RequestMethod.GET)
    public String getProduct(@PathVariable("productId") String productId){
        System.out.println("Product Id : " + productId);
        return "success";
    }
    * */
}
```
--@Controller注解：采用注解的方式，可以明确地定义该类为处理请求的Controller类；
--@RequestMapping()注解：用于定义一个请求映射，value为请求的url，值为 /helloworld 说明，该请求首页请求，method用以指定该请求类型，一般为get和post；
--return "success"：处理完该请求后返回的页面，此请求返回 success.jsp页面。
success.jsp:
```jsp
<%--
  Created by IntelliJ IDEA.
  User: one
  Date: 2017/11/25
  Time: 上午10:14
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>我的第一个maven工程</title>
</head>
<body>
<h1>我的第一个maven工程</h1>
</body>
</html>

```
## 需要配置Tomcat来运行该项目。
Run->Edit Configurations
点击左上角的"+"号，选择Tomcat Server，再选择Local：
点击 Application server 右边的 Configure，导入Tomcat 目录：
![](http://okjl482qy.bkt.clouddn.com/idea_1_07.png)
在配置好Tomcat的路径后，如下图所示，发现依然存在警告，且左方的Tomcat8图标上有一个警告标记，说明还没有配置完全：
我们还需要将项目部署到 Tomcat 服务器中。点击 Deployment，再点击右边的"+"号，添加一个Artifact.
选择第二个：war exploded，点击OK，这样，该项目就已经部署到了tomcat中.
再点击OK，整个Tomcat配置结束.
启动 Tomcat 了，其控制台输出将在IDEA下方显示
启动后，浏览器将自动弹出项目首页.
输入http://localhost:8080/helloworld
输出:
![](http://okjl482qy.bkt.clouddn.com/idea_1_08.png)
http://localhost:8080/body/4543
输出
![](http://okjl482qy.bkt.clouddn.com/idea_1_09.png)

