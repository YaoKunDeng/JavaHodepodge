# 微服务阶段

JavaSE : OOP

mysql : 持久化

html+css+jqury+框架：视图，框架不熟练，css不好

javaweb:独立开发MVC三层架构网站

ssm框架：简化了我们的开发流程，配置也开始较为复杂

war包:tomcat运行

spring再简化：SpringBoot-jar:内嵌tomcat； 微服务架构！

服务越来越多：springcloud；



![image-20220509092223080](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220509092223080.png)



# SpringBoot：快速入门

## 什么是Spring

Spring是一个开源框架，2003年兴起的一个轻量级的java开发框架，作者：Rob Johnson。

**Spring是为了解决企业级用用开发的复杂性而创建的，简化开发**



## Spring是如何简化Java开发的

​	为了降低Java开发的复杂性，Spring采用了以下4中关键策略：

1. 基于POJO的轻量级和最小侵入性编程
2. 基于IOC，依赖注入（DI）和面向接口实现松耦合
3. 基于切面（AOP）和惯例进行声明式编程
4. 通过切面和模板减少样式代码；



## 什么是SpringBoot

​	学过javaweb的同学就知道，开发一个web应用，从最初开始接触Servlet结合Tomcat，跑出一个Hello World程序，是要经过特别多的步骤；后来就用了框架Struts，再后来是SpringMVC，到了现在的SpringBoot，过一两年又会有其他web框架出现；不知道你们有没有经历过框架不断地演进，然后自己开发项目所有的技术也不断的变化、改造。

​	什么是SpringBoot呢？

​		就是一个javaweb的开发框架，和SpringMVC类似，对比其他javaweb框架的好处，官方说是简化开发，约定大于配置，“you can just run”， 能够迅速的开发web应用，几行代码开发一个http接口。



​	所有的技术框架发展似乎都遵循一条主线规律：从一个复杂应用场景，衍生一种规范框架，人们只需要进行各种配置而不需要自己去实现它，这时候强大的配置功能成了有点；发展到一定程度之后，人们根据实际生产应用情况，选取其中实用功能和设计精华，重构出一些轻量级的框架；之后为了提高开发效率，嫌弃原先的各类配置过于麻烦，于是开始提倡“约定大于配置”,紧儿衍生出一些一站式的解决方案。

​	

​	这就是java企业级应用-->J2EE-->Spring-->Spring Boot的过程。



​	随着Spring不断发展，涉及的领域也越来越多，项目整合开发需要配合各种各样的文件，慢慢变得不那么简单，违背了最初的理念，甚至人称配置地狱。Spring Boot正式这样的一个背景下被抽象出来的开发框架，目的是为了让大家更加容易的使用Spring，更容易的集成各种常用的中间件、开源软件；



​	Spring Boot基于Spring开发，Spring Boot 本身并不提供Spring框架的核心特性以及扩展功能，只是用于快速、敏捷地开发新以带基于Spring框架的应用程序。也就是说，它并不是用来替代Spring的解决方案，而是和Spring框架紧密结合用于提升Spring开发者体验的工具。Spring Boot 以**约定大于配置的核心思想**，默认帮我们进行了很多设置，多数Spring Boot应用只需要很少的Spring配置，同时它集成了大量常用的第三方库配置（例如 Redis、MongoDB、Jpa、RabbitMQ、Quartz等等）， Spring Boot应用中这些第三方库几乎可以零配置的开箱即用，

​	简单来说就是Spring Boot 其实不是什么新的框架， 它默认配置了很多框架的使用方式，就像maven整合了所有的jar包， spring boot整合了所有的框架。

​	Spring Boot出生名门，从一开始就站在一个比较高的七点，又经过这几年的发展，生态足够完善，Spring Boot已经当之无愧成为Java领域最热门的技术。



​	Spring Boot的主要优点：

- 为所有Spring开发者更快的入门
- 开箱即用，提供各种默认配置来简化项目配置
- 内嵌式容器简化web项目
- 没有冗余代码生成和XML配置的要求



# 微服务

## 什么是微服务？

