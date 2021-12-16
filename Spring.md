# 快速开始

创建一个空 Maven 项目，并且导入一下坐标

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>5.0.5.RELEASE</version>
    </dependency>
</dependencies>
```



创建对应的 Dao 接口和对应的实现

```java
// UserDao
public interface UserDao {

    public void save();

}

// UserDaoImpl
public class UserDaoImpl implements UserDao {
    @Override
    public void save() {
        System.out.println("Save running...");
    }
}
```



创建 Spring 的配置文件（ID为Spring获取对象所需的标识符）

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    
    <bean id="userDao" class="com.xxx.dao.impl.UserDaoImpl"/>
    
</beans>
```



最后在 Main 里通过框架来创建想要的对象（通过 getBean("id")的方式获取对象）

```java
public class UserDaoDemo {

    public static void main(String[] args) {
        ApplicationContext app = new ClassPathXmlApplicationContext("applicationContext.xml");

        UserDao userDao = (UserDao) app.getBean("userDao");
        userDao.save();

    }

}
```





# IOC-Bean管理



![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20211011031650.png)



## Bean 标签基本配置

用于配置对象，并且交给 Spring 来创建，默认情况下调用类的空参构造函数

两个基本属性：

- id: Bean 实例在 Spring 容器中的唯一标识
- class: Bean 的全限定名

默认情况下，从容器中获取到的对象是单例的，除非在配置文件中修改 scope 属性

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20211010004755.png)



不同 scope 的配置，Bean 创建时机不一样：

1. singleton: **加载完配置文件之后会创建对象**，并添加到容器中
2. prototype: 每 getBean 一次，创建一次对象



## Bean 生活周期配置*

有两个基本的属性可以配置（还是配置在 bean 标签上）：

- init-method: 指定类中的初始化方法名称
- destroy-method: 指定类中销毁方法名称

```xml
<bean id="userDao" class="com.xxx.dao.impl.UserDaoImpl" scope="singleton" init-method="init" destroy-method="destroy" />
```

其中的 init 和 destroy 方法是声明在 UserDaoImpl 中的

通过 Spring 创建对象，每个对象会经历以下几个步骤：

1. 执行无参构造创建 Bean 实例
2. 调用 set 方法设置属性值
3. 执行初始化方法
4. 获取到对象
5. 执行销毁方法 (app.close())







## Bean 实例化三种方法

1. 无参构造方法实例化

2. 工厂静态方法实例化

   创建 `factory.StaticFacoty` 类，并且在类中写入一个静态方法

   ```java
   public class StaticFactory {
   
       public static UserDao getUserDao() {
           return new UserDaoImpl();
       }
   
   }
   ```

   修改配置文件（主要是配置 factory-method 这个属性）

   ```xml
   <bean id="userDao" class="com.xxx.factory.StaticFactory" factory-method="getUserDao"/>
   ```

   

3. 工厂实例方法实例化

   基本和工厂静态方法实例化很接近，但是 get 方法没有 static 修饰，主要在配置文件上

   ```xml
   <bean id="factory" class="com.xxx.factory.DynamicFactory"/>
   <bean id="userDao" factory-bean="factory" factory-method="getUserDao"/>
   ```

   



## Bean 依赖注入

当一个 Bean 对象里，有一些 filed 需要让 Spring 帮我们去设置进去的话，比如 Service 对象里想有一个 Dao 对象来作为成员变量，就可以通过 Spring 依赖注入来设置它

有两种方式：

1. 通过 Set 方法：

   在对应的 Bean 对象中，声明需要注入的对象，然后生成对应的 set 方法，最后修改配置文件

   ```xml
   <bean id="userDao" class="com.xxx.dao.impl.UserDaoImpl"/>
   <bean id="userService" class="com.xxx.service.impl.UserServiceImpl">
       <!--下面这个 name 需要和 UserServiceImpl 的 set 方法中的名字对应，且首字母小写-->
       <property name="userDao" ref="userDao"/>
   </bean>
   ```

   set 还可以通过命名空间的方式来简化配置内容

   首先在 Spring 配置文件开头，加入新的命名空间

   ```xml
   xmlns:p="http://www.springframework.org/schema/p"
   ```

   然后修改对于 UserService 的配置

   ```xml
   <bean id="userService"
             class="com.xxx.service.impl.UserServiceImpl"
             p:userDao-ref="userDao">
       </bean>
   ```

   

