---


creation date: 2021-11-27 12:20:48

last modified: 2022-01-06 10:29:58

title: SpringMVC

categories:

- back-end
- spring

tags:
- java
- spring-mvc

---

Spring MVC 是基于 Spring 的，是其中的一个模块，其底层依赖 Servlet 实现。Spring MVC 的核心类是 DispatcherServlet，它的父类是 HttpServlet，也叫做前端控制器。

# 环境配置

## 打包方式修改

在 pom 文件中设置打包方式为 war 包

```xml
<packaging>war</packaging>
```

## 添加依赖

```xml
<dependencies>
    <!-- SpringMVC -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>5.3.1</version>
    </dependency>
    <!-- 日志 -->
    <dependency>
        <groupId>ch.qos.logback</groupId>
        <artifactId>logback-classic</artifactId>
        <version>1.2.3</version>
    </dependency>
    <!-- ServletAPI -->
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>javax.servlet-api</artifactId>
        <version>3.1.0</version>
        <scope>provided</scope>
    </dependency>
    <!-- Spring5和Thymeleaf整合包 -->
    <dependency>
        <groupId>org.thymeleaf</groupId>
        <artifactId>thymeleaf-spring5</artifactId>
        <version>3.0.12.RELEASE</version>
    </dependency>
</dependencies>
```

## 添加 web.xml

在 main 下创建 `webapp` 目录，然后在 “Project Structure -> Modules -> current project -> 选择 Web 模块 -> Deployment Descriptors -> 添加 web.xml (4.0)

注意：路径应该修改成 `.\src\main\webapp\WEB-INF\web.xml`

## 配置 Spring MVC Controller

在 web.xml 中添加注册 servlet

```xml
<servlet>
    <servlet-name>SpringMVC</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <!--扩展：配置SpringMVC配置文件的位置和名称-->
    <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:springMVC.xml</param-value>
    </init-param>
    <!-- 避免第一次访问时间太久，要求只要服务器启动就初始化 Controller -->
    <load-on-startup>1</load-on-startup>
</servlet>

<servlet-mapping>
    <servlet-name>SpringMVC</servlet-name>
    <!-- / 匹配请求可以 /XXX 或 .html 或 .js 或 .css 不能匹配 .jsp -->
    <url-pattern>/</url-pattern>
</servlet-mapping>
```

## 配置 SpringMVC.xml

(也就是配置 Spring 配置文件)

```xml
<context:component-scan base-package="com.demo.controller"/>

<!-- 配置Thymeleaf视图解析器 -->
<bean id="viewResolver" class="org.thymeleaf.spring5.view.ThymeleafViewResolver">
    <!--配置优先级-->
    <property name="order" value="1"/>
    <property name="characterEncoding" value="UTF-8"/>
    <property name="templateEngine">
        <bean class="org.thymeleaf.spring5.SpringTemplateEngine">
            <property name="templateResolver">
                <bean class="org.thymeleaf.spring5.templateresolver.SpringResourceTemplateResolver">
                    <!-- 视图前缀 -->
                    <property name="prefix" value="/WEB-INF/templates/"/>
                    <!-- 视图后缀 -->
                    <property name="suffix" value=".html"/>
                    <property name="templateMode" value="HTML5"/>
                    <property name="characterEncoding" value="UTF-8"/>
                </bean>
            </property>
        </bean>
    </property>
</bean>
```

# [@RequestMapping ](/RequestMapping ) 注解 

## 作用位置

该注解可以使用在 Class 上和 Method 上

作用在 Class 上的时候，该 Controller 会有自己的一个 base path，通常一个大的模块对应一个大的 path，比如 user，order，profile 之类的；

作用在 Method 上的时候，如果 Class 上有该注解，那么 Method 上该注解匹配的请求路径就是 base path 下的一个子路径。比如 Class 上作用了一个 `@RequestMapping("/order")` 然后有一个方法上面作用的是 `@RequestMapping("/status")` 那么想请求 status 页面的话，完整路径将会是 `XXX/order/status`