微服务是一种架构风格，它要求我们在开发一个应用的时候，这个应用必须构建成一系列小服务的组合；可以通过http的方式进行互通。要说微服务架构，先得说说过去我们得单体应用架构。



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





# 第一个Spring Boot程序

- JDK 1.8

- maven3.6.1

- spring boot：最新版

- IDEA

  

官方：提供了一个快速生成的网站！IDEA集成了这个网站

- 可以在官网直接下载后，导入idea
- 直接使用idea创建一个springboot项目由（一般开发直接在IDEA中创建）
  

### 项目结构分析

创建基础项目之后，就自动生成以下文件。



- 程序的主程序类
- 一个application.properties配置文件
- 一个测试类

生成的DemoApplication和测试包下的DemoApplicationTests类都可以直接于宁来启动当前创建的项目，由于目前该项目未配合任何数据访问或Web模块，程序会在加载完Spring之后结束运行。

![image-20220509153837446](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220509153837446.png)



### pom.xml分析

​	打开pom.xml，看看Spring Boot项目的依赖：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.6.7</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.kun</groupId>
    <artifactId>helloword</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>helloword</name>
    <description>Demo project for Spring Boot</description>
    <properties>
        <java.version>11</java.version>
    </properties>
    <dependencies>
<!--        web依赖：tomcat，dispatcherServlet.xml-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

<!--        单元测试-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

<!--    打包jsr包插件-->
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

</project>

```

如上所示：主要有四部分：

- 项目元数据信息：创建时候输入的Project Metadata部分，也就是Maven项目的基本元素，包括：groupId、artifactId、version、name、description等
- parent：继承spring-boot-starter-parent的依赖管理，控制版本与打包等内容
- dependencies：项目具有依赖，这里包含了spring-boot-starter-web用于实现HTTP接口（该依赖中包含了Spring MVC），官网对它的描述是：使用Spring MVC构建Web（包括RESTful） 应用程序的入门者，使用Tomcat作为默认嵌入式容器；spring-boot-starter-test用于编写单元测试的依赖包。更多功能模块的使用我们将在后面逐步展开。
- build：构建配置部分。默认使用了spring-boot-maven-plugin，配合spring-boot-starter-parent就可以把Spring Boot应用打包成JAR来直接运行。

### 

### 编写HTTP接口

1. 在主程序的统计目录下，新建一个controller包【一定要在同级目录下，否则识别不到】

![image-20220509154802035](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220509154802035.png)



# 原理初探

自动配置：



**pom.xml**

- spring-boot-dependencies:核心依赖在父工程中

- 我们在写或者引入一些Spring Boot依赖的时候，不需要指定版本，就因为有这些版本仓库



**启动器：**

- ```xml
  <!--        启动器-->
          <dependency>
              <groupId>org.springframework.boot</groupId>
              <artifactId>spring-boot-starter</artifactId>
          </dependency>
  ```

- 启动器：说白了就是Springboot的启动场景

- 比如：spring-boot-web，他就会帮我们自动导入web环境所有的依赖

- springboot会将所有的场景，都变成一个个的启动器

- 我们要使用什么功能，就只需要找到对应的启动器就可以了 ‘starter’





**主程序**

```java
//@SpringBootApplication ：标注这个类是一个springboot的应用
@SpringBootApplication
public class Springboot01HelloworldApplication {