2. 通过构造函数

   首先在对应的 Bean 对象中，声明好构造函数，然后修改配置文件

   ```xml
   <bean id="userService" class="com.XXX.service.impl.UserServiceImpl">
       <!--下面这个 name 对应的是构造函数的参数名-->
       <constructor-arg name="userDao" ref="userDao"/>
   </bean>
   ```

   

Bean 的依赖注入的数据类型有三种：

1. 普通数据类型：name 后面设置 value 属性

2. 引用数据类型：上面的例子都是引用数据类型

3. 集合数据类型：针对不同的类型，要在每个 property 的标签体内进行单独的设置

   假设在 UserDao 中添加三个 fields，分别为 List，Map，和 properties 类型，然后修改配置文件

   ```xml
   <!--集合注入-->
   <bean id="userDao" class="com.guanyu.dao.impl.UserDaoImpl">
       <property name="arr">
           <list>
               <value>aaa</value>
               <value>bbb</value>
               <value>ccc</value>
           </list>
       </property>
   
       <property name="userMap">
           <map>
               <entry key="user1" value-ref="user1"/>
               <entry key="user2" value-ref="user2"/>
           </map>
       </property>
   
       <property name="prop">
           <props>
               <prop key="p1">ppp1</prop>
               <prop key="p2">ppp2</prop>
               <prop key="p3">ppp3</prop>
           </props>
       </property>
   </bean>
   ```




## 抽取集合

如果配一个 List 的内容在一个对象下，内容可能会很多，比较乱，可以通过 util 把 List 的内容都抽出去

首先添加对应命名空间和约束：

```xml
xmlns:util="http://www.springframework.org/schema/util"

http://www.springframework.org/schema/util
http://www.springframework.org/schema/util/spring-util.xsd
```



然后创建提取集合出来：

```xml
<util:list id="list">
	<value>AAA</value>
    <value>BBB</value>
    <value>CCC</value>
</util:list>
```



## 后置处理器

后置处理器可以对整个配置文件中要生成的 Bean 对象进行处理，其运行在执行初始化方法之前和之后。需要实现 BeanPostProcessor 借口：

```java
public class MyBeanPost implements BeanPostProcessor {

    @Override
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("before...");
        return bean;
    }

    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("after...");
        return bean;
    }
}
```



然后在配置文件中创建这个 Bean 对象即可





## 引入其他配置文件（分模块开发）

实际开发中，Spring的配置内容非常多，这就导致Spring配置很繁杂且体积很大，所以可以将部分配置拆解到其他配置文件中，而在Spring主配置文件通过import标签进行加载

```xml
<!-- applicationContext.xml 文件中 -->
<import resource="applicationContext-user.xml"/>
```



# ApplicationContext 

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20211011034153.png)



## ApplicationContext 的实现类

1. ClassPathXmlApplicationContext：从类的根路径下加载配置文件（推荐）
2. FileSystemXMLApplicationContext：从磁盘路径上加载配置文件，配置文件可以在磁盘任意位置
3. AnnotationConfigApplicationContext：使用注解配置容器对象时，需要使用此类来创建 Spring 容器，用来读取注解



## getBean() 方法使用

主要有两个常用的

1. ```java
   public Object getBean(String name) throw BeansException {};dd
   ```

   这种方式允许配置文件中出现多个同类型的 Bean 对象，用不同的 ID 进行获取

2. ```java
   public <T> T getBean(Class<T> requiredType) throws BeansException {}
   ```

   这种方式不允许配置文件中出现多个同类型的  Bean 对象

   





# 注解开发



## 开启路径扫描

只有开启路径扫描，Spring 才会去指定路径扫描文件是否带有注解

```xml
<!--配置组件扫描-->
<context:component-scan base-package="com.XXX" />
```



