# 1.简介

## 1.1、什么是Mybatis



<img src="https://mybatis.org/images/mybatis-logo.png" alt="MyBatis logo" style="zoom:150%;" />



- MyBatis是一款优秀的**持久层框架**
- 它支持定制化SQL、存储过程以及高级映射
- MyBatis避免了几乎所有的JDBC代码和手动设置参数以及获取结果集
- MyBatis 可以使用简单的**XML或注解**来配置和映射原生类型，接口和Java的POJO（Plain Old Java Object，普通老师java对象）为数据库中的记录
- MyBatis本是apache的一个开源项目iBatis，2010年这个项目有apache software foundation迁移到了google code，并且改名为MyBatis
- 2013年11月迁移到了Github



如何获得MyBatis

- maven仓库：

  ```xml
  <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
  <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>3.5.2</version>
  </dependency>
  
  ```

  

  

- Github：https://github.com/mybatis/mybatis-3

- 中文文档：https://mybatis.org/mybatis-3/zh/index.html



## 1.2、持久化

数据持久化：

- 持久化就是将程序的数据在持久状态和瞬时状态
- 的转化过程
- 内存：断电即失
- 数据库（JDBC）、IO文件持久化

**为什么需要持久化**？

- 有一些对象不能让他丢掉
- 内存太贵



## 1.3、持久层

Dao层、Service层、Controller层、、、

- 完成持久化工作的代码块
- 层界限十分明显



## 1.4为什么需要MyBatis？

- 方便
- 传统的JDBC代码太复杂——>简化——>框架——>自动化
- 不用Mybatis也可以，**技术没有高低之分**
- 优点：
  - 简单易学
  - 灵活
  - SQL和代码的分离，提高了可维护性
  - 提供映射标签、支持对象与数据库的orm字段的关系映射
  - 提供了对象关系映射标签，支持对象关系组维护
  - 提供了XML标签，支持编写动态SQL

**最重要的一点：使用的人多**



# 2、第一个Mybatis程序

思路：搭建环境--->导入Mybatis-->编写代码--->测试！

## 2.1、搭建环境

搭建数据库

```mysql
CREATE DATABASE mybatis;

USE mybatis;
CREATE TABLE user
(
id INT(20) NOT NULL PRIMARY KEY,
name VARCHAR(30) DEFAULT NULL,
pwd VARCHAR(30) DEFAULT NULL

)ENGINE=INNODB DEFAULT CHARSET=utf8;

INSERT INTO user(id, name, pwd) VALUES
(1, '狂神', '123456'),
(2, '张三', '123456'),
(3, '李四', '123456')
```

新建项目：

1. 新建一个普通的maven项目
2. 删除src目录   ：这样处理的结果是，在全局pom.xml中导入maven依赖之后，在之后创建的每个module中，子工程都有有相应的依赖，而父工程的pom.xml文件中，也会多出的一module
3. 导入maven

```xml
<!--导入依赖-->
    <dependencies>
        <!--mysql驱动-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.23</version>

        </dependency>

        <!--mybatis-->
        <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.2</version>
        </dependency>

        <!--junit-->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>

        </dependency>

    </dependencies>
```

4.配置idea的JDK版本,如果不配置的话，每次maven项目重新编译，JDK版本就会变回到1.5

```xm
<properties>
    <maven.compiler.source>14</maven.compiler.source>
    <maven.compiler.target>14</maven.compiler.target>
</properties>
```



## 2.2、创建一个模块

- 编写mybatis的核心配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<!--configuration核心配置文件-->
<configuration>
    <environments default="development">
        <environment id="development">
            <!--transactionManager事务管理-->
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis?userSSL=true&amp;useUnicode=true&amp;characterEncoding=UTF-8"/>
                <property name="username" value="root"/>
                <property name="password" value="dyk160513140."/>
            </dataSource>
        </environment>
    </environments>