    public static void main(String[] args) {
        //将springboot应用启动
        SpringApplication.run(Springboot01HelloworldApplication.class, args);
    }

}
```

- 注解[//一级一级往下点]

  - ```xml
    @SpringBootConfiguration:	springboot的配置  
    	@Configuration :	spring配置类
    		@Component :	说明这也是一个spring组件
    ```

  - ```xml
    @EnableAutoConfiguration ：	自动配置
    	@AutoConfigurationPackage : 自动配置包
    		@Import({Registrar.class}) :	自动配置'包注册'
    	@Import({AutoConfigurationImportSelector.class}) :	自动配置导入选择
    
    ```



结论：springboot所有自动配置都是在启动的时候扫描并加载：spring.factries所有的自动配置类都在这里面，但不一定生效，要判断条件是否成立，只要导入了对应的start，就有对应的启动器了，有了启动器，我们自动装配就会生效，然后就配置成功！



1. springboot在启动的时候，从类路径下/META-INFO/spring.factories获取指定的值；
2. 将这些自动配置的类导入容器，自动配置就会生效，帮我们进行自动配置
3. 以前我们需要手动配置的东西，现在springboot帮我们做了
4. 整合JavaEE，解决方案和自动配置的东西都在spring-boot-autoconfigure-2.6.7.jar这个包下
5. 它会把所有需要导入的组件，以类名的方式放回，这些组件就会被添加导容器；
6. 容器中也会存在非常多的xxxAutoConfiguration的文件（@Bean），这就是这些类给容器中导入了这个场景需要的所有组件；并自动配置， @Configuration， JavaConfig！
7. 有了自动配置类，免去了我们手动编写配置文件的工作！



### **run**

​	run方法不是运行了一个main方法，而是开启一个服务！

```java
//@SpringBootApplication ：标注这个类是一个springboot的应用：启动类下所有资源被导入
@SpringBootApplication
public class Springboot01HelloworldApplication {

    public static void main(String[] args) {
        //将springboot应用启动
        //SpringApplication类
        //run方法
        SpringApplication.run(Springboot01HelloworldApplication.class, args);
    }

}

```

**SpringApplication.run分析**

​	分析该方法主要分为两部分，一部分SpringApplication的实例，二是run方法的执行；



### **SpringApplication**

​	这个类主要做了以下四件事

1. 推断应用的类型式普通的项目还是Web项目
2. 查找并加载所有可用初始化器，设置导initialzers属性中
3. 找出所有的应用程序监听器，设置导listeners属性中
4. 推断并设置main方法定义类，找到运行的主类



关于SpringBoot，谈谈你的理解：

- 自动装配
- run()



# SpringBoot: 配置文件及自当配置原理

1. **配置文件**

   SpringBoot使用一个全局的配置文件， 配置文件名称是固定的

   - application.properties
     - 语法：key=value
   - application.yaml
     - 语法结构：key:空格 value

   **配置文件的作用**：修改SpringBoot自动配置的默认值，因为SpringBoot在底层都给我们自动配置好了



2. **YAML**

   YAML是“YAML Ain't a Markup Language” （YAML不是一种置标语言）的递归缩写。

   在开发的这种语言是，YAML的意思其实是：“Yet Another Markuo Language”（仍是一种置标语言）

   

   3.**标记语言**

   ​	以前的配置文件，大多数都是使用xml来配置，比如一个简单的端口配置，我们来对比以下yaml和xml

   yaml配置：

   ```yaml
   server:
   	port: 8080
   ```

   xml配置：

   ```xml
   <server>
   	<port>8080</port>
   </server>
   ```

   

   3.**yaml的语法**

```yaml
# 普通的key-value
# 对空格的要求十分高

# 可以注入我们的配置类中
name: akun

#对象 对空格的要求机器严格
student:
    name: akun
    age: 3

#行内写法
student2: {name: akun,age: 3}

#数组
pets:
  - cat
  - dog
  - pig

pets2: [cat,dog,pig]
```

也可以在yaml中使用一些EL表达式【yaml已经不支持】

比如：

```yaml
#对象 对空格的要求机器严格
student:
    name: {peoson.hello:hello}  #前面的值不存在，就填写后面的值
    # name: {peoson.hello:hello}_旺财  //相当于字符串拼接
    age: ${random.int}
```



### Yaml直接给实体类赋值

实体类

**Person**

```java
@Component
@ConfigurationProperties
public class Person {
    private String name;
    private Integer age;
    private Boolean happy;
    private Date birth;
    private Map<String, Object> maps;
    private List<Object> lists;
    private Dog dog;
}
```

**Yaml**

```yaml
person:
  name: akun
  age: 3
  happy: false
  birth: 2022/5/9
  maps: {k1: v1,k2: v2}
  lists:
    - code
    - music
    - girl
# 属性名要跟类的对应
```

通过@ConfigurationProperties配置

![image-20220509203810070](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220509203810070.png)

上面打开文档，是官方要你加入一个依赖，加不加无所谓，作用是写依赖的时候有一些提示，没什么用

```xml
<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-configuration-processor</artifactId>
        </dependency>