还可以添加子标签来设置 include 或 exclude

```xml
<context:component-scan base-package="com.XXX">
	<context:include type="annotation" expression="全类名" />
</context:component-scan>
```





## Spring 原始注解注入

![image-20211031162406100](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/image-20211031162406100.png)

在使用注解开发的时候，只需要在对应的类上加上对应注解即可，第二个到第四个注解都是具有语义化，实际与 Component 一样

示例：

```java
@Component("userService")
public class UserServiceImpl implements UserService {
	
    // 下面 Autowired 的建议添加到 Constructor 或者 set 方法上面
    // 这种方式注入不推荐
    @Autowired
    @Qualifier("userDao")
    private UserDao userDao;

    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }

    public UserServiceImpl() {}
    public UserServiceImpl(UserDao userDao) {
        this.userDao = userDao;
    }

    @Override
    public void save() {
        userDao.save();
    }
}
```

注意事项：

1. 如果省略 Qualifier 的话，Autowired 会让程序从 Spring 容器中通过数据类型进行匹配，然后注入（但是需要容器中只有一个该类型的对象）
2. Qualifier 则是通过 id 的方式从容器中进行选择，解决有多个同类型对象的问题，搭配 Autowired 使用
3. Resource 注解也可以拿来注入，起需要给 name 属性赋一个对应的 bean 的 id，其效果和上面两个结合起来一样，但是不推荐使用，其属于 javax 包下



@value注解用于原始数据类型（这里包括 String）的注入



## 完全注解开发

完全注解即不需要 xml 配置文件，取而代之的是配置类

```java
// SpringConfig.java
@Configuration
@ComponentScan(basePackages = {"com.XXX"})
public class SpringConfig {
}
```





# AOP



## AOP相关术语

1. 连接点

   类中理论上可被增强的方法，称为连接点

2. 切入点

   实际类中被真正增强的方法，称为切入点

3. 通知（增强）

   - 实际增强的（新添加的逻辑）部分，就称为增强或通知

   - 通知分为以下五种：
     - 前置通知
     - 后置通知
     - 环绕通知
     - 异常通知
     - 最终通知

4. 切面

   切面是一个动作，它就是把通知应用到切入点的过程



## AOP 操作

Spring 框架一般是基于 AspectJ 实现 AOP 操作； AspectJ 不是 Spring 的组成部分，是一个独立的 AOP 框架，一般是把 Spring 框架和 Spring 框架一起使用，进行 AOP 操作

实现 AOP 操作可以通过以下两种方式：

1. 基于 xml 配置文件实现
2. 基于注解方式实现（一般使用这种）



打开注解扫描和代理对象生成，先配置命名空间：

```xml
<beans xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/aop
       http://www.springframework.org/schema/aop/spring-aop.xsd">
```



```xml
<!--开启注解扫描-->
<context:component-scan base-package="aopanno" />
<!--开启aspectJ生成代理对象-->
<aop:aspectj-autoproxy />
```





## 注解操作

创建代理对象类，并且为其添加相应注解

（假设有一个叫 User 的类，且该类存在一个叫 add 的方法）

```java
@Component
@Aspect  // 生成代理对象
public class UserProxy {
    
    @Before("execution(* aopanno.User.add(..))")
    public void before() {
        System.out.println("Before...");
    }

    @After("execution(* aopanno.User.add(..))")
    public void after() {
        System.out.println("after...");
    }

    @Around("execution(* aopanno.User.add(..))")
    public void around(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
        System.out.println("around before...");
        proceedingJoinPoint.proceed();
        System.out.println("around after...");
    }

    @AfterReturning("execution(* aopanno.User.add(..))")
    public void afterReturning() {
        System.out.println("afterReturning...");
    }

    @AfterThrowing("execution(* aopanno.User.add(..))")
    public void afterThrowing() {
        System.out.println("afterThrowing...");
    }
}

```



五种通知无异常情况下执行顺序如下：

- Around Before
- Before
- Method
- Around After
- After - 最终通知
- After Returning - 后置通知

若有异常，则如下顺序：

- Around Before
- Before
- (Method)
- After
- After Throwing



