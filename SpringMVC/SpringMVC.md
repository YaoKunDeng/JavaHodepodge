# SpringMVC概述

ssm：mybatis+Spring+SpringMVC  

Spring的关键就是：视图解析器、控制层、处理映射器、处理适配器



MVC三层架构：模型（dao，service）、视图（jsp）、控制器（servlet）

<img src="C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220425231944051.png" alt="image-20220425231944051" style="zoom:80%;" />

dao

service

servlet：转发，重定向

jsp/html



真实开发中，有这样的概念：

前端、数据传输、实体类（前端传的东西，实体类不一样要）

实体类：用户名，密码，生日，爱好。。。。。20ge

前端：用户名，密码



pojo:User

vo:Uservo(视图（实体类），只写两个字段)

dto：数据传输时的对象



**JSP：本质就是一个Servlet**





假设面试官问：你的项目的架构，时设计好的，还是**演进**的？

- Alibaba  从国外买PHP项目，改一改
- 随着用户量越来越大，改用java
- 改java还是不行=>王坚  去IOE  MySQL
- MySql：MySQL->AliSQL、AliRedis
- All in one  -->微服务



1. 创建新项目，删除src成为父工程

2. 导入公共依赖

   ```xml
    <!--    导入依赖-->
       <dependencies>
           <dependency>
               <groupId>junit</groupId>
               <artifactId>junit</artifactId>
               <version>4.13.1</version>
               <scope>test</scope>
           </dependency>
   
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-webmvc</artifactId>
               <version>5.3.19</version>
           </dependency>
   
           <dependency>
               <groupId>javax.servlet</groupId>
               <artifactId>servlet-api</artifactId>
               <version>2.2</version>
           </dependency>
   
           <dependency>
               <groupId>javax.servlet</groupId>
               <artifactId>jstl</artifactId>
               <version>1.2</version>
           </dependency>
       </dependencies>
   
   
   ```


**我们为什么要学习SpringMVC呢？**

SpringMVC的特点：

1. 轻量级，简单易学
2. 高效，基于请求响应的MVC框架
3. 与Spring兼容性好，无缝结合
   1. Spring：大杂烩，我们可以将SpringMVC中所有要用到的bean，，注册到Spring中！
4. 约定优于配置
5. 功能强大:RESTful，数据验证、格式化、本地化、主题等
6. 简介灵活

Spring 的web框架围绕DispatchServlet【调度Servlet】设计



**中心控制器**

​	Spring的web框架围绕DispatchServlet设计。DispatchServlet的作用是将请求分发到不同的处理器。从Spring2.5开始，使用java5或者以上版本的用户可以采用基于注解的controller声明方式



SpringMVC框架像许多其他MVC框架一样，**以请求为驱动，围绕一个中心Servlet分派请求及提供其他功能，DispatchServlet是一个实际的Servlet（它继承自HttpServlet基类）**

![image-20220501233728906](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220501233728906.png)

（只要实现了Servlet接口的类，就叫Servlet，DispatchServlet也实现了Servlet，本质也是Servlet）



下图展示了Spring Web MVC的`DispatcherServlet`处理请求的工作流。熟悉设计模式的朋友会发现，`DispatcherServlet`应用的其实就是一个“前端控制器”的设计模式（其他很多优秀的web框架也都使用了这个设计模式）。

![image-20220501234300159](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220501234300159.png)



![image-20220507114656929](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220507114656929.png)



# 1.第一个SpringMVC程序

## 1.创建web应用并导入jar包

```xml
    <!--    导入依赖-->
    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13.1</version>
            <scope>test</scope>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.3.19</version>
        </dependency>

        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>servlet-api</artifactId>
            <version>2.2</version>
        </dependency>

        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>jstl</artifactId>
            <version>1.2</version>
        </dependency>
    </dependencies>

```



## 2.SpringMVC配置