## value 和 method 注解参数

value 属性**必须设置**，其可以是一个数组，当传入一个数组。如果是数组的话，就是多个请求地址对应一个处理方法

```java
@RequestMapping(
	value = {"/hello", "/test"}
)
public String hello() { return "hello"; }
```

method 属性也可以设置多种请求方式，当设置 method 属性的时候通过 `RequestMehod` 枚举来设置

```java
@RequestMapping(
    value = {"/test", "testmapping"},
    method = { RequestMethod.GET }
)
public String success() { return "success"; }
```

## [@RequestMapping ](/RequestMapping ) 派生注解 

如果不想设置 method 属性可以用以下注解来设置，**这些注解依然要设置 value 属性**：

1. 处理 get 请求：`@GetMapping`
2. 处理 post 请求：`@PostMapping`
3. 处理 put 请求：`@PutMapping`
4. 处理 delete 请求：`@DeleteMapping`

## params 注解参数

RequestMapping 注解的 params 属性通过请求的请求参数匹配请求映射

[@RequestMapping ](/RequestMapping ) 注解的 params 属性是一个字符串类型的数组，可以通过四种表达式设置请求参数和请求映射的匹配关系 

1. 
"param"：要求请求映射所匹配的请求必须携带 param 请求参数
```java
@RequestMapping(params = {"username"}) // 表示请求必须包含 username 参数，不限制参数值
```


2. 
"!param"：要求请求映射所匹配的请求不可以带有 param 请求参数
```java
@RequestMapping(params = {"!username"}) // 表示请求不允许带有 username 参数
```


3. 
"param = value"：要求请求映射所匹配的请求必须携带 param 请求参数且 parama == value
```java
@RequestMapping(params = {"username = admin"}) // 表示请求必须包含 username 参数且值必须为 admin
```


4. 
"param != value"：要求请求映射所匹配的请求必须携带 param 请求参数但 parama != value
```java
@RequestMapping(params = {"username != admin"}) // 表示请求必须包含 username 参数且值不可以为 admin
```



params 属性如果设置了多个属性值，则必须全部要匹配上

注意一点，如果要使用 thymeleaf 在 URL 上拼接参数的话，直接在 URL 后面用 `?` 拼接会有红线报错（实际使用不影响），如果想要消除红线，则通过括号的方式来追加参数：

```html
<a th:href="@{/test(username=123)}">通过 GET 方式请求并携带 params -> /test</a>
```

## headers注解属性

headers 注解使用的基本和 params 的使用方法一致，其规定请求头中必须包含、不包含某个 key，并且还可以规定某个 key 必须为一个特定的 value 或者不为一个特定的 value。

具体写法就不写了，但是要注意一点：如果 headers 有不匹配的地方，会报 404 的错误；value 不匹配也是 404；method 不匹配是 405；参数不匹配是 400

## ant 风格路径

ant 风格其实也就是模糊匹配，可以在 [@RequestMapping ](/RequestMapping ) 注解的 value 属性中设置一些模糊匹配的条件： 

1. ?：表示任意单个字符（特殊字符除外）
2. *：表示任意的 0 个或多个字符
3. **：表示任意的一层或多层目录

需要注意的一点是：在使用 ** 的时候，只能使用 /**/xxx 的方式

## 路径中的占位符（重要）

当通过 GET 方式添加参数的时候，原始方式是这样的：

```
/deleteUser?id=1
```

REST 方式就是这样的：

```
/deleteUser/1
```

通过直接来设置占位符：

```java
@RequestMapping("/testPath/{id}")  
public String testPath(@PathVariable("id") Integer id) {  
    System.out.println(id);  
    return "success";  
}
```

想要获取占位符的值的方式比较单一，通过给参数设置 `@PathVariable` 注解的方式来获取，其中注解的 value 值要设置的和占位符的 name 一致