## 注解操作：抽取切入点

切入点抽取：

创建一个 method 并且添加上 @Pointcut 注解，然后把切入点配置给这个注解，后续对该切入点设置通知，只需要调用该方法即可获取到切入点

```java
// 抽取切入点
@Pointcut("execution(* aopanno.User.add(..))")
public void addPointCut(){};

// 使用切入点
@Before("addPointCut()")
public void before() {
    System.out.println("Before...");
}
```



## 注解操作：优先级

当有多个增强类对目标类进行增强的话，可以设置优先级，来决定谁先执行谁后执行，通过 @Order 注解来实现

```java
@Component
@Aspect
@Order(1)
public class PersonProxy {}
```



## 注解操作：完全注解开发（补充）

在 SpringConfig 这个 class 中，添加 @EnableAspectJAutoProxy 这一条注解，并且将 proxyTargetClass 设置为 true

```java
@Configuration
@ComponentScan(basePackages = {"com.XXX"})
@EnablkeAspectJAutoProxy
public class SpringConfig{}
```



# SpringTemplate



## 数据库配置文件

导入连接池和数据库链接包

```xml
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.32</version>
</dependency>

<dependency>
    <groupId>c3p0</groupId>
    <artifactId>c3p0</artifactId>
    <version>0.9.1.2</version>
</dependency>

<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.1.20</version>
</dependency>
```

创建配置文件

```properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/test
jdbc.username=root
jdbc.password=123456
```



## Spring配置数据源

以上连接池都有无参构造，并且都是通过 set 方法来配置关键信息，所以可以通过依赖注入的方式来让 Spring 生成对应的连接池对象，并且存放在 Spring 容器中

添加到配置文件中：

```xml
<!--配置数据源-->
<context:property-placeholder location="classpath:jdbc.properties" />
<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource" destroy-method="close">
    <property name="driverClassName" value="${jdbc.driver}" />
    <property name="url" value="${jdbc.url}" />
    <property name="username" value="${jdbc.username}" />
    <property name="password" value="${jdbc.password}" />
</bean>

<!--JdbcTemplate 对象配置-->
<bean
      id="jdbcTemplate"
      class="org.springframework.jdbc.core.JdbcTemplate"
      p:dataSource-ref="dataSource"/>
```

## 添加操作 



在 DaoImpl 中执行添加操作，通过 jdbcTemplate 中的 update 方法实现，假设我们有一个 BookDaoImpl 的方法：

```java
@Repository("bookDao")
public class BookDaoImpl implements BookDao {

    @Autowired
    @Qualifier("jdbcTemplate")
    private JdbcTemplate jdbcTemplate;

    @Override
    public void add(Book book) {
        String sql = "insert into t_book values(?,?,?)";
        Object[] args = {book.getBookId(), book.getBookName(), book.getBookStatus()};
        jdbcTemplate.update(sql, args);
    }
}
```



其中 update 方法的第一个参数是 sql 语句，第二个参数为一个可变参数，也可以是一个参数数组，以此替换到sql 语句的 "?" 



## 修改和删除（略）

和添加基本一样



## 查询操作（具体值示例）

通过 jdbcTemplace 的 queryForObject 来实现，比如想要查询一个表中一共有多少条数据：

```java
public int selectCount() {
    String sql = "select count(*) from t_book";
    Integer count = jdbcTemplate.queryForObject(sql, Integer.class);
    return count;
}
```



## 查询操作（对象示例）

和查值基本一样，但是需要多传入一个 Mapper 参数，以此让程度通过 Bean 的 set 方法来生成对象：

```java
public Book findByID(String id) {
    String sql = "select * from t_book where book_id = ?";
    Book book = jdbcTemplate.queryForObject(sql, new BeanPropertyRowMapper<Book>(Book.class), id);
    return book;
}
```

Mapper 调用 set 方法的时候，是去找 Bean 方法名中能对应上的，而不是去找 filed 的名字



## 查询操作（集合示例）

比如查询图书列表、分页，此时会从数据库拿到一个集合，同样也是通过 queryForObject 来获取：