```



配置直接绑定就可以了

![image-20220509203927976](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220509203927976.png)



**@ConfigurationProperties的作用**

- 将配置文件中配置的每一个属性的值，映射到这个组件中；

- 告诉springboot将本类中所有的属性和配置文件中相关的配置进行绑定
- 参数 prefix = "person"：将配置文件中的persion下面的属性一一对应



只有这个组件时容器的组件，才能使用容器提供的@ConfigurationProperties功能



![image-20220509213629042](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220509213629042.png)



## JSR303定义的校验类型

| 空检查    |                                                              |
| --------- | ------------------------------------------------------------ |
| @Null     | 验证对象是否为null                                           |
| @NotNull  | 验证对象是否不为null, 无法查检长度为0的字符串                |
| @NotBlank | 检查约束字符串是不是Null还有被Trim的长度是否大于0,只对字符串,且会去掉前后空格. |
| @NotEmpty | 检查约束元素是否为NULL或者是EMPTY.                           |

| Booelan检查  |                               |
| ------------ | ----------------------------- |
| @AssertTrue  | 验证 Boolean 对象是否为 true  |
| @AssertFalse | 验证 Boolean 对象是否为 false |

| 长度检查            |                                                              |
| ------------------- | ------------------------------------------------------------ |
| @Size(min=, max=)   | 验证对象（Array,Collection,Map,String）长度是否在给定的范围之内 |
| @Length(min=, max=) | Validates that the annotated string is between min and max included. |

| 日期检查 |                                              |
| -------- | -------------------------------------------- |
| @Past    | 验证 Date 和 Calendar 对象是否在当前时间之前 |
| @Future  | 验证 Date 和 Calendar 对象是否在当前时间之后 |
| @Pattern | 验证 String 对象是否符合正则表达式的规则     |

| 数值检查                    | 建议使用在Stirng,Integer类型，不建议使用在int类型上，因为表单值为“”时无法转换为int，但可以转换为Stirng为"",Integer为null |
| --------------------------- | ------------------------------------------------------------ |
| @Min                        | 验证 Number 和 String 对象是否大等于指定的值                 |
| @Max                        | 验证 Number 和 String 对象是否小等于指定的值                 |
| @DecimalMax                 | 被标注的值必须不大于约束中指定的最大值. 这个约束的参数是一个通过BigDecimal定义的最大值的字符串表示.小数存在精度 |
| @DecimalMin                 | 被标注的值必须不小于约束中指定的最小值. 这个约束的参数是一个通过BigDecimal定义的最小值的字符串表示.小数存在精度 |
| @Digits                     | 验证 Number 和 String 的构成是否合法                         |
| @Digits(integer=,fraction=) | 验证字符串是否是符合指定格式的数字，interger指定整数精度，fraction指定小数精度。 |

| @Range(min=, max=)                                    | 检查数字是否介于min和max之间. |
| ----------------------------------------------------- | ----------------------------- |
| @Range(min=10000,max=50000,message=“range.bean.wage”) | private BigDecimal wage;      |

| @Valid                                       | 递归的对关联对象进行校验, 如果关联对象是个集合或者数组,那么对其中的元素进行递归校验,如果是一个map,则对其中的值部分进行校验.(是否进行递归验证) |
| -------------------------------------------- | ------------------------------------------------------------ |
| @CreditCardNumber                            | 信用卡验证                                                   |
| @Email                                       | 验证是否是邮件地址，如果为null,不进行验证，算通过验证。      |
| @ScriptAssert(lang= ,script=, alias=)        |                                                              |
| @URL(protocol=,host=, port=,regexp=, flags=) |                                                              |



# 多环境配置：可以选择激活指定文件

properties版本：

![image-20220509221112595](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220509221112595.png)

**指明后缀就可以了**



yaml版本：

![image-20220509221349777](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220509221349777.png)

用横杠指明有多个版本



![image-20220509221751410](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220509221751410.png)



# 自动装配原理再理解

还没学习





# SpringBoot Web开发

重点：【自动装配】

思考：springboot到底帮我们配置了什么？我们能不能进行修改？能修改哪些东西？能不能扩展？

- xxxxAutoConfiguration：向容器中自动配置组件
- xxxxProperties: 自动配置类，装配配置文件中自定义的一些内容！



要解决的问题：

- 导入静态资源，.....
- 首页
- jsp：学模板引擎Thymeleaf
- 装配扩展SpringMVC
- 增删改查
- 拦截器
- 国际化！



# 深入理解Springboot原理

### **自动配置**

核心在pom.xml中

1. 初创建的SpringBoot功能，是一个子工程，它还有一个父工程

![image-20220510195015133](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220510195015133.png)

进入父工程，这个父工程主要进行了资源的过滤，以及插件的配置

2. 父工程中还有一个父工程

![image-20220510195306026](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220510195306026.png)

进入父工程，可发现，这个工程不再包含父工程，而且里面管理了大量的jar包版本

总结：

​	springboot的核心依赖在父工程中，我们在写或者引入SpringBoot依赖的时候，不需要指定版本，因为有这些版本仓库



### **启动器**

回到自己创建的SpringBoot工程中，查看pom.xml文件

![image-20220510200049316](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220510200049316.png)



默认有一个starter启动器【第一个】，如果去掉，springboot项目就崩了

第二个starter-web就是一个web服务器，springboot已将将他们封装成一个一个的模块

- 启动器：说白了就是SpringBoot的启动场景
- 比如spring-boot-starter-web,它就会帮我们自动导入web环境所有的依赖
- SpringBoot会将所有的功能场景都变成一个个启动器，用的时候显示的引用以下，springboot就会帮我们自动装配
- 我们要使用什么功能，就只需要找到对应的启动器就可以了。【可以从官网上查看对应的启动器】





**主程序【重点】**【程序入口】

```java
//@SpringBootApplication： 标注这个类是一个springBoot的应用，必须要配置
@SpringBootApplication
public class Springboot03WebApplication {

