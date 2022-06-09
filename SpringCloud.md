# SpringCloud

**这个阶段应该如何学习~**

```java
三层架构
    
框架演进：
    pring IOC AOP
    
    SpringBoot， 新一代的javaEE标准，自动装配
    
    模块化 ~  all in one
    
    模块化开发===all in one， 代码没变化
    
    
微服务架构4个核心问题？
    1.服务很多，客户端该怎么访问？
    2.这么多服务？服务之间如何通信？
    3.这么多服务？如何治理？
    3.服务挂了怎么办？
    
解决方案：
    Springcloud  并不是一个技术，而是一个生态！他就是为了解决上面四个问题。
     
    1.spring cloud NetFlix   一站式解决方案！
    	api网关，zuul组件
    	Feign   ---HttpClient -----  http通讯方式，同步，阻塞
    	服务注册发现：Eureka
    	熔断机制：Hystrix
    。。。。。。
    
    2.Apache Bubbo Zookeeper   半自动，需要整合别人的！
    	API：没有，找第三方组件，或者自己实现
    	Dubbo
    	Zookeeper
    	熔断机制没有，借助 Hystrix
    
   		Dubbo这个方案并不完善~
    
    3.Spring Cloud Alibaba		一站式解决方案！更简单
    	
    	
新概念：服务网格 ~Server Mesh
    istio
    
    
万变不利其宗
    1.API网关
    2.Http，RPC
    3.注册和发现
    4.熔断机制
    
    
为什么衍生出这四个问题？
    网络不可靠！会出现丢帧，拦截。。。。
    
```



常见面试题：

1. 什么是微服务
2. 微服务之间如何独立通讯的？
3. SpringCloud和Dubbo有哪些区别？
4. SpringBoot和SpringCloud，请你谈谈对他们的理解
5. 什么是服务熔断？什么是服务降级
6. 微服务的优缺点分别是什么？说下你在项目中开发中遇到的坑
7. 你所知道的微服务技术栈有哪些？请列举一二
8. eureka和zookeeper都可以提供服务注册的功能，请说说两个的区别。

。。。。。。。。。



# 1、什么是微服务？

- 就目前而言，对于微服务，业界并没有一个统一的，标准的定义。
- 但统统而言，微服务架构是一种架构模式，或者说是一种架构风格。**它提倡将单一的应用程序划分成一组小的服务**，每个服务运行在其独立的自己的进程内，服务之间互相协调，互相配置，为用户提供最终价值。服务之间采用轻量级的通信机制互相沟通，每个服务都围绕着具体的业务进行构建，并且能够被独立的部署到生产环境中，另外，应尽量避免统一的，集中式的服务管理机制，对具体的一个服务而言，应根据业务上下问，选择合适的语言，工具对其进行架构，可以有一个非常轻量级的集中市管理来协调这些服务，可以使用不同的语言来编写服务，也可以使用不同的数据存储。

简单来说：微服务是一种架构风格，它要求我们在开发一个应用的时候，这个应用必须构建成一系列小服务的组合；可以通过http的方式进行互通。



从技术维度来理解：

- 微服务化的核心就是将传统的一站式应用，根据业务拆分成一个一个的服务，彻底地去耦合，每个微服务提供单个业务功能的服务，一个服务做一个事情，从技术角度看就是一种小而独立的处理过程，类似进程概念，能够自行单独启动或销毁，拥有自己独立的数据库。



## 1、微服务与微服务架构



**微服务**

强调的是服务的大小，他关注的是某一个点，是具体解决某一个问题/提供落地对应服务的一个服务应用，下一的看，可以看作是IDEA中的一个个微服务工程，或者Moudle

```java
IDEA 工具里面使用Maven开发的一个个独立的小Moudle，它具体是使用springboot开发的一个小模块，专业的事情交给专业的模块来做，一个模块就做着一件事情
    
强调的是一个个的个体，每个个体完成一个具体的任务或者功能！
```



**微服务架构**

一种新的架构形式，Martin Fowler， 2014年提出

微服务架构是一种架构模式，它提倡将单一应用程序划分称为一组小的服务，服务之间互相协调，互相配合，为用户提供最终价值。每个服务运行在其独立的进程中，服务与服务之间采取轻量级的通讯机制互相协作，每个服务都围绕着剧吐的业务进行构建，并且能够独立的部署到生产环境中，另外，应尽量避免统一的，集中式的服务管理机制，对具体的一个服务而言，应更具业务上下文，选择合适的语言，工具对其进行构建。



## 2、微服务的优点

**优点：**

- 单一职责原则
- 每个服务足够内聚，足够小，代码容易理解，这样能聚焦一个指定的业务功能或业务需求
- 开发简单，开发效率高，一个服务可能就是专一的只干一件事；
- 微服务能够被小团队单独开发，这个小团队是2-5人的开发人员组成
- 微服务是松耦合的，是有功能意义的服务，无论是在开发阶段或部署阶段都是独立的。
- 微服务能使用不同的语言开发
- 易于和第三方集成，微服务允许容易且灵活的方式集成自动部署，通过持续集成工具，如jenkins，Hudson，bamboo
- 微服务是易于被一个开发人员理解，修改和维护，这样小团体能够更关注自己的工作成果，无需通过合作才能体现价值。
- 微服务允许你利用融合最新技术。
- **微服务只是业务逻辑代码，不会和HTML，css或其他界面混合**
- **每个微服务都有自己的储存能力，可以有自己的数据库，也可以有统一的数据库**



