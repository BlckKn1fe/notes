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





# Spring配置文件



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
   <bean id="userService" class="com.guanyu.service.impl.UserServiceImpl">
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

   

## 引入其他配置文件（分模块开发）

实际开发中，Spring的配置内容非常多，这就导致Spring配置很繁杂且体积很大，所以可以将部分配置拆解到其他配置文件中，而在Spring主配置文件通过import标签进行加载

```xml
<!-- applicationContext.xml 文件中 -->
<import resource="applicationContext-user.xml"/>
```



# Spring API

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

   



# 配置数据源



## 手动配置数据源

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

创建 connection 对象

```java
@Test
public void createDruidTest() throws Exception {
    ResourceBundle rb = ResourceBundle.getBundle("jdbc");
    String driver = rb.getString("jdbc.driver");
    String url = rb.getString("jdbc.url");
    String username = rb.getString("jdbc.username");
    String password = rb.getString("jdbc.password");
    
    DruidDataSource source = new DruidDataSource();
    
    source.setDriverClassName(driver);
    source.setUrl(url);
    source.setUsername(username);
    source.setPassword(password);
    
    DruidPooledConnection connection = source.getConnection();
    System.out.println(connection);
    
    connection.close();
}
```



## Spring配置数据源

以上连接池都有无参构造，并且都是通过 set 方法来配置关键信息，所以可以通过依赖注入的方式来让 Spring 生成对应的连接池对象，并且存放在 Spring 容器中

添加到配置文件中：

```xml
<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
    <property name="driverClassName" value="com.mysql.jdbc.Driver" />
    <property name="url" value="jdbc:mysql://localhost:3306/test" />
    <property name="username" value="root" />
    <property name="password" value="111111" />
</bean>
```