注意：一旦设置占位符，那么请求路径中必须要包含参数，否则就会 404

里面还有个 `required` 属性可以设置

# SpringMVC 获取请求参数

有很多种方式可以获取到请求相关的参数，比如可以通过 `ServletAPI` 获取，还可以通过控制器方法的形参获取等等

## 通过 ServletAPI 获取

这种方式不常用，只做了解

```java
@RequestMapping("/testServletAPI")  
public String testServletAPI(HttpServletRequest req) {  
 String username = req.getParameter("username");  
 String password = req.getParameter("password");  
 System.out.println("username: " + username + "\npassword: " + password);  
  
 return "success";  
}
```

## 通过控制器方法的形参获取

这种方式只要保证控制器方法的形参的名字和请求参数的名字保持一致就可以获取到，比如说前端的一个请求里有一个叫 `username` 和一个叫 `password` 的参数，那么方法中只要保证形参名字也是 `username` 和 `password` 即可

```java
@RequestMapping("/testParam")  
public String testParam(String username, String password) {  
 System.out.println("username: " + username + "\npassword: " + password);  
 return "success";  
}
```

如果是多个参数的话，有两种方式获取：

1. 使用字符串类型形参：这种情况，框架会把多个参数用**逗号**把参数拼接起来
2. 使用字符串数组形参

## 请求参数和控制器方法形参映射

通过 `@RequestParam` 注解来实现映射，value 填写请求参数中的 name，一旦这么设置了之后，形参名字就随便起

```java
@RequestMapping("/testParam")  
public String testParam(  
 @RequestParam("user_name") String username,  
 String password,  
 String hobbies) {  
 System.out.println("username: " + username + "\npassword: " + password + "\nhobbies: " + hobbies);  
 return "success";  
}
```

里面还有个 `required` 属性可以设置，如果设置为 false 的时候，没有传过来的话就是 null，如果设置 true 但是没传就报 400；

还有 `defaultValue` 属性可以设置，如果请求中根本没有某个参数，或者有这个参数但是没有值，那么这个参数就会被设置为提前设定好的默认值

## 其他注解获取请求相关值

1. `@RequestHeader` 注解通过设置 value 可以获得请求头中特定的某条属性的值，这个注解也可以设置 `required` 属性和 `defaultValue` 属性
2. `@CookieValue` 可以获取到 `JSESSIONID` ，用法没什么区别

## 通过 POJO 封装请求参数

使用方法也很简单，就是让请求参数的名字与提前设计好的 Bean 的 field name 保持一致，然后在形参的地方直接用这个 Bean 来封装就可以了，大概写法如下：

```java
@RequestMapping("/testPOJO")  
public String testPOJO(User user) {  
 System.out.println(user);  
 return "success";  
}
```

前端会提交一个表单，表单里的数据有 `username`, `password`, `sex`, `age`, 和 `email`；User 对象里也有这五个 field

## 设置 CharacterEncodingFilter

在 web.xml 文件中添加：

```xml
<filter>  
 <filter-name>CharacterEncodingFilter</filter-name>  
 <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>  
 <init-param>  
 <param-name>encoding</param-name>  
 <param-value>UTF-8</param-value>  
 </init-param>  
</filter>
```

只要设置了 encoding 这个属性，它会强制要求 request 的参数用 UTF-8 编码，如果要求 response 也用的话，在多写一个 `init-param` 给那个 forceResponseEncoding 一个 true 就可以了（点进源码看）

# 域对象共享数据

有三（四）种不同作用范围的域对象，还有一个 JSP 不用了：

1. request 域对象：作用范围为一次请求，转发不会使 request 域对象失效
2. session 域对象：作用范围为一次会话，只要浏览器窗口打开没有关闭，则 session 一直一直存在，session 默认有效时长为 30 分钟
3. application 域对象：作用范围为整个 WEB 应用，只要服务器不关闭，这域对象一直有效