    public static void main(String[] args) {
        //将Springboot应用启动
        SpringApplication.run(Springboot03WebApplication.class, args);
    }

}
```

- **注解**

- 点进@SpringBootApplication注解，里面还有很多其他注解

  ![image-20220510202154528](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220510202154528.png)

  - @SpringBootConfiguration ： springboot的配置----->点进去
    - ![image-20220510202627183](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220510202627183.png)
    - 在点进去
      - ![image-20220510202817140](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220510202817140.png)
      - **以上@SpringBootApplication注解完结**

  - @EnableAutoCOnfiguration:自动配置【重要】----->点进去

    ![image-20220510204744873](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220510204744873.png)

    - 点入@AutoConfigurationPackage----->

      ![image-20220510204513945](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220510204513945.png)

      ​	注册包跟扫描包是连起来的，扫描的包这理注册。

    - 点进AutoConfigurationImportSelector.class ---->

      ![image-20220510213353028](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220510213353028.png)

      debug要在配置文件中配置

      ![image-20220510214422228](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220510214422228.png)

      **查看getCandidateConfigurations()方法**

      ![image-20220510215225579](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220510215225579.png)

      【改】：图中的文字有错误！EnableAutoConfiguration这个类点开其实是一个接口注解，并不是加载这个类下的包，只是兜兜转转又回到了这个类【从刚开始的注解点开也是这个类】真正加载时通过loadFactoryNames这个方法

      

      查看loadFactoryNames方法，从中查看配置文件

      ![image-20220510223610570](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220510223610570.png)

      
      
      ```java
      //点进去查看getCandidateConfigurations()方法
      
      protected List<String> getCandidateConfigurations(AnnotationMetadata metadata, AnnotationAttributes attributes) {
              List<String> configurations = SpringFactoriesLoader.loadFactoryNames(this.getSpringFactoriesLoaderFactoryClass(), this.getBeanClassLoader());
              Assert.notEmpty(configurations, "No auto configuration classes found in META-INF/spring.factories. If you are using a custom packaging, make sure that file is correct.");
          //这句代码的意思是：当configurations为空的时候，就会输出后面这句话   Assert断言
          //换句话说，就是：保证这个configurations不为空，否则就会输出：没有在META-INF/spring.factories这个目下找到
          //自动配置的类。如果你正在使用习惯的包，请确保文件是正确的。
              return configurations;
          }
      ```

      

      **META-INF/spring.factories：**自动配置的核心文件【在autoconfigure这个包下】

      ![image-20220510220747483](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220510220747483.png)

      点入spring.factories，所有的自动配置都在里面，如果这个东西没有，就要手动配置
      
      ![image-20220510221125019](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220510221125019.png)
  
  



核心注解：@ConditionalOnXXX：如果这里面的条件都满足，才会生效

![image-20220511101615001](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220511101615001.png)





**配置文件到底可以写什么东西？和spring.factories有什么**

一个Properties类

![image-20220511103520907](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220511103520907.png)

​	在配置文件中能配置的东西，都存在一个固有的规律   在xxxAutoConfiguration的类中【有默认值】，有一个@EnableAutoConfiguration去加载一个XXXProperties类，XXXProperties和配置文件绑定，我就可以使用自定义配置了。

【配置文件总结】：

​		springboot帮我们装配，会从xxxProperties中去取默认值，如果像修改默认值，只需要从配置文件中按它的格式去修改默认值。





【总结】：

​	兜兜转转，一条主线：

​		主程序中的注解@SpringBootApplication继承@EnableAutoCOnfiguration，而@EnableAutoCOnfiguration中，引入了一个类AutoConfigurationImportSelector.class，这个类的而作用就是选择在pom.xml中配置的启动器加载相应的包，用一个方法getCandidateConfigurations()去加载这些包，而这个方法最后通过一个laodFactoryNames方法，读取META-INF/spring.factories目录下的文件。

​	所以：	主程序中的注解@SpringBootApplication的作用不仅是标注这是一个springboot应用，它还启动类下所有的资源导入。



结论：springboot所有的配置都是在启动的时候扫描并加载：spring.factories所有的自动配置类都在这里面，但是不一定生效，要判断条件是否成立，只要导入了对应的start，就有对应的启动器了，有了启动器，我们自动装配就会生效，然后就配置成功！

​	步骤：

1. springboot在启动的时候，从类路径下META-INF/spring.factories获取指定的值【会进行判断】
2. 将这些自动配置的类导入容器，自动配置配置类就会生效，帮我们进行自动配置
3. 以前我们需要配置的东西，现在Spring帮我们做了
4. 整个javaEE，解决方案和自动配置的东西，都在spring-boot-autoconfigure-2.6.7.jar这个包下
5. 它会把所有需要导入的组件，以类名的方式反回，这些组件就会被添加到服务器；
6. 容器中也会存在非常多的XXXXAutoConfiguration的文件（里面存在很多@Bean），就是这些类给容器中导入了这个场景需要的所有组件



整体总结：【狂神】

1. ​	springboot启动会加载大量的配置类
2. 我们看我们需要的功能有没有在springBoot默认写好的自动类配置当中；
3. 我们再来看这个自动配置类中到底配置了那些组件；（主要我们要用的组件存在其中，我们就不需要在手动配置了）
4. 给容器中自动配置类添加组件的时候，会从properties类中获取某些属性，我们只需要在配置文件中指定这些属性即可；

xxxAutoConfiguration：自动配置类：给容器中添加组件

xxxxProperties：封装配置文件中相关属性



### **了解组程序类怎么运行**

​	这个main方法，开启了一个服务

```java
public class Springboot03WebApplication {

