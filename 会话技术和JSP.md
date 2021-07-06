---
title: 会话技术和JSP
date: 2020-11-15 06:38:36
categories: 
- 后端
tags: 
- 会话技术
- Cookie
- Session
- JSP
---



# 会话技术

- **会话**：一次会话中包含多次请求和响应，浏览器第一次给服务器资源发送请求，会话建立，直到有一方断开为止

- **功能**：在一次会话的范围内的多次请求间，共享数据
- **方式**：
  	1. 客户端会话技术：Cookie
   2. 服务器端会话技术：Session





## Cookie

客户端会话技术，将数据保存到客户端



### 创建使用

- 创建 Cookie 对象，绑定数据

  ```Java
  new Cookie(String name, String value) 
  ```

- 发送 Cookie 对象

  ```java
  response.addCookie(Cookie cookie) 
  ```

- 获取 Cookie，拿到数据

  ```java
  Cookie[] request.getCookies()
  ```



**Cookie 的实现依赖于响应头 set-cookie 和请求头 cookie**



### 生命周期

Cookie 默认是关掉浏览器之后删除掉，也可以人为设置

通过 ```setMaxAge(int seconds)``` 方法

- 正数：将Cookie数据写到硬盘的文件中，并指定 Cookie 存活时间
- 负数：默认值
- 0：删除cookie信息



### 存中文

Tomcat 8 之前的版本不能存中文，解决方案：

将中文转码，使用 URL 编码格式

Tomcat 8 之后可以存中文



### 共享

默认情况下，Cookie 是不会在项目和项目之间共享的，如果想在同一个 Tomcat 容器下进行共享：

使用 ```setPath(String path)```：设置cookie的获取范围。

默认情况下，设置当前的虚拟目录

如果想要共享，将其设置为 ```/``` 即可



跨域共享的话，则需要使用 ```setDomain(String path)``` 来设置，比如

```java
setDomain(".baidu.com");
```

凡是以 ```.baidu.com``` 结尾的，都可以共享



### 特点和作用

1. Cookie 存储数据在客户端
2. 浏览器对于单个 Cookie 大小有显示（4kb上下）
3. 同一域名下的总 Cookie 数有显示



- cookie一般用于存出少量的不太敏感的数据
- 在不登录的情况下，完成服务器对客户端的身份识别



## Session

Session 是服务器端会话技术，在一次会话的多次请求中共享数据，这个数据是保存在服务端对象中的，HttpSession，它也是一个**域对象**

它对应的也有一些类似的方法，来设置、删除数据等操作：

​		获取属性值

- ```java
  Object getAttribute( string name);
  ```

  设置属性值

- ```java
  void setAttribute(string name，object value);
  ```

  

- ```java
  void removeAttribute(string name)
  ```



### 原理

Session 是依赖于 Cookie的，当 Client 请求数据的时候，Server 会去判断 Cookie 中存不存在一个叫 JSESSIONID 的属性，如果没有的话，则会在 Response Body 中添加这样一个 JSESSIONID 的属性

![image-20201130230243605](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/image-20201130230243605.png)

等 Client 再发送请求来的时候，Request Body 中就会携带 JSESSIONID，Server 就会发现有这样一个属性叫 JSESSIONID 的，然后其对应的 Session 对象。

### 注意的细节

**关闭浏览器，不关闭服务器的情况：**

因为 Session 依赖 Cookie，默认情况下 Cookie 是在浏览器关闭之后就会删除，所以如果想实现关闭浏览器之后，从新打开，还能使用到同一个 Session 的话，需要手动创建一个 Cookie，设置生命周期：

```java
HttpSession session = request.getSession();
Cookie c = new Cookie("JSESSIONID", session.getID());
c.setMaxAge(60 * 60);
response.addCookie(c);
```



**关闭服务器的情况：**

因为服务器重启，会导致 Session 的丢失，所以要在服务器关闭/重启之前，先把 Session 做钝化（其实就是序列化），然后等再次开启服务器后再活化（反序列化）

这个过程不需要我们来做，Tomcat 已经帮我们实现好了，但是没办法通过 IDEA 来演示出来。

首先要项目打包成 war 包，然后丢到 Tomcat 容器中（webapps），它会自己部署好项目。

![image-20201130232524994](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/image-20201130232524994.png)

随后，当有请求出现的时候，Tomcat 会把 Session 暂时缓存到内存中。然后，当 Tomcat **正常关闭 **的时候，会把 Session 序列化到本地，存放在 work 目录下。

![image-20201130232704163](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/image-20201130232704163.png)

等再次启动项目的时候，这个序列化的 Session 记录就会被重新读取到内存中，并且删除

（IDEA 的话，虽然停掉项目的时候会生成序列化的文件，但是它重启项目之后，因为会删除掉 work 目录，所以它会反序列化失败）



### Session 被销毁的时间

1. 服务器关闭

2. session 对象调用 ```invalidate()``` 方法自杀

3. session 默认的失效时间为 30 分钟，可以在项目中生成 web.xml 文件然后选择性的修改，会覆盖全局配置

   ```xml
   <seesion-config>
   	<session-timeout>60</session-timeout>
   </seesion-config>
   ```

   

### 特点

1. session 用于存储一次会话中多次请求的数据，存在服务器端
2. session 可以存任意类型数据，任意大小



# JSP

JSP 全称为 Java Server Pages，可以简单的理解为一个特殊的页面，其中可以写 HTML，也可以写 Java 代码，其主要的用途就是为了**简化书写**

其实现原理是把 JSP 文件转换成了一个 java 文件，然后编译成 class 文件，在把页面写给浏览器的；HTML 的内容都是通过一行行 out 写出来的。



## 脚本

1. ```<% %>``` ：定义的 Java 代码，在 service 方法中。方法中可以定义什么，这里就能定义什么
2. ```<%! %>``` ：这个代码会在 JSP 转换后的 Java 类中的成员变量的位置
3. ```<%= %>``` ：输出语句可以输出什么，这里就可以写什么，会把内容直接输出到页面上



## 内置对象

内置对象有 7 个，这里先说三个。

因为 JSP 本身其实可以看做一个 Servlet，所以它也有 **request** 和 **response**。

还有一个内置对象 **out**，它可以把数据输出到页面上，和 ```response.getWriter()``` 然后输出到页面的效果类似，但是他们俩有一点区别：

- Tomcat 在对客户端做出响应之前，会先去 response 的缓冲区，然后在去 out 的缓冲区
- ```response.getWriter()``` 的数据输出永远在 ```out.write()``` 之前





