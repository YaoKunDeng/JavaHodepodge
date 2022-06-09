# 1、JavaWeb

**java  web两部分**



## 1、基本概念

web开发：

- web，网页的意思，www.baidu.com
- 静态web
  - html，css
  - 提供给所有人看的，数据始终不会发生变化
- 动态web
  - 淘宝，几乎是所有的网站
  - 提供给所有人看的数据始终会发生变化，每个人在不同的实践，不同的地点看到的信息各不相同
  - 技术栈：Servlet/JSP , ASP, PHP

在java中，动态web资源开发的技术统称为javaweb



## 1.2、Web应用程序

web应用程序：可以提供浏览器访问的程序

- a.html 、b.html  .......多个web资源，这些web资源可以被外界提供访问
- 你们能访问到的任何一个页面或者资源，都存在与这个世界的某一个角落的计算机上
- URL
- 这个统一的Web资源会被放在同一个文件夹下，web应用程序 -->Tomcat:服务器
- 一个web应用由多部分组成（静态web，动态web）
  - html、css、js
  - jsp、servlet
  - java程序
  - jar包
  - 配置文件（properties）

web应用程序白那些完毕后，若向提供给外界访问，需要一个服务器来统一管理



## 1.3、静态web

- *.htm, *.html这些都是网页后缀，如果服务器上一直存在这些东西，我们就可以直接读取。通路：

![image-20220426100730936](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220426100730936.png)

- 静态web存在的缺点
  - web页面无法动态更新，所有用户看到的都是同一个页面
    - 轮播图，点击特效：伪动态
    - Javascript【实际开发中，它用的最多】
    - VBScript
  - 它无法和数据库交互（数据无法持久化，用户无法交互）





## 1.4、动态Web

页面会动态展示：“Web的页面展示的效果因人而异”;

![image-20220426101643365](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220426101643365.png)

缺点

- 加入服务器的动态web资源出现了错误，我们需要重新编写我们的后台程序，重新发布；
  - 停机维护



优点：

- web页面可以动态更新，所有用户看到的都不是同一个页面
- 它可以与数据库交互（数据持久化，注册，商品信息，用户信息）





# 2、Web服务器

## 2.1、技术讲解

APS

- 微软：国内最早流行的就是ASP
- 在HTML中嵌入了VB的脚本，ASP+COM
- 在ASP开发中，基本一个页面都有几千行的业务代码，页面机器混乱
- 维护成本高！
- C#
- IIS





PHP

- PHP开发速度很快，功能很强大，跨平台，代码很简单（70%，WP）
- 无法承载大访问量的情况（局限性）



JSP/Servlet：

B/S：浏览器和服务器

C/S：客户端和服务器

- sum公司主推B/S架构
- 基于java语言的（所有大公司，或者一些开源组件，都是java写的）
- 可以承载三高问题带来的影响
- 语法像ASP，ASP->JSP，加强市场速度



## 2.2、Web服务器

**服务器时一种被动的操作，用来处理用户的一些请求和给用户的一些响应信息；**

**IIS**

微软的：ASP...,Windows自带的



**Tomcat**

面向百度编程：

​	Tomcat是Apache软件基金会（Apache SoftWare Foundation）的Jakarta项目中的一个核心项目，最新的Servlet和JSP规范总是能在Tomcat中得到体现，因为Tomcat技术先进、性能稳定，而且**免费**，因而深受ava爱好者的喜爱，并得到了部分软件开发商的认可，成为目前比较流行的Web应用程序。



Tomcat服务是一个免费的开源的Web应用程序，属于轻量级应用服务器，在中小型系统和并发访问用户不是很多的场合下被普遍使用，是开发和调试JSP程序的首选。对于一个初学者来说，可以这样认为，当一台机器上配置好Apache服务器，可利用它响应HTML（标准通用标记语言下的一个应用）页面的访问请求。实际上Tomcat是Apache服务器的扩展，但运行时它是独立运行的，所以当你运行tomcat时，它实际上作为一个与Apache独立的进行单独运行的。【对初学者，最佳选择】

诀窍是，当配置正确是时，Apache为HTML页面服务，而**Tomcat实际上运行JSP页面和Servlet**。

。。。。。

**工作3-5年之后，可以尝试手写Tomcat服务器**



下载Tomcat：

1. 安装 or 解压
2. 了解配置文件及目录结构
3. 这个东西的作用



# 3、Tomcat

## 3.1、安装Tomcat

![image-20220426105254003](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220426105254003.png)

![image-20220427121337138](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220427121337138.png)

## 3.2、Tomcat启动和配置

文件夹作用：

![image-20220426110539213](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220426110539213.png)

**启动，关闭**

![image-20220426110712506](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220426110712506.png)

访问测试：http://localhost:8080/

可能遇到的问题：

1、java环境变量没有配置

2、闪退问题，需要配置兼容性

3、乱码问题：配置文件中设计

## 3.3、配置

![image-20220426111652772](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220426111652772.png)

可以配置启动的端口号

- tomcat默认端口号为：8080
- mysql：3306
- http：80
- https：443



```xml
 <Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
```



可以配置主机的名称

- 默认的主机名为：localhost-->127.0.0.1
- 默认的网站应用存在的位置为：webApps

```xml
  <Host name="localhost"  appBase="webapps"
            unpackWARs="true" autoDeploy="true">
```



高难度面试题：

请你谈谈网站时如何进行访问的！

1. 输入一个域名；回车【比如访问localhost：8080】

2. 检查本机的C:\Windows\System32\drivers\etc\hosts配置下有没有这个域名映射

   1. 有：直接返回对应的ip地址,有我们需要访问的web程序，可以直接访问

      ```xml
      127.0.0.1       localhost
      ```

    2.没有：去DNS服务器找，找到的就返回，找不到就返回找不到

![image-20220426113248735](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220426113248735.png)

3.配置环境变量（可选项）【还没配，之前配的错误】



## 3.4、发布一个web网站

- 将自己写的网站，放到服务器（Tomcat）中指定的web应用的文件夹下（webapps）下，就可以访问了

网站应该有的结构