## 通过 ServletAPI 向域对象共享数据

使用方法为在控制器方法中获取到本次的 request，并且通过 set, get, remove 等方法对共享的数据进行操作（不推荐使用 ServletAPI）

```java
@RequestMapping("/testRequestByServletAPI")  
public String testRequestByServletAPI(HttpServletRequest req) {  
 req.setAttribute("testRequestScope", "hello ");  
  
 return "success";  
}
```

然后，通过 thymeleaf 获取到 request 中共享的数据：

```html
<!--/*@thymesVar id="testRequestScope" type="java.lang.String"*/-->
<p th:text="${testRequestScope}"></p><br>
```

## 通过 ModelAndView 实现

这个方式就是先创建 `ModelAndView` 对象，然后设置好 Model 数据，然后设置好视图名称，将其作为结果返回

```java
@RequestMapping("/testModelAndView")  
public ModelAndView testModelAndView() {  
 ModelAndView mav = new ModelAndView();  
 mav.addObject("testModelAndView", "Hello Model and View");  
 mav.setViewName("success");  
 return mav;  
}
```

在视图中获取数据的方式和之前一样

```html
<!--/*@thymesVar id="testModelAndView" type="java.lang.String"*/-->
<p th:text="${testModelAndView}"></p><br>
```

## 通过 Model 实现

其实这个最终也是要依靠 `ModelAndView` 来实现，先把 Model 创建出来，然后把需要的数据封装好，最后再 return 一个视图名称出去

```java
@RequestMapping("/testModel")  
public String testModel(Model model) {  
 model.addAttribute("testModel", "Hello Model");  
 return "success";  
}
```

在视图中获取（略）

## 通过 Map 实现

基本和 Model 一样，形参变成 Map，实现过程大同小异

```java
@RequestMapping("/testMap")  
public String testMap(Map<String, Object> map) {  
 map.put("testMap", "Hello Map");  
 return "success";  
}
```

## 通过 ModelMap 实现（略）

## 对以上三种方法的总结

实际上，这三个方法最后使用的实例对象都是同一个对象，并且这个对象实现了 Model 接口、实现了 Map 接口、同时也是 ModelMap 的子类

实际运行的示例的对象是：BindingAwareModelMap，这东西是继承的 ExtendedModelMap，而 ExtendedModelMap 在继承 ModelMap 的同时，还实现了 Model 接口

```java
public interface Model{}
public class ModelMap extends LinkedHashMap<String, Object> {}
public class ExtendedModelMap extends ModelMap implements Model {}
public class BindingAwareModelMap extends ExtendedModelMap {}
```

推荐使用 ModelAndView，因为后面这三个方法，最后都会封装进 ModelAndView 中，如果想研究源码的话，就去 DispatcherServlet 中去找

## 向 session 域对象共享数据

推荐使用原生 ServletAPI 来实现，在控制器方法上给一个 `HttpSession` 对象作为形参，来获取到 session 对象，并且设置共享数据：

```java
@RequestMapping("/testSession")
public String testSession(HttpSession session) {
	session.setAttribute("testSession", "Hello Session");
	return "success";
}
```

在视图中获取这个数据，只需要通过 `session.key` 的方式就可以获取到

## 向 application 域对象共享数据

这个也是通过 ServletAPI 先获取到 application 域对象，通过 `HttpSession` 先获取 session，然后在通过它的 `getServletContext()` 获取到：

```java
@RequestMapping("/testApplication")  
public String testApplication(HttpSession session) {  
 ServletContext application = session.getServletContext();  
 application.setAttribute("testApplication", "Hello Application");  
 return "success";  
}
```

# SpringMVC 视图

SpringMVC中的视图是View接口，视图的作用渲染数据，将模型 Model 中的数据展示给用户

SpringMVC视图的种类很多，默认有**转发视图**和**重定向视图**

当工程引入jstl的依赖，转发视图会自动转换为JstlView；

