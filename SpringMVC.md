

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



# @RequestMapping 注解



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



## @RequestMapping 派生注解

如果不想设置 method 属性可以用以下注解来设置，**这些注解依然要设置 value 属性**：

1. 处理 get 请求：`@GetMapping`
2. 处理 post 请求：`@PostMapping`
3. 处理 put 请求：`@PutMapping`
4. 处理 delete 请求：`@DeleteMapping`



## params 注解参数

 RequestMapping 注解的 params 属性通过请求的请求参数匹配请求映射
@RequestMapping 注解的 params 属性是一个字符串类型的数组，可以通过四种表达式设置请求参数和请求映射的匹配关系

1. "param"：要求请求映射所匹配的请求必须携带 param 请求参数

   ```java
   @RequestMapping(params = {"username"}) // 表示请求必须包含 username 参数，不限制参数值
   ```

2. "!param"：要求请求映射所匹配的请求不可以带有 param 请求参数

   ```java
   @RequestMapping(params = {"!username"}) // 表示请求不允许带有 username 参数
   ```

3. "param = value"：要求请求映射所匹配的请求必须携带 param 请求参数且 parama == value

   ```java
   @RequestMapping(params = {"username = admin"}) // 表示请求必须包含 username 参数且值必须为 admin
   ```

4. "param != value"：要求请求映射所匹配的请求必须携带 param 请求参数但 parama != value

   ```java
   @RequestMapping(params = {"username != admin"}) // 表示请求必须包含 username 参数且值不可以为 admin
   ```

   

params 属性如果设置了多个属性值，则必须全部要匹配上