Spring MVC 是基于 Servlet 的，DispatcherServlet 是整个 Spring MVC  框架的核心，主要负责截获请求并将其分派给相应的处理器处理。所以配置 Spring MVC，首先要定义 DispatcherServlet。跟所有 Servlet 一样，用户必须在 web.xml 中进行配置。

### 	1）定义DispatcherServlet

在开发 Spring MVC 应用时需要在 web.xml 中部署 DispatcherServlet，代码如下

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns="http://java.sun.com/xml/ns/javaee" xmlns:web="http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
    xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
    version="3.0">
    <display-name>springMVC</display-name>
    <!-- 部署 DispatcherServlet -->
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!-- 表示容器再启动时立即加载servlet -->
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <!-- 处理所有URL -->
        <url-pattern>/</url-pattern>
    </servlet-mapping>
</web-app>
```

Spring MVC 初始化时将在应用程序的 WEB-INF 目录下查找配置文件，该配置文件的命名规则是“servletName-servlet.xml”，例如 springmvc-servlet.xml。

 也可以将 Spring MVC 的配置文件存放在应用程序目录中的任何地方，但需要使用 servlet 的 init-param  元素加载配置文件，通过 contextConfigLocation 参数来指定 Spring MVC 配置文件的位置，示例代码如下。

```xml
<!-- 部署 DispatcherServlet -->
<servlet>
    <servlet-name>springmvc</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet </servlet-class>
    <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:springmvc-servlet.xml</param-value>
    </init-param>
    <!-- 表示容器再启动时立即加载servlet -->
    <load-on-startup>1</load-on-startup>
</servlet>

