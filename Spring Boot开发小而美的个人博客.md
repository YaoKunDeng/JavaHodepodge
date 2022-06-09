# Spring Boot开发小而美的个人博客

**| 作者：邓尧坤**



## 1.1用户故事

​	用户故时模板：

- As a(role of user), I want (some feature) so that (some business value).
- 作为一个（某个角色）使用者，我们可以做（某个功能）事情，如此可以有（某个商业价值）的好处



​	举例：

- 作为一个招聘网站 **注册用户**，我想**查看最近3天发布的招聘信息**，以便 **了解最新的招聘信息**
- 作为公司，可以张贴新工作



**个人博客系统的用户故时**

角色：普通访客，管理员（我）

- 访客：可以分页查看所有的博客
- 访客，可以快速查看博客数最多的6个分类
- 访客，可以查看所有的分类
- 访客，可以查看某个分类下的博客列表
- 访客，可以查看所有的标签
- 访客，可以查看某个标签下的博客列表
- 访客，可以根据年度时间线查看博客列表
- 访客，可以快速查看最新的推荐博客
- 访客，可以用关键字全局搜索博客
- 访客，可以查看单个博客的内容
- 访客，可以赞赏博客内容
- 访客，可以微信扫码阅读博客内容
- 访客，可以在首页扫描微信公众号二维码关注我
- 我，可以用户名和密码登录后台管理
- 我，可以管理博客
  - 我，可以发布新博客
  - 我，可以对博客进行分类
  - 我，可以对博客打标签
  - 我，可以修改博客
  - 我，可以删除博客
  - 我，可以根据标题，分类，标签查询博客
- 我，可以管理博客分类
  - 我，可以新增一个分类
  - 我，可以修改一个分类
  - 我，可以删除一个分类
  - 我，可以根据分类名称查询分类
- 我，可以管理标签
  - 我，可以新增一个标签
  - 我，可以修改一个标签
  - 我，可以删除一个标签
  - 我，可以根据名称查询标签



# 2、前端插件集成

​	**编辑器Markdown** : https://pandao.github.io/editor.md/

​	**内容排版typo.css** ：https://github.com/sofish/typo.css/

​	如何引用：

1. 下载代码

2. 将css文件copy到文件目录下

3. 在页面引入css文件【link】

4. ![image-20220513224752415](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220513224752415.png)

   参照官方html文件，在外部div使用typo typo-selection类	

5.typo与semantic-ui有点冲突，需要做一点改动，改typo的css文件，将其封闭起来，只用在type下面的样式有效

​	**动画 animatie.css**：https://animate.style/  

​	**代码高亮 prism**：https://prismjs.com/

​		![image-20220513234337988](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220513234337988.png)

​	

​	**滚动侦测 waypoints**：https://github.com/imakewebthings/waypoints

​	**平滑滚动jquery.scrollTo ：**https://github.com/flesler/jquery.scrollTo



​	**目录生成 tocbot** ：http://tscanlin.github.io/tocbot/【用于博客详情】

1. 下载源码
2. 导入dist目录下的js、css
3. ![image-20220514102416251](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220514102416251.png)

​	**二维码生成qrcode.js**：http://davidshimjs.github.io/qrcodejs/





# 3、构建项目与配置

## 3.1、构建框架

1. 引入Spring Boot 模块
   1. web
   2. Thymeleaf
   3. JPA 【简化数据库相关的所有操作】
   4. MySql
   5. Aspect
   6. DevTools   热部署



2、数据库配置

```yaml
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3308/blog?useUnicode=true&characterEncoding=utf-8
    username: root
    password: dyk160513140.
```

3、JPA配置

```yaml
jpa:
    hibernate:
      ddl-auto: update
    show-sql: true
```

作用：每次连接数据库的时候比对当前实体类对象与数据库映射是否一样，，设置了update就是当对象的属性改变，数据库的字段也会自动改变，