</configuration>
```

- userSSL=true&am：

​	Mysql在高版本需要指明是否进行SSL连接：

​		SSL协议提供服务主要：

​			1）认证用户服务器，确保数据发送到正确的服务器

​			2）加密数据，防止数据传输途中被窃取

​			3）维护维护数据完整性，验证数据在传输的途中是否丢失

- useUnicode=true&amp;characterEncoding=UTF-8

  添加的作用：指定字符编码、解码格式

  ​	1：存数据时：

  ​		数据库在存放项目数据的时候会先用UTF-8格式将数据解码成字节码，然后再将解码后的字节码重新使用GBK编码存放到数据库中

  ​	2：取数据时：

  ​		在从数据中取数据的时候，数据库会先将数据库中的数据按GBK格式解码成字节码重新按UTF-8格式编码数据，最后再将数据返回给客户端



​	

- 编写mybatis工具类

```java
public class MybatisUtils {
    private static SqlSessionFactory sqlSessionFactory;//提升作用域
    static {
        try {
            //使用Mybatis第一步：获取sqlSessionFactory对象
            String resource = "org/mybatis/example/mybatis-config.xml";
            InputStream inputStream = Resources.getResourceAsStream(resource);
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    //既然有了sqlSessionFactory,顾名思义，我们救可以从中获得SqlSession的实例了
    //SqlSession 定义包含了面向数据库执行SQL命令所需的所有方法

    public static SqlSession getSqlSession(){
        return  sqlSessionFactory.openSession();
    }
}
```



## 2.3、编写代码

- 实体类

```java

//实体类
public class User {
    private int id;
    private String name;
    private String pws;

    public User() {

    }

    public User(int id, String name, String pws) {
        this.id = id;
        this.name = name;
        this.pws = pws;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getPws() {
        return pws;
    }

    public void setPws(String pws) {
        this.pws = pws;
    }
}

```



- Dao接口：

```java
public interface UserDao {
   public List<User> getUserList();
}
```



- 接口实现类由原来的UserDaoImpl转变为一个Mapper配置文件

```java
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--namespace：绑定一个对应的Dao/Mapper接口-->
<mapper namespace="com.kuang.dao.UserDao">
    <!--select查询语句， id相当于原来实现接口的方法名字 -->
    <select id="getUserList" resultType="com.kuang.pojo.User"></select>
</mapper>
```

## 2.4、测试

### 遇到的问题

注意点：

Type interface com.kuang.dao.UserDao is not known to the MapperRegistry.

**MapperRegistry是什么？**

核心配置文件中注册mapper

- Junit测试

```java
public class UserDaoTest {
    @Test
    public void test(){
        //第一不：过去Sqlsession对象
        SqlSession sqlSession = MyBatisUtils.getSqlSession();
        //执行SQL
        UserDao mapper = sqlSession.getMapper(UserDao.class);
        List<User> userList = mapper.getUserList();

        for (User user : userList) {
            System.out.println(user.toString());
        }
        sqlSession.close();
    }
}

```

可能回遇到的问题：

1. 配置文件没有注册
2. 绑定接口错误
3. 方法名不对
4. 返回类型不对
5. maven导出资源问题

> 1、Type interface com.kuang.dao.UserDao is not known to the MapperRegistry.
>
> ![image-20211214174053818](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20211214174053818.png)

> ###### 2、Error building SqlSession.
> ###### The error may exist in com/kuang/dao/UserMapper.xml
> ###### Cause: org.apache.ibatis.builder.BuilderException:
>
> 出现问题原因：在java包下创建了XML文件，但是maven运行时找的是resource文件下配置的文件，不能导入java包下的配置文件
>
> 在这个错误中，把UserMapper.xml复制到target下运行的.class文件的同目录就可以运行，但不可能要每次都手动录入

出现这个问题的原因：

​	maven由于他们的约定大于配置，我们之后可能遇到我们写的配置文件，无法被导出或者生效的问题，解决方案：

```java
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





6：![image-20220412163409198](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220412163409198.png)

解决方法：https://blog.csdn.net/qq_51263533/article/details/120209830

# 3、namespace

### 	1、namespaace

​			namespace中的包名要和Dao/mapper接口的包名一致

### 	2、select

​			选择、查询语句

- id：就是对应的namespace中的方法名
- resultType：Sql语句执行的返回值
- parameterType：参数类型



1.编写接口

```java
//    根据ID查询用户
    User getUserById(int id);
```



2.编写对应的mapper中的sql语句

```java
<select id="getUserById" parameterType="int" resultType="com.kuang.pojo.User">
    select * from mybatis.user where id = #{id}
</select>
```

3.测试

```java
  @Test
    public void getUserById(){
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);

        User userById = mapper.getUserById(1);
        System.out.println(userById);

        sqlSession.close();
    }

```

注意点：增删改需要提交事务



### 3、insert

```java

<insert id="addUser" parameterType="com.kuang.pojo.User">
        insert into mybatis.user( id, name, pwd ) value (#{id}, #{name}, #{pwd})
    </insert>
```



### 4、update

```java
<update id="updateUser" parameterType="com.kuang.pojo.User">
        update mybatis.user set name=#{name},pwd=#{pwd} where  id = #{id}
    </update>
```



### 5、Delete

```java
<delete id="deleteUser" parameterType="int">
        delete from mybatis.user where id = #{id};
</delete>
```



### 6、错误分析

- ​	标签不要匹配错
- resource绑定mapper，需要使用路径！
- 程序配置文件必须符合规范
- NullPointerException 没有注册到资源
- 输出的xml文件重存在中文乱码问题
- maven资源没有导出问题



### 7、万能Map

假设，我们的实体类，或者数据库中的表，字段或者参数过多，我们应当考虑使用Map!

```java
//    万能的Mapper,不需要知到数据库里有什么
    int addUser2(Map<String, Object> map);

```

```java
<!--    传递map中的key-->
    <insert id="addUser2" parameterType="map">
        insert into mybatis.user( id, name, pwd ) values (#{userid}, #{UserName}, #{password})
    </insert>
```

```java
    @Test
    public void addUser2(){
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        Map<String, Object> map = new HashMap<>();
        map.put("userid", 8);
        map.put("UserName", "hello");
        map.put("password","2333333");

        mapper.addUser2(map);
        sqlSession.commit();
        sqlSession.close();

    }
```



Map传递参数，直接在sql中取出key即可

对象传递参数，直接在sql中取对象的属性即可

只有一个基本类型的情况下，可以直接在sql中取到

多个参数用Map，**或者注解**



### 8、思考题

模糊查询怎么写？

1、Java代码执行的时候，传递通配符

```java
List<User> userList = mapper.getUserLike("%李%");
```

2、在sql拼接中使用通配符！

```java
select * from mybatis.user where name like "%"#{value}"%"
```





# 4、配置解析

- mybatis-config.xml

- Mybatis 的配置文件包含了会深深影响Mybaits行为的设置和属性信息

  ```xml
  configuration（配置）
  
      properties（属性）
      settings（设置）
      typeAliases（类型别名）
      typeHandlers（类型处理器）
      objectFactory（对象工厂）
      plugins（插件）
      environments（环境配置）
          environment（环境变量）
              transactionManager（事务管理器）
              dataSource（数据源）
      databaseIdProvider（数据库厂商标识）
      mappers（映射器）
  
  ```

### 2、配置环境（environment）

Mybatis可以配置成适应多种环境

**不过要记住：尽管可以配置多个环境，但每个SqlSessionFactory实例只能选择一种环境**

学会使用配置多套运行环境

Mybatis默认事务管理器就是JDBC，连接池：POOLED

注：

- 事务管理器还有：MANAGED
  - JDBC-这个配置直接使用了JDBC的提交和回滚设施，他依赖从数据源获得的连接来管理事务作用域
  - MANAGED-这个配置几乎没做什么，他从不提交或回滚一个连接，而是让容器来管理事务的整个生命周期（比如JEE应用服务器的上下文）。默认情况下他会关闭连接，然而一个写容器并不希望关闭连接，因此需要将closeConnection属性设置为false来组织默认关闭的行为。
- 数据连接池还有：UNPOOLED、**JNDI** 
  - POOLED：这种数据源的实现利用“池”的概念将JDBC连接对象组织起来，避免了创建新的连接实例所必须的初始化和认证事件。这种处理方式很流行，能使并发Web应用快速响应请求。
  - UNPOOLED：这个数据源的实现会每次请求时打开和关闭连接。虽然有点慢，但对那些数据库连接可用性要求不高的简单应用程序来说，是一个很好的选择。



### 3、（属性）properties

我们可以**通过properties属性来实现引用配置文件**

这些属性都是外部配置且可动态替换的，既可以在典型的java属性文件中配置，亦可以通过properties元素的子元素来传递

编写一个配置文件

![image-20220413180958009](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220413180958009.png)

db.properties

```properties
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/mybatis?UseSSL=true&amp;UseUnicode=true&amp;CharacterEncoding=UTF-8
username=root
password=dyk160513140.
```

在核心配置文件中引入

```xml
<!--引入外部配置文件,优先使用外部文件-->
    <properties resource="db.properties">
        <property name="user" value="root"/>
    </properties>
```

- 可以直接引入外部文件
- 可以在其中增加一些配置属性
- 如果有两个文件有同一个字段，优先使用外部配置文件的！



### 4、类型别名（typeAliases）

- 类型别名是java类型设置的一个短的名字

- 存在意义仅在用来减少类完全限定名的冗余

  ```xml
      <typeAliases>
   		<typeAlias type="com.kuang.pojo.User" alias="User"></typeAlias>
      
      </typeAliases>
      
  ```

  也可以指定一个包名，Mybatis会在包名下面搜索需要的Java Bean,比如：

  ​	扫描实体类的包，他的默认别名就为这个类的类名，首字母小写！

  ```xml
     <typeAliases>
          <package name="com.kuang.pojo"/>
      </typeAliases>
  ```

  在实体类比较少的时候，使用第一种方式

  如果实体类十分多，建议使用第二种

  注意：第一种可以DIY，第二种不可以；如果非要改，需要在实体上增加注解

![image-20220413202010971](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220413202010971.png)



### 5、设置

这是Mybatis中极为重要的调整设置，他们会改变MyBatis的运行时行为

![image-20220413202559788](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220413202559788.png)

![image-20220413202545027](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220413202545027.png)



### 6、其他配置

- [typeHandlers（类型处理器）](https://mybatis.org/mybatis-3/zh/configuration.html#typeHandlers)
- [objectFactory（对象工厂）](https://mybatis.org/mybatis-3/zh/configuration.html#objectFactory)
- [plugins（插件）](https://mybatis.org/mybatis-3/zh/configuration.html#plugins)
  - mybatis-generator-core
  - mybatis-plus
  - 通用mapper



### 7、映射器（mappers）

注意：核心配置最好不要有中文注释

MapperRegistry:注册绑定我们的Mapper文件：

方式一：

```xml
<!--    每一个Mapper.xml都有需要在Mybtis核心配置文件重注册-->
    <mappers>
        <mapper resource="com/kuang/dao/UserMapper.xml"></mapper>
    </mappers>
```

方式二：使用class文件绑定注册

```
    <mappers>
        <mapper class="com.kuang.dao.UserMapper"></mapper>
    </mappers>
```

- 注意点：接口和他的Mapper配置文件必须同名
- 接口和他的Mapper配置文件必须在同一个包下



方式三：使用扫描包进行注入半丁

```xml
    <mappers>
        <package name="com.kuang.dao"/>
    </mappers>

```

**注意点同方式二**



练习时间：

- 将数据库配置文件外部引入
- 实体类名
- 保证UserMapper接口和UserMapper.xml改为一致，并且放在同一个包下



### 8、生命周期

**生命周期和作用域**是至关重要的，因为错误的使用会导致非常严重的**并发问题**

![image-20220413215535103](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220413215535103.png)

**SqlSessionFactory:**

- ​	一旦创建了SqlSessionFactory就不再需要它了
- 局部变量

**SqlSessionFactory：**

- 说白了就可以想象为：数据库连接池
- SqlSessionFactory一旦被创建就应该在应用的运行期间一致纯在，**没有任何理由丢弃它或重新创建另外一个实例**
- 因此SqlSessionFactory的最佳作用域是应用作用域
- 最简单的就是使用单例模式或者静态单例模式

**SqlSession**

- 连接到数据池的一个请求
- SqlSession的实例不是线程安全的，因此是不能被共享的，所以它的最佳作用域是请求或方法作用域
- 用完之后需要赶紧关闭，否则资源被占用



# 5、解决属性名和字段不一致的问题

数据库中的字段

![image-20220413220932223](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220413220932223.png)

新建一个项目，拷贝之前的，情况测试实体类字段不一致的

```java
public class User {
    private int id;
    private String name;
    private String password;

```

测试结果：

![image-20220414084824557](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220414084824557.png)

解决方法：

```xml
// select *from mybatis.user where id=#{id}
//类型处理器
// select id,name,pwd from mybatis.user where id = #{id}
```



- 起别名：

  ```xml
  <select id=getUserList resultType="com.kun.pojo.User">
  	select id， name, pws as password from mybatis.user where id = #{id}
  </select>
  ```



### 2、resultMap

结果集映射

```xml
id name pwd
id name password
```

```xml
<resultMap id="UserMap" type="User">

        <result property="id" column="id"/>
        <result property="name" column="name"/>
        <result property="password" column="pwd"/>
</resultMap>

    <select id="getUserList" resultMap="UserMap">
        select * from mybatis.user;
    </select>
```



- resultMap元素是Mybatis中最重要最强大的元素

- ResultMap的设计思想是，对于简单的语句根本不需要配置显式的结果映射，而对于复杂一点的语句，只需要描述他们的关系就行了

- ResultMap最优秀的地方在于，虽然你对它相当了解了，但是根本不需要西显式的用到它们

  ![image-20220414093644937](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220414093644937.png)





# 6、日志

### 6.1、日志工厂

​	如果一个数据库操作，出现了异常，我们需要排错，日志就是最好的助手

曾经：sout、debug

现在：日志工厂

![image-20220414094348965](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220414094348965.png)

- SLF4J 
- LOG4J【掌握】
- LOG4J2 
-  JDK_LOGGING 
-  COMMONS_LOGGING 
-  STDOUT_LOGGING 【掌握】
-  NO_LOGGING              

在Mybatis中，具体使用哪一个日志实现，在设置中设定！

STDOUT——LOGGING标准日志输出

在mybatis核心配置文件中，配置我们的日志！

![image-20220414101204828](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220414101204828.png)

![image-20220414101223941](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220414101223941.png)



### 6.2、LOG4J

什么时log4j？

- log4j是Apache的一个开源项目，通过使用Log4j，我们可以控日志信息输送的目的地是控制台、文件、GUI组件
- 我们也可以控制每一条日志输出的格式；
- 通过定义每一条日志信息的级别，我们能够更加习致地控制日志的生成过程
- 通过一个**配置文件**来灵活地进行配置，而不需要修改应用代码



1.先导入log4j的包

```xml
    <dependencies>
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.17</version>
        </dependency>
    </dependencies>

```

2、log4j.properties

```properties
#将等级为DEBUG的日志信息输出到console和file这两个目的地，console和file的定义在下面的代码

log4j.rootLogger = DEBUG,console,file

#控制台输出的相关设置
log4j.appender.console = org.apache.log4j.ConsoleAppender
log4j.appender.console.Target = System.out
log4j.appender.console.Threshold = DEBUG
log4j.appender.console.layout = org.apache.log4j.PatternLayout
log4j.appender.console.layout.ConversionPattern=[%c]-%m%n

#文件输出的相关设置

log4j.appender.file = org.apache.log4j.RollingCalendar
log4j.appender.file.File= ./log/kun.log
log4j.appender.file.Threshold = DEBUG
log4j.appender.file.layout = org.apache.log4j.PatternLayout
log4j.appender.file.layout.ConversionPattern=[%p][%d{yy-mm-dd}][%c]%m%n

#日志输出级别

log4j.logger.org.mybatis=DEBUG
log4j.logger.java.sql=DEBUG
log4j.logger.java.sql.Statement=DEBUG
log4j.logger.java.sql.ResultSet=DEBUG
log4j.logger.java.sql.PreparedStatement=DEBUG
```



3、配置log4j为日志实现

```xml
<settings>
        <setting name="logImpl" value="LOG4J"/>
   </settings>
```

4、Log4j的使用！直接测试运行刚才的查询

![image-20220414105407330](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220414105407330.png)

![image-20220414105509989](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220414105509989.png)

**简单使用**

1、在要使用Log4j的类中，导入包

2、日志对象，参数为当前类的class

```java
static Logger logger = Logger.getLogger(UserMapper.class);
```





# 7、分页

**思考：为什么要分页？**

- 减少数据的处理量



### **7.1使用Limit分页**

```xml
语法：SELECT * from user limit startIndex, pageSize
SELECT * from user limit3;#[0,3]
```



使用Mybatis实现分页，核心SQL

1、接口

```java
List<User> getUserLimit(Map<String,Integer> map);
```

2、Mapper.xml

```xml
<select id="getUserLimit" parameterType="map" resultType="user">
    select * from  mybatis.user limit #{startIndex},#{pageSize}
</select>
```

3、测试

```java
@Test
public void getUserLimit(){
    SqlSession sqlSession = MybatisUtils.getSqlSession();
    UserMapper mapper = sqlSession.getMapper(UserMapper.class);
    HashMap<String, Integer> map = new HashMap<>();
    map.put("startIndex",0);
    map.put("pageSize", 2);
    List<User> userLimit = mapper.getUserLimit(map);
    for(User user : userLimit){
        System.out.println(user);
    }
    sqlSession.close();

}
```



7.2分页插件

​	Mybatis pageHelper

了解即可，玩意以后公司的架构师说要使用，你需要知道他是什么东西



# 8、使用注解开发



### 8、1面向接口编程

-大家之前都学过面向对象编程，也学习过接口，但在真正的开发中，很多时候我们都会选择面向接口编程

-**根本原因：解耦，可扩展，提高复用，分层开发中，上层不用管具体的实现，大家都遵守共同的标准，使得开发变得容易，规范性更好**

-在一个面向对象的系统中，系统的各种功能是由许许多多的不同对象写作完成的。在这种情况下，各个对象内部是如何实现自己的，对系统设计人员来说就不那么重要了

-而各个对象之间的协作关系则成为系统设计的关键。小到不同类之间的通信，大到各模块之间的交互，在系统设计支出都是要着重考虑，这也是系统设计的主要内容。面向接口编程就是按这种思想来编程。



**关于接口的理解：**

-接口从更深层次的理解，应是定义（规范、约束）与实现（名实分离的原则）的分里

-接口的本身反映了系统设计人员对系统的抽象理解

-接口应有两类：

​	-第一类失对一个个体的抽象，它可以对应为一个抽象提（abstract class）

​	-第二类是对一个个体某一方面的抽象，即形成一个抽象面（interface）

-一个体有可能有多个抽象面。抽象体与抽象面是由区别的。



**三个面向区别**

-面向对象是指：我们考虑问题的时候，以对象为单位，考虑他的属性及方法

-面向过程是指：我们考虑问题时，以一个具体的流程（事务过程）为单位，考虑它的实现。

-接口设计与非接口设计是针对复用技术而言的，与面向对象（过程）不是一个问题，更多体现就是对系统整体的架构。





### 8.2、使用注解开发

1、注解在接口上实现

```java
public interface UserMapper {
    @Select("select * from mybatis.user")
    List<User> getUsers();

}
```

2、需要在配置文件中绑定接口

```
<mappers>
    <mapper class="com.kun.dao.UserMapper"></mapper>
</mappers>
```



本质：反射机制实现（待学）

底层：动态代理（待学）

![image-20220415111510827](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220415111510827.png)



### 8.3、CRUD

我i们可以在工具类创建的时候实现自动提交事务

```java
public static SqlSession getSqlSession(){
    return sqlSessionFactory.openSession(true);
    
}
```

![image-20220415113836588](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220415113836588.png)

箭头两个id对应，与int id无关，@Param相当于定义参数名字

测试类：

【注意：我们必须要将接口注册绑定到我们的核心配置文件中！】



#### **关于@Param()注解**

- 基本类型的参数或者String类型，需要加上
- 引用类型不需要加
- 如果只有一个基本类型的话，可以忽略，但是建议大家都加上
- 如果在SQL中引用就是我们这里的@Param()中设定的属性名

**#{} ${}的区别**

${}不安全，会导致SQL注入



# 9、Lombok

```xml
Project Lombok is a java library that automatically plugs into your editor and build tools, spicing up your java.
Never write another getter or equals method again, with one annotation your class has a fully featured builder, Automate your logging variables, and much more. 
```

- java library
- plugs
- build tools
- with one annotion your class



使用步骤：

1、在EDEA中安装Lombok插件

2、在项目重导入Lombok的jar包

```xml
 <dependencies>
        <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>1.18.20</version>
        <scope>provided</scope>
    </dependency>

    </dependencies>
```

3、在实体类上加注解即可!

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
```

4、注解详细

```xml
@Getter and @Setter
@FieldNameConstants
@ToString
@EqualsAndHashCode
@AllArgsConstructor, @RequiredArgsConstructor and @NoArgsConstructor
@Log, @Log4j, @Log4j2, @Slf4j, @XSlf4j, @CommonsLog, @JBossLog, @Flogger, @CustomLog
@Data
@Builder
@SuperBuilder
@Singular
@Delegate
@Value
@Accessors
@Wither
@With
@SneakyThrows
@val
@var
experimental @var
@UtilityClass
```

说明：

```xml
@Data:无参构造，get、set、tostring、hashcode、equals
@AllArgsConstructor
@NoArgsConstructor
@EqualsAndHashCode
@ToString
@Getter
```



# 10、多对一处理

多对一：多个学生对应一个老师

对于学生这边而言，关联...多个学生，关联一个老师【多对一】

对于老师而言，集合，一个老师有很多学生【一对多】



SQL：

```xml
use mybatis;
CREATE TABLE teacher(
id INT(10) not null,
name varchar(30) default null,
primary key(id)
)engine=InnoDB default charset=utf8;

insert into teacher(id,name) values ('1',"秦老师");

create table student(
	id int(10) not null,
    name varchar(30) default null,
    tid int(10) default null,
    primary key(id),
    key fkid(tid),
    constraint fktid foreign key (tid) references teacher(id)
)engine=InnoDB default charset=utf8;

insert into student(id,name,tid) values ('1',"小明", '1');
insert into student(id,name,tid) values ('2',"小红", '1');
insert into student(id,name,tid) values ('3',"小张", '1');
insert into student(id,name,tid) values ('4',"小李", '1');
insert into student(id,name,tid) values ('5',"小王", '1');

```



实体类：

```java
package com.kun.pojo;

import org.apache.ibatis.type.Alias;

@Alias("Student")
public class Student {
    private int id;
    private String name;

//    学生需要关练一个老师
    private Teacher teacher;

}

```



```java
package com.kun.pojo;

public class Teacher {
    private int id;
    private String name;

  
}

```





### 按照查询嵌套处理

```xml
<!--
    思路：
        1、查询所有的学生信息
        2、根据擦汗寻出来的学生的tid，寻找对应的老师！ 子查询
-->
    <select id="getStudent" resultMap="StudentTeacher">
        select * from mybatis.student;
    </select>
    
    <resultMap id="StudentTeacher" type="Student">
        <result property="id" column="id"></result>
        <result property="name" column="name"></result>
<!--        复杂的属性，我们需要单独处理
            对象：association
            集合：collection
-->
        <association property="teacher" column="tid" javaType="Teacher" select="getTeacher"></association>
        //property: 学生对象的字段   colum：数据库的对象
        //这里将每个学生的数据库字段的tid通过select作为参数，传到id为getTeacher的选择SQL语句
    </resultMap>
    
    <select id="getTeacher" resultType="Teacher">
        select * from mybatis.teacher where id=#{tid}
    </select>
    
    
```



### 按照结果嵌套处理

```xml
<!--    按照结果嵌套处理-->
    <select id="getStudent2" resultMap="StudentTeacher2">
        select s.id sid, s.name sname, t.name tname from student s, teacher t where s.tid=tid;
    </select>

    <resultMap id="StudentTacher2" type="Student">
        <result property="id" column="sid"></result>
        <result property="name" column="sname"></result>
        <association property="teacher" javaType="Teacher">
<!--        将结果集关联到一个对象   
            property=teacher  就是关练到Student对象的teacher属性 而这个属性就是java类型-->
            <result property="name" column="tname"></result>
            //查询出来老师id为0，因为teacher对象中的id没有赋值
        </association>
    </resultMap>


```





# 11、一对多处理

比如：一个老师拥有多个学生

对于老师而言，就是一对多的关系



实体类：

```java
public class Student {
    private int id;
    private String name;
    private int tid;
}
```

```java
public class Teacher {
    private int id;
    private String name;

    //一个老师对应多个学生
    private List<Student> students;
}
```



### 按照结果嵌套处理

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.kun.dao.TeacherMapper">


    <select id="getTeacher" resultMap="TeacherStudent">
        select s.id sid, s.name sname, t.name tname, t.id tid from student s, teacher t
        where s.tid = t.id and t.id = #{tid}
    </select>

<!--    按结果集查询-->
    <resultMap id="TeacherStudent" type="Teacher">
        <result property="id" column="tid"></result>
        <result property="name" column="tname"></result>
        <!--        复杂的属性，我们需要单独处理  对象：association 集合：collection
                javaType=“” 指定属性类型
                集合中的泛型信息，我们使用ofType获取
    -->
        <collection property="students" ofType="Student">  //不用javaType，因为他不用返回值，也可以加
            <result property="id" column="sid"></result>
            <result property="name" column="sname"></result>
            <result property="tid" column="tid"></result>
        </collection>
    </resultMap>
</mapper>
```



### 按照查询嵌套处理

```java
 <select id="getTeacher2" resultMap="TeacherStudent1">
        select * from mybatis.teacher where id = #{tid}
    </select>

    <resultMap id="TeacherStudent1" type="Teacher">
        <result property="id" column="id"></result>
        <result property="name" column="name"></result>
        <collection property="students" column="id" javaType="ArrayList" ofType="Student" select="getStudentByTeacherId"></collection>
    </resultMap>

    <select id="getStudentByTeacherId" resultType="Student">
        select * from mybatis.student where tid = #{tid}
    </select>
```





#### 小结：

1、关联-association【多对一】

2、集合-collection【一对多】

3、javaType & ofType

​	1、javaType用来指定实体类中的属性类型

​	2、ofType用来指定映射到List或者集合中的pojo类型，泛型中的约束类型



注意点：

- ​	保证SQL的可读性，尽量保证通熟易懂
- 注意一对多和多对一，属性名和字段相同
- 如果问题不好排查错误，可以使用日志，建议使用log4j



**慢SQL**

面试高频：

- Mysql引擎
- InnoDB底层原理
- 索引
- 索引优化





# 12、动态SQL

**什么是动态SQL：动态SQL就是根据不同的条件生成不用的SQL**



### 搭建环境

```SQL
use mybatis;
create table blog(
    id varchar(50) not null comment '博客id',
    title varchar(100) not null comment  '博客标题',
    author varchar(30) not null comment '博客作者',
    create_time datetime not null comment '创建时间',
    view int(30) not null comment '浏览量'
)engine=InnoDB default charset=utf8
```



创建一个基础工程

​	1、导包

​	2、编写配置文件

​	3、编写实体类

```java
public class Blog {
    private int id;
    private String title;
    private String author;
    private Date createTime;
    private int views;
}
```

​	4、编写实体类对应的Mapper接口和Mapper.xml文件

```java

public interface BlogMapper {

    //插入数据

    Integer addBook(Blog blog);
}

```



```xml
错误：Mapper method 'com.kun.dao.BlogMapper.addBook attempted to return null from a method with a primitive return type (int).
解决方法：将BlogMapper中的方法返回值改为Integer
```



### IF

```xml
<select id="queryBlogIf" parameterType="map" resultType="Blog">
        select * from mybatis.blog where 1=1
        <if test="title != null">  //传入的参数title
            and title = #{title}  //第一个title是数据库字段， 第二个是传入参数
        </if>

        <if test="author != nu;;">
            and author = #{author}
        </if>
    </select>

//意思是：如果传入title，则sql语句后面将进行拼接
//where 1=1 是为了后面的拼接更加方便
```

### WHERE:当所有条件不满足，where标签自动省略

```xm
<select id="queryBlogIF" parameterType="map" resultType="Blog">
        select * from mybatis.blog 
        <where>
            <if test="title != null">
                title = #{title}
            </if>

            <if test="author != null">
                and author = #{author}
            </if>
        </where>
    </select>
    //用where标签更加规范
```

*where* 元素只会在子元素返回任何内容的情况下才插入 “WHERE” 子句。而且，若子句的开头为 “AND” 或 “OR”，*where* 元素也会将它们去除。



### choose（when,otherwise)

```xml
<select id="queryBlogChoose" parameterType="map" resultType="Blog">
    select * from  mybatis.blog
    <where>
        <choose>
            <when test="title != null">
                title = #{title}
            </when>
            <when test="author != null">
                and author = #{author}
            </when>
            <otherwise>
                and views = #{views}
            </otherwise>
        </choose>
    </where>
</select>
```



### set

```xml
<select id="updateBlog" parameterType="map">
    update mybatis.blog
    <set>
        <if test="title != null">
            title = #{title},
        </if>
        <if test="author != null">
            author = #{author}
        </if>
    </set>
    where id = #{id}
</select>
```



所谓的动态SQL，本质还是SQL语句，只是可以在SQL层面，去执行一个逻辑代码

if、where choose，when



### SQL片段

有的时候，我们可能会将一些功能的部分抽取出来，方便复用！

​	1、使用SQL标签抽取公共部分

```xml
<sql id="if-title-author">
    <if test="title != null">
        title = #{title}
    </if>

    <if test="author != null">
        and author = #{author}
    </if>
</sql>
```

​	2、在需要使用的地方使用Include标签引用即可

```xml
<select id="queryBlogIF" parameterType="map" resultType="Blog">
    select * from mybatis.blog
    <where>
        <include refid="if-title-author"></include>
    </where>
</select>
```



注意事项：

- 最好基于单表来定义
- 不要存在where标签





### Foreach

```java
select * from user where 1=1 and 
    <foreach item="id" index="index" collection="ids"
    	open="(" separator="," close=")">
    </foreach>
    
    (id=1 or id=2 or  id=3)
```



```xml
<!--    select * from mybatis.blog where 1=1 and (id=1 or id=2 or id=3)
        我们现在传递一个万能的map，这map中可以存在一个集合
-->
<select id="queryBlogForeach" parameterType="map" resultType="Blog">
        select * from mybatis.blog
        <where>

            <foreach collection="ids" item="id" open="and (" separator="or" close=")">
                id=#{id}
            </foreach>
        </where>
    </select>
```



==动态SQL就是在拼接SQL语句，我们只要保证SQL的正确行，按照SQL的格式，去排列组合就可以了==

建议：

- ​	现在Mysqll中写出完成的SQL，再对应的去修改成为我们的动态SQL实现通用即可



# 13、缓存

### 13.1、简介

```xml
查询：连接数据库，耗资源
	一次查询结果，给他暂存在一个可以直接取到的地方！-->内存

我们再次查询相同数据的时候，直接走缓存，就不用走数据库
```



1. 什么是缓存[Cache]？
   - 存在内存中的临时数据
   - 将用户经常查询的数据存放在缓存中（内存）中，用户查询数据就不用从磁盘上（关系型数据库数据文件）查询，从缓存中查询，从而提高查询效率，解决了高并发系统的性能问题。
2. 为什么使用缓存？
   - 减少和数据库的交互次数，减少系统开销，提高体统效率。
3. 什么样的数据能够使用缓存？
   - 经常查询并且不经常改变的数据。【可以使用缓存】





### 13.2、Mybatis缓存

- Mybatis包含一个非常强大的查询缓存特性，他可以非常方便地定制和配置缓存，缓存可以极大的提升查询效率。
- Mybatis系统中默认定义了两级缓存：以及缓存和二级缓存
  - 默认情况下，只有一级缓存开启。（SqlSession级别缓存，也称为本第缓存）
  - 二级缓存需要手动开启和配置，它是基于namespqce级别的缓存
  - 为了提高扩展性，Mybatis定义了缓存接口Cache。我们可以通过Cache接口来定义二级缓存



### 13.3、一级缓存

- 一级缓存也叫本地缓存：SqlSession
  - 与数据库同一次会话期间查询到的数据放在本地缓存中
  - 以后如果需要获取相同的数据，直接从缓存中拿，没必须再去查询数据库



测试步骤：

1、开启日志

2、测试再一个Session中查询两次相同记录

```java
public void test(){
    SqlSession sqlSession = MybatisUtils.getSqlSession();

    UserMapper mapper = sqlSession.getMapper(UserMapper.class);

    User user = mapper.queryUserById(1);
    System.out.println(user);

    System.out.println("=================================");

    User user1 = mapper.queryUserById(1);
    System.out.println(user1);

    System.out.println(user==user1);


    sqlSession.close();
}
```

3、查看日志输出

![image-20220418095338324](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220418095338324.png)



缓存失效情况：

1. 查询不同的东西
2. 增删改操作，可能会改变原来的数据，所以必定会刷新缓存
3. 查询不同的Mapper.xml
4. 手动清理缓存

```java
sqlSession.clearCache();
```

小结：一级缓存是默认开启的，也不能手动进行关闭

一级缓存相当于一个Map



### 13.4、二级缓存

- 二级缓存也叫全局缓存，一级缓存作用域太低，所以诞生了二级缓存，
- 基于namespace级别的缓存，一个名称空间，对应一个二级缓存；
- 工作机制
  - 一个会话查询一条数据，这个数据就会放在当前会话的一级缓存中；
  - 如果当前会话关闭，这个会话对应的一级缓存就没了；但是我们想要的是，会话关闭了，一级缓存中的数据被保存到二级缓存中；
  - 新的会话查询信息，就可以从二级缓存中获取内容；
  - 不同的mapper查出的数据会放在自己对应的缓存（map）中



步骤：

​	1、开启全局缓存

```xml
<!--显式的开启全局缓存-->
<setting name="cacheEnabled" value="true"/>
```



2、在要使用二级缓存的Mapper中开启

```xml
<cache
  eviction="FIFO"
  flushInterval="60000"
  size="512"
  readOnly="true"/>
```



3、测试

1. ​	问题：我们需要将实体类序列化！否则会报错！

   ```xml
   Caused by: java.io.NotSerializableException: com.kun.pojo.user
   ```

   解决：

   ```java
   public class User implements Serializable 
   ```



小结：

- 只要开启了二级缓存，在同一个Mapper下就有效
- 所有的数据都会先放在一级缓存中；
- 只有当会话提交，或者关闭的时候，才会提交到二级缓存中

​	

### 