```java
public List<Book> findAll() {
    String sql = "select * from t_book";
    List<Book> books = jdbcTemplate.query(sql, new BeanPropertyRowMapper<Book>(Book.class));
    return books;
}
```



## 批量操作（添加示例）

同时操作表中的多条记录，比如批量添加，可以通过 jdbcTemplate 的 **batchUpdate** 来实现：

```java
batchUpdate(String sql, List<Object[]> batchArgs)
```

第一个参数为 sql 语句，第二个参数为 sql 语句所需参数

```java
// DaoImpl
public void batchAdd(List<Object[]> batchArgs) {
    String sql = "insert into t_book values(?, ?, ?)";
    int[] ints = jdbcTemplate.batchUpdate(sql, batchArgs);
    System.out.println(Arrays.toString(ints));
}

// Test
@Test
public void batchAddTest() {
    ApplicationContext app = new ClassPathXmlApplicationContext("ApplicationContextConfig.xml");
    BookService service = app.getBean("bookService", BookService.class);

    Object[] b1 = {"3", "C++", "A1"};
    Object[] b2 = {"4", "C#", "B1"};
    Object[] b3 = {"5", "Go", "C1"};

    ArrayList<Object[]> args = new ArrayList<>();
    args.add(b1);
    args.add(b2);
    args.add(b3);

    service.batchAdd(args);
}

```



## 批量操作（修改/删除）略



# 事务

事务是数据库操作最基本的单元，逻辑上的一组操作，要么全部成功；如果有一个失败则全部都失败。典型的一个使用场景就是银行转账。

事务有四个特性（ACID）

1. 原子性（Atomic）：一个 set 的逻辑不可分割，要么都成功，要么都失败
2. 一致性（Consistency）：操作前后总量不变
3. 隔离性
4. 持久性



## 注解式事务管理

基于 AOP 实现，想要开启注解式事务管理需要以下几个步骤：

1. 在 XML 配置文件中创建事务管理器

   ```xml
   <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
   	<property name="dataSource" ref="dataSource" />ty>
   </bean>
   ```

   

2. 然后开启事务注解，开启事务注解之前需要引入 tx 命名空间

   ```xml
   <!-- 设置命名空间 -->
   xmlns:tx="http://www.springframework.org/schema/tx"
   http://www.springframework.org/schema/tx
   http://www.springframework.org/schema/tx/spring-tx.xsd"
   
   <!-- 开启事务注解 -->
   <tx:annotation-driven transaction-manager="transactionManager" />
   ```

   

3. 将 @Transactional 注解添加到 Class 上面或者加到 Method 上面；加到 Class 上面意思是该 Class 内的所有 methods 都会开启事务；加到 Method 上面的话，只给单独的 Method 开启事务



## 注解参数（传播级别）

当事务方法（对数据库有修改）被调用时，需要对其进行管理，规定是否开启事务，可以通过设置 @Transactional 中的 propagation 属性来做设置

Spring 框架有 7 种传播行为：

1. REQUIRED（默认）：当 A 事务方法在事务当中，并且调用了 B 事务方法，则 B 会继续在 A 的事务中运行；若 A 不在事务当中，则 B 开启一个新的事务，并在其中运行
2. REQUIRED_NEW：无论 A 事务方法是否运行在事务中，当调用 B 的时候，都会开启一个新的事务，并且在其中运行

![image-20211124042508307](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/image-20211124042508307.png)

使用示例：

```java
@Transactional(propagation = Propagation.REQUIRED)
```



## 注解参数（隔离级别）

隔离级别主要是用来解决并发的时候出现的一些问题：脏读、不可重复读和幻读

脏读：一个未提交的事务读取到了另外一个未提交的事务的数据

不可重复读：一个未提交的事务读取到了另外一个提交的事务修改后的数据，**前后多次读取，数据内容不一致**

幻读：一个未提交的事务读取到了另外一个提交的事务新添加的数据，**前后多次读取，内容总量不一致**



隔离级别设置：