**缺点：**

- 开发人员要处理分布式系统的复杂性
- 多服务运维难度，随着服务的增加，运维的压力也在增大
- 系统部署依赖。
- 服务间通信成本。
- 数据一致性
- 系统集成测试
- 性能监控



## 3、微服务技术栈

![image-20220604111202361](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220604111202361.png)



## 4、为什么选择SpringCloud作为微服务架构

1、**选型依据**

- 整体解决方案和框架成熟度
- 社区热度
- 可维护性
- 学习曲线

2、**当前各大IT公司用的微服务架构有哪些？**

- 阿里：dubbo+HFS
- 京东：JSF
- 新浪：Motan
- 当当网Dubbox
- 。。。。

# 2、微服务架构

要说微服务架构，先得说说过去我们得单体应用架构。



## 单体应用架构

​	所谓单体应用架构风格（all in one）是指，我们将一个应用中的所有服务都封装在一个应用中。

​	无论ERP、CRM或者其他什么系统，你都把数据库访问，web访问，等等各个功能放到一个war包内。

- 这样做的好处是，易于开发和测试；也十分方便部署，当需要扩展时，只需要将war赋值多份，然后放到这个服务器上，再做个负载均衡就可以了。
- 单体应用架构的缺点是：哪怕我要修改一个非常小的地方，我都需要停调整个服务，重新打包、部署这个应用war包。特别是对一个大型应用，我们不可能把所有内容都放在一个应用里面，如何维护如何分工合作都是问题。



## 微服务架构

​	all in one 的架构方式，我们把所有的功能单元放在一个应用里面，然后我们把整个应用部署导服务器上，如果负载能力不行，我们将整个应用进行水平赋值，进行扩展，然后再负载均衡。



​	所谓微服务架构，就是打破之前all in one的架构方式，把每个功能元素都独立出来。把独立出来的功能元素动态组合，需要的功能元素才去拿来组合，需要多一些时可以整合多个功能元素。所以微服务架构是对需要的功能元素进行复制，而没有对整个应用进行复制。



这样做的好处是：

1. 节省了调用资源
2. 每个功能元素的服务都是一个可替换的，可独立升级的软件代码。



论文地址：https://www.cnblogs.com/liuning8023/p/4493156.html



# 3、SpringCloud入门概述

## 1、SpringCloud是什么？