若使用的视图技术为Thymeleaf，在 SpringMVC 的配置文件中配置了 Thymeleaf 的视图解析器，由此视图解析器解析之后所得到的是 ThymeleafView

如果视图名称没有任何前缀和后缀的话，那么 SpringMVC 就会根据配置文件中配置的视图解析器去创造对应的视图

## 转发视图

（用的比较少）

SpringMVC 中默认的转发视图是 InternalResourceView，当控制器方法中所设置的视图名称以 `forward:` 为前缀的时候，就会创建 InternalResourceView 视图，此时的视图名称，不会被 SpringMVC 配置文件中所配置文件中所配置的视图解析器解析，而是会先去掉 forward 前缀，然后剩余的部分以转发的形式实现跳转

```java
@RequestMapping("testForward")
public String testForward() {
	return "forward:/testXXX";
}
```

## 重定向视图

SpringMVC中默认的重定向视图是RedirectView，当控制器方法中所设置的视图名称以"redirect:"为前缀时，创建RedirectView视图，此时的视图名称不会被SpringMVC配置文件中所配置的视图解析器解析

```java
```java
@RequestMapping("testRedirect")
public String testRedirect() {
	return "redirect:/testXXX";
}
```

# RESTFul

RESTFul 是一种风格，全程为 Representational State Transfer，用一图概括了

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/ZCSCNZ%7BI0%60XB%25MBW81YANTC.png#alt=)

## RESTFul 的实现

具体说，就是 HTTP 协议里面，四个表示操作方式的动词：GET、POST、PUT、DELETE。

它们分别对应四种基本操作：GET 用来获取资源，POST 用来新建资源，PUT 用来更新资源，DELETE 用来删除资源。

**REST 风格提倡 URL 地址使用统一的风格设计**，从前到后各个单词使用斜杠分开，不使用问号键值对方式携带请求参数，而是将要发送给服务器的数据作为 URL 地址的一部分，以保证整体风格的一致性。

| 操作 | 传统方式 | REST 风格 |
| --- | --- | --- |
| 查询操作 | getUserById?id=1 | user/1 --> GET 请求方式 |
| 保存操作 | saveUser | user --> POST 请求方式 |
| 删除操作 | deleteUser?id=1 | user/1 --> DELETE 请求方式 |
| 更新操作 | updateUser | user --> PUT 请求方式 |


然后在对应的控制器方法的 [@RequestMapping ](/RequestMapping ) 注解中，配置对应需要的参数；比如 `/user/{id}` 这个请求路径，设定死 GET 方式就是对应查询操作，DELETE 就是删除操作，具体例子就不写了

## 处理 PUT 和 DELETE 请求

想要处理 PUT 和 DELETE 以及更多的请求方式，需要通过配置拦截器来实现，要去 web.xml 文件中注册一个叫 `HiddenHttpMethodFilter` ，大致配置如下：

```xml
<filter>  
 <filter-name>HiddenHttpMethodFilter</filter-name>  
 <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>  