```java
--webapps: Tomcat服务器的web目录
    -Root
    -kunstudy:网站的目录名
        -WEB-INF
        	-classes:java程序
            -lib:web应用所依赖 的jar包
            -web.xml：网站的配置文件
        -index.thml默认的首页
        -staic
           -css
                -style.css
           -js
           -img
                
    -........
    
```



# 4、Http

## 4.1、什么时HTTP？

超文本传输协议（Hyper Text Transfer Protocol，HTTP）是一个简单的请求-响应协议，它通常运行在[TCP](https://baike.baidu.com/item/TCP/33012)之上。

- 文本：html，字符串，~。。。。

- 超文本：图片，音乐，视频，定位，地图...........

- 端口：80

  

https：安全的

- 端口443



## 4.2、两个时代

- http1.0
  - HTTP/1.0：客户端可以与web服务器连接后，只能获得一个web资源，断开连接
- http2.0
  - HTTP/1.1：客户端可以与web服务器连接后，可以获得多个web资源。



## 4.3、HTTP请求

- 客户端--发请求---服务器

百度:

```java
Request URL: https://baike.baidu.com/    请求地址
Request Method: GET        get方法/post方法
Status Code: 200 OK              状态码：2000
Remote Address: 111.45.3.101:443      remote：远程
```

### 1、请求行

- 请求行中的请求方式：GET
- 请求方式有：**Get，Post**，HEAD。。。。
  - get：请求能够携带的而参数比较少，大小有限制，会在浏览器的URL地址栏显示数据内容，不安全，但高效
  - post请求能够携带的而参数没有限制，大小没有限制，不会在浏览器的URL地址栏显示数据内容，安全，但不高效



### 2、消息头

```xml
Accept:告诉浏览器，它支持的数据类型
Accept-Encoding：支持那种解码格式 GBK  UTF-8
Accept-Language：告诉浏览器，他的语言黄静
Cache-Control：缓存控制
```



## 4.4、HTTP响应

- 服务器--响应--客户端

  百度：

```java
Cacher-Control:private    缓存控制
Connection: keep-alive    连接 
COntent-Encoding:gzip	  编码
Content-Type:text/html	  类型
```

### 1、响应体：

```xml
Accept:告诉浏览器，它支持的数据类型
Accept-Encoding：支持那种解码格式 GBK  UTF-8
Accept-Language：告诉浏览器，他的语言黄静
Cache-Control：缓存控制
Reflesh:告诉客户端，多久刷新一次
Location：让网页重新定位
```

### 2、响应状态码

200：请求响应成功

3**：请求重定向

- 重定向：你重新到喔给你写的新位置去

4xx：找不到资源 404

- 资源不存在 

5xx：服务器代码错误  500   502：网关错误



常见面试题：

当你的浏览器中地址栏输入地址并回车的一瞬间页面能够展示回来，经历了什么？



# 5、Maven

**我们为什么要学习这个技术**

1. 在javaweb开发中，需要使用大量的jar包，我们手动去导入

2. 如何能够让一个东西自动帮我们导入和配置这个jar包

   由此，maven诞生了



## 5.1Maven项目架构管理工具

我们目前用来就是方便导入jar包的！

maven的核心思想：**约定大于配置**

- 有约束，不要去违反

Maven会规定好你该如何去编写我们的java代码，必须按照这个规范来

## 5.2、下载安装Maven

建议：电脑上的所有环境都放在一个文件夹方便管理



## 5.3、配置环境变量

在我们的系统环境变量中，配置如下配置：

- M2——HOME   maven目录下的bin目录

- MAVEN_HOME  maven的目录

- 在系统的path中配置MAVEN——HOME%\bin

  

## 5.4、阿里云镜像

- 镜像:mirrors

  - 作用：加速下载

  ```xml
  <mirror>
        <id>nexus-aliyun</id>
        <mirrorOf>*,!jeecg,!jeecg-snapshots</mirrorOf>
        <name>Nexus aliyun</name>
        <url>https://maven.aliyun.com/nexus/content/groups/public</url>
      </mirror>
  ```

  



## 5.5、本地仓库localRepository

在本地的仓库，相对远程仓库

建立一个本地仓库

```xml
<localRepository>E:\软件下载\apache-maven-3.6.3\maven-repo</localRepository>
```



## 5.6、在IDEA中使用Maven

1.启动IDEA

2.创建一个Maven项目

![image-20220426165153882](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220426165153882.png)

<img src="C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220426165250227.png" alt="image-20220426165250227" style="zoom:80%;" />

<img src="C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220426165446567.png" alt="image-20220426165446567" style="zoom:80%;" />

<img src="C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220426165758942.png" alt="image-20220426165758942" style="zoom:80%;" />

3、等待项目初始化完毕

4、观察Maven仓库中多了什么东西？

5、IDEA中的Maven设置

注意：IDEA项目创建成功后，看一眼Maven的配置

![image-20220426170537540](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220426170537540.png)

6.到这里Maven在IDEA中的配置就掌握了

## 5.7、创建一个普通的Maven项目

![image-20220426171226310](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220426171226310.png)

## 5.8、标记文件夹功能

![image-20220426172013918](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220426172013918.png)



![image-20220426172111198](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220426172111198.png)

![image-20220426172319450](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220426172319450.png)

跟从setting配置一样



## 5.9、配置Tomcat

![image-20220426172939794](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220426172939794.png)

![image-20220426173400351](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220426173400351.png)

解决警告问题：

**为什么回有这个问题：我们访问网站，必需要指定一个文件夹的名字**

![image-20220427002429542](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220427002429542.png)

## 5.10、解决遇到的问题

1.maven3.6.2

​	解决方法：降级为3.6.1

2.tomcat闪退

配置环境变量

3.IDEA中每次都要配置

​	从首页的config更改全局变量

4.Maven项目中Tomcat无法配置

5、maven默人web项目中的web.xml版本问题

![image-20220427171705135](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220427171705135.png)

可以参考tomcat里面webapps文件下的xml

```xml
<?xml version="1.0" encoding="UTF-8"?>

<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
                      http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0"
         metadata-complete="true">

</web-app>

```

## 5.11、maven仓库的使用

```url
https://mvnrepository.com/
```

HttpServlet依赖以下包

![image-20220427173641071](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220427173641071.png

![image-20220427174325989](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220427174325989.png)

# 6、Servlet

## 6.1、Servlet简介

- Servlet就是sun公司开发动态Web的一门技术【每个人去请求网页，看到的页面不一样】
- Sun公司在这些API中提供一个接口叫做Serlet，如果你想开发一个Servlet程序，只需要完成两个小步骤：
  - 编写一个类，实现Servlet接口
  - 
  - 把开发好的java类部署到web服务器中

**把实现了Servlet接口的java程序叫做：Servlet**



## 6.2、HelloServlet

Servlet接口：sun公司有两个默认的实现类：HttpServlet、GenericServlet



1. 构建一个Maven项目，删除里面的src目录，以后我们的学习就在这个项目里面建立moudel；这个空的工程就是Maven主工程
2. 关于maven父子工程的理解

父项目中会有：

```xml
<modules>
        <module>servlet-01</module>
    </modules>
```

子项目会有

```xml
<parent>
        <artifactId>javaweb-02-servlet</artifactId>
        <groupId>org.example</groupId>
        <version>1.0-SNAPSHOT</version>
</parent>
```

父项目的java子项目可以直接使用

```java
son extends father
```

3. Maven环境优化
   1. 修改web.xml为最新配置
   2. 将Maven的结构搭建完整
4. 编写一个Servlet程序
   1. 编写一个普通类
   2. 实现Servlet接口，这里我们可以直接继承HttpServlet

![image-20220427224618926](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220427224618926.png)

```java
package com.kun.servlet;


import javax.servlet.ServletException;
import javax.servlet.ServletOutputStream;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintStream;
import java.io.PrintWriter;

public class HelloServlet extends HttpServlet {
    //由于get或post只是请求实现不同的方式，可以相互调用，业务逻辑都一样
    //ctrl+insert选择覆盖实现父类方法
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //ServletOutputStream outputStream = resp.getOutputStream();
        PrintWriter write = resp.getWriter();//响应流
        write.print("Hello, Servlet");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req,resp);
    }
}

```

5. 编写Servlet的映射

**为什么需要映射：**

​		我们写的是java程序，但是要通过浏览器访问，而浏览器需要连接web服务器，所以我们需要在web服务中注册我们写的servlet,注册之后还要给他一个浏览器能够访问的路径

```xml
<?xml version="1.0" encoding="UTF-8"?>

<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
                      http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0"
         metadata-complete="true">

<!--    注册Servlet-->
    <servlet>
        <servlet-name>hello</servlet-name>
        <servlet-class>com.kun.servlet.HelloServlet</servlet-class>
    </servlet>

<!--    Servlet 的请求路径-->
    <servlet-mapping>
        <servlet-name>hello</servlet-name>
        <url-pattern>/hello</url-pattern>
<!--        第二个hello意思为：在前端请求hello这个路径，就会运行servletName为hello这个servlet对应的类，这个servlet就有处理对应业务的逻辑代码-->
    </servlet-mapping>

</web-app>

```

![image-20220427230843105](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220427230843105.png)

在浏览器中输入hello，web服务器就会到web.xml去查找对应的servlet

6. 配置Tomcat

注意 ：配置项目的发布路径就可以了

7. 启动测试



## 6.3.Servlet原理

Servlet是由Web服务器调用，Web服务器在收到浏览器请求之后，会：

![image-20220427233012210](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220427233012210.png)

## 6.4、Mapping

1. 一个Servlet可以指定一个映射路劲

   ```xml
       <servlet-mapping>
           <servlet-name>hello</servlet-name>
           <url-pattern>/hello</url-pattern>
   <!--        第二个hello意思为：在前端请求hello这个路径，就会运行servletName为hello这个servlet对应的类，这个servlet就有处理对应业务的逻辑代码-->
       </servlet-mapping>
   ```

2. 一个Servlet也可以指定多个映射路径

   ```xml
       <servlet-mapping>
           <servlet-name>hello</servlet-name>
           <url-pattern>/hello</url-pattern>
       </servlet-mapping>
    <servlet-mapping>
           <servlet-name>hello</servlet-name>
           <url-pattern>/hello2</url-pattern>
       </servlet-mapping>
   ```

   

3. 一个Servlet可以指定通用映射路径

   ```xml
   <servlet-mapping>
           <servlet-name>hello</servlet-name>
           <url-pattern>/hello/*</url-pattern>
       </servlet-mapping>
   ```

4. 默认请求路径

   ```xml
   <servlet-mapping>
           <servlet-name>hello</servlet-name>
           <url-pattern>/*</url-pattern>
       </servlet-mapping>
   
   //直接进去Servlet，赶调主页
   ```

5. 指定一些后缀或者前缀等等.....

   注意点：*.前面不能加项目映射的路径

   ```xml
   <servlet-mapping>
           <servlet-name>hello</servlet-name>
           <url-pattern>*.kun</url-pattern>
       </servlet-mapping>
   ```

   6.优先级问题

   ​	指定了固定的映射路径的优先级最高，如果找不到就会走默认的处理请求；

   可以使用以下代码处理错误信息

```xml
    <servlet-mapping>
        <servlet-name>hello</servlet-name>
        <url-pattern>/hello</url-pattern>
<!--        第二个hello意思为：在前端请求hello这个路径，就会运行servletName为hello这个servlet对应的类，这个servlet就有处理对应业务的逻辑代码-->
    </servlet-mapping>

    <servlet>
        <servlet-name>error</servlet-name>
        <servlet-class>com.kun.servlet.ErrorServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>error</servlet-name>
        <url-pattern>/*</url-pattern>
    </servlet-mapping>


```

## 6.5、ServletContext

Servlet容器在启动的时候，他会为每一个web程序都创建一个对应的ServletContext对象，它代表了当前的web应用；【多对一】

- 共享数据
  - 我在这个Servlet中保存的数据，可以在另外一个Servlet中拿到

![image-20220428092014018](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220428092014018.png)

【每个继承Http接口的类就是一个servlet】

```java
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
//        this.getInitParameter();  初始化参数
//        this.getServletConfig();  Servlet配置
//        this.getServletContext();  Servlet上下文
        ServletContext context = this.getServletContext();

        String userName = "阿坤";  //数据

        context.setAttribute("userName",userName);  //将一个数据保存在Servlet中，名字为userName。值userName


    }
```

```java
//读取 
protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        ServletContext context = this.getServletContext();
        String userName = (String) context.getAttribute("userName");
        resp.setContentType("text/html");
        resp.setCharacterEncoding("utf-8");
        resp.getWriter().print("名字"+userName);
    }
```



配置xml

```xml
 <servlet>
        <servlet-name>hello</servlet-name>
        <servlet-class>com.kun.servlet.HelloServlet</servlet-class>
    </servlet>
    
    <servlet-mapping>
        <servlet-name>hello</servlet-name>
        <url-pattern>/hello</url-pattern>
    </servlet-mapping>

    <servlet>
        <servlet-name>getc</servlet-name>
        <servlet-class>com.kun.servlet.GetServlet</servlet-class>
    </servlet>

    <servlet-mapping>
        <servlet-name>getc</servlet-name>
        <url-pattern>/getc</url-pattern>
    </servlet-mapping>
```





## 2、获取XML参数

```xml
<!--    配置一些web应用初始化参数-->
    <context-param>
        <param-name>url</param-name>
        <param-value>jdbc:mysql://localhost:3306/mybatis</param-value>
    </context-param>
```

```xml
   protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        ServletContext context = this.getServletContext();

        String url = context.getInitParameter("url");
        resp.getWriter().print(url);
    }

```

## 3、请求转发

```java
 protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        ServletContext servletContext = this.getServletContext();
        System.out.println("进入了demo04");

        //RequestDispatcher requestDispatcher = servletContext.getRequestDispatcher("/gp");   //转发的请求路径
        //requestDispatcher.forward(req,resp);  //调用forward实现请求转发

        servletContext.getRequestDispatcher("/gp").forward(req,resp);  //一般通过req去做，而不会通过context
     //这里的/就表示当前应用

    }
```

```xml
    <servlet>
        <servlet-name>sd4</servlet-name>
        <servlet-class>com.kun.servlet.ServletDemo04</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>sd4</servlet-name>
        <url-pattern>/sd4</url-pattern>
    </servlet-mapping>
    
```

【请求sd4的时候，页面会跳转到/gp页面，但是链接不会改变】



## 4、读取资源文件

properties

- 在java目录下新建properties
- 在resource目录下新建properties

发现：都被打包到了同一个路径下，classes，我们俗称这个路径为classpath；

思路：需要一个文件流

```properties
username = root
password=123456

```



```java

public class ServletDemo05 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        InputStream resourceAsStream = this.getServletContext().getResourceAsStream("/WEB-INF/classes/db.properties");
        //为什么是直接/WEB-INF
        //web项目，生成的目录，从target中找
        Properties properties = new Properties();
        properties.load(resourceAsStream);
        String username = properties.getProperty("username");
        String password = properties.getProperty("password");

        resp.getWriter().print(username+password);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req,resp);
    }
}

```

## 6.6、HttpServletRespone

web服务器接收到客户端的http请求，针对这个请求，分别创建一个代请求的HttpServletRequest对象，代表响应的一个HttpServletResponse；

- 如果要获取客户端请求过来的参数，找HttpServletRequest
- 如果要给客户端响应一些西南西，找HttpServletRequest

### 1、简单分类

**负责向浏览器发送数据的方法**

```java
ServletOutputStream getOutputStream();

PrintWriter getWriter() throws IOException;  //写中文用
```

**负责向浏览器发送响应头的方法**

```java
void setCharacterEncoding(String var1);

    void setContentLength(int var1);

    void setContentLengthLong(long var1);

    void setContentType(String var1);

void sendRedirect(String var1) throws IOException;

    void setDateHeader(String var1, long var2);

    void addDateHeader(String var1, long var2);

    void setHeader(String var1, String var2);

```

响应的代码

200 成功

3xx  重定向

4xx 找不到资源

5xx 服务器问题



### 2、常见应用

1. 向浏览器输出消息

   ```java
   resp.getWriter().print(username+password);
   ```

   

2. **下载文件**

   1. 要获取下载文件的路径

   2. 下载的文件名是啥

   3. 设置想办法让浏览器支持下载我们需要的东西

   4. 获取下载文件的输入流

   5. 创建缓冲区

   6. 获取OutputStream对象

   7. 将FileOutStream流写入到buffer缓冲区，使用OutStream将缓冲区中的数据输出到客户端

      ```java
      package com.kun.servlet;
      
      import javax.servlet.ServletException;
      import javax.servlet.ServletOutputStream;
      import javax.servlet.http.HttpServlet;
      import javax.servlet.http.HttpServletRequest;
      import javax.servlet.http.HttpServletResponse;
      import java.io.FileInputStream;
      import java.io.IOException;
      
      public class FileServlet extends HttpServlet {
          @Override
          protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
      //
              String realPath = "E:\\JavaCode\\javaweb-02-servlet\\response\\target\\classes\\1.jpg";
              System.out.println("下载的文件路径:"+realPath);
      //        2. 下载的文件名是啥
              String filename = realPath.substring(realPath.lastIndexOf("\\") + 1);
      //        3. 设置想办法让浏览器支持下载我们需要的东西,中文文件名URLEncoder.encode编码，否则可能乱码
              resp.setHeader("Content-disposition","attachment;filename="+ URLEncoder.encode(filename,"UTF-8"));
      //        4. 获取下载文件的输入流
              FileInputStream in = new FileInputStream(realPath);
      //        5. 创建缓冲区
              int len = 0;
              byte[] buffer = new byte[1024];
      //        6. 获取OutputStream对象
              ServletOutputStream out = resp.getOutputStream();
      //        7. 将FileOutStream流写入到buffer缓冲区,使用OutStream将缓冲区中的数据输出到客户端
              while ((len=in.read(buffer))>0){
                  out.write(buffer,0,len);
              }
              in.close();
              out.close();
      
      
      
          }
      
          @Override
          protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
              doGet(req,resp);
          }
      }
      
      ```

      

3. ### 验证码功能

验证码怎么来的？

- 前端实现
- 后端实现，需要用到java的图片类，生产一个图片

```java
public class ImageServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //如何让浏览器5秒自动刷新一次
        resp.setHeader("refresh","3");

        //在内存中创建一个图片
        BufferedImage bufferedImage = new BufferedImage(80,20,BufferedImage.TYPE_3BYTE_BGR);

        //得到图片
        Graphics2D graphics = (Graphics2D) bufferedImage.getGraphics();  //笔

        //设置图片的背景颜色
        graphics.setColor(Color.WHITE);  //选择画笔颜色
        graphics.fillRect(0,0,80,20);
        //给图片写数据
        graphics.setColor(Color.BLUE);
        graphics.setFont(new Font(null,Font.BOLD,20));
        graphics.drawString(makeNum(),0,20);

        //告诉浏览器，这个请求用图片的方式打开
        resp.setContentType("image/jpeg");

        //网站存在缓存，不让浏览器缓存
        resp.setDateHeader("expires",-1);
        resp.setHeader("Cache-Control","no-cache");
        resp.setHeader("pragma","no-cache");

        //把图片写给浏览器
        boolean jpg = ImageIO.write(bufferedImage, "jpg", resp.getOutputStream());

    }

    //生成随机数
    private String makeNum(){
        Random random = new Random();
        String num = random.nextInt(9999) + "";
        StringBuffer stringBuffer = new StringBuffer();
        for(int i=0;i<4-num.length();i++){
            stringBuffer.append("0");  //假如num不是4位数，用0来填充
        }
        return stringBuffer.toString() + num;  //字符串拼接
    }

```

### 4.实现重定向

​	一个web资源收到客户端A请求后，B他会通知客户端去访问另外一个web资源，这个过程叫重定向

常见场景：

- 用户登录



测试：

```java
   protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
//        resp.setHeader("Location","/response_war/imageServlet");
//        resp.setStatus(302);
        
        resp.sendRedirect("/response_war/imageServlet");
    }

```

面试题：请你聊聊重定向和转发的区别

相同点：

- 页面都会实现跳转

不同点：

- 请求转发的时候，url不会发生变化
- 重定向的时候，url地址栏会发生变化

## 6.7、HttpServletRequest

HttpServletRequest代表客户的请求，用户通过Http协议访问服务器，HTTP请求中的所有信息会被封装到HttpServletRequest中，通过这个HttpServletRequest的方法获得客户端的所有信息

1. **获取前端传递的参数**

   ![image-20220429104131410](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220429104131410.png)

2. 请求转发

   ![image-20220429110644512](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220429110644512.png)



# 7、Cookie、Session

## 7.1、会话

**会话**：用户打开一个浏览器，点击了很多超链接，访问多个web资源，关闭浏览器，这个过程，可以称之为会话

**有状态会话**：一个同学来过教室，下次再来教室，我们就会知道这个同学曾经来说，称之为有状态会话

一个网站怎么证明你来过？

客户端					    		服务端

1. ​	服务端给客户端一个信件，客户端下次访问服务端带上信件就可以了；
2. 服务器登记你来过了，下次你来的时候我来匹配你；session



## 7.2、保存会话的两种技术

**cookie**

- 客户端技术（响应，请求）





**session**

- 服务器技术，利用这个技术，可以保存用户的会话信息。我们可以把信息或者数据放在session中



常见场景：网站登录之后，下次你不用再登录了，第二次访问直接就上去了！



## 7.3、Cookie

![image-20220430114249975](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220430114249975.png)

1、从请求中拿到cookie信息

2、服务器响应给客户端cookie

```java
protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //服务器告诉你，你来的时间，把这个时间封装成为一个信件，你下次带来，我就知道你了

        //解决中文乱码问题
        req.setCharacterEncoding("utf-8");
        resp.setCharacterEncoding("gbk");  //utf-8不能解决中文问题

        PrintWriter out = resp.getWriter();

        //Cookie,服务器端从客户端获取呀
        Cookie[] cookies = req.getCookies();//这里返回数数组，说明Cookie可能存在多个

        //判断Cookie是否存在
        if (cookies != null) {
            //如果存在怎么办

            out.write("你上一次访问的时间是：");
            for (Cookie cookie : cookies) {
                if (cookie.getName().equals("lastLoginTime")) {
                    //获取cokie中的值

                    long l = Long.parseLong(cookie.getValue());
                    Date date = new Date(l);
                    out.write(date.toLocaleString());
                }
            }
        } else {
            out.write("这是您第一次访问本站");
        }

        //服务器给客户端响应一个cookie
        Cookie cookie = new Cookie("lastLoginTime", System.currentTimeMillis() + "");//新建一个cookie
        cookie.setMaxAge(24*60*60);//设置cookie有效期

        resp.addCookie(cookie);响应给客户端一个cookie


    }
```

**cookie：一般会保存再本地的用户目录下appdata**



一个网站cookie是否存在上限！**聊聊细节问题**

- 一个Cookie只能保存一个信息
- 一个web站点剋给浏览器发送多个cookie，最多存放20个
- Cookie大小有限制4kb
- 300个cookie浏览器上限
- 



删除Cookie：

- 不设置有效期，关闭浏览器，自动失效
- 设置有效期时间为0



编码解码：

```java
URLEncoder.encode("阿坤","utf-8");
URLDecoder.decode(cookie.getValue(),"UTF-8");
```

## 7.4、Session重点【重点】

![image-20220430114422505](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220430114422505.png)

什么是Session：

- 服务器会给每一个用户(浏览器)创建一个Session对象
- 一个Session独占一个浏览器，只要浏览器没有关闭，这个Session就存在
- 用户登录之后，整个网站它都可以访问！-->保存用户的信息；保存购物车的信息



Session和Cookie区别：

- Cookie是把用户的数据写给用户的浏览器，浏览器保存
- Session是把用户的数据写到用户独占的Session中，服务器端保存（保存重要的信息，减少服务器的资源的浪费）
- Session对象由服务创建；



使用场景：

- 保存一个登录用户的信息
- 购物车信息；
- 在整个网站中经常会使用的数据，我们将它保存在Session中

```java
public class SessionDemo01 extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

        //解决乱码问题
        req.setCharacterEncoding("UTF-8");
        resp.setCharacterEncoding("UTF-8");
        resp.setContentType("text/html;charset=utf-8");

        //得到Session
        HttpSession session = req.getSession();

        //给Session中存东西
        session.setAttribute("name","阿坤");

        //获取Session的ID
        String id = session.getId();
        

        //判断session是不是新创建按的
        if(session.isNew()){
            resp.getWriter().write("Session创建成功，ID："+id);
        }else {
            resp.getWriter().write("Session已经在服务器中存在了"+id);
        }


        //Sessionz创建的时候做了什么事情
        //Cookie cookie = new Cookie("JESSIONID", id);
        //resp.addCookie(cookie);
    }
```



会话自动过期：web.xml配置

```xml
<!--设置Session默认的失效时间-->
    <session-config>
<!--        15分钟后Session自动失效，以分钟为单位-->
        <session-timeout>15</session-timeout>
    </session-config>
```

# 8、JSP

## 8.1、什么是JSP

java Server Page：java服务器页面，也和Servlet一样，用于动态web

最大特点：

- 写jsp就像在写HTML
- 区别
  - HTML只给用户提供静态数据
  - JSP页面中可以嵌入Java代码，为用户提供动态数据





## 8.2、JSP原理

思路：JSP到底怎么执行的

- 代码层面没有任何问题
- 服务器内部工作
  - tomcat中有一个word目录
  - 在IDEA中使用Tomcat的话会在IDEA的 tomcat中产生一个word目录



**浏览器向服务器发送请求，不管访问什么资源，其实都是在访问Servlet**！

JSP最终也会转换为java类！

# **JSP本质就是一个Servlet**【HttpJspBase】

<img src="C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220430234530455.png" alt="image-20220430234530455" style="zoom:80%;" />

在JSP页面中，只要是Java代码就会原封不动的输出；

如果是HTML代码，就会转换为：

```java
out.write("<html>\r\n")
```

这样的格式，输出到前端！



## 8.3、JSP基础语法

1. 新建项目

2. 导入依赖

   ```xml
       <dependencies>
   <!--        servlet依赖-->
           <dependency>
               <groupId>javax.servlet.jsp</groupId>
               <artifactId>javax.servlet.jsp-api</artifactId>
               <version>2.3.3</version>
           </dependency>
   <!--        JSP依赖-->
           <dependency>
               <groupId>javax.servlet</groupId>
               <artifactId>javax.servlet-api</artifactId>
               <version>4.0.1</version>
           </dependency>
   <!--        JSTL表达式依赖-->
           <!-- https://mvnrepository.com/artifact/javax.servlet.jsp.jstl/jstl-api -->
           <dependency>
               <groupId>javax.servlet.jsp.jstl</groupId>
               <artifactId>jstl-api</artifactId>
               <version>1.2</version>
           </dependency>
   <!--        standard依赖-->
           <dependency>
               <groupId>taglibs</groupId>
               <artifactId>standard</artifactId>
               <version>1.1.2</version>
           </dependency>
       </dependencies>
   ```

任何语言都有自己的语法，JAVA中有。JSP作为java技术的一种应用，它拥有一些自己的扩充语法（了解即可），java所有语法都支持！



**JSP表达式**

```
<%--  JSP表达式
  作用：用来将程序输出，输出为到客户端
  <%= 变量或表达式%>
--%>
  <%= new java.util.Date()%>
```

**JSP脚本片段**

```jsp
<%--jsp脚本片段--%>
  <%
    int sum = 0;
    for(int i=1; i<=100; i++){
      sum+=i;
    }
    out.println("<h1>Sum="+sum+"</h1>");
  %>
```



**在代码中放入HTMl元素**

```
%--在代码中放入HTMl元素--%>
<%
  for (int i=0; i<5; i++){
%>
  <h1>Hello ,akun</h1>
<%}%>
```



**JSP声明**【全局方法】

```jsp
<%!
    static{
    	System.out.println("Loading Servlet!");
	}
	private int globalvar = 0;
	
	public void kun(){
        System.out.println("进入了方法kun");
    }
    %>
```

JSP声明：会被编译到JSP生成的Java类中，其他的，都会被生成道_jspService方法中！

在JSP，嵌入java代码即可



JSP的注释，不会再客户端显示，HTML就会！



## 8.4、JSP指令

```jsp
<%@ page args....%>
<%@ include file=""%>//嵌入网页相同部分内同，比如网页头部和网页尾部
可以用<JSP:include page=""></JSP:include>代替
```

@include会将页面合二为一；如果有两个文件都有相同的变量，那么变量就会重名，就会报错

JSP:include  :拼接页面，本质还是三个【一般用这个】



## 8.5、9大内置对象

- pageContext  存东西  【保存数据只在一个页面中有效】
- request【保存数据只在一次请求中有效，请求转发会携带这个数据】
- response
- session   存东西【保存数据只在一次会话中有效，从打开浏览器道关闭浏览器】
- application【ServletContext】 存东西【保存数据只在服务器中有效，从打开服务器，到关闭服务器】
- config【ServletConfig】
- out
- page【几乎不用】
- exception



request:客户端向服务器发送请求，产生的数据，用户看完了就没用了，比如：新闻，用户看完没用的！

session：客户端向服务器发送请求，产生的数据，用户用完一会还有用，比如：购物车

application:客户端向服务器发送请求，产生的数据，一个用户用完了，其他用户可能使用，比如：聊天数据





## 8.6、JSP标签，JSTL标签、EL表达式

```xml
<dependency>
            <groupId>javax.servlet.jsp.jstl</groupId>
            <artifactId>jstl-api</artifactId>
            <version>1.2</version>
        </dependency>
<!--        standard依赖-->
        <dependency>
            <groupId>taglibs</groupId>
            <artifactId>standard</artifactId>
            <version>1.1.2</version>
        </dependency>
```

EL表达式：${}

- **获取数据**
- **执行运算**
- **获取web开发常用对象**



**JSP标签**

```jsp
<%--jsp:include--%>

<%--
http://localhost:8080/jsptag.jsp?name=ztyshen&age=12
--%>

<jsp:forward page="/jsptag2.jsp">
    <jsp:param name="name" value="ztyshen"></jsp:param>
    <jsp:param name="age" value="12"></jsp:param>
</jsp:forward>

```



**JSTL表达式**

JSTL 标签库的使用就是为了弥补 HTML 标签的不足；它自定义许多标签，可以供我们使用，标签的功能和 Java 代码一样！

**格式化标签**

**SQL标签**

**XML 标签**

**核心标签** （掌握部分）

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020110221251720.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2MTg4MTI3,size_16,color_FFFFFF,t_70#pic_center)

**JSTL标签库使用步骤**

- 引入对应的 taglib
- 使用其中的方法
- **在 Tomcat 也需要引入 JSTL 的包，否则会报错：JSTL 解析错误**



c:if

```jsp
<head>
    <title>Title</title>
</head>
<body>

<h4>if测试</h4>
<hr>
<form action="coreif.jsp" method="get">
    <%--
    EL表达式获取表单中的数据
    ${param.参数名}
    --%>
    <input type="text" name="username" value="${param.username}">
    <input type="submit" value="登录">
</form>
<%--判断如果提交的用户名是管理员，则登录成功--%>
<c:if test="${param.username=='admin'}" var="isAdmin">
    <c:out value="管理员欢迎您！"/>
</c:if>
<%--自闭合标签--%>
<c:out value="${isAdmin}"/>

</body>

```

c:choose c:when

```jsp
<body>

<%--定义一个变量score，值为85--%>
<c:set var="score" value="55"/>

<c:choose>
    <c:when test="${score>=90}">
        你的成绩为优秀
    </c:when>
    <c:when test="${score>=80}">
        你的成绩为一般
    </c:when>
    <c:when test="${score>=70}">
        你的成绩为良好
    </c:when>
    <c:when test="${score<=60}">
        你的成绩为不及格
    </c:when>
</c:choose>

</body>

```

c:forEach

```jsp
<%

    ArrayList<String> people = new ArrayList<>();
    people.add(0,"张三");
    people.add(1,"李四");
    people.add(2,"王五");
    people.add(3,"赵六");
    people.add(4,"田六");
    request.setAttribute("list",people);
%>


<%--
var , 每一次遍历出来的变量
items, 要遍历的对象
begin,   哪里开始
end,     到哪里
step,   步长
--%>
<c:forEach var="people" items="${list}">
    <c:out value="${people}"/> <br>
</c:forEach>

<hr>

<c:forEach var="people" items="${list}" begin="1" end="3" step="1" >
    <c:out value="${people}"/> <br>
</c:forEach>

```

# 9 JavaBean

实体类
JavaBean有特定的写法：

```java
必须要有一个无参构造
属性必须私有化
必须有对应的get/set方法；
```

一般用来和数据库的字段做映射 ORM；

ORM ：对象关系映射

```java
表—>类
字段–>属性
行记录---->对象
```

| id   | name   | age  | address |
| ---- | ------ | ---- | ------- |
| 1    | 阿坤01 | 3    | 茂名    |
| 2    | 阿坤02 | 18   | 茂名    |
| 3    | 阿坤03 | 100  | 茂名    |

```java
class People{
    private int id;
    private String name;
    private int id;
    private String address;
}

class A{
    new People(1,"张天泳1号",3，"西安");
    new People(2,"张天泳2号",3，"西安");
    new People(3,"张天泳3号",3，"西安");
}

```

过滤器实现登录拦截功能，放一个常量标记是否已登录

# 10 MVC 三层架构

什么是 MVC ： Model View Controller 模型、视图、控制器

## 10.1 以前

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201102213932114.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2MTg4MTI3,size_16,color_FFFFFF,t_70#pic_center)用户直接访问控制层，控制层就可以直接操作数据库；

```xml
ervlet--CRUD-->数据库
弊端：程序十分臃肿，不利于维护  
servlet的代码中：处理请求、响应、视图跳转、处理JDBC、处理业务代码、处理逻辑代码

架构：没有什么是加一层解决不了的！
程序猿调用
|
JDBC
|
Mysql Oracle SqlServer ....

```

## 10.2 MVC 三层架构

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201102214221869.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2MTg4MTI3,size_16,color_FFFFFF,t_70#pic_center)
Model

- 业务处理：业务逻辑（Service）
- 数据持久层：CRUD

View

- 展示数据
- 提供链接发起 Servlet 请求（a,form,img…）

Controller （Servlet）

- 接收用户的请求：（req：请求参数、Session 信息…）

- 交给业务层处理对应的代码

- 控制试图的跳转

  ```
  登录--->接收用户的登录请求--->处理用户的请求（获取用户登录的参数，username，password）---->交给业务层处理登录业务（判断用户名密码是否正确：事务）--->Dao层查询用户名和密码是否正确-->数据库
  ```

  

# 11 Filter（重点）

Filter：过滤器，用来过滤网站的数据；

- 处理中文乱码
- 登录验证

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201102214811292.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2MTg4MTI3,size_16,color_FFFFFF,t_70#pic_center)

Filter 开发步骤：

1. 导包
2. 编写过滤器   
   - 导包不要错
   - ![在这里插入图片描述](https://img-blog.csdnimg.cn/20201102214949591.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2MTg4MTI3,size_16,color_FFFFFF,t_70#pic_center)

```java
ublic class CharacterEncodingFilter implements Filter {

    //初始化：web服务器启动，就以及初始化了，随时等待过滤对象出现！
    public void init(FilterConfig filterConfig) throws ServletException {
        System.out.println("CharacterEncodingFilter初始化");
    }

    //Chain : 链
    /*
    1. 过滤中的所有代码，在过滤特定请求的时候都会执行
    2. 必须要让过滤器继续同行
        chain.doFilter(request,response);
     */
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        request.setCharacterEncoding("utf-8");
        response.setCharacterEncoding("utf-8");
        response.setContentType("text/html;charset=UTF-8");

        System.out.println("CharacterEncodingFilter执行前....");
        chain.doFilter(request,response); //让我们的请求继续走，如果不写，程序到这里就被拦截停止！【先写这句】
        System.out.println("CharacterEncodingFilter执行后....");
    }

    //销毁：web服务器关闭的时候，过滤会销毁
    public void destroy() {
        System.out.println("CharacterEncodingFilter销毁");
    }
}

```

在 web.xml 中配置Filter

```xml
<filter>
    <filter-name>CharacterEncodingFilter</filter-name>
    <filter-class>com.zty.filter.CharacterEncodingFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>CharacterEncodingFilter</filter-name>
    <!--只要是 /servlet的任何请求，会经过这个过滤器-->
    <url-pattern>/servlet/*</url-pattern>
    <!--<url-pattern>/*</url-pattern>-->
</filter-mapping>

```



# 12 监听器

实现一个监听器；

1. 编写一个监听器
    实现监听器的接口…

```java
//统计网站在线人数 ： 统计session
public class OnlineCountListener implements HttpSessionListener {

    //创建session监听： 看你的一举一动
    //一旦创建Session就会触发一次这个事件！
    public void sessionCreated(HttpSessionEvent se) {
        ServletContext ctx = se.getSession().getServletContext();

        System.out.println(se.getSession().getId());

        Integer onlineCount = (Integer) ctx.getAttribute("OnlineCount");

        if (onlineCount==null){
            onlineCount = new Integer(1);
        }else {
            int count = onlineCount.intValue();
            onlineCount = new Integer(count+1);
        }

        ctx.setAttribute("OnlineCount",onlineCount);

    }

    //销毁session监听
    //一旦销毁Session就会触发一次这个事件！
    public void sessionDestroyed(HttpSessionEvent se) {
        ServletContext ctx = se.getSession().getServletContext();

        Integer onlineCount = (Integer) ctx.getAttribute("OnlineCount");

        if (onlineCount==null){
            onlineCount = new Integer(0);
        }else {
            int count = onlineCount.intValue();
            onlineCount = new Integer(count-1);
        }

        ctx.setAttribute("OnlineCount",onlineCount);

    }


    /*这个案例启动就有三个人在线，原因是tomcat初次启动会失败，直到创建成功
    网页关闭网站在线人数不一定减少，原因是session还存在
    Session销毁：
    1. 手动销毁  getSession().invalidate();
    2. 自动销毁
     */
}

```

2.在 web.xml 中注册监听器

```xml
<!--注册监听器-->
<listener>
    <listener-class>com.zty.listener.OnlineCountListener</listener-class>
</listener>

```

3.看情况是否使用！



# 13、过滤器、监听器常见应用

用户登录之后才能进入主页！用户注销后就不能进入主页了！

1. 用户登录之后，向Session中放入用户的数据
2. 进入主页的时候判断用户是否已经登录

通过过滤来实现

![img](https://img-blog.csdnimg.cn/img_convert/08583e0a62a10b7a54682afc5d2c4588.png)

过滤器解决乱码问题

- 过滤器类

```java
package com.kun.filter;
import javax.servlet.*;
import java.io.IOException;

public class EncodingFilter implements Filter {
    public void init(FilterConfig filterConfig) throws ServletException {

    }

    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        servletRequest.setCharacterEncoding("utf-8");
        servletResponse.setCharacterEncoding("utf-8");

        filterChain.doFilter(servletRequest,servletResponse);   //一定要把链不断的传下去
    }

    public void destroy() {

    }
}

```

web.xml配置过滤器

```xml
    <filter>
        <filter-name>encoding</filter-name>
        <filter-class>com.kun.filter.EncodingFilter</filter-class>
    </filter>

    <filter-mapping>
        <filter-name>encoding</filter-name>
        <url-pattern>/</url-pattern>
    </filter-mapping>
    
```





# 14、JDBC

什么是JDBC：java链接数据库！

![image-20220501173951623](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220501173951623.png)

连接数据库，需要jar支持：

- java.sql
- javax.sql
- mysql-connector-java.....连接驱动（必须要导入）



实验环境搭建：

```sql
use jdbc;
create table users(
    id int primary key ,
    name varchar(40),
    password varchar(40),
    email varchar(60),
    birthday date
);

insert into users(id, name, password, email, birthday) VALUES (1,'张三','123456','zs@qq.com','2000-01-01');
insert into users(id, name, password, email, birthday) VALUES (2,'李四','123456','ls@qq.com','2000-01-01');
insert into users(id, name, password, email, birthday) VALUES (3,'王五','123456','ww@qq.com','2000-01-01');

select * from users;
```

导入数据库依赖

```xml
<!--        mysql的驱动-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.46</version>
        </dependency>
```

**JDBC固定步骤：**

1. 加载驱动
2. 连接数据库，代表数据库
3. 向数据库发送SQL的对象Statement：CRUD
4. 编写SQL（根据业务，不同的SQL）
5. 执行SQL
6. 关闭连接

```java
package com.kun.test;

import java.sql.*;

public class TestJdbc {
    public static void main(String[] args) throws ClassNotFoundException, SQLException {
        //配置信息
        String url = "jdbc:mysql://localhost:3306/jdbc?userUnicode=true&characterEncoding=utf-8&userSSL=true";
        String username = "root";
        String password = "123456";

        //1.加载驱动
        Class.forName("com.mysql.jdbc.Driver");

        //2.连接数据库
        Connection connection = DriverManager.getConnection(url, username, password);

        //3.向数据库发送SQL的对象Statement，PreparedStatement【安全】 ：CRUD
        Statement statement = connection.createStatement();
        

        //4.编写SQL

        String sql = "select * from users;";

        //5.执行SQL,返回一个ResultSet：结果集
        ResultSet resultSet = statement.executeQuery(sql);


        while (resultSet.next()){
            System.out.println("id="+resultSet.getObject("id"));
            System.out.println("id="+resultSet.getObject("name"));
            System.out.println("id="+resultSet.getObject("password"));
            System.out.println("id="+resultSet.getObject("email"));
            System.out.println("id="+resultSet.getObject("birthday"));

        }
        //6.关闭连接，释放资源（一定要关）
    }
}

```