![点击查看图片来源](https://pics5.baidu.com/feed/cb8065380cd79123cb65da47c87b7785b3b780d6.jpeg?token=66550c060ef8428b57da2f1eceb992a8)

![image-20220604143648968](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220604143648968.png)

SpringCloud是**基于SpringBoot提供了一套微服务解决方案**，包括服务注册与发现，配置中心，全链路监控，服务网关，负载均衡，熔断器等组件，除了基于NetFlix的开源组件做高度抽象封装之外，还有一些选型中立的开源组件。

SpringCloud利用SpringBoot的开发便利性，巧妙地简化了分布式系统基础设施的开发，SpringCloud为开发人员提供了快速构建分布式的一些工具，**包括装配管理，服务发现，断路器，路由，微代理，事件总线，全局锁，决策竞选，分布式会话等等，**他们都可以用SpringBoot的开发风格做到一键启动和部署。

SpringBoot并没有重复造轮子，他只是将目前各家公司开发的比较成树，经得起实际考研的服务框架组合起来，通过SpringBoot风格再封装，屏蔽掉了复杂的配置和实现原理，**最终给开发者留出了一套简单易懂，易部署和易维护的分布式系统开发工具包。**

SpringCloud是分布式微服务框架下的一站式解决方案，是各个微服务架构落地技术的集合体，俗称微服务全家通。



## 2、SpringCloud和SpringBoot关系

- SpringBoot专注于快速方便的开发单个个体微服务
- SpringCloud是关注全局的为微服务协调整理治理框架，它将SpringBoot开发的一个个单体微服务整合并管理起来，为各个微服务之间提供：配置管理，服务发现，断路器，路由，微代理，事件总线，全局锁，决策竞选，分布式会话等等集成服务。
- SpringBoot可以离开SpringCloud独立使用，开发项目，但是SpringCloud离不开SpringBoot，属于依赖关系。
- **SpringBoot专注于快速、方便的开发单个个体微服务，SpringCloud关注全局的服务治理宽假**

SpringBoot用来是对微服务的实现，SpringCloud用来是对微服务架构的实现，



## 3、Dubbo和SpringCloud技术选型

### 	1、分布式+服务治理Dubbo

​			目前最成熟的互联网架构：应用服务化拆分+消息中间件

![image-20220604150741957](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220604150741957.png)



### 	2、Dubbo和SpringCloud对比

​		可以看一下社区活跃度

​		https://github.com/dubbo

​		https://github.com/spring-cloud

![image-20220604151242150](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220604151242150.png)

**最大区别：SpringCloud抛弃了Dubbo的RPC通信，采用的是基础HTTP的REST方式**

严格的说，这两种方式各有优势。虽然从一定程度上说，后者牺牲了服务调用的性能，但也避免了上面提到的原生RPC带来的问题。而且REST相比RPC更为灵活，服务提供方和调用方的依赖只依靠一纸契约，不存在代码级别的强依赖，这在强调快速演化的微服务环境下，显得更加合适。

**品牌机与组装机的区别**

很明显，SpringCloud的功能比Dubbo更加强大，涵盖面更广，而且作为Spring的拳头项目，它也能够与SpringFramework、SpringBoot、SpringData、SpringBatch等其他Sprig项目完美融合，这些对于微服务而言是至关重要的。使用Dubbo构建的微服务架构就像组装电脑，各环节我们的选择自由度很好，但是最终结果很有可能因为一条内存质量不行就点不亮了，总是让人不怎么放心，但是如果你是一名高手，那这些都不是问题；而SpringCloud就像品牌机，在SpringSource的整合下，做了大量的兼容性测试，保证了机器拥有更高的稳定性，但是如果要在使用非原装组件外的东西，就需要有对其基础有足够的了解。

**社区支持与更新力度**

最为重要的是，Dubbo停止了5年左右的更新，虽然2017.7重启了。对于技术发展的新需求，需要由开发者自行扩展升级（比如当当网弄出了Dubbox），这对很多想要采用微服务架构的中小软件组织，显然是不太合适的，中小公司没有这么强大的技术能力去修改Dubbo源码+周边的一整套解决方案，并不是每个公司都有阿里的大牛+真实的线上生产环境测试过。

**设计模式+微服务才分思想：软实力，活跃度**

**总结：**

曾风靡国内的开源RPC服务框架Dubbo在重启维护后，令许多用户为止雀跃，但同时也迎来了一些质疑的声音。互联网技术发展迅速，Dubbo是否还能跟上时代？Bubbo与SpringCloud相比又有何优势和差异？是否会有关举措保证Dubbo的后续更新频率。

人物：Dubbo重启维护开发的刘军，主要负责人之一。

刘军，阿里巴巴中间件高级研发工程师，主导了Dubbo重启维护以后的几个发版计划，专注于高性能RPC框架和微服务相关领域。曾负责网易考拉RPC框架的研发及指导在内部使用，参与了服务治理平台、分布式跟踪系统、分布式一致性框架等从无到有的设计与开发过程。

**解决的问题于不一样，Dubbo的定位是一款RPC框架，SpringCloud的目标是微服务框架下的一站式解决方案**



## 4、SpringCloud能干什么？

- Distributed/versioned configuration（分布式/版本控制）
- Service registration and discovery（服务注册与发现）
- Routing（路由）
- Service-to-service calls （服务到服务的调用）
- Load balancing    （负载均衡配置）
- Circuit Breakers   （断路由）
- Global locks       （全局锁）
- Leadership election and cluster state
- Distributed messaging（分布式消息管理）

  

**文档命名规则**

![image-20220604154803057](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220604154803057.png)

```java
SpringCloud 是一个由众多独立子项目组成的大型综合项目，每个子项目有不同的发行节奏，都维护者自己的发布把版本号。Spring Cloud通过一个资源清单BOM（Boll of Materials）来管理每个版本的子项目清单。为了避免与子项目的发布号混淆，所有没有采用版本号的方式，而是通过命名的方式。
    
这些版本名称的命名方式采用了伦敦地铁站的名称，同时根据字母表的顺序来对应版本时间顺序，比如：最早的Release版本：Angel，第二个Relearse版本；Birxton，然后是Camden、Dalston、EdgWare。
   
现在的命名方式采用了日期的方式
```



参考书：

- https://www.springcloud.cc/spring-cloud-netflix.html
- 中文API文档：https://www.springcloud.cc/spring-cloud-dalston.html
- SpringCloud中国社区：http://springcloud.cn/
- SpringCloud中文网：https://www.springcloud.cc/



# 4、环境搭建

## 1、总体介绍

![image-20220604161121823](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220604161121823.png)

## 2、创建项目

### 1、创建一个普通的maven项目

![image-20220604161801584](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220604161801584.png)

这个项目是一个父项目，可以删除成功创建后的src目录。

![image-20220604162123948](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220604162123948.png)

### 2、编写总项目pom文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.kun</groupId>
    <artifactId>springcloud</artifactId>
    <version>1.0-SNAPSHOT</version>

<!--    打包方式 pom-->
    <packaging>pom</packaging>

<!--   版本号 -->
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
        <junit.version>4.12</junit.version>
        <lombok.version>1.18.24</lombok.version>
        <log4j.version>1.2.17</log4j.version>
    </properties>

<!--    依赖管理-->
    <dependencyManagement>
        <dependencies>
<!--            SpringCloud的依赖-->
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>Greenwich.SR1</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>

<!--            SpringBoot依赖-->
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-dependencies</artifactId>
                <version>2.1.4</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
<!--            数据库-->
            <dependency>
                <groupId>mysql</groupId>
                <artifactId>mysql-connector-java</artifactId>
                <version>8.0.28</version>
            </dependency>
<!--            数据源-->
            <dependency>
                <groupId>com.alibaba</groupId>
                <artifactId>druid</artifactId>
                <version>1.2.8</version>
            </dependency>
<!--            SpringBoot启动器-->
            <dependency>
                <groupId>org.mybatis.spring.boot</groupId>
                <artifactId>mybatis-spring-boot-starter</artifactId>
                <version>2.2.2</version>
            </dependency>
<!--            以下四个包主要用于日志测试-->
<!--            logback-->
            <dependency>
                <groupId>ch.qos.logback</groupId>
                <artifactId>logback-core</artifactId>
                <version>1.2.11</version>
            </dependency>
            
<!--            junit-->
            <dependency>
                <groupId>junit</groupId>
                <artifactId>junit</artifactId>
                <version>${junit.version}</version>
                <scope>test</scope>
            </dependency>
<!--            lombok-->
            <dependency>
                <groupId>org.projectlombok</groupId>
                <artifactId>lombok</artifactId>
                <version>${lombok.version}</version>
            </dependency>
<!--            log4j-->
            <dependency>
                <groupId>log4j</groupId>
                <artifactId>log4j</artifactId>
                <version>${log4g.version}</version>
            </dependency>
        </dependencies>
    </dependencyManagement>
    

</project>


```

注：写入依赖时，并没有导入包，因为dependencyManagement只是管理，真正导入你还需要去子项目中使用，他会指向父类的依赖

### 3、第一个模块【接口模块，实现实体类】

1. ​	新建一个module

![image-20220604164947062](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220604164947062.png)

​	2.导入依赖

```xml
<!--    当前的Module自己需要的依赖，如果父依赖中已经配置了版本，这里就不用写了-->
    <dependencies>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
    </dependencies>

```

3. 创建数据库

![image-20220604170421000](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220604170421000.png)

4、创建一个pojo的包

![image-20220604172327889](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220604172327889.png)

5、编写pojo实体类

```java
package com.kun.springcloud.pojo;

import lombok.Data;
import lombok.NoArgsConstructor;
import lombok.experimental.Accessors;
import java.io.Serializable;
@Data
@NoArgsConstructor
@Accessors(chain = true)  //链式写法
/*
* 链式写法：
*   Dept dept = new Dept();
*   dept.setDeptNo(11).setDName("ssss").setDb_source('001)
*/
//网络通信中，所有的实体类务必实现序列化
public class Dept implements Serializable {   //Dept 实体类， orm  类表关系映射

    private Long deptNo;    //主键
    private String dName;

    //这个数据时存在哪个数据库的字段~~微服务，一个服务对应一个数据库,同一个信息可能存在不同的数据库
    //如果数据库的字段由下横线，要写驼峰命名，不然获取不到值
    //private String db_source;
    private String dbSource;

    //构造器只需要一个参数就够了，第一主键自增，db_source由数据库函数自动生成
    public Dept(String dName) {
        this.dName = dName;
    }


}

```

至此，这个微服务就写完了，因为要进行服务才分，这个类只管理pojo

### 4、第二个模块【服务提供者】

1、再次新建一个模块，命名为springcloud-provider-dept-8001

2、在pom文件导入相关依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>springcloud</artifactId>
        <groupId>com.kun</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>springcloud-provider-dept-8001</artifactId>
    <dependencies>
<!--        我们需要拿到实体类，所以要配置api module-->
        <dependency>
            <groupId>com.kun</groupId>
            <artifactId>springcloud-api</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
<!--        junit-->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <scope>test</scope>
        </dependency>
<!--        logback-->
        <dependency>
            <groupId>ch.qos.logback</groupId>
            <artifactId>logback-core</artifactId>
        </dependency>
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
        </dependency>
<!--        test-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-test</artifactId>
            <version>2.6.7</version>
        </dependency>
<!--        这是一个web项目-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <version>2.6.7</version>
        </dependency>
<!--        jetty-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jetty</artifactId>
            <version>2.1.4.RELEASE</version>
        </dependency>
<!--        热部署工具-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <version>2.6.7</version>
        </dependency>
        <!--        数据源druid-->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.2.8</version>
        </dependency>
         <dependency>
                <groupId>mysql</groupId>
                <artifactId>mysql-connector-java</artifactId>
                <version>8.0.28</version>
            </dependency>

    </dependencies>

</project>
```

3、创建核心配置文件application.yml

```yaml
server:
  port: 8081
#mybatis配置
mybatis:
  type-aliases-package: com.kun.srpingcloud.pojo
  config-location: classpath:mybatis/mybatis-config.xml
  mapper-locations: classpath:mybatis/mapper/*.xml
  #将驼峰命名转换为键值映射
  configuration:
    map-underscore-to-camel-case: true
#spring的配置
spring:
  application:
    name: springcloud-provider-dept  #服务启动者
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource   #数据源
    driver-class-name:  com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/db01?userUnicode=true&&characterEncoding=utf-8
    username: root
    password: dyk160513140.

```

4、创建Mapper类

5、编写Mapper文件

6、编写service层

1. ​	写service接口，跟Mapper接口一样
2.  写service接口实现，直接调用mapper实现的东西

7、写controller层



**报错处理**

1、配置和配置文件不能同时存在。

Property 'configuration' and 'configLocation' can not specified with together

![image-20220604221053028](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220604221053028.png)

解决方案：删除configuration



### 5、第三个模块【服务消费者】

1、创建新的模块springcloud-consumer-dept-80

2、导入相应的包

3、编写配置文件，只需配置端口号

4、编写配置类，配置RestTemplate的bean，使用Configuration注释，编写函数返回RestTemplate，再使用bean注释

5、编写Controller类，然后使用RestTemplate去调用上一个模块的地址。



### 6、配置Eureka项目

#### 1、配置注册服务端

（在前面两个模块的基础之上）

1、创建注册服务端模块

2、导入依赖

```xml
 <!-- https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-starter-eureka-server -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-eureka-server</artifactId>
            <version>1.4.7.RELEASE</version>
        </dependency>

```

3、编写配置文件application.yaml

```yaml
server:
  port: 7001   #此项目的运行端口

#eureka配置
eureka:
  instance:
    hostname: localhost    #设置实例名
  client:
    register-with-eureka: false   #表示是否向eureka注册中心注册自己，因为这是服务端，所以不用注册
    fetch-registry: false   #如果为false，则表示自己为注册中心
    service-url:   # 监控页面，就是注册中心的一个地址，客户端需要注册到这个地址
      defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/
```

4、启动服务

在入口程序中加上注解@EnableEurekaServer



#### 2、配置服务提供者端

回到服务提供者模块

1、导入依赖

```xml
<!-- https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-starter-eureka -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-eureka</artifactId>
    <version>1.4.7.RELEASE</version>
</dependency>
```

2、编写配置文件application.yaml

```yaml
#Eureka的配置，服务注册到那里
eureka:
  client:
    service-url:
      defaultZone: http://localhost:7001/eureka/   #注册到服务端

  instance:
    instance-id: springcloud-provider-dept-8001   #修改eureka上的默认描述信息
```

3、开启功能

​	在主程序入口中，增加注解@EnableEurekaClient 



### 7、配置服务器集群

![image-20220605214141918](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220605214141918.png)

1、创建三个Eureka服务提供者，本质史跟7001端口的项目一样，只需要改端口号

2、在每个服务中，注册服务器上绑定其他的注册服务器地址

3、在服务提供者注册服务时，也要绑定所有的的注册服务中心地址

![image-20220605214425839](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220605214425839.png)

### 8、实现负载均衡

ribbon是基于Nexfit Ribbon实现的一套客户端负载均衡,因此，我们实现负载均衡是在客户端，也是再服务消费者端

1、导入依赖，在springcloud-consumer-dept-80模块

```xml
<!--        Ribbon-->
        <!-- https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-starter-netflix-ribbon -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-ribbon</artifactId>
            <version>1.4.6.RELEASE</version>
        </dependency>

<!--        eureka-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-eureka</artifactId>
            <version>1.4.6.RELEASE</version>
        </dependency>
```

2、编写配置文件

```yml

#Eureka配置
eureka:
  client:
    register-with-eureka: false # 不向eureka注册自己
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/
    fetch-registry: true
```

3、启动负载均衡服务

![image-20220606162021808](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220606162021808.png)

因为请求时由RestTemplate去分发，所以在它的基础上实现负载均衡

4、启动eureka服务

![image-20220606162123008](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220606162123008.png)



# 5、Eureka服务注册与发现

## 5.1、什么是Eureka

- Eureka怎么读？
- Netfix在设计Eureka时，遵循的就是AP原则
- Eureka是Netfix的一个子模块，也是核心模块之一。Eureka是一个基于REST的服务，用于定位服务，以实现云端中间层服务发现和故障转移，服务注册与发现对于微服务来说是非常重要的，有了服务注册与发现，只需要使用服务的标识符，就可以访问到服务，而不是修改服务调用的配置文件了，功能类似于Dubbo的注册中心，比如Zookeeper;



## 5.2、原理详解

- Euraka基于架构

  - SpringCloud封装了NetFix公司开发的Eureka模块实现服务注册和发现（对比Zookeeper）
  - Eureka采用了C-S的架构设计，EurekaServer作为服务注册功能的服务器，他是服务注册中心
  - 而系统中其他微服务。使用Eureka的客户端连接到EurekaServer并维持心跳连接。这样系统的维护人员就可以通过EurekaSrever来监控系统中各个微服务是否正常运行，SpringCloud的一些其他模块（比如Zuul）就可以通过EurekaServer来发现系统中的其他微服务，并执行相关的逻辑
  - 和Dubbo架构对比

  ![点击查看图片来源](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fpic3.zhimg.com%2Fv2-d84816b9fc2f769172e13fb00018280a_b.jpg&refer=http%3A%2F%2Fpic3.zhimg.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=auto?sec=1657004294&t=4763c9e2a20edcbe89cb47d634478644)

  **Dubbo**

  ![点击查看图片来源](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fimage.mamicode.com%2Finfo%2F201801%2F20180118233049340612.png&refer=http%3A%2F%2Fimage.mamicode.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=auto?sec=1657004333&t=a3b8bf02d340b3eac34abbf59a44044a)
  - Eraka包含两个组件：**Eureka Server和Eureka Client**
  - Eureka Server提供服务之策服务，各个节点启动后，会在EurekaServer中进行注册，这样Eureka Server中的服务注册表中将hi存储所有可用服务节点的信息，服务节点的信息可以在界面中直观看到。
  - Eureka Client是一个java客户端，用户简化EurekaServer的交互，客户端同时也具备一个内置的，使用轮询负载算法的负载均衡器。在应用启动后，将会向EurekaServer发送心跳（默认周期为30秒）。如果EurekaServer在多个心跳周期内没有收到某个节点的心跳，Eureka将会从服务注册表中把这个服务节点移除掉（默认周期为90秒）



- 三大角色
  - Eureka Server：提供服务注册于发现
  - Service Provider：将自身服务注册到Eureka中，从而使消费方面能够找到
  - Service Consumer：服务消费方从Eureka中获取注册服务列表，从而找到消费服务。



## 5.3、Eureka自我保护机制

一句话总结：某时刻一个微服务不可用了，eureka不会立刻清理，依旧会对该微服务的信息进行保存！

- 默认情况下，如果EurekaServer在一定时间内没有接收到某个微服务实例的心跳，EurekaServer将会注销该实例（默认90秒）。但是当网络分区故障发生时，微服务与Eureka之间无法正常通行，以上行为可能变得非常危险了--因为微服务本身其实是健康的，**此时本不应该注销这个服务。**Eureka通过**自我保护机制**来解决这个问题--当EurekaServer节点在短时间内丢失过多客户端时（可能发生了网络分区故障），那么这个节点就会进入自我保护模式。一旦进入该模式，EurekaServer就会保护服务注册表中的信息，不再删除服务注册表中的数据（也就是不会注销任何微服务）。当网络故障恢复后，该EurekaServer节点就会自动退出保护模式。
- 在自我保护模式中，EurekaServer会保护服务注册表中的信息，不再销毁任何服务实例。当它收到的心跳数恢复到阈值以上时，该EurekaServer节点就会自动退出自我保护模式，它的设计哲学就是宁可保留错误的服务注册信息，也不盲目注销任何可能健康的服务实例。一句话：好死不如赖活。
- 综上，自我保护模式是一种应对网络异常的安全保护措施。它的架构这学是宁可同时保留所有的微服务（健康的微服务和不健康的微服务都会保留），也不盲目注销任何健康的微服务。使用自我保护模式，可以让Eureka集群更加的健壮和稳定。
- 在SpringCloud中，可以使用eureka.server.enable-self-presevation=false禁用自我保护模式【不推荐关闭自我保护机制】



## 5.4、对比Zookeeper

**回顾CAP原则**

RDBMS（Mysql，Oracle、sqlServer）=====》ACID

NoSQL（Redis，mongdb）====》CAP



**ACID是什么？**

- A(Atomicity)原子性
- C（Consistency）一致性
- I（Isolation）隔离性
- D（Durability）持久性



**CAP是什么？**

- C（Consistency）强一致性
- A（Available）可用性
- P（Partition tolerance）可区容错性



CAP的三进二：CA、AP、CP（无法保证三个原则都满足）



**CAP理论核心**

- 一个分布式系统不可能同时很好的满足一致性，可用性和区分容错性这三个需求
- 根据CAP原理，将NoSQL数据库分成了满足CA原则，满足CP原则，满足AP原则三大类：
  - CA：单点集群，满足一致性，可用性系统，通常可扩展性较差
  - CP：满足一致性，分区容错性的系统，通常性能不是特别高
  - AP：满足可用性，分区容错性的系统，通常可能对一致性要求低一些。



#### 作为服务注册中心，Eureka比Zookeeper好在哪里？

著名的CAP理论指出，一个分布式系统不可能同时满足C（一致性），A（可用性）、p（容错性）。

由于分区容错性p在分布式系统中是必须要保证的，因此我们只能在A和C中进行权衡。

- Zookeeper保证的是CP；
- Eureka保证的是AP



**zookeeper保证的是CP**（一致性和容错性）

​	当向注册中心查询服务列表时，我们容忍注册中心返回的是几分钟以前的注册信息，但不能接受服务直接down掉不可用。也就是说，服务注册功能对可用性的要求要高于一致性。但zk会出现这样一种情况，当master节点因为网络故障与其他节点失去联系时，剩余节点会重新进行leader选举。问题在于，选举leader的时间太长，30-112s，且选举期间整个zk集群都是不可用的，这就导致在选举期间注册服务瘫痪。在云部署的环境下，因为网络问题使得zk集群失去master节点是较大概率会发生的事件，虽然服务最终能恢复，但是漫长的选举事件导致的注册长期不可用是不能容忍的。



**Eureka保证的是AP**

​	Eureka看明白了这一点，因此在设计时就优先保证可用性。**Eureka各个节点都是平等的，**几个节点挂掉不会影响正常节点的工作，剩余的节点依然可以提供注册和查询服务。而Eureka的客户端在向某个Eureka注册时，如果发现连接失败，则会自动切换至其他接待你，只要有一台Eureka还在，就能保证注册服务的可用性，只不过查到的信息可能不是最新的，除此之外，Eureka还有一种自我保护机制，如果在15分钟内超过85%的节点都没有正常心跳，那么Eureka就认为客户端与注册中心出现了网络故障，此时会出现以下几种情况：

1. Eureka不再从注册列表中移除，因为长时间按没收到心跳而应该过期该服务
2. Eureka仍然能够接收新服务的注册和查询请求，但是不会被同步到其他节点上（即保证当前节点依然可用）
3. 当网络稳定时，当前实例新的注册信息会被同步到其他节点中。



**因此，Eureka可以很好的应对网络故障导致部分节点失去联系的情况，而不会像Zookeeper那样使整个注册服务瘫痪。**



# 6、Ribbon

## 6.1、ribbon是什么?

- SpringCloud Ribbon是基于Netfix Ribbon实现的一套**客户端负载均衡的工具。**【重点，客户端】
- 简单的说，Ribbon是Netfix发布的开源项目，主要功能是提供客户端的的软件负载均衡算法，将NetFix的中间层服务连接在一起。Ribbon的客户端组件提供一系列完整的配置项如：连接超时、重试等等。简单的说，就是在配置文件中列出LoadBalancer（简称LB：负载均衡）后面所有的机器，Ribbon会自动的帮助你基于某种规则（如简单的**轮询**，**随机连接**等等）去连接这些机器。我们也很容易使用Ribbon实现自定义的负载均衡算法！



## 6.2、ribbon能干嘛？

- LB，即负载均衡（Load Balance），在微服务或分布式集群中经常用的一种应用。
- 负载均衡简单的说就是将用户的请求平摊的分配到多个服务上，从而达到系统的HA（高可用）。
- 常见的负载均衡软件有Nginx，Lvs等等
- dubbo、SpringCloud中均给我们提供了负载均衡，**SpringCloud的负载均衡算法可以自定义**
- 负载均衡简单分类：
  - 集中式LB
    - 即在服务的消费方式和提供方之间使用独立的LB设施，如Nginx，由该设施负责把访问请求通过某种策略转发至服务的提供方！
  - 进程式LB
    - 将LB逻辑集成到消费方，消费方从服务注册中心获知哪些地址可用，然后自己再从这些地址中选出一个合适的服务器。
    - Ribbon就属于进程内LB，它只是一个类库，集成与消费方进程，消费方通过它来获取到服务提供方的地址！



![image-20220606163920434](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220606163920434.png)

总结：Ribbon是一个客户端负载均衡的工具，主要就是将同一个微服务【同一个服务的名字一定要相同】放在多个服务器中，每个微服务提供者向Eureka服务注册中心注册，Ribbon在客户端实现，通过服务名称去轮询分发这些请求。



## 6.3自定义负载均衡算法

负载均衡源码

![image-20220606191851333](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220606191851333.png)

这里的注解主要是开启负载均衡。

负载均衡算法，主要由IRule接口实现

![image-20220606193633584](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220606193633584.png)

![image-20220606200716540](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220606200716540.png)





# 7、Feign负载均衡

## 7.1、简介

feign是声明式的web service客户端，它让微服务之间的调用变得更加简单了，类似controller调用service。Spring Cloud集成了Ribbon和Eureka，可在使用Feign时提供负载均衡的http客户端。

只需要创建一个接口，然后添加注解即可。



feign，主要是社区，大家都习惯面向接口编程，这个时很多开发人员的规范。调用微服务访问的两种方法。

1. 微服务名字【ribbon】
2. 接口和注解【feign】



**Feign能干什么?**

- Feign旨在使编写Java Http客户端变得更加容易。
- 前面在使用Ribbon+RestTemplate时，利用RestTemplate对Http请求的封装处理，形成了一套模板化的调用方法。但是在实际开发中，由于对服务依赖的调用可能不止一处，往往一个接口会被多处调用，所以通常会针对每个微服务自行封装一些客户端类来包装这些依赖的服务的调用。所以，feign在此基础上做了进一步封装，由他来帮助我们定义和实现依赖服务接口的定义，**在feign的实现下，我们只需要创建一个接口并使用注解的方式来配置它（类似以前Dao接口上标注Mapper注解，现在时一个微服务接口上面标注一个Feign注解即可）。**即可完成对服务提供方的接口绑定，简化了使用Spring Cloud Ribbon时，自动封装服务调用客户端的开发量。



# 8、Hystri

**分布式系统面临的问题**

复杂分布式体系结构中的应用程序有数十个依赖关系，每个依赖关系在某些时候将不可避免的失败！



**服务雪崩**

​	多个微服务之间调用的时候，假设微服务A调用微服务B和微服务C，weifuwuB和微服务C又调用其他微服务，这就是所有的“扇出“、如果扇出的链路上某个微服务的调用响应时间过长或者不可用，对微服务A的调用就会占用越来越多的系统资源，进而引起系统崩溃，所谓的”雪崩效应“。

​	对于高流量的应用来说，单一的后端依赖可能会导致所有服务器上的所有资源都在几秒钟内饱和。比失败更糟糕的时，这些应用程序还可能导致服务之间的延迟增加，备份队列，线程和其他系统资源紧张，导致整个系统发生更多的级联故障，这些都表示需要对故障和延迟进行隔离和管理，以便单个依赖关系的失败，不能取消整个应用程序或系统。

​	我们需要 ”弃车保帅“



**什么是Hystrix**

​	Hystrix是一个用于分布式系统的延迟和容错的开源库，在分布式系统里，许多依赖不可避免的会调用失败，比如超时，异常等，Hystrix能够保证在一个依赖出问题的情况下，不会导致整体服务失败，避免级联故障，以提高分布式系统的弹性。

​	”断路器“本身是一种开关装置，当某个服务单元发生故障之后，通过断路器的故障监控（类似熔断保险丝），**向调用方向返回一个服务预期的，可处理的备选响应（FallBack），而不是长时间的等待或者抛出调用方法无法处理的异常，这样就可以保证了服务调用方的线程不会被长时间，不必要的占用**，从而避免了故障在分布式系统中蔓延，乃至雪崩。

![点击查看图片来源](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fpic1.zhimg.com%2Fv2-49338de5cffa32fdddd7d69c184ccc30_b.png&refer=http%3A%2F%2Fpic1.zhimg.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=auto?sec=1657152601&t=d1d378096f38c39721afec06439df739)

**Hystrix能干嘛？**

- 服务降级
- 服务熔断
- 服务限流
- 接近实时的监控
- .......



## 8.1、服务熔断

**服务熔断是什么？**

​	熔断机制是对应雪崩效应的一种微服务链路保护机制。

​	当扇出链路的某个微服务不可用或者响应时间太长时，就会进行服务降级，**进而熔断该节点微服务的调用，快速返回错误的响应信息。**当检测到该节点微服务调用响应正常后恢复调用链路。在SpringCloud框架里熔断机制通过Hystrix实现。Hystrix实现。Hystrix会监控微服务间调用的状况，当失败的调用到一个阈值，缺省时5秒内20次调用失败就会启动熔断机制。熔断机制的注解是**@HystrixCommand。**





### 代码实现：

1. 创建一个跟8001服务提供一样的模块
2. 导入依赖

```xml
<dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-hystrix</artifactId>
        <version>1.4.6.RELEASE</version>
    </dependency>
```

3、在Controller层写入对应方法增加@HystrixCommand(fallbackMethod = "hystrixGet")，参数是对应的备用方法

![image-20220607092130196](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220607092130196.png)

4、在主程序中启动熔断器

![image-20220607092159426](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220607092159426.png)



## 8.2、服务降级

服务熔断：服务端~某个服务器超时或者异常，引起熔断~ 保险丝

服务降级：客户端~从整体网站请求负载考虑，但某个服务熔断或者关闭之后，服务将不再被调用。

​					此时在客户端，我们可以准备一个fallbackFactoru，返回一个默认值（缺省值），整体的服务水平下降了，但是好歹能用~

## 8.3、Dashboard流监控



1、创建项目springcloud-consumer-hystrix-dashboard，Dashboard在客户端，因此这个项目跟springcloud-consumer-dept-80一样

2、导入依赖

3、只需要主启动类

4、开启服务@EnableHystrixDashboard

5、http://localhost:9001/hystrix启动服务，就是一个监控页面	

6、在提供服务端，去配置监控请求

![image-20220607152317493](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220607152317493.png)





# 9、Zuul 路由网关

## 概述

**什么是Zuul？**

​	Zuul包含了对请求的路由和过滤两个最主要的功能：

​	其中路由功能负责将外部请求转发到具体微服务实例上，是实现外部访问统一的入口，而过滤器功能则负责将请求的处理过程进行干预，是实现请求的校验，服务聚合等功能的基础。Zuul和Eureka进行整合，将Zuul自身注册为Eureka服务治理下的应用，同时从Eureka中获得其他微服务的消息，也即以后的访问微服务都是通过Zuul跳转后获得。



​	注意：Zuul服务最终还是会注册Eureka

​	提供：代理+路由+过滤三大功能！



Zuul能干嘛？

- 过滤
- 路由



# 10、SpringCloud config分布式配置

![image-20220607161623266](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220607161623266.png)

![image-20220607165134404](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220607165134404.png)