    public static void main(String[] args) {
        SpringApplication.run(Springboot03WebApplication.class, args);
    }

}
```

**SpringApplication.run分析**

​	分析该方法分为主要两部分，一部分是SpringApplication的实例化，而是run方法的执行

​	SpringApplication这个类主要做了以下四件事：

1. 推断这个应用的类型是普通项目还是web项目
2. 查找并加载所有可用初始化器，设置initializers属性中
3. 找出所有的应用程序监听器，设置到listeners属性中
4. 推断并设置main方法的定义类，找到运行的主类



**查看构造器**

```java
public SpringApplication(ResourceLoader resourceLoader, Class<?>... primarySources) {
		this.resourceLoader = resourceLoader;
		Assert.notNull(primarySources, "PrimarySources must not be null");
		this.primarySources = new LinkedHashSet<>(Arrays.asList(primarySources));
		this.webApplicationType = WebApplicationType.deduceFromClasspath();
		this.bootstrapRegistryInitializers = new ArrayList<>(
				getSpringFactoriesInstances(BootstrapRegistryInitializer.class));
		setInitializers((Collection) getSpringFactoriesInstances(ApplicationContextInitializer.class));
		setListeners((Collection) getSpringFactoriesInstances(ApplicationListener.class));
		this.mainApplicationClass = deduceMainApplicationClass();
	}