4、logback日志配置

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<configuration>
    <!--包含Spring boot对logback日志的默认配置-->
    <include resource="org/springframework/boot/logging/logback/defaults.xml" />
    <property name="LOG_FILE" value="${LOG_FILE:-${LOG_PATH:-${LOG_TEMP:-${java.io.tmpdir:-/tmp}}}/spring.log}"/>
    <include resource="org/springframework/boot/logging/logback/console-appender.xml" />

    <!--重写了Spring Boot框架 org/springframework/boot/logging/logback/file-appender.xml 配置-->
    <appender name="TIME_FILE"
              class="ch.qos.logback.core.rolling.RollingFileAppender">
        <encoder>
            <pattern>${FILE_LOG_PATTERN}</pattern>
        </encoder>
        <file>${LOG_FILE}</file>
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <fileNamePattern>${LOG_FILE}.%d{yyyy-MM-dd}.%i</fileNamePattern>
            <!--保留历史日志一个月的时间-->
            <maxHistory>3</maxHistory>
            <!--
            Spring Boot默认情况下，日志文件10M时，会切分日志文件,这样设置日志文件会在100M时切分日志
            -->
            <timeBasedFileNamingAndTriggeringPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
                <maxFileSize>10MB</maxFileSize>
            </timeBasedFileNamingAndTriggeringPolicy>

        </rollingPolicy>
    </appender>

    <root level="INFO">
        <appender-ref ref="CONSOLE" />
        <appender-ref ref="TIME_FILE" />
    </root>

</configuration>
        <!--
            1、继承Spring boot logback设置（可以在appliaction.yml或者application.properties设置logging.*属性）
            2、重写了默认配置，设置日志文件大小在100MB时，按日期切分日志，切分后目录：
                my.2017-08-01.0   80MB
                my.2017-08-01.1   10MB
                my.2017-08-02.0   56MB
                my.2017-08-03.0   53MB
                ......
        -->
```



5、配置多套环境

​	生产环境  application-pro

​	开发环境 application-dev

​	总配置   application



## 3.2、框架搭建-异常处理

### 1、异常处理

- 定义错误页面

  - 404
  - 500
  - error

- 全局处理异常

  - 统一异常处理

  - ```java
    /*
     * Created by Daniel on 2022/5/15
     * */
    @ControllerAdvice  //这个注解会拦截所有Controller这个注解的类,但是下面的ExceptionHandler标注了只会拦截Exception级别的异常并作处理
    public class ControllerExceptionHandler {
        private Logger logger = LoggerFactory.getLogger(this.getClass());
    
        @ExceptionHandler(Exception.class) //标注这个方法是用来做异常处理的,参数代表拦截的信息是Excepttion级别的
        public ModelAndView exceptionHandler(HttpServletRequest request, Exception e)throws Exception{
            //使用logger记录错误信息
            logger.error("Request URL:{},Exception:{}",request.getRequestURL(),e);
            if(AnnotationUtils.findAnnotation(e.getClass(), ResponseStatus.class)!=null){
                throw e;
            }
    
            ModelAndView mv = new ModelAndView();
            mv.addObject("url",request.getRequestURL());
            mv.addObject("exception",e);
            mv.setViewName("error/error");
            return mv;
        }
    }
    ```

    @ControllerAdvice介绍：https://blog.csdn.net/qq_36829919/article/details/101210250

    

  - 错误页面异常信息显示处理

    ```html
    <body>
        <h1>错误</h1>
    
        <div>
    <!--        th:remove:把元素标签移除掉，就是在显示页面中，div就没了-->
            <div th:utext="'&lt;!--'" th:remove="tag"></div>
            <div th:utext="'Failed Request URL: ' + ${url}" th:remove="tag"></div>
            <div th:utext="'Exception message : ' + ${exception.message} " th:remove="tag"></div>
            <ul th:remove="tag">
                <li th:each="st : ${exception.stackTrace}" th:remove="tag"><span th:utext="${st}" th:remove="tag"></span></li>
            </ul>
            <div th:utext="'--&gt;'" th:remove="tag"></div>
    
        </div>
    
    
    </body>
    </html>
    ```

    

## 3、3日志处理

1. 记录日志内容【采用aop方式】
   1. 请求rul
   2. 访问者ip
   3. 调用方法
   4. 参数args
   5. 返回内容

```java
public class LogAspect {
    private final Logger logger = LoggerFactory.getLogger(this.getClass());