</filter>  
<filter-mapping>  
 <filter-name>HiddenHttpMethodFilter</filter-name>  
 <url-pattern>/*</url-pattern>  
</filter-mapping>
```

这个拦截器主要就是过滤出那些是 PUT 和 DELETE 和 PATCH 这样的请求，它会先判断请求首先是不是一个 POST 包装的，如果是的话记下来会检查一个叫 `_method` 的一个属性，如果这个属性是 PUT、DELETE、或 PATCH 的其中一个，它会 new 一个 `HttpMethodRequestWrapper` 来代替本次请求

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20211226050949.png#alt=)

## 模拟 PUT 和 DELETE 请求

在表单中，使用 type 为 hidden 的 input 来给一个叫 `_method` 的属性复制，值为请求方式的大写字符串：

```html
<!-- 用表单的形式 -->
<form th:action="@{/user}" method="post">  
 <input type="hidden" name="_method" value="PUT">
 用户名: <input type="text" name="username">  
 密码: <input type="password" name="password">  
 <input type="submit" value="修改">  
</form>
```

## 过滤器设置顺序

需要把编码过滤器写在请求过滤器的前面，因为在请求过滤器中，它有获取 `_method` 属性的操作，就算后面设置本次请求的编码也不生效，所以需要先让编码过滤器在最开始先设定好编码格式

`CharacterEncodingFilter` 设置在 `HiddenHttpMethodFilter` 的前面！

# RESTFul 案例

## 配置环境

1. 
pom 文件配置 JDK 和打包形式
```xml
<properties>  
    <maven.compiler.source>11</maven.compiler.source>  
    <maven.compiler.target>11</maven.compiler.target>  
</properties>  

<packaging>war</packaging>
```


2. 
添加依赖
```xml
<dependencies>
    <!-- SpringMVC -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>5.3.1</version>
    </dependency>
    <!-- 日志 -->
    <dependency>
        <groupId>ch.qos.logback</groupId>
        <artifactId>logback-classic</artifactId>
        <version>1.2.3</version>
    </dependency>
    <!-- ServletAPI -->
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>javax.servlet-api</artifactId>
        <version>3.1.0</version>
        <scope>provided</scope>
    </dependency>
    <!-- Spring5和Thymeleaf整合包 -->
    <dependency>
        <groupId>org.thymeleaf</groupId>
        <artifactId>thymeleaf-spring5</artifactId>
        <version>3.0.12.RELEASE</version>
    </dependency>

</dependencies>
```


3. 
通过 Project Structure 添加 Web 模块

4. 
配置 web.xml 文件
```xml
<!-- Web.xml -->
<!--Config Character Encoding Filter-->
<filter>
    <filter-name>CharacterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>UTF-8</param-value>
    </init-param>
    <init-param>
        <param-name>forceResponseEncoding</param-name>
        <param-value>true</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>CharacterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>

<!--Config Hidden Http Method Filter-->
<filter>
    <filter-name>HiddenHttpMethodFilter</filter-name>
    <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>HiddenHttpMethodFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>

<!--Config Dispatcher Servlet-->
<servlet>
    <servlet-name>DispatcherServlet</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:SpringMVC.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
</servlet>
<servlet-mapping>
    <servlet-name>DispatcherServlet</servlet-name>
    <url-pattern>/</url-pattern>
</servlet-mapping>
```


5. 
创建 SpringMVC.xml 文件进行配置
```xml
<!-- Config Annotation Scan -->
<context:component-scan base-package="com.demo" />

<!-- Config Thymeleaf View Resolver -->
<bean id="viewResolver" class="org.thymeleaf.spring5.view.ThymeleafViewResolver">
    <!-- Config Priority -->
    <property name="order" value="1"/>
    <property name="characterEncoding" value="UTF-8"/>
    <property name="templateEngine">
        <bean class="org.thymeleaf.spring5.SpringTemplateEngine">
            <property name="templateResolver">
                <bean class="org.thymeleaf.spring5.templateresolver.SpringResourceTemplateResolver">
                    <!-- Config View Prefix -->
                    <property name="prefix" value="/WEB-INF/templates/"/>
                    <!-- Config View Suffix -->
                    <property name="suffix" value=".html"/>
                    <property name="templateMode" value="HTML5"/>
                    <property name="characterEncoding" value="UTF-8"/>
                </bean>
            </property>
        </bean>
    </property>
</bean>
```


6. 
创建需要的 Bean 对象
```java
package com.demo.bean;

public class User {

    private Integer id;
    private String lastName;

    private String email;
    //1 male, 0 female
    private Integer gender;

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getLastName() {
        return lastName;
    }

    public void setLastName(String lastName) {
        this.lastName = lastName;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public Integer getGender() {
        return gender;
    }

    public void setGender(Integer gender) {
        this.gender = gender;
    }

    public User(Integer id, String lastName, String email, Integer gender) {
        super();
        this.id = id;
        this.lastName = lastName;
        this.email = email;
        this.gender = gender;
    }

    public User() {
    }
}
```


7. 
配置 UserController
```java
@Controller()
public class UserController {

    private UserDao userDao;

    @Autowired
    @Qualifier("userDao")
    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }
}
```



## 实现

已经上传到 Github 上了：

[https://github.com/BlckKn1fe/restful-demo](https://github.com/BlckKn1fe/restful-demo)

## 访问静态资源

由于 DispatcherServlet 处理的 URL 路径是 `/`，所以它会覆盖 Tomcat 本身默认的 DefaultServlet 处理的路径，这就导致请求静态资源的时候，这个请求被 DispatcherServlet 接收了，但是它没法处理静态资源。

为了能访问到静态资源，要在 Spring 配置文件中添加配置：

```
<mvc:default-servlet-handler />
<mvc:annotation-driven />
```

# HttpRequestConverter

HttpMessageConverter，报文信息转换器，将请求报文转换为Java对象，或将Java对象转换为响应报文

HttpMessageConverter提供了两个注解和两个类型：`@RequestBody`，`@ResponseBody`，`RequestEntity`，`ResponseEntity`

## [@RequestBody ](/RequestBody ) 

@RequestBody可以获取请求体，需要在控制器方法设置一个形参，使用@RequestBody进行标识，当前请求的请求体就会为当前注解所标识的形参赋值

```html
<form th:action="@{/testRequestBody}" method="post">
    用户名：<input type="text" name="username"><br>
    密码：<input type="password" name="password"><br>
    <input type="submit">
</form>
```

```java
@RequestMapping("/testRequestBody")
public String testRequestBody(@RequestBody String requestBody){
    System.out.println("requestBody:"+requestBody);
    return "success";
}
```

## RequestEntity

RequestEntity 封装请求报文的一种类型，需要在控制器方法的形参中设置该类型的形参，当前请求的请求报文就会赋值给该形参，可以通过 getHeaders() 获取请求头信息，通过 getBody() 获取请求体信息

```java
@RequestMapping("/testRequestEntity")
public String testRequestEntity(RequestEntity<String> requestEntity){
    System.out.println("requestHeader:"+requestEntity.getHeaders());
    System.out.println("requestBody:"+requestEntity.getBody());
    return "success";
}
```

## [@ResponseBody ](/ResponseBody ) 

@ResponseBody用于标识一个控制器方法，可以将该方法的返回值直接作为响应报文的响应体响应到浏览器

```java
@RequestMapping("/testResponseBody")
@ResponseBody
public String testResponseBody(){
    return "success";
}
```

结果：浏览器页面显示success

## SpringMVC处理json

导入jackson的依赖

```xml
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.12.1</version>
</dependency>
```

在SpringMVC的核心配置文件中开启mvc的注解驱动，此时在 HandlerAdaptor 中会自动装配一个消息转换器：MappingJackson2HttpMessageConverter，可以将响应到浏览器的Java对象转换为Json格式的字符串

```
<mvc:annotation-driven />
```

在处理器方法上使用@ResponseBody注解进行标识

将Java对象直接作为控制器方法的返回值返回，就会自动转换为Json格式的字符串

```java
@RequestMapping("/testResponseUser")
@ResponseBody
public User testResponseUser(){
    return new User(1001,"admin","123456",23,"男");
}
```

## [@RestController ](/RestController ) 注解 

[@RestController ](/RestController ) 注解是 springMVC 提供的一个复合注解，标识在 **控制器的类上**，就相当于为类添加了 [@Controller ](/Controller ) 注解，并且为其中的每个方法添加了[@ResponseBody ](/ResponseBody ) 注解 

## ResponseEntity

ResponseEntity用于控制器方法的返回值类型，该控制器方法的返回值就是响应到浏览器的响应报文

## 文件上传与下载