```



### Spring-MVC配置原理：springboot为MVC自动配置了一些东西

![image-20220511110405932](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220511110405932.png)

如果，你想div一些定制化的功能，只需要写这个组件，然后将它交给springboot【@bean】，spring就会帮我们自动装配

![image-20220511112850172](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220511112850172.png)



在SpringBoot中，自动帮我们配置了很多的类，有时候并不需要导入到，只需要使用相应的类，SpringBoot对象会判断是否满足相应的条件，激活相应的包，比如使用ObjectMapper类。

在SpringBoot的自动配置jacksonAutoConfiguration中有这样的初始化代码

```java
@Configuration(proxyBeanMethods = false)
@ConditionalOnClass(ObjectMapper.class)  //如果系统中，有指定的类，就激活该配置
public class JacksonAutoConfiguration {

	private static final Map<?, Boolean> FEATURE_DEFAULTS;

	static {
		Map<Object, Boolean> featureDefaults = new HashMap<>();
		featureDefaults.put(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS, false);
		featureDefaults.put(SerializationFeature.WRITE_DURATIONS_AS_TIMESTAMPS, false);
		FEATURE_DEFAULTS = Collections.unmodifiableMap(featureDefaults);
	}

```

















# 静态资源

```java
 public void addResourceHandlers(ResourceHandlerRegistry registry) {
            if (!this.resourceProperties.isAddMappings()) {
                logger.debug("Default resource handling disabled");
            } else {
                this.addResourceHandler(registry, "/webjars/**", "classpath:/META-INF/resources/webjars/");
                this.addResourceHandler(registry, this.mvcProperties.getStaticPathPattern(), (registration) -> {
                    registration.addResourceLocations(this.resourceProperties.getStaticLocations());
                    if (this.servletContext != null) {
                        ServletContextResource resource = new ServletContextResource(this.servletContext, "/");
                        registration.addResourceLocations(new Resource[]{resource});
                    }

                });
            }
        }
```



总结：

1. 在springboot，我们可以使用以下方式处理静态资源
   1. webjars
   2. public、static、/**、resources  
2. 优先级：resources>static(默认)>public



# 模板引擎

​		前端交给我们的页面，是html页面。如果我们以前开发，我们需要把他们转成jsp页面，jsp好处就是当我们查出一些数据转发到JSP页面以后，我们可以用jsp轻松实现数据的显示，及交互等。jsp支持非常强大的功能，包括能写java代码。但是呢，我们现在的这种情况，SpringBoot这个项目首先是以jar的方式，不是war，第二，我们用的还是嵌入式的Tomcat，所以呢，现在默认不支持jsp的。



​	那不支持jsp，如果我们直接用纯静态页面的方式，那给我们的开发会带来非常大的麻烦，那怎么办呢？SpringBoot推荐你可以来使用模板引擎。



​	那么这模板引擎，其实大家都听到很多，jsp就是一个模板引擎，还有以前用的比较多的freemarker，包括SpringBoot给我们推荐的Thyme leaf，模板引擎有非常多，但再多的模板引擎，他们的思想都是一样的。

![image-20220510102535677](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220510102535677.png)

pom依赖【在springboot中】

```xml
<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
 </dependency>
```





结论：需要使用thymeleaf，只需要导入对应的依赖就可以了！我们将HTML页面放在我们的templates目录下即可！

```java
public static final String DEFAULT_PREFIX = "classpath:/templates/";
public static final String DEFAULT_SUFFIX = ".html";
```





# 整合JDBC



1. 导入依赖

```xml
//springboot对jdbc代码的封装
<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>

//mysql连接驱动，必须导入
<dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
</dependency>
```



2. 配置数据源application.yaml

```yaml
spring:
  datasource:
    username: root
    password: dyk160513140.
    url: jdbc:mysql://localhost:3306/mybatis?serverTimezone=UTC&useUnicode=True&characterEncoding=utf-8&useSSL=True
    driver-class-name: com.mysql.cj.jdbc.Driver
```



3.SpringBoot对JDBC进行了一个很全面的封装，有一个封装类：JdbcTemplate，它包含了对数据可的所有操作

```java
@RestController
public class JDBCController {