    @Pointcut("execution(* com.kun.web.*.*(..))")
    public void log(){

    }

    @Before("log()")   //表示这个方法在切面之前执行
    public void doBefore(JoinPoint joinPoint){ //JoinPoint切面对象
        //拿到request,用request去获取需要的东西
        ServletRequestAttributes attributes = (ServletRequestAttributes) RequestContextHolder.getRequestAttributes();
        HttpServletRequest request = attributes.getRequest();
        String url = request.getRequestURL().toString();
        String ip = request.getRemoteAddr();
        String classMethod = joinPoint.getSignature().getDeclaringTypeName() + "." +joinPoint.getSignature().getName();
        Object[] args = joinPoint.getArgs();
        RequestLog requestLog = new RequestLog(url ,ip, classMethod, args);
        logger.info("Request:{}",requestLog);

    }
    @Before("log()")
    public void doAfter(){
//        logger.info("--------------Aftere--------------");
    }

    @AfterReturning(returning = "result",pointcut = "log()")
    public void doAfterReturn(Object result){
        logger.info("Result:{}",result);
    }

//    内部类，目的是将需要用到的参数封装
    private class RequestLog{
        private String url;  //请求的rul
        private String ip;  //访问者ip
        private String className;   //调用方法
        private Object[] args;  //参数args

    public RequestLog(String url, String ip, String className, Object[] args) {
        this.url = url;
        this.ip = ip;
        this.className = className;
        this.args = args;
    }

    @Override
    public String toString() {
        return "RequestLog{" +
                "url='" + url + '\'' +
                ", ip='" + ip + '\'' +
                ", className='" + className + '\'' +
                ", args=" + Arrays.toString(args) +
                '}';
    }
}
}

```



## 3.4、页面处理

1. 静态页面导入project
2. thyme leaf布局
   1. 定义fragment
   2. 使用fragment布局
3. 错误页面美化



# 4、设计与规范

## 4.1、实体设计

实体类：

- 博客Blog
- 博客分类Type
- 博客标签Tag
- 博客评论Comment
- 用户User

![image-20220519204211713](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220519204211713.png)

Type和Blog：Blog包含一个实体类：Blog————>Type :多对一    一个博客只有一个分类，一个分类有多个博客



![image-20220519204740064](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220519204740064.png)

![image-20220519205031467](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220519205031467.png)

双线圈的是一个对象，也可能是一个List里面有多个对象

Blog是关系维护方

![image-20220519205449516](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220519205449516.png)



![image-20220519205542696](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220519205542696.png)



![image-20220519205640807](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220519205640807.png)



![image-20220519205734880](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220519205734880.png)



![image-20220519205904031](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220519205904031.png)





# 5、后台管理

## 5.1、登录

1. 构建登录页面和后台管理首页
2. UserService和UserRepository
3. LoginController实现登录
4. MD5加密
5. 登录拦截器



## 5.2、分类管理

1. 分类管理页面
2. 分类列表分页
3. 分类新增、修改、删除



## 5.3、标签管理

还没实现



## 5.4、博客管理



1、博客分页管理

2、博客新增

3、博客修改

4、博客删除



## 5.5、页面展示

1. ?
2. top分类
3. top标签
4. 最新博客推荐
5. MarkDown 转换HTML
   - https://github.com/commonmark/commonmark-java
   - 除了基本的转换，还需要两个扩展 table、id的处理：以便生成目录



## 5.6、评论功能

- 评论信息提交与回复
- 评论信息列表展示功能
- 管理员回复功能



待学点：



# 6、导航页处理

​	

1. 分页页面
2. 标签页面
3. 归档页面
4. 关于我



3. 1 springboot日志、JPA，MD5加密、拦截器，request.re，th:text="${#arrays.length(type.blog)