|                      | 脏读 | 不可重复读 | 幻读 |
| :------------------: | :--: | :--------: | :--: |
| **READ UNCOMMITTED** |  有  |     有     |  有  |
|  **READ COMMITTED**  | 没有 |     有     |  有  |
| **REPEATABLE READ**  | 没有 |    没有    |  有  |
|   **SERIALIZABLE**   | 没有 |    没有    | 没有 |



使用示例：

```java
@Transactional(isolation = Isolation.REPEATABLE_READ)
```



## 注解参数（其他参数）

1. **timeout**：事务需要在一定时间内提交，否则回滚，默认值 -1，单位秒
2. **readOnly**：是否只读，默认为 false；为 true 时，只能查询，反之增删改查都可以
3. rollbackFor：设置为哪些异常进行回滚
4. noRollbackFor：设置不为哪些异常进行



# 其他功能



## 整合日志框架

使用 Log4j ，先添加依赖：

```xml
<dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-api</artifactId>
    <version>2.14.1</version>
</dependency>

<dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-core</artifactId>
    <version>2.14.1</version>
</dependency>
```



然后创建配置文件 `log4j2.xml`

> 具体其他配置方式参考 https://logging.apache.org/log4j/2.x/manual/configuration.html



## 整合 JUnit4/5

之前写单元测试需要每一次都获取 ApplicationContext 很麻烦，可以配合注解的方式直接给单元测试类配好容器，然后在通过 @Autowired 和 @Qualifier 直接拿到想要的 Bean

首先引入 Spring 的 test 包：

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-test</artifactId>
    <version>5.2.5.RELEASE</version>
</dependency>
```



如果是整合 JUnit 4 的话，在单元测试类上面添加两个注解：

```java
@RunWith(SpringJUnit4ClassRunner.class)  // 指定单元测试框架
@ContextConfiguration("classpath:ApplicationContextConfig.xml")  // 配置 Spring 容器的配置文件
public class JTest {}
```

然后想要测试某个 Bean 对象的话，直接声明 field 就可以拿到了

```java
public class JTest {
    @Autowired
    private UserService userService
}
```



如果是整合 JUnit 5 的话，先引入 JUnit 5 的包，并且给单元测试类添加 @SpringJUnitConfig 注解：

```java
@SpringJUnitConfig(locations = "classpath:ApplicationContextConfig.xml")
public class JTest {}
```

并且 @Test 注解要使用 JUnit 5 包里的



# Spring WebFlux

> 官方文档：
>
> https://docs.spring.io/spring-framework/docs/current/reference/html/web-reactive.html

简单来说它和 Spring MVC 是一个很类似的框架，都是用来操作 Web 请求的，然后 WebFlux 是异步，非阻塞，响应式，支持函数式的框架，WebFlux 更轻量级，得益于异步操作可以有更大的吞吐量。另外 WebFlux 基于 Reactor 框架，而 Reactor 框架基于 Flow（JDK 9），其实际是一种 Publisher/Subscriber 的模型

 

## Reactor 实现响应式

Reactor 满足 Reactive 规范，其有两个核心类，Mono 和 Flux，这两个类都实现了 Publisher 接口。Flux 作为发布者对象，可以返回 N 个元素；而 Mono 只能返回 0 或者 1 个对象。他们两个都是数据流（Stream）的发布者，他们两个可以发布三种数据信号：元素值，错误信号，和完成信号，后两个都代表终止信号，终止信号用于告诉 Subscriber 数据流技术了，其中错误信号终止数据流并且把错误信息传递给 Subscriber



## WebFlux 注解式

```java
@RestController
public class UserController {

    @Autowired
    private UserService userService;

    // id 查询
    @GetMapping("/user/{id}")
    public Mono<User> getUserById(@PathVariable int id) {
        return userService.getUserById(id);
    }

    // 查询所有
    @GetMapping("/user")
    public Flux<User> getUsers() {
        return this.userService.getAllUser();
    }

    // 添加
    @PostMapping("/saveuser")
    public Mono<Void> addUser(@RequestBody User user) {
        Mono<User> userMono = Mono.just(user);
        return userService.saveUserInfo(userMono);
    }
}
```