    @Autowired
    JdbcTemplate jdbcTemplate;

    //查询数据库所有信息
    //没有实体类，数据库中的东西怎么获取？  Map
    @RequestMapping("/userList")
    private List<Map<String,Object>> userList(){
        String sql = "select * from user";
        List<Map<String, Object>> maps = jdbcTemplate.queryForList(sql);
        return maps;
    }
}
```

**在springBoot里面，有很多xxxxTemplate的东西，是SpringBoot已经配置好的bean，我们只要将他装配之后，拿来即用**

![image-20220602194522115](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220602194522115.png)

如果要使用其他数据源，则导入相应的依赖依赖，然后再配置文件中指定数据源，比如使用Druid加一个type：com.alibaba.druid.pool.DruidDataSource





# 整合Mybatis

1. 导入依赖

```xml
<!--自研的-->
        <!-- https://mvnrepository.com/artifact/org.mybatis.spring.boot/mybatis-spring-boot-starter -->
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.2.2</version>
        </dependency>
```

2. 创建实体类

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {

    private int id;
    private String name;
    private String pwd;
}

```

3. 接口实现

   ```java
   //这个注解表示了这是一个mybatis接口
   @Mapper
   @Repository
   public interface UserMapper {
   
   
       List<User> queryUserList();
   
       User queryUserById();
   
       int addUser(User user);
   
       int update(User user);
   
       int deleteUser(int id);
   }
   ```

   4. mepper.xml

      ```xml
      <?xml version="1.0" encoding="UTF-8" ?>
      <!DOCTYPE mapper
              PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
              "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
      <mapper namespace="com.kun.springboot05mybatis.mapper.UserMapper">
          <select id="queryUserList" resultType="User">
          select * from mybatis.user;
          </select>
      </mapper>
      ```

   5.配置文件

   ```properties
   spring.datasource.username=root
   spring.datasource.password=dyk160513140.
   spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
   spring.datasource.url=jdbc:mysql://localhost:3306/mybatis?serverTimezone=UTC&characterEncoding=utf-8&useUnicode=True
   
   
   mybatis.type-aliases-package=com.kun.springboot05mybatis.pojo
   mybatis.mapper-locations=classpath:mybatis/mapper/*.xml
   ```





# SpringSecurity（安全）

在web开发中，安全第一位！过滤器，拦截器~

安全是功能续需求：否

做网站：安全应该在设计之初考虑！



shiro、SpringSecurity：很像~除了类不一样，名字不一样

认证，授权

```java
Spring Security is a powerful and highly customizable authentication and access-control framework. 
SpringSecurity 是一个有能量并且高度可自定义的身份认证和权限控制的框架
```



权限分为哪些权限？

- 功能权限
- 访问权限
- 菜单权限

实现以上这些，以前用拦截器、过滤器，大量的原生代码~冗余



**简介**

SpringSecurity是针对Spring项目的安全框架，也是SpringBoot底层安全模块默认的技术选型，它可以实现强大的web安全控制，对于安全机制，我们仅需要引入spring-boot-security模块，进行少量的配置，即可实现强大的安全管理！

记住几个类：

- WebSecurityConfigureAdapter：自定义security策略
- AuthenticationManagerBuilder：自定义认证策略
- @EnableWebSecurity：  开启WebSecurity模式      @Enable xxxx 开启某个功能

spring Security的两个主要目标是“认证”和“授权”（访问控制）

“认证” （Authentication）

“授权” （Authorication）

这个概念是通用的，而不是只在Spring Security中存在
