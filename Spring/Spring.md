

## 1 Spring

### 1.1 简介

2002首次推出spring框架的雏形，interface21框架

Rod Johnson Spring Framework创始人，是悉尼大学的博士，**专业是音乐学**。

SSH: Struct2 + Spring + Hibernate!

SSM: SpringMvc + Spring + Mybatis!

官网：[https://spring.io/projects/spring-framework](https://link.zhihu.com/?target=https%3A//spring.io/projects/spring-framework)

官方下载地址：[https://repo.spring.io/release/org/springframework/spring/](https://link.zhihu.com/?target=https%3A//repo.spring.io/release/org/springframework/spring/)

GitHub:[https://github.com/spring-projects/spring-framework](https://link.zhihu.com/?target=https%3A//github.com/spring-projects/spring-framework)



 org.springframework

 spring-webmvc

 5.2.0.RELEASE



 org.springframework

 spring-jdbc

 5.2.0.RELEASE



### 1.2 优点

开源免费

轻量级

**控制反转（IOC），面向切面（AOP）**

支持事务的处理 ，对构架整合的支持

### 1.3 组成

![img](https://pic3.zhimg.com/80/v2-faef2323190fc03f02b89efee6b98742_720w.jpg)







### 1.4 拓展

在Spring的官网有这个介绍：现代化的java开发！说白就是基于Spring的开发

![img](https://pic3.zhimg.com/80/v2-dbb62fcfa79cf618c054847557503d76_720w.jpg)

- Spring Boot
  - 一个快速开发的脚手架
  - ·可以快速开发单个微服务
  - 约定大于配置

- Spring Cloud
  - 基于Spring Boot实现的
  - 学习Spring Boot 需要完全掌握Spring和SpringMVC
  - 弊端：**配置地狱**



## 2 IOC理论推导

传统的java代码编写过程如下：

- UserDao 接口
- UserDaoImpl 实现类
- UserService 业务接口
- UserServiceImp 业务实现类

在我们之前的业务中，用户的需求可能会影响我们原来的代码，我们需要根据用户的需求去修改源代码！如果程序代码量十分巨大，修改一次代码的成本十分安规



在UserServiceImp 业务实现类中需要使用set方法，不然用户修改需求之后要修改代码

```java
private userDao userDao;

//利用set进行动态注入

public void setUserDao(UserDao userDao) {

this.userDao = UserDao;

}
```



- 之前，程序是主动创建对象！控制权的程序员手上
- 使用set注入之后，程序不再具有注定行，而是变成了被动的接收对象！



这种思想从本质上解决了问题，我们程序员不再需要去管理对象的创建了，耦合性大大地降低，可以更加专注的在业务实现上，这就是IOC的原型





**IOC本质**

Inversion 倒置，颠倒

控制反转IoC(Inversion of Control), 是一种设计思想，DI(依赖注入)是实现IoC的一种方式。

控制反转是一种通过描述（XML或注解）并**通过第三方**去生产或获取特定对象的方式。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200607202256223.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyNzIyMjc4,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200607202301824.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyNzIyMjc4,size_16,color_FFFFFF,t_70)

- 采用XML方式配置Bean的时候，bean的定义信息和实现分离的，而采用注入的方式可以把两者合为一体，Bean的定义信息直接以注解的形式定义在实体类中，从而达到了零配置的目的。
- **控制反转时一种通过描述（XML或注解）并通过第三方去生产或获取特定对象的方式，在Spring中实现控制反转的是IoC容器，其实现方法是依赖注入（Dependence Injection）**



![img](https://img-blog.csdnimg.cn/20200607202308730.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyNzIyMjc4,size_16,color_FFFFFF,t_70)



## 3、HelloSpring

编写实体类：

```java
package com.kun.pojo;

public class Hello {
    private String str;

    public Hello() {
    }

    public Hello(String str) {
        this.str = str;
    }

    public String getStr() {
        return str;
    }

    public void setStr(String str) {
        this.str = str;
    }

    @Override
    public String toString() {
        return "Hello{" +
                "str='" + str + '\'' +
                '}';
    }
}

```

编写配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

<!--    使用Spring来创建对象，在Spring这些都称为Bean
        类型  变量名 = new 类型();
        Bean = 对象 ->new Hello()

        id = 变量名
        class = new 的对象
        property 相当于有给对象中的属性设置一个值！
-->
    <bean id="hello" class="com.kun.pojo.Hello">
        <property name="str" value="Spring"></property>

    </bean>

</beans>
```

测试：

```java
public class MyTest {
    public static void main(String[] args){
        //获取Spring的上下文对象
        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
        //我们的对象现在都在Spring中管理了，我们要使用，直接去里面取出来就可以了
        Hello hello = (Hello) context.getBean("hello");

        System.out.println(hello.toString());
    }
}

```

### 思考问题

> - ​	Hello对象是谁创建的？
>
>   Hello对象是由Spring创建的
>
> - Hello对象的属性怎么设置的
>
>   Hello对象的属性是Spring容器设置的
>
> 

这个过程叫控制反转：

控制：谁来控制对象的创建，传统应用程序的对象是由程序本身控制创建的，使用Spring之后，对象是由Spring来创建的

反转：程序本身不创建对象，而变成被动的接收对象

依赖注入：就是利用set方法来进行注入的

IOC是一种变成思想，由主动的变成变成被动的接收

可以通过newClassPathXmlApplicationContext去浏览一下底层源码

**OK,到了现在，我们彻底不用在程序中去改动了，要实现不同的操作，只需要在xml配置文件中去修改，所谓的loc，一句话搞定：**

**对象由Spring来创建，管理，装配**





## 4、IOC创建对象的方式



1. 使用无参构造方法创建对象，默认！

   ```xml
      <bean id="user" class="com.kun.pojo.User">
   		<property name="name" value="阿坤"></property>
      </bean>
   ```

   

2. 假设我们要使用有参构造创建对象

   1. 下标赋值

      ```java
         <bean id="user" class="com.kun.pojo.User">
              <constructor-arg index="0" value="阿坤学java"></constructor-arg>
          </bean>
      ```

   2. 类型

      ```xml
       <bean id="user" class="com.kun.pojo.User">
              <constructor-arg type="java.lang.String" value="坤坤"></constructor-arg>
          </bean>
      
      ```

      **不建议使用，当由两个参数都是String就报错了**

   3. 参数名

   ```xml
     <bean id="user" class="com.kun.pojo.User">
           <constructor-arg name="name" value="阿坤"></constructor-arg>
       </bean>
   
   ```

3. 总结：在配置文件加载的时候，容器中管理的对象就已经初始化了





### 5、Spring配置



### 5.1、别名：

```xml
<!--    别名，如果添加了别名，我们也可以用别名获取到这个对象-->
    <alias name="user" alias="userNew"></alias>
```



### 5.2、Bean的配置

```xml
<!--    
    id:bean的唯一标识符，也就相当于我们学的对象名
    class：bean对象所对应的全限定名：包名+类型
    name：也是别名，而且name可以同时去多个别名
-->
    
    <bean id="user" class="com.kun.pojo.User" name="user2 u2">
        <constructor-arg name="name" value="阿坤"></constructor-arg>
    </bean>

```



### 5.3、import

这个import，一般用于团队开发使用，它可以将多个配置文件，导入合并为一个





## 6、依赖注入



### 6.1、构造器注入

前面已经说过



### 6.2、Set方式注入【重点】

- 依赖注入：set注入！
  - 依赖：bean对象的创建依赖于容器
  - 注入：bean对象中的所有属性，由容器来注入！



【环境搭建】

1. ​	复杂类型

   ```java
   package com.kun.pojo;
   
   public class Address {
       private String address;
   
       public String getAddress() {
           return address;
       }
   
       public void setAddress(String address) {
           this.address = address;
       }
   
   }
   
   ```

   2. 真实测试对象

   

   ```java
   public class Student {
       private String name;
       private Address address;
       private String[] books;
       private List<String> hobbys;
       private Map<String,String> card;
       private Set<String> games;
       private String wife;
       private Properties info;
   }
   ```

   3.beans.xml

```xml
<bean id="address" class="com.kun.pojo.Address"></bean>

<!--    普通值注入：，value-->
    <bean id="student" class="com.kun.pojo.Student">
        <property name="name" value="阿坤"></property>

<!--        Bean注入，ref-->
        <property name="address" ref="address"></property>

<!--        数组注入：array-->
        <property name="books">
            <array>
                <value>红楼梦</value>
                <value>西游戏</value>
                <value>水浒传</value>
                <value>三国演义</value>
            </array>
        </property>

<!--        List注入-->
        <property name="hobbys">
            <list>
                <value>听歌</value>
                <value>看电视</value>
            </list>
        </property>

<!--        Map注入-->
        <property name="card">
            <map>
                <entry key="" value=""></entry>
                <entry key="" value=""></entry>
            </map>
        </property>
<!--        Set注入-->
        <property name="games">
            <set>
                <value>LOL</value>
                <value>COC</value>
            </set>
        </property>
        
<!--        null值注入-->
        <property name="wife">
            <null></null>
        </property>
<!--        property-->
        <property name="info">
            <props>
                <prop key="学号">201941412102</prop>
            </props>
        </property>
    </bean>
```

4. 测试类

```java
public class MyTest {
    public static void main(String[] args){
        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
        Student student = (Student) context.getBean("student");
        System.out.println(student.toString());
    }

}
```



### 6.3扩展方式注入

我们可以使用p命名空间和c命名空间进行注入

![image-20220421002748841](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220421002748841.png)

使用：

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"  //加入
       xmlns:c="http://www.springframework.org/schema/c"	//加入
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

<!--    p-命名空间秩注入，可以直接注入属性的值：property-->
    <bean id="user" class="com.kun.pojo.User" p:name="阿坤" p:age="18"></bean>


<!--    c命名空间注入，通过构造器注入：construct-args-->
    <bean id="user2" class="com.kun.pojo.User" c:age="18" c:name="阿坤"></bean>

</beans>
```



注意点：p命名和c命名空间不能直接使用，需要导入xml约束

```xml
 xmlns:p="http://www.springframework.org/schema/p"  
 xmlns:c="http://www.springframework.org/schema/c"	
```



### 6.4、bean的作用域

<img src="C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220421090743381.png" alt="image-20220421090743381" style="zoom:80%;" />

1. 单例模式（Spring默认机制）

```xml
<bean id="user2" class="com.kun.pojo.User" c:age="18" c:name="阿坤" scope="singleton"></bean>

```

2. 原型模式：每次从容器中get的时候，都会产生一个新对象

```xml
<bean id="user2" class="com.kun.pojo.User" c:age="18" c:name="阿坤" scope="prototype"></bean>
```



3. 其余的request、session、application、这些个只能在web开发中使用





## 7、Bean的自动装配

- 自动装配式Spring满足bean依赖一种方式！
- Spring会在上下文中自动寻找，并自动给bean装配属性



在Spring中由三种装配的方式

1. 在xml中显示的配置
2. 在java中显示的配置
3. 隐式的自动装配bean【重要】



### 7.1、测试

1. 环境搭建：一个人有两个宠物



**实体类**

```java
public class People {
    //如果定义了Autowired的required属性为false，说明这个对象可以为null，否则不允许为空


    private Cat cat;


    private Dog dog;

    private String name;
}
```



### 7.2、ByName自动装配

```xml
<!--    byName:会自动在容器上下文中查找，和自己对象set方法后面的值对应的beanId！
		byType:会自动在上其上下文中查找，和自己对象属性类型相同的bean！
-->
    <bean id="people" class="com.kuang.pojo.People" autowire="byName">
        <property name="name" value="阿坤呀"></property>    //已经在配置了Dog，Cat
        
    </bean>
```



### 7.3、ByType自动装配

```xml
    <bean class="com.kuang.pojo.Cat"></bean>
    <bean class="com.kuang.pojo.Dog"></bean>


<!--    byName:会自动在容器上下文中查找，和自己对象set方法后面的值对应的beanId->
    <bean id="people" class="com.kuang.pojo.People" autowire="byName">
        <property name="name" value="阿坤呀"></property>

    </bean>

```

小结：

- byname的时候，需要保证所有的bean的id唯一，并且这个bean需要和自动注入的属性的set方法的值一致
- byType的时候，需要保证所有bean的class唯一，并且这个bean需要和自动注入的属性的类型一致，可以不用id也能实现



## 7.4、使用注解实现自动装配

jdk1.5支持的注解，Spring2.5就支持注解了！

The introduction of annotation-based configuration raised the question of whether this approach is “better” than XML

要使用注解须知：

1. 导入约束，context约束
2. 配置注解的支持： **<context:annotation-config/>**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"  //新加
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <context:annotation-config/>

</beans>
```

**@Autowired**

直接在属性上使用即可！也可以在set方式上使用

使用Autowired我们可以不用编写set方法了，前提是你这个自动装配的属性在IOC（Spring）容器存在，且符合

科普：

```xml
@Nullable 字段标记了这个注解，说明这个字段可以为null
```

```xml
public @interface Autowired{
	boolean required() defaulf true;
}
@Autowired(required=false)
//如果定义了Autowired的required属性为false，说明这个对象可以为null，否则不允许为空
//null跟完全没有定义，是不一样的概念
```

测试代码：

```java
public class People {
    //如果定义了Autowired的required属性为false，说明这个对象可以为null，否则不允许为空
    @Autowired(required = false)
    private Cat cat;
    @Autowired
    private Dog dog;

    private String name;
}
```



如果@Autowired自动装配的环境比较复杂，自动装配无法通过一个注解【@Autowired】完成的时候，我们使用@Qualifier（value=“xxx”）去配置@Autowired的使用，指定一个唯一的bean对象注入。

```java
public class People {
    //如果定义了Autowired的required属性为false，说明这个对象可以为null，否则不允许为空
    @Autowired(required = false)
    @Qualifier(value = "cat")
    private Cat cat;
    @Autowired
    @Qualifier(value = "dog")
    private Dog dog;

    private String name;
}
```



@Resource注解

```java
public class People {
    
    @Resource(name="cat")  //JDK版本15以上
    private Cat cat;
    @Autowired
    private Dog dog;

    private String name;
}
```

小结：

@Resource和@Autowired的区别：

- 都是用来自动装配的，都可以放在属性字段上
- @Autowired 通过byType的方式实现,而且要求这个对象存在！
- @Resource默认通过byname的方式实现，如果找不到名字，则通过byType实现！如果两个都找不到的情况下，就报错！【常用】
- 执行顺序不同：@AutoType的方式实现。@Resource默认通过byname的方式实现



## 8、使用注解开发

在Spring4之后，要使用注解开发，必须要保证aop的包导入了

![image-20220421143348940](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220421143348940.png)



使用注解需要导入context约束，增加注解的支持！

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"  
        xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

<context:annotation-config/>

        </beans>
```

1. bean
2. 属性如何注入

```java
package com.kun.pojo;


import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

//等价于<bean id="user" class="com.kun.pojo.User"> </bean>
//@Component组件
@Component
public class User {
    //相当于<property name="name" value="阿坤";/>
    @Value("阿坤")
    public String name;
}

```

3. 衍生的注解

   @Component有几个衍生注解，我们在web开发中，会按照mvc三层架构分层

   - dao【@Repository】
   - service【@Service】
   - controller【@Controller】

   这四个注解功能都一样，都是代表将某个类注册到Spring中，装配Bean

4. 自动装配置

```java
public class People {
    //如果定义了Autowired的required属性为false，说明这个对象可以为null，否则不允许为空
    @Autowired(required = false)
    private Cat cat;
    @Autowired
    private Dog dog;

    private String name;
}
```



3. 作用域

```java
package com.kun.pojo;


import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Scope;
import org.springframework.stereotype.Component;

//等价于<bean id="user" class="com.kun.pojo.User"> </bean>
//@Component组件
@Component
@Scope("prototype")
public class User {
    //相当于<property name="name" value="阿坤";/>
    @Value("阿坤")
    public String name;
}

```



3. 小结

xml与注解：

- xml更加万能，适用于任何场合！维护简单方便
- 注解 不是自己的类使用不了，维护相对复杂！

xml与注解最佳实践：

- xml用来管理bean
- 注解只负责完成属性注入；
- 我们在使用的过程中，只需要注意一个问题：必须让注解生效，就需要开启注解的支持



## 9、使用java的方式配置Spring

我们现在完全不适用Spring的xml配置，全权交给java来做！

JavaConfig是Spring的一个子项目，在Spring4之后，它成为了一个核心功能

![image-20220421151718601](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220421151718601.png)



实体类：

```JAVA
//这里这个注解的意思，就是说明这个类被Spring接管了，注册到了容器之中
@Component
public class User {

    private String name;

    public String getName() {
        return name;
    }
    @Value("阿坤")
    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "User{" +
                "name=" + name +
                '}';
    }
}
```



配置类！

```JAVA
package com.kun.config;

import com.kun.pojo.User;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Import;
import org.springframework.stereotype.Component;

//这个也会被Spring容器托管，注册到容器中，因为它本身就是一个@Component
// @Configuration代表这是一个配置类，就和我们之前看的bean.xml一样
@Configuration
@ComponentScan("com.kun.pojo")
@Import(kunConfig2.class)
public class KunConfig {
    //注册一个备案，就相当于我们之前写的一个bean标签
    //这个方法的名字就相当于bean标签的id属性
    //这个方法的返回值，就相当于bean标签中的class属性
    @Bean
    public User getUser(){
        return new User(); //就是返回要注入到bean的对象
    }
}

```

```java
测试类！
public class MyTest {
    @Test
    public void test(){
        //如果完全使用了配置类方式去做，我们就只能通过AnnotationConfig上下文来获取容器，通过配置类class对象加载！
        ApplicationContext context = new AnnotationConfigApplicationContext(KunConfig.class);
        User getUser = (User) context.getBean("getUser");

        System.out.println(getUser.getName());
    }
}

```

这种纯java的配置方式，在SpringBoot中随处可见！



## 10、代理模式

为什么要学习代理模式？因为这就是SpringAOP的底层！【SpringAOP 和 SPringMVC】

代理模式分类：

- 静态代理
- 动态代理

![image-20220421164942630](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220421164942630.png)

### 10.1、静态代理

角色分析：

- 抽象角色：一般会使用接口或抽象类来解决
- 真实角色：被代理的角色
- 代理角色：代理真实角色，代理真实角色后，我们一般会做一下附属操作
- 客户：访问代理对象的人



代码步骤：

1. 接口

```java
public interface Rent {
    public void rent();
}

```

2. 真实角色

```
//房东
public class Host implements Rent{

    public void rent() {
        System.out.println("房东要出租房子");
    }
}
```

3. 代理角色

```xml
package com.kun.demo01;

public class Proxy implements Rent{
    private Host host;

    public Proxy() {
    }

    public Proxy(Host host) {
        this.host = host;
    }

    public void rent(){
        host.rent();
    }
}

```

4. 客户端访问代理角色

```java
package com.kun.demo01;

public class Client {
    public static void main(String[] args){
        Host host = new Host();
        Proxy proxy = new Proxy(host);
        proxy.rent();
    }
}

```



代理模式的好处：

- 可以使真实角色的操作更加纯粹！不用去光柱一些公共的业务
- 公共也就交给代理角色！实现了业务分工
- 公共业务发生扩展的时候，方便集中管理

缺点：一个真实角色就会产生一个代理角色，代码量会翻倍~开发效率会变低



### 10.2加深理解

代码：对应08-demo02

聊聊AOP

![image-20220421204322410](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220421204322410.png)



### 10.3、动态代理

- 动态代理和静态代理角色一样
- 动态代理的代理类使动态生成的，不是我们直接写好的
- 动态代理分为两大类：基于接口的动态代理，基于类的动态代理
  - 基于接口--JDK动态代理【我们在这里使用】
  - 基于类--cglib
  - java字节码：javasist



需要了解两个类：Proxy：代理，InvocationHandler：调用处理程序

InvocationHandler是由代理实例的调用处理程序实现的接口，每一个proxy代理实例都有一个关联的调用处理程序；在代理实例调用方法时，方法调用被编码分派到调用处理程序的invoke方法。



Proxy类作用：生成动态代理这个实例的

```java
//生成得到代理类
    public Object getProxy(){
      return   Proxy.newProxyInstance(this.getClass().getClassLoader(),target.getClass().getInterfaces(),this);
    }

//newProxyInstance，方法有三个参数：

//loader: 用哪个类加载器去加载代理对象

//interfaces:动态代理类需要实现的接口

//h:动态代理方法在执行时，会调用h里面的invoke方法去执行

```

```java
  public static Object newProxyInstance(ClassLoader loader,
                                          Class<?>[] interfaces,
                                          InvocationHandler h)
        throws IllegalArgumentException
```





```java
/处理代理实例，并返回结果
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        log(method.getName());
        //动态代理的本质，就是使用反射机制
        Object result = method.invoke(target, args);
        return result;
    }

```



代理类：

```java

//等会我们用这个类，自动生成代理类!
public class ProxyInvocationHandler implements InvocationHandler{
    //被代理的接口
    private Object target;

    public void setTarget(Object target) {
        this.target = target;
    }

    //生成得到代理类
    public Object getProxy(){
      return   Proxy.newProxyInstance(this.getClass().getClassLoader(),target.getClass().getInterfaces(),this);
    }

    //处理代理实例，并返回结果
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        log(method.getName());
        //动态代理的本质，就是使用反射机制
        Object result = method.invoke(target, args);
        return result;
    }

    public void log(String msg){
        System.out.println("执行了"+msg+"方法");
    }


}

```

测试类：

```java
public class Client {
    public static void main(String[] args) {
        //真实角色
        UserService userService = new UserServiceImpl();

        //代理角色，不存在
        ProxyInvocationHandler proxyInvocationHandler = new ProxyInvocationHandler();

        proxyInvocationHandler.setTarget(userService);

        //动态生成代理类
        UserService proxy = (UserService) proxyInvocationHandler.getProxy();

        proxy.add();
    }
}

```





动态代理的好处：

- 可以使真实角色操作更加纯粹!不用去关注一些公共的业务
- 公共也就交给了代理角色！实现业务分工
- 公共业务发生扩展的时候，方便管理
- 一个动态代理类代理的是一个接口，一般就是对应的一类业务
- 一个动态代理类可以代理多个类，只要实现了同一个接口即可





# 11、AOP

## 11.1、什么是AOP

AOP（Aspect Oriented Program) 意为：面向切面编程，通过预编译方式和运行期动态代理实现程序功能的统一维护技术。AOP是OOP的延续，是软件开发的一个热点，也是Spring框架中的一个重要内容，是函数式编程的一种衍生泛型，利用AOP可以对业务逻辑的各个部分进行隔离，从而使业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发效率.

![image-20220424111855450](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220424111855450.png)



## 11.2、AOP在Spring中的作用

提供声明式事务：允许用户自定义切面

- 横切关注点：跨越应用程序多个模块的方法或功能。即是，与我们业务逻辑无关的，但我们需要关注的部分，就是横切关注点。如日志，安全，缓存，事务等等
- 切面（ASPECT）：横切光注点，被模块化的特殊对象。即，它是一个类
- 通知（Advice）：切面必须要完成的工作。即，它是类中的一个方法【Log】
- 目标（Target）：被通知的对象
- 代理（Proxy）：向目标对象应用通知后创建的对象
- 切入点（PointCut）：切面通知执行的“地点”的定义
- 连接点（JoinPoint）：与切入点匹配的执行方法



![image-20220424112606557](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220424112606557.png)

SpringAOP中，通过Advice定义横切逻辑，Spring中支持5中类型的Advice：

![image-20220424112849957](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220424112849957.png)

即AOP在不改变原有的代码的情况下，去增加新的功能



## 11.3、使用Spring实现AOP

【重点】使用AOP植入，需要导入一个依赖包

```xml
<!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.9.4</version>
    <scope>runtime</scope>
</dependency>

```





**方式一：使用Spring的接口**【主要SpringAPI接口实现】

```xml
实现接口MethodBeforeAdvice该拦截器会在调用方法前执行

实现接口   AfterReturningAdvice该拦截器会在调用方法后执行

实现接口  MethodInterceptor该拦截器会在调用方法前后都执行，实现环绕结果。

```



```java
public class AfterLog implements AfterReturningAdvice {

    //returnValue:返回值
    public void afterReturning(Object returnValue, Method method, Object[] args, Object target) throws Throwable {
        System.out.println("执行了"+method.getName()+"方法，返回结果为:"+returnValue);
    }
}

```

```java
public class Log implements MethodBeforeAdvice {
    //method:要执行的目标对象的方法
    //args:参数
    //target：目标对象
    public void before(Method method, Object[] args, Object target) throws Throwable {
        System.out.println(target.getClass().getName()+"的"+method.getName()+"被执行了");
    }
}

```

```java
public interface UserService {
    public void add();
    public void delete();
    public void update();
    public void select();
}

```

```java
public class UserServiceImpl implements UserService {
    public void add() {
        System.out.println("增加了一个用户");
    }

    public void delete() {
        System.out.println("删除了一个用户");

    }

    public void update() {
        System.out.println("修改了一个用户");

    }

    public void select() {
        System.out.println("查询了一个用户");

    }
}

```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd">

<!--   注册bean-->
    <bean id="userService" class="com.kun.service.UserServiceImpl"></bean>

    <bean id="log" class="com.kun.log.Log"></bean>
    <bean id="afterLog" class="com.kun.log.AfterLog"></bean>

<!--    配置aop,需要导入aop约束-->
    <aop:config>
<!--        切入点:expression:表达式.excution：（要执行的位置！ ******）-->

        <aop:pointcut id="pointcut" expression="execution(* com.kun.service.UserServiceImpl.*(..))"/>

<!--        执行环绕增强-->
        <aop:advisor advice-ref="log" pointcut-ref="pointcut"></aop:advisor>
        <aop:advisor advice-ref="afterLog" pointcut-ref="pointcut"></aop:advisor>
        //advice-ref说明切别人的程序是什么,advice的英文翻译是“通知”，意思是主业务程序执行到某个方法之前之后发出的通知。
		//pointcut-ref说明被切的业务主程序是什么。
    </aop:config>
</beans>

1、execution(): 表达式主体 (必须加上execution)。

 2、第一个*号：表示返回值类型，*号表示所有的类型。

 3、包名：表示需要拦截的包名，后面的两个句点表示当前包和当前包的所有子包，cn.smd.service.impl包、子孙包下所有类的方法。

 4、第二个*号：表示类名，*号表示所有的类。

 5、*(..):最后这个星号表示方法名，*号表示所有的方法，后面括弧里面表示方法的参数，两个句点表示任何参数。

```

测试类：

```java
public class MyTest {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");

        //动态代理代理的是接口：注意点
        UserService userSerivce = (UserService) context.getBean("userService");

        userSerivce.add();
    }
}

```



**方式二：自定义来实现AOP**【主要是切面定义】

执行的方法：

```java
public class DiyPointCut {
    public void before(){
        System.out.println("###################方法执行前####################3");
    }

    public void after(){
        System.out.println("###################方法执行后####################3");
    }
}

```





配置文件更改

```xml
<!--    方式二:自定义类-->
    <bean id="diy" class="com.kun.diy.DiyPointCut"></bean>
    
    <aop:config>
<!--        自定义切面，ref要引用的类-->
        <aop:aspect ref="diy">   //相比上面增加了切面
            <!--        切入点-->
            <aop:pointcut id="point" expression="execution(* com.kun.service.UserServiceImpl,*(..))"/>
            
            <aop:before method="before" pointcut-ref="point"></aop:before>
            <aop:after method="after" pointcut-ref="point"></aop:after>
            
        </aop:aspect>

        
    </aop:config>
```



**方式三:使用注解实现**！

无法使用@Aspect注解和@before注解，是没有导入两个包：

```xml
    <!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.9.4</version>
            <scope>runtime</scope>
        </dependency>

        <!-- https://mvnrepository.com/artifact/aspectj/aspectjrt -->
        <dependency>
            <groupId>aspectj</groupId>
            <artifactId>aspectjrt</artifactId>
            <version>1.5.4</version>
        </dependency>

```



```java
package com.kun.diy;

//方式三：使用注解方式实现AOP


@Aspect
public class AnnotationPoint {
    @Before("execution(* com.kun.service.UserServiceImpl.*(..))")
    public void before(){
        System.out.println("#################方法执行前########################");
    }

    @After("execution(* com.kun.service.UserServiceImpl.*(..))")
    public void after(){
        System.out.println("###################方法执行后##############################");
    }

    @Around("execution(* com.kun.service.UserServiceImpl.*(..))")
    public void around(ProceedingJoinPoint pj) throws Throwable {
        System.out.println("环绕前");
        Object proceed = pj.proceed();
        System.out.println("环绕后");

    }
}

```



配置文件

```java
<!--    方式三-->
    <bean id="annotationPointCut" class="com.kun.diy.AnnotationPoint"></bean>
<!--    开启注解支持-->
    <aop:aspectj-autoproxy></aop:aspectj-autoproxy>

```



# 12、整合Mybatis

步骤：

1. 导入相关jar包

   - Junit
   - mybatis
   - mysql数据库
   - Spring相关
   - aop织入
   - mybatis-spring【new】

   ```xml
   <dependencies>
           <dependency>
               <groupId>junit</groupId>
               <artifactId>junit</artifactId>
               <version>4.12</version>
               <scope>test</scope>
           </dependency>
   
           <dependency>
               <groupId>mysql</groupId>
               <artifactId>mysql-connector-java</artifactId>
               <version>8.0.23</version>
           </dependency>
   
           <dependency>
               <groupId>org.mybatis</groupId>
               <artifactId>mybatis</artifactId>
               <version>3.5.2</version>
           </dependency>
   
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-webmvc</artifactId>
               <version>5.3.19</version>
           </dependency>
   
   <!--        Spring操作数据库的化，还需要一个spring-jdbc-->
           <dependency>
               <groupId>springframework</groupId>
               <artifactId>spring-jdbc</artifactId>
               <version>5.1.9.RELEASE</version>
           </dependency>
   
           <dependency>
               <groupId>org.aspectj</groupId>
               <artifactId>aspectjweaver</artifactId>
               <version>1.9.4</version>
           </dependency>
   
           <dependency>
               <groupId>org.singledog</groupId>
               <artifactId>mybatis-spring</artifactId>
               <version>2.0.2</version>
           </dependency>
       </dependencies>
   
   ```

   

2. 编写配置文件

3. 测试





## 12.1、回忆mybatis



1. 编写实体类
2. 编写核心配置文件
3. 编写接口
4. 编写Mapper.xml
5. 测试



## 12.2、Mybatis-Spring



方式一：

1、编写数据源

```
package com.kun.pojo;

import lombok.Data;


public class User {
    private int id;
    private String name;
    private String pwd;

    public User() {
    }

  //下面省略set方法
}
```

2、配置数据库

```xml
 <!--DataSource：使用Spring的数据源替换Mybatis的配置  c3p0 dbcp druid
        我们这里使用Spring提供的JDBC：org.SpringFramework.jdbc.datesource
    -->
<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        <property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
        <property name="url" value="jdbc:mysql://localhost:3306/mybatis?userSSL=true&amp;useUnicode=true&amp;characterEncoding=UTF-8"></property>
        <property name="username" value="root"></property>
        <property name="password" value="dyk160513140."></property>
    </bean>
```





3、sqlSessionFactory【重点】

```xml
   <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
<!--        绑定Mybatis配置文件-->
        <property name="configLocation" value="classpath:mybatis-config.xml"></property>
        <property name="mapperLocations" value="classpath:com/kun/mapper/*.xml"></property>
    </bean>

```



3、sqlSessionTemplate【重点】

```java
public class UserMapperImpl implements UserMapper{

    //我们的所有操作，都使用sqlSession来执行在原来，现在都使用SqlSessionTemplate；

    private SqlSessionTemplate sqlSession;

    public void setSqlSession(SqlSessionTemplate sqlSession) {
        this.sqlSession = sqlSession;
    }

    @Override
    public List<User> selectUser() {
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        return  mapper.selectUser();
    }
}
```



4、需要给接口加实现类

5、将自己写的实现类，注入到Spring中

```java
   <bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
<!--      只能使用构造器注入sqlSessionFactory,因为它没有set方法  -->
        <constructor-arg index="0" ref="sqlSessionFactory"></constructor-arg>
    </bean>

```



6、测试使用即可

```java
public class Mytest {
    @Test
    public void test() throws IOException {
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");

        UserMapper userMapper = context.getBean("userMapper", UserMapper.class);
        for (User user : userMapper.selectUser()) {
            System.out.println(user);
        }
    }
}
```



**方式二：**SqlSessionDaoSupport实现【将接口注入spring】

配置文件：

```xml
<bean id="userMapper2" class="com.kun.mapper.UserMapperlmpl2">
        <property name="sqlSessionFactory" ref="sqlSessionFactory"></property>
    </bean>
```



类：

```java
public class UserMapperlmpl2 extends SqlSessionDaoSupport implements UserMapper {

    @Override
    public List<User> selectUser() {
        SqlSession sqlSession = getSqlSession();
        return sqlSession.getMapper(UserMapper.class).selectUser();
    }
}

```



# 13、声明式事务

## 1、回顾事务

- 把一组业务当程一个业务来做，要么都成功，要么都失败要么成功，要么失败
- 事务在项目开发中，十分的重要，涉及到数据的一致性问题，不能马虎
- 确保完整性和一致性



事务的ACID原则：

- 原子性
- 一致性
- 隔离性
  - 多个业务个能操作同一个资源，防止数据损坏
- 持久性
  - 事务一旦提交，无论系统发生什么问题，结果都不会再被影响，被持久化的写到了存储器中！





## 2、Spring中的事务管理

- 声明式事务：AOP
- 编程事务：需要在代码中，进行事务的管理



思考：为什么需要事务?

- ​	如果不配置事务，可能存在数据提交不一致的情况
- 如果我们不再Spring中去配置声明式事务，我们就需要在代码中手动配置事务
- 事务在i昂木中的开发十分重要，涉及到事务的一致性和完整性，不容马虎



```xml
<!--    配置声明式事务-->


    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <constructor-arg ref="dataSource" />
    </bean>

<!--    结合AOP实现事务的织入-->
<!--    配置事务通知:-->
    <tx:advice id="txAdvice" transaction-manager="transactionManager">
<!--        给哪写方法配置事务-->
<!--        配置事务的传播特性：新东西  propagation-->
        <tx:attributes>
            <tx:method name="add" propagation="REQUIRED"/>
            <tx:method name="delete" propagation="REQUIRED"/>
            <tx:method name="update" propagation="REQUIRED"/>
            <tx:method name="query" read-only="true"/>
            <tx:method name="*" propagation="REQUIRED"/>
            
        </tx:attributes>
    </tx:advice>

<!--    配置事务的切入-->
    <aop:config>
        <aop:pointcut id="txpointCut" expression="execution(* com.kun.mapper.*.*(..))"/>
        <aop:advisor advice-ref="txAdvice" pointcut-ref="txpointCut"></aop:advisor>
    </aop:config>
</beans>

//完全不改变原有的代码
```

