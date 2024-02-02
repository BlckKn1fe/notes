---


creation date: 2022-01-06 10:36:40

last modified: 2022-01-20 01:44:51

title: SpringBoot

categories:

- spring
- back-end

tags:
- springBoot

---

文档地址：[https://yuque.com/atguigu/springboot](https://yuque.com/atguigu/springboot)

本视频源码地址：[https://gitee.com/leifengyang/springboot2](https://gitee.com/leifengyang/springboot2)

SpringBoot 通过 pom 来引入各种依赖，并且这些依赖有它自己官方给你配置好的默认配置，不需要我们去手动操作。SpringBoot 通过 starter 为单位，为不同的开发场景提供不同 set of dependencies

# 容器

## [@Configuration ](/Configuration ) 注解 

可以使用一个配置类来代替配置文件，需要在类上添加 `@Configuration` 注解

```java
@Configuration
public class MyConfig { 
 	@Bean
    public User user01() { return new User("张三", 23); }
}
```

[@Configuration ](/Configuration ) 注解中，有一个叫 proxyBeanMethods 的属性，默认是 true，其作用就是为了保证 MyConfig 实例去调用声明的方法的时候，返回的都是单一实例，确保从容器内拿到的对象是同一个。 

当 proxyBeanMethods 设置为 false 的时候，是一种 lite 模式，此时容器中的组件之间不应该存在依赖关系；proxyBeanMethods 设置为 true 的时候，为 full 模式，可以满足组件之间的依赖关系

[@Bean ](/Bean ) 注解作用在方法体上，用来告诉 SpringBoot 方法声明的是一个容器中的组件对象 

## [@Import ](/Import ) 注解 

`@Import` 注解用来给容器中导入组件，可以作用在任意一个容器对象的类上，推荐写在配置类上面；注解的参数为一个 class 对象的数组，框架会调用该类的无参构造来创建对象在容器中，**默认组件的名字就是全类名**

## [@Conditional ](/Conditional ) 注解 

条件装配：满足Conditional指定的条件，则进行组件注入，它有很多派生注解

比如说，有一个 User 对象，它的 field 中有一个 Pet 类，如果说容器中没有存在 Pet 对象来给 User 设置的话，就可以通过 [@Conditional ](/Conditional ) 注解来规定在容器中是否要生成 User 对象 

```java
// 配置类中某个方法
@ConditionalOnBean(name = "cat")
@Bean
public User user01() {
    User user = new User("name", 20);
    // cat() 为配置类中配置的另外一个 Pet 对象
    user.setPet(cat());
    return user;
}
```

上面这段 demo 的意思就是，只有咋容器中存在 name 叫 cat 的的 Bean 对象的时候，才会创建 user01 这个对象

如果注解设置在 class 上的话，那么所有配置的 Bean 都需要满足设置的条件

## [@ImportResource ](/ImportResource ) 注解 

这个方式主要是用在配置类上，导入 xml 配置文件的，有的老项目如果全部改成用注解的方式会比较麻烦，可以通过在配置类上标注 [@ImportResource ](/ImportResource ) 的方式来加载 xml 配置文件 

注意：如果没有这个注解，SpringBoot 是不会自己去读取配置文件的

```java
@ImportResource("classpath:beans.xml")
```

## 配置绑定（properties）

@ConfigurationProperties 可以把配置文件中的值读取到 Java 对象中，这个注解中可以对 prefix 或 value 进行设置，假设有一个配置文件

```properties
mycar.brand=BYD
mycar.price=100000
```

这个 mycar 就是前缀，然后我们在写一个 Car 对象来装配这些配置信息

```java
@Component("car")
@ConfigurationProperties("mycar")
public class Car {

    private String brand;
    private Integer price;
	
    // 略...
}
```

此时，brand 和 price 就会对应上，然后装配上

还有一种方式可以达到一样的效果

通过 @EnableConfigurationProperties + @ConfigurationProperties 的方式

## 自动配置原理

@SpringBootApplication 注解相当于：

- @SpringBootConfiguration
- @EnableAutoConfiguration
- [@ComponentScan ](/ComponentScan ) 

三个注解加在一起，其中 @EnableAutoConfiguration 中还有一个@AutoConfigurationPackage，它的作用是读到程序入口，然后获取入口所在包下的所有组件，并且将他们全部注册到容器中

@EnableAutoConfiguration 的另外一个注解是 @Import(AutoConfigurationImportSelector.class)，它归根结底是通过底层加载配置文件然后通过 SpringFactories 去生成需要的组件，配置文件的位置在 META-INFO/spring.factories 里，主要在 spring-boot-autoconfigure-version.RELEASE.jar 中

实际运行的时候，它并不会把 spring.factories 中全部的组件都加载进去，它会按需、按照场景加载，会根据 [@Conditional ](/Conditional ) 注解来限制，比如说某一个包中的某个组件是否加载，取决于实际工作场景中的 class 加载的全不全，如果不全就说明不需要加载这个组件 

加载顺序大概就是先从 XXXAutoConfiguration 中去根据条件往容器中添加满足条件的组件，然后这些组件的具体配置在从 XXXProperties 对象中去拿值，而 XXXProperties 又会去 application.properties 去找值

# 开发技巧

## Lombok

引入 Lombok 依赖

```xml
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
</dependency>
```

通过 Lombok 可以直接在 Bean 对象上直接添加

- [@AllArgsConstructor ](/AllArgsConstructor ) 
- [@NoArgsConstructor ](/NoArgsConstructor ) 
- [@Data ](/Data ) 

这些直接，它会自动的添加 set、get、构造函数、toString 等方法

并且，可以直接在任意类上添加 Logger 的相关注解，在类中可以直接通过 log.XXX 的方式使用 logger 相关的功能

更多其他使用参考官网：

[https://projectlombok.org/features/all](https://projectlombok.org/features/all)

## Dev-Tolls

感觉没啥调用，可以重新 reload 项目，还有其他功能，具体看官网吧

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <optional>true</optional>
</dependency>
```

Ctrl + F9 进行 reload

## Spring Initializer

就是 IDEA 自带的那个，可以提前设置好很多 starter，IDEA 也会帮我们自动创建好项目目录结构等等

# 配置文件

## yaml 形式

除了用 properties 的方式，SpringBoot 还支持用 yaml 的格式来配置 SpringBoot

它的基本语法：

1. 键值对形式，`key: value`，注意冒号后面有空格
2. 大小写敏感
3. 缩进表示层级关系
4. 缩进多少不重要，只要相同层级元素左对齐
5. "#" 来表示注释

表示对象：

```yaml
# 行内写法
k: {k1: v1, k2: v2, k3: v3}
# 或者
k:
	k1: v1
	k2: v2
	k3: v3
```

表示数组：

```yaml
# 行内写法
k: [v1, v2, v3]
# 或者
k:
	- v1
	- v2
	- v3
```

注意一点，单引号之间的内容会将特殊字符直接输出出来，会被转义，但是双引号之间的内容特殊字符会生效，不被转义。比如单引号的 `\n` ，控制台打印是 `\n`前端收到的是 `\\n`；而双引号的话，控制台会直接幻皇，前端收到的是 `\n`

# Web

## 静态资源访问

静态资源目录可以在类路径下，起名为 `/static`, `/public`, `/reources` 等。默认情况下，在浏览器中直接访问根路径后接这些资源的文件名就可以访问到这些资源

由于静态资源映射的 pattern 默认是 `/**`，所以如果我们没有手动配置 controller 或者 controller 处理不了，他就默认去直接找对应的静态资源了

如果想要修改 pattern 的话，需要在配置文件中修改 `spring.mvc.static-path-pattern` ，一般推荐设置为 `/res/**`

所以最终浏览器请求路径为：

```
项目 + static-path-pattern + 静态资源名
```

可以手动修改静态资源的路径（覆盖默认的），修改 `spring.resources.static-locations` 这一条参数（注意是个数组）

注意：这个会影响到静态资源访问

提一嘴 webjar，很多 JS，CSS 文件可以打包进 jar 包，然后装配到项目中，它自己会配置好这些文件的静态资源访问路径。

[https://www.webjars.org/](https://www.webjars.org/)

比如引入 jQuery 的 jar，那么当项目运行之后，在 `webjars/jquery/x.x.x/jquery.js` 就可以请求的到

## 欢迎页和 facicon

以 `index.html` 和 `favicon.ico` 命名的文件，如果放在静态资源目录下的话，框架会自动找到这些文件并且配置上。

## REST 相关

原理省略，之前学过了

如果想要修改 filter 拦截的 `_method` 的属性名，将其修改成自己喜欢的名字的话，可以通过配置类来实现，首先创建一个配置类，自己去配置 HiddenHttpMethodFilter

```java
@Configuration(proxyBeanMethods = false)
public class WebConfig {
    
    @Bean
    public HiddenHttpMethodFilter hiddenHttpMethodFilter() {
        HiddenHttpMethodFilter hiddenHttpMethodFilter = new HiddenHttpMethodFilter();
        hiddenHttpMethodFilter.setMethodParam("_preferred");
        return hiddenHttpMethodFilter;
    }

}
```

## 常用参数、注解

1. 
@PathVariable()：获取占位符中的参数数值
```java
@RequestMapping("/car/{id}")
public Map<String, Object> getCar(@PathVariable("id") Integer id) {
    return null;
}
```


当然，上述方式只是单独拿到一个个参数；还有一个方式是被该注解修饰的 `Map<K,V>` 集合，里面的 key 和 value 就会对应占位符里的内容

2. 
@RequestHeader(): 获取请求头中的内容
```java
@RequestHeader("User-Agent") String userAgent
```


同样，也可以传入一个 Map、MultiValueMap 或 HttpHeaders，来获取全部的请求头内容

3. 
@RequestParam(): 通常这个都是用来获取 GET 请求（用 `?` 拼装的）中的参数用的，里面写 parameter name，通常对应表单中 input 标签的 name，当然它也可以获取集合参数
```java
@RequestParam("age") Integer age;
@RequestParam("interests") List<String> interests
```


当然，它也可以写 Map 集合来作为形参获取到全部参数

4. 
@CookieValue(): 获取 Cookie 中的某一项的值，也可以使用 Cookie 来接

5. 
@RequestBody: 可以用 String 来接，获取到全部请求体的内容；如果想通过对象的方式获取请求体的话，不使用这个注解，然后使用 RequestEntity 作为形参

6. 
@RequestAttribute: 获取 request 域对象中的某个属性，一般是在 forward 之后的那个控制方法中通过这个注解获取到同一次请求中的 request 域对象中的某个属性

7. 
@MatrixVariable: 获取矩阵变量，参数在 URL 中用分号分开，可以当 cookie 被禁用的时候，使用这种方式来获取到 cookie 值（这个不展开说了）

**注意：SpringBoot 默认禁用了获取矩阵变量的功能**


# 数据访问

## 配置数据源

先导入 JDBC 场景

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jdbc</artifactId>
</dependency>
```

然后导入数据库驱动，注意版本匹配：

1. 可以直接引入想要的版本的坐标
2. 在 pom 文件的 properties 标签中，添加 <mysql.version> 来设置

```xml
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.49</version>
</dependency>
```

然后配置 yaml 文件：

```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/user_db?useSSL=false
    username: 
    password: 
    driver-class-name: com.mysql.jdbc.Driver
```

此时，我们已经可以使用 JdbcTemplate 了

## 配置 Druid

先说一遍手动配置，引入依赖：

```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.2.8</version>
</dependency>
```

然后创建配置类：

```java
@Configuration
public class DataSourceConfig {

    // 这个会优于默认的 Hikari 创建并替换
    @Bean
    @ConfigurationProperties("spring.datasource")
    public DataSource dataSource() throws SQLException {
        DruidDataSource druidDataSource = new DruidDataSource();
        // 开启统计功能
        druidDataSource.setFilters("stat");
        return druidDataSource;
    }

    // 开启 Druid 监控页面
    @Bean
    public ServletRegistrationBean<StatViewServlet> statViewServlet() {
        StatViewServlet svs = new StatViewServlet();
        return new ServletRegistrationBean<StatViewServlet>(svs, "/druid/*");
    }

}
```

有很多设置都需要手动一个个加入，一个个开启，麻烦，所以还可以引入官方给的 starter

```xml
<dependency>
   <groupId>com.alibaba</groupId>
   <artifactId>druid-spring-boot-starter</artifactId>
   <version>1.1.17</version>
</dependency>
```

之后在 spring.datasource 配置下，会多出一个 druid 的配置项：

```yaml
spring:
  datasource:
	# url, username, password, ....
    druid:
      filters: stat
      stat-view-servlet:
        enabled: true
        login-username: admin
        login-password: admin

      web-stat-filter:
        enabled: true
```

更多配置参考官方文档：

[https://github.com/alibaba/druid/tree/master/druid-spring-boot-starter](https://github.com/alibaba/druid/tree/master/druid-spring-boot-starter)

[https://github.com/alibaba/druid/wiki/常见问题](https://github.com/alibaba/druid/wiki/%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98)

## 配置 MyBatis

```xml
<dependency>
	<groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.2.1</version>
</dependency>
```

之后配置一下 mapper 文件的路径，开启驼峰命名

```yaml
mybatis:
  mapper-locations: classpath:mybatis/mapper/*xml
  configuration:
    map-underscore-to-camel-case: true
```

之后创建对应 Bean 对象的 Mapper

```java
@Mapper
public interface CityMapper {

    @Select("select * from city where id = #{id}")
    public City getCityById(Long id);

}
```

通过注解的方式类来实现 SQL 语句，如果是很复杂的，可以写一个对应的 mapper 的 xml 文件，xml 文件大概格式如下：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!-- namespace 写对应 mapper 接口的全类名 -->
<mapper namespace="">
	<!-- id 为 mapper 中的方法名 -->
    <select id="" resultType="">
        
    </select>
</mapper>
```

## MyBatis-Plus