<!--
	在pringmvc中， /   /*
	/:只匹配所有请求，不回去匹配.jsp
	/*:匹配所有的请求，包括jsp页面
-->
<servlet-mapping>
    <servlet-name>springmvc</servlet-name>
    <url-pattern>/</url-pattern>
</servlet-mapping>
```

此处使用 Spring 资源路径的方式进行指定，即 `classpath:springmvc-servlet.xml`。

 上述代码配置了一个名为“springmvc”的 Servlet。该 Servlet 是 DispatcherServlet 类型，它就是 Spring MVC 的入口，并通过 `<load-on-startup>1</load-on-startup>` 配置标记容器在启动时就加载此 DispatcherServlet，即自动启动。然后通过 servlet-mapping 映射到“/”，即 DispatcherServlet 需要截获并处理该项目的所有 URL 请求。

### 2）创建Spring MVC配置文件

在 WEB-INF 目录下创建 springmvc-servlet.xml 文件，如下所示。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

<!--    处理器映射器:这个处理器要通过bean映射Controller类，以后不用-->
    <bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>

<!--    处理器适配器-->
    <bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"/>
<!--    上面这两个配置不用手动配置，这里只是讲原理-->

<!--    视图解析器:模板引擎 Thymeleaf  Freemarker....-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
<!--        前缀-->
        <property name="prefix" value="/WEB-INF/jsp/"/>
<!--        后缀-->
        <property name="suffix" value=".jsp"/>
    </bean>

<!--    BeanNameUrlHandlerMapping:bean-->
    <bean id="/hello" class="com.kun.controller.HelloController"></bean>

</beans>
```

## 3.Controller类实现

```java
package com.kun.controller;

import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.mvc.Controller;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class HelloController implements Controller {
    @Override
    public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {
        ModelAndView mv = new ModelAndView();

        //业务代码
        String result = "HelloSpringMVC";

        mv.addObject("msg",result);

        //视图跳转
        mv.setViewName("test");

        return mv;
    }
}

```

使用Springmvc后，不再需要实现Servlet接口，直接实现Controller接口替换Srvlet；因为springmvc本质上就是一个Servlet，DispatcherServlet已经实现了Servlet接口，而Springmvc就是围绕DispatcherServlet来实现请求和转发的！



**注：代码正确，但是报404错误的时候，有时候是环境问题，在artifacet下添加lib文件，导入相关的jar报即可**

![image-20220507095045572](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220507095045572.png)

【总结】

创建项目步骤：

​	创建新项目-->添加web框架支持【最新版】--->注册DispatcherServlet--->关联SpringMVC的配置文件--->启动级别为1---->映射路径为/【不要用/*，会404】-->配置文件中，配置视图解析器



# 2.使用注解开发

sprinmvc配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="
            http://www.springframework.org/schema/beans
            http://www.springframework.org/schema/beans/spring-beans.xsd
            http://www.springframework.org/schema/context
            http://www.springframework.org/schema/context/spring-context.xsd
            http://www.springframework.org/schema/mvc
            http://www.springframework.org/schema/mvc/spring-mvc.xsd">
<!--    自动扫描包，让指定包下的注解生效，由IOC容器统一管理-->
    <context:component-scan base-package="com.kun.Controller" />
<!--    让Spring MVC不要处理静态资源   .css .js .html. mp3 .mp4-->
    <mvc:default-servlet-handler/>
<!--
    支持mvc注解驱动
        在Spring中一般采用@RequestMapping注解来完成映射关系
        要想使@RequestMapping注解生效
        必须向上下文中注册DefaultAnnotationHandlerMapping
        和一个AnnotationMethodHandleAdapter实例
        这两个实例分别在类级别和方法级别处理
        而annotation-driven配置帮我嗯自动完成相面两个实例的注入
-->
    <mvc:annotation-driven/>
    <bean id="InternalResourceViewResolver"
          class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <!--前缀 -->
        <property name="prefix" value="/WEB-INF/jsp/" />
        <!--后缀 -->
        <property name="suffix" value=".jsp" />
    </bean>
</beans>
```

web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc-servlet.xml</param-value>
        </init-param>
        
        <load-on-startup>1</load-on-startup>
    </servlet>
    
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
    
</web-app>
```

Controller类

```java
package com.kun.Controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller  //代表这个类会被spring接管，这个注解的类中的所有方法，如果返回值使String，并且由具体的页面可以跳转，那么就会被视图解析器解析
public class HelloController {

    @RequestMapping("/hello")
    public String hello(Model model){
        //封装数据
        model.addAttribute("msg","Hello,SpringMVCAnnotation!");

        return "hello";   //会被视图解析器处理
    }


}

```

**注意：使用注解开发，Controller类和spring.xml配置文件与不用注解开发是有区别的**



# 3.RestFul风格

**概念**

​	RestFul就是一个资源定位及资源操作的风格。不是标准也不是协议，只是一种风格。基于这个风格设计的软件可以更加简洁，更有层次，更易于实现缓存等机制.

**功能**

- 资源：互连网所有的事务都可以被抽象为资源
- 资源操作：使用POST、DELETE、PUT、GET，使用不同方法对资源进行操作
- 分别对应添加、删除、删除、查询，



**传统方式操作资源：**通过不同的参数来实现不同的效果！方法单已，post和get

- http：//127.0.0.1/item.queryItem.action?id=1  查询，GET
- http：//127.0.0.1/item.queryItem.action?id=1  新增，POST



使用RestFul操作资源：可以通过不同的请求方式来实现不同的效果！如下：请求地址一样，但功能可以不同！

- http://127.0.0.1/item/1 查询get
- http://127.0.0.1/item  新增POST



**学习测试**

1. 新建一个类RestFulController

   ```java
   @Controller
   public class RestFulController {
   }
   ```

   

2. 在springMVC中可以使用@PathVariable注解，让方法参数的值对应绑定导一个URI模板变量上

```java
public class RestFulController {

    //原来的   :   http://localhost:8080/add?a=1&b=2
    //RestFul   :   http://localhost:8080/add/a/b    http://localhost:8080/add/1/2

    //@RequestMapping(value = "/add/{a}/{b}",method = RequestMethod.GET)
    @GetMapping("/add/{a}/{b}")
    public String test1(@PathVariable int a, @PathVariable int b, Model model){
        int res = a+b;
        model.addAttribute("msg","结果为："+res);
        return "hello";
    }
}
```

GET、POST、DELETE、PUT有对应的Mapping

![image-20220507203659731](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220507203659731.png)



# 4、SpringMVC：前端数据处理

### **1、提交的域名称和处理方法的参数名一致**

提交数据：http://locaohost:8080/hello?name=kuangsheng

处理方法：

```java
@RequestMapping("/hello")
public String hello(String name){
	System.out.pringtln(name);
    return "hello";
}
```

后台输出：kuangsheng

### **2、提交的域名称和处理方法的参数不一致**

提交数据：http://locaohost:8080/hello?username=kuangsheng

处理方法：

```java
//@RequstParam("username")   :   username 提交的域的名称
@RequestMapping("/hello")
public String hello(@RequestParam("username") String name){
	System.out.pringtln(name);
    return "hello";
}
```

### **3、提交的是一个对象**

要求提交的表单域和对象的属性名一致，参数使用对象即可

1. 实体类

```java
public class User{
    private int id;
    private String name;
    private int age;
    //构造
    //get/set
    //toString()
}
```

2. 提交数据：http://localhost:8080/mvc04/user?name=kuangshen&id=1&age=15
3. 处理方法

```java
@RequestMapping("/user")
public String user(User user){
	System.out.println(user);
    return "hello"
}
```



# 5、乱码问题

通过配置SpringMVC的乱码过滤器来实现【也可以自己写过滤器】

```xml
<!--   2.配置SpringMVC的乱码过滤-->
   <filter>
        <filter-name>encoding</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>utf-8</param-value>
        </init-param>
    </filter>

    <filter-mapping>
        <filter-name>encoding</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
```

**注：极端情况下，这个过滤器对get支持不好，不能解决乱码问题**



处理方法：

1. 修改tomcat配置文件：server.xml设置编码！

```xml
<Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" 
               URIEncoding="UTF-8"/>
```



2. 自定义通用过滤器

```java

import javax.servlet.*;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletRequestWrapper;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.UnsupportedEncodingException;
import java.util.Map;

public class GenericEncodingFilter implements Filter {

    @Override
    public void destroy() {
    }

    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        //处理response的字符编码
        HttpServletResponse myResponse=(HttpServletResponse) response;
        myResponse.setContentType("text/html;charset=UTF-8");

        // 转型为与协议相关对象
        HttpServletRequest httpServletRequest = (HttpServletRequest) request;
        // 对request包装增强
        HttpServletRequest myrequest = new MyRequest(httpServletRequest);
        chain.doFilter(myrequest, response);
    }

    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
    }

}

//自定义request对象，HttpServletRequest的包装类
class MyRequest extends HttpServletRequestWrapper {

    private HttpServletRequest request;
    //是否编码的标记
    private boolean hasEncode;
    //定义一个可以传入HttpServletRequest对象的构造函数，以便对其进行装饰
    public MyRequest(HttpServletRequest request) {
        super(request);// super必须写
        this.request = request;
    }

    // 对需要增强方法 进行覆盖
    @Override
    public Map getParameterMap() {
        // 先获得请求方式
        String method = request.getMethod();
        if (method.equalsIgnoreCase("post")) {
            // post请求
            try {
                // 处理post乱码
                request.setCharacterEncoding("utf-8");
                return request.getParameterMap();
            } catch (UnsupportedEncodingException e) {
                e.printStackTrace();
            }
        } else if (method.equalsIgnoreCase("get")) {
            // get请求
            Map<String, String[]> parameterMap = request.getParameterMap();
            if (!hasEncode) { // 确保get手动编码逻辑只运行一次
                for (String parameterName : parameterMap.keySet()) {
                    String[] values = parameterMap.get(parameterName);
                    if (values != null) {
                        for (int i = 0; i < values.length; i++) {
                            try {
                                // 处理get乱码
                                values[i] = new String(values[i]
                                        .getBytes("ISO-8859-1"), "utf-8");
                            } catch (UnsupportedEncodingException e) {
                                e.printStackTrace();
                            }
                        }
                    }
                }
                hasEncode = true;
            }
            return parameterMap;
        }
        return super.getParameterMap();
    }

    //取一个值
    @Override
    public String getParameter(String name) {
        Map<String, String[]> parameterMap = getParameterMap();
        String[] values = parameterMap.get(name);
        if (values == null) {
            return null;
        }
        return values[0]; // 取回参数的第一个值
    }

    //取所有值
    @Override
    public String[] getParameterValues(String name) {
        Map<String, String[]> parameterMap = getParameterMap();
        String[] values = parameterMap.get(name);
        return values;
    }
}

```

在web.xml中配置

```xml
<filter>
    <filter-name>encoding</filter-name>
    <filter-class>com.kun.Filter.GenericEncodingFilter</filter-class>
</filter>

<filter-mapping>
    <filter-name>encoding</filter-name>
    <url-pattern>/*</url-pattern>  //这里一定要/*;/只是处理部分，/*包括了jsp页面
</filter-mapping>
```





# 6、使用Json



## 1、导入依赖

```xml
<!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-databind -->
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.13.1</version>
</dependency>

```



## 2、配置web.xml

```xml
 <servlet>
        <servlet-name>springmvc-servlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>

        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc-servlet.xml</param-value>
        </init-param>

        <load-on-startup>1</load-on-startup>
    </servlet>

    <servlet-mapping>
        <servlet-name>springmvc-servlet</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>

    <filter>
        <filter-name>encoding</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>encoding</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>

```

## 3、配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="
            http://www.springframework.org/schema/beans
            http://www.springframework.org/schema/beans/spring-beans.xsd
            http://www.springframework.org/schema/context
            http://www.springframework.org/schema/context/spring-context.xsd
            http://www.springframework.org/schema/mvc
            http://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <context:component-scan base-package="com.kun.controller"/>
    <mvc:annotation-driven/>
    <mvc:default-servlet-handler/>
    
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <property name="suffix" value=".jsp"/>
    </bean>

    
</beans>
```



## 4、controller类

```
public class UserController {
    @RequestMapping(value = "/j1",produces = "application/json;charset=utf-8")
    @ResponseBody   //加了这个注解，就不会走视图解析器，会直接放回一个字符串
    public String json1() throws JsonProcessingException {

        //jackson  ObjectMapper
        ObjectMapper mapper = new ObjectMapper();

        User user = new User("阿坤", 3, "男");

        String s = mapper.writeValueAsString(user);
        return s;
    }
}
```

**produces = "application/json;charset=utf-8"**：解决json乱码问题



## 5、JSON乱码问题统一解决

```xml
<mvc:annotation-driven>
    <mvc:message-converters register-defaults="true">
        <bean class="org.springframework.http.converter.StringHttpMessageConverter">
            <constructor-arg value="UTF-8"/>
        </bean>
        <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
            <property name="objectMapper">
                <bean class="org.springframework.http.converter.json.Jackson2ObjectMapperFactoryBean">
                    <property name="failOnEmptyBeans" value="false"/>
                </bean>
            </property>
        </bean>
    </mvc:message-converters>
</mvc:annotation-driven>
```

## FastJson

​	fastjon.jar是阿理开发的一款专门用于java开发的包，可以方便的实现json对象与javaBean对象的转换，实现JavaBean对象与Json字符串的转换，实现json对象与json字符串的转换。实现json的转换方法很多，最后的实现结果都是一样得。

​	fastjon的pom依赖！

```xml
<!-- https://mvnrepository.com/artifact/com.alibaba/fastjson -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.78</version>
</dependency>

```



# 6、注解区别

@Controller 作用域在类上面，标记下面的方法，都会走视图解析器

@Resttroller 作用域在类上面，标记下面的方法，不会走视图解析器

@ResponseBody 作用域在方法上，标记该方法步走视图解析器，直接放回，配置@Controller使用

@RequestMapping 作用域在类或者方法上面，有父子关系



# 7、Tomcat启动失败员用

可能是IDEA不能导入相应的包，需要手动导入

![image-20220508095323751](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220508095323751.png)

<img src="C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220508095342728.png" alt="image-20220508095342728" style="zoom:80%;" />

![image-20220508095423335](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220508095423335.png)



# 8、SpringMVC：整合ssm

**创建数据库**

```sql
create database ssmbuild;

use ssmbuild;

drop table if exists books;

create table books (
bookID  int(10) not null auto_increment comment '书id',
bookName varchar(10) not null comment '书名',
bookCounts int(11) not null comment '数量',
detail varchar(200) not null comment '描述',
key bookID (bookID)
)engine=InnoDB default charset=utf8;

insert into books(bookid,bookName,bookCounts,detail) values
(1,'Java',1,'从入门到放弃'),
(2,'MySQL',10,'从删库到跑路'),
(3,'Linux',5,'从进门到进牢');
```



**基本环境搭建**

1. 新建一个Maven项目！
2. 导入相关的pom依赖！

```xml
<!--    依赖:junit,数据库驱动，连接池，servlet，jsp，mybatis，mybatis-spring，spring，-->
    <dependencies>
<!--        Junit-->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>
<!--数据库驱动-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.46</version>
        </dependency>
<!--数据库连接池-->
        <dependency>
            <groupId>com.mchange</groupId>
            <artifactId>c3p0</artifactId>
            <version>0.9.5.2</version>
        </dependency>
<!--Servlet jsp-->
        <dependency>
            <groupId>javax.servlet.jsp.jstl</groupId>
            <artifactId>jstl-api</artifactId>
            <version>1.2</version>
        </dependency>

        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>4.0.1</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet.jsp</groupId>
            <artifactId>javax.servlet.jsp-api</artifactId>
            <version>2.3.3</version>
        </dependency>

<!--        mybatis-->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.2</version>
        </dependency>

<!--        mybatis-spring-->
        <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis-spring -->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
            <version>2.0.6</version>
        </dependency>
<!--spring-->
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.3.18</version>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>5.3.18</version>
        </dependency>
```

**静态资源导出问题**

```xml
<!--    在build中配置resources，来防止我们资源导出失败的问题-->
<build>
    <resources>
        <resource>
            <directory>src/main/resources</directory>
            <includes>
                <include>**/*.properties</include>
                <include>**/*.xml</include>
            </includes>
        </resource>
        <resource>
            <directory>src/main/java</directory>
            <includes>
                <include>**/*.properties</include>
                <include>**/*.xml</include>
            </includes>
        </resource>
    </resources>
</build>
```

3. IDEA关联数据库
4. 编写Mybatis的核心配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
  //配置数据源交给spring去做
  <mappers>
    <mapper resource="org/mybatis/example/BlogMapper.xml"/>
  </mappers>
</configuration>
```

5. 编写spring配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">

   <bean id="helloWorld" class="com.tutorialspoint.HelloWorld">
       <property name="message" value="Hello World!"/>
   </bean>

</beans>
```

6.编写database.properties文件

```properties
jdbc.driver=com.mysql.jdbc.Driver
# 如果使用的是MySQL8.0+, 增加一个失去的配置；serverTimeZone=Asia/Shanghai
jdbc.url=jdbc:mysql://localhost:3306/ssmbuild?useSSL-true&useUnicode=true&characterEncoding=utf-8&serverTimeZone=Asia/Shanghai
jdbc.username=root
jdbc.password=dyk160513140.
```



## 整合mybatis层

**pojo层**

```java
public class Books {
    private int bookID;
    private String bookName;
    private int bookCounts;
    private String detail;

    public Books() {
    }

    public Books(int bookID, String bookName, int bookCounts, String detail) {
        this.bookID = bookID;
        this.bookName = bookName;
        this.bookCounts = bookCounts;
        this.detail = detail;
    }

    public int getBookID() {
        return bookID;
    }

    public void setBookID(int bookID) {
        this.bookID = bookID;
    }

    public String getBookName() {
        return bookName;
    }

    public void setBookName(String bookName) {
        this.bookName = bookName;
    }

    public int getBookCounts() {
        return bookCounts;
    }

    public void setBookCounts(int bookCounts) {
        this.bookCounts = bookCounts;
    }

    public String getDetail() {
        return detail;
    }

    public void setDetail(String detail) {
        this.detail = detail;
    }

    @Override
    public String toString() {
        return "Books{" +
                "bookID=" + bookID +
                ", bookName=" + bookName +
                ", bookCounts=" + bookCounts +
                ", detail='" + detail + '\'' +
                '}';
    }
}

```





**Dao层：**

BookMapper

```java
import com.kun.pojo.Books;
import org.apache.ibatis.annotations.Param;

import java.awt.print.Book;
import java.util.List;

public interface BookMapper {
    //增加一本书
    int addBook(Books books);

    //删除一本书
    int deleteBookById(@Param("bookId") int id);

    //更新一本书
    int updateBook(Books books);

    //查询一本书
    Books queryBookById(@Param("bookId") int id);

    //查询全部的书
    List<Books> queryAllBook();
}

```

BookMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.kun.dao.BookMapper">

    <insert id="addBook" parameterType="Books">
        insert into ssmbuild.books(bookName, bookCounts, detail,) values (#{bookName},#{bookCounts},#{detail})
    </insert>

    <delete id="deleteBookById" parameterType="int">
        delete from ssmbuild.books where bookId = #{bookId}
    </delete>

    <update id="updateBook" parameterType="Books">
        update ssmbuild.books set bookName=#{bookName},bookCounts=#{bookCounts},
        detail=#{detail} where bookId=#{bookId}
    </update>

    <select id="queryBookById" resultType="Books" parameterType="int">
        select * from ssmbuild.books where bookId = #{}
    </select>

    <select id="queryAllBook" resultType="Books">
        select * from ssmbbuild.books
    </select>
</mapper>
```



**Service层**

BookService

```java
import com.kun.pojo.Books;
import org.apache.ibatis.annotations.Param;

import java.util.List;

public interface BookService {
    //增加一本书
    int addBook(Books books);

    //删除一本书
    int deleteBookById(int id);

    //更新一本书
    int updateBook(Books books);

    //查询一本书
    Books queryBookById(int id);

    //查询全部的书
    List<Books> queryAllBook();
}

```

BookServiceImpl

```java
import com.kun.dao.BookMapper;
import com.kun.pojo.Books;

import java.util.List;

public class BookServiceImpl implements BookService {

    //Service层调DAO层：    组合Dao   本质就是代理模式
    private BookMapper bookMapper;

    public void setBookMapper(BookMapper bookMapper) {
        this.bookMapper = bookMapper;
    }

    public int addBook(Books books) {
        return bookMapper.addBook(books);
    }

    public int deleteBookById(int id) {
        return bookMapper.deleteBookById(id);
    }

    public int updateBook(Books books) {
        return bookMapper.updateBook(books);
    }

    public Books queryBookById(int id) {
        return bookMapper.queryBookById(id);
    }

    public List<Books> queryAllBook() {
        return bookMapper.queryAllBook();
    }
}

```

mybatis配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

<!--    配置数据源，交给spring去做-->
    <typeAliases>
        <package name="com.kun.pojo"/>
    </typeAliases>

    <mappers>
        <mapper class="com.kun.dao.BookMapper"></mapper>
    </mappers>
</configuration>
```

## 整合Spring层

在配置了mybatis层之后，整合spring层，主要目的就是把mybatis层的类注入到sring容器中，让spring去托管，如果spring有多个配置文件，要让多个配置文件整合到一起，有两种方式：让IDEA自动合成，手动用import标签去合成

**注入dao层**

spring-dao.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

<!--    1.关联数据库配置文件-->
    <context:property-placeholder location="classpath:database.properties"/>


<!--    2.连接池
        dbcp:半自动化操作，不能自动连接
        c3p0:自动化操作（自动化的加载配置文件，并且可以自动设置到对象中）
        druid:
        hikari:
-->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="driverClass" value="${jdbc.driver}"/>
        <property name="jdbcUrl" value="${jdbc.url}"/>
        <property name="user" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
<!--        c3p0连接池私有属性-->
        <property name="maxPoolSize" value="30"/>
        <property name="minPoolSize" value="10"/>
<!--        关闭连接后不自动commit-->
        <property name="autoCommitOnClose" value="false"/>
<!--        获取连接超时时间-->
        <property name="checkoutTimeout" value="10000"/>
<!--        当前获取连接失败重试次数-->
        <property name="acquireRetryAttempts" value="2"/>
    </bean>

<!--    3.sqlSessionFactory-->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
<!--        绑定mybatis配置文件-->
        <property name="configLocation" value="classpath:mybatis-config.xml"/>

    </bean>
<!--    4.配置dao接口扫描包，动态的实现Dao接口可以注入到Spring容器中！-->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
<!--        注入sqlSessionFactory-->
        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
<!--        要扫描的dao包-->
        <property name="basePackage" value="com.kun.dao"></property>
    </bean>



</beans>
```

**合并service层**

```xml
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">


<!--    1.扫描service下的包-->
    <context:component-scan base-package="com.kun.service"/>

<!--    2.将我们所有的业务类，注入到Spring，可以通过配置，或者注解实现-->
    <bean id="BookServiceImpl" class="com.kun.service.BookServiceImpl">
        <property name="bookMapper" ref="bookMapper"></property>
    </bean>

<!--    3.声明式事务配置-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
<!--       注入数据源-->
        <property name="dataSource" ref="dataSource"></property>
    </bean>

<!--    4.aop事务支持-->
</beans>
```

整合SpringMVC层

跟平时一样



在执行时，可能出现的问题

- 空指针问题
  - 原因：spring的配置文件众多，在web.xml中，配置xml文件时，没有配置总的文件，导致有一些bean没有注册，从而出现空指针异常问题

- 查询之外的操作需要提交事务





# 9、SpringMVC：Ajax技术

## 简介：

- AJAX = Asynchronous JavaScript and XML（异步的javascript和XML）

- AJAX 是一种在无需重新加载整个网页的情况下，能够更新部分网页的技术

- **AJax 不是一种新的编程语言，而是一种用于创建更好更快以及交互性更强的web应用程序的技术**

- 在2005年，Google通过其Google Suggest使Ajax变得流行起来。Google Suggest能够自动帮你完成搜索单词。

- Google Suggest使用Ajax创造出动态性极强的web界面：当你在谷歌的搜索框输入关键字时，JavaScript会把这些字符发送到服务器，然后服务器会反回一个搜索建议的列表

- 就和国内百度的搜索框一样：

  ![image-20220508225631885](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220508225631885.png)



- 传统的网页（即不用Ajax技术的网页），想要更新内容或者提交一个表单，都需要重新加载整个网页。
- 使用Ajax技术的网页，通过后台服务器进行少量的数据交换，都可以实现异步局部更新。
- 使用Ajax，用户可以创建接近本地桌面应用的直接、高可用、更丰富、更动态的web用户界面。

