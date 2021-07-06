---
title: Tomcat和Servlet
date: 2020-11-02 22:25:25
categories: 
- 后端
tags:
- 后端
- Tomcat
- Servlet
---



# Servlet

Servlet，全称 Java Servlet。是用 Java 编写的服务器端程序。其主要功能在于交互式地浏览和修改数据，生成动态 Web 内容。狭义的 Servlet 是指 Java 语言实现的一个接口，广义的 Servlet 是指任何实现了这个 Servlet 接口的类，一般情况下，人们将 Servlet 理解为后者。



## 快速开始

1. 创建JavaEE项目
	
2. 定义一个类，实现Servlet接口

  ```java
  public class ServletDemo1 implements Servlet
  ```

3. 实现接口中的抽象方法

4. 配置Servlet，在web.xml中配置：

    ```xml
    <!-- 配置 Servlet -->
    <servlet>
        <servlet-name>demo</servlet-name>
        <servlet-class>cn.guanyu.web.servlet.Demo</servlet-class>
    </servlet>
    <!-- 映射 -->
    <servlet-mapping>
        <servlet-name>demo</servlet-name>
        <url-pattern>/demo</url-pattern>
    </servlet-mapping>
    ```

    当再次运行的时候，访问到 ```localhost:8080/demo``` 的时候，就会去调用实现的抽象方法，观察控制台。



## 生命周期

1. Servlet 实现类中的 ```init()``` 和 ```destroy()``` 方法只会被调用一次，当被访问到的时候执行

   而 ```service()``` 方法则每次访问都会执行



2. 一个实现类的加载也可以设置在特定的时机，比如在服务启动的时候就进行加载。在 web.xml 文件中的 Servlet 标签中，添加：

    ```xml
    <load-on-startup>0</load-on-startup>
    ```
    
    当数字为负数的时，只有访问的时候才会被加载；当数大于等于 0 的时候，则在启动服务的时候就会加载。
    
    
    
3. Servlet 的实例对象在内存中只存在一个，单例对象，所以当多个用户同时访问的时候，可能会存在线程安全问题。解决办法：尽量不在 Servlet 中定义成员变量， 如果定义了，那么就不要对这个成员变量进行修改操作。

    

4. ```destroy()``` 只有在服务器正常关闭的时候才会执行



## 注解配置

随着以后 Servlet 的类越来越多，web.xml 的配置文件会越来越臃肿。在 web.xml 文件中的 Servlet Name 只是起到的匹配标示的作用，所以在 Servlet 3.0 (Jave EE 6) 开始，支持了注解注释。只需要在 Servlet 的实现类上，添加特定的注解即可完成 Servlet URL 和实现类的映射绑定

```java
@WebServlet(urlPatterns = "/demo")  // 这里也可以直接写 "/demo"
public class Demo implements Servlet {}
```



ultPatterns 还可以配置多个路径，只需要写一个 String 数组在里面即可。

路径的定义上有不同的规则：

1. ```
   单层目录结构：/xxx
   ```

2. ```
   多层目录结构：/xxx/xxx
   ```

3. ```
   *.do  注意这种写法前面没有 "/"
   ```





## HTTPServlet

创建 Servlet 实体的时候，推荐通过继承 HTTPServlet 的方式来创建，里面已经封装好了很多已经写好的方法。HTTPServlet 中的 service 方法会先判断请求的类型，然后通过请求类型的不同，来调用 ```doGet(), doPost() ```等方法。

我们只需要复写 ```doGet(), doPost() ``` 等方法即可，这里已经把 Request 和 Response 对象传进来了。

```java
@WebServlet("/request")
public class RequestDemo extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("do get...");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("do post...");
    }
}
```



## HTTP

HTTP 全称 HyperText Transfer Protocol，是一个基于 TCP/IP 的高级协议。

默认端口为 80

目前有两个版本：

1. HTTP 1.0：每次请求都会建立新的连接
2. HTTP 1.1：复用连接（connection）



### 请求行

请求行的格式为：```请求url 请求协议/版本```

如：```GET /login.html HTTP/1.1```



请求方式在 HTTP 协议中有 7 种方式，常用的为：

1. GET：
   - 请求的参数在请求行中（就是会在地址栏显示出）
   - 请求的 url 长度有限
   - 不太安全
2. POST：
   - 请求的参数在请求体中
   - 请求的 url 长度没有限制
   - 相对安全（拦包一样能看到）





### 请求头

常见的请求头有：

1. User-Agent：浏览器告诉服务器，我访问你使用的浏览器版本信息
2. Accept：告诉服务器，该浏览器可以接受什么样式的数据
3. Refere：告诉服务器，该请求是来自哪个网站来的
   - 防止盗链
   - 统计
   - 防止恶意请求



### 请求体

请求体中主要是封装，POST 请求的各种参数的



### 响应行

响应行的格式为：协议/版本 状态码 状态码描述

```http
HTTP/1.1 200
```

状态码：本次请求+响应的状态（结果）

- 1xx：表示服务器接收客户端消息，但是没有接收完全，等待一段时间后发送状态码给浏览器
- 2xx：成功，典型的有 200
- 3xx：重定向，也是一种成功，典型代表有 302（重定向），304（访问缓存）
- 4xx：客户端错误，404（请求路径错误），405（请求的方式没有对应 doXXX 方法

### 响应头

响应头也是以键值对的形式出现，下面只列出一些常用的

```http
// 请求头
Accept-Ranges: bytes
Last-Modified: Fri, 13 Nov 2020 15:35:09 GMT
Content-Type: text/html
Content-Length: 182
Date: Fri, 13 Nov 2020 15:44:16 GMT
Keep-Alive: timeout=20
Connection: keep-alive
```

补充一个：

```http
// 请求头
Content-disposition:in-line
```

这个 inline 是默认值，如果改成 ```attachment;filename=XXX``` 的形式的话就会变成下载文件



### 响应体

这就是服务器返回给浏览器的数据内容

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Response</title>
    </head>
    <body>
        Hello Response!
    </body>
</html>
```





## Request 对象

Request 和 Response 对象是服务器自己创建的，而我们是使用者。

Request 的继承体系如下：

```
ServletRequest  --  接口
    |
HTTPServletRequest  --  接口
    |
org.apache.catalina.connector.RequestFacade 类(tomcat)
```



### 获取请求行数据

1. 获取请求方式 ：GET

   ```Java
   String getMethod()  
   ```

2. (*)获取虚拟目录：/servlet

   ```java
   String getContextPath()
   ```

3. 获取Servlet路径: /demo

   ```java
   String getServletPath()
   ```

4. 获取get方式请求参数：name=zhangsan

   ```java
   String getQueryString()
   ```

5. (*)获取请求URI：/servlet/demo
    ```java
    String getRequestURI():  //  /servlet/demo
    StringBuffer getRequestURL()  //  http://localhost/servlet/demo
    ```

    

### 获取请求头数据

1. 通过请求头名称：

   ```java
   String getHeader(String name)
   ```

2. 获取所有请求头名称：（这个值是请求头名称）

   ```java
   Enumeration<String> getHeaderNames()
   ```



通过请求头数据可以判断用户的服务器类型，referer，等属性值，以此来做一些操作。



### 请求体数据

只有 POST 的方式才会有请求体，在请求体中以 stream 的形式封装了请求参数。

获取字节流对象：

```java
BufferedReader getReader()
```

获取字符流对象：

```java
ServletInputStream getInputStream()
```

读取 POST 请求的参数：

```java
protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    BufferedReader reader = request.getReader();
    String line = null;
    while ((line = reader.readLine()) != null) {
        System.out.println(line);
    }
}
```



### 其他常用方法

1. **获取请求参数的通用方式**：（GET 和 POST 获取参数的方法不同）

   不论是 GET 还是 POST 都能兼容以下方法

   ```java
   String getParameter(String name);  //  根据参数名称获取参数值
   String[] getParameterValues(String name);  //  多用于复选框
   Enumeration<String> getParameterNames();  //  获取所有参数名称
   Map<String,String[]> getParameterMap()  //  所有参数的 map 集合
   ```

   以最后一个举例写一段例子来获取到全部的参数名称以及参数值：

   ```java
   protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
       Map<String, String[]> map = request.getParameterMap();
       Set<String> set = map.keySet();
       for (String name : set) {
           String[] values = map.get(name);
           System.out.println("Attribute:");
           for (String value : values) {
               System.out.println(value);
           }
           System.out.println("-----------");
       }
   }
   ```

   获取（打印） GET/POST 请求中的中文参数乱码问题，修改流编码：

   ```java
   request.setCharacterEncoding("utf-8");  // 这个编码和 HTML 的编码一致
   ```

   （在 Tomcat8 开始，GET 请求的乱码问题已经解决）

   

   

2. **请求转发：**一种在服务器内部的资源跳转方式

   先获取**转发器对象：**

   ```java
   RequestDispatcher getRequestDispatcher(String path)
   ```

   调用 Dispatcher 对象的forward 方法：

   ```java
   void forward(ServletRequest request, ServletResponse response);
   ```

   特点：

   - 浏览器地址路径不变化
   - 只能转发到当前服务器内部资源中
   - 转发可以得到多个资源，但是浏览器只会想服务器发送一次请求
   

   
3. **数据共享：**

   Servlet 之间通过转发之后，要互相知道数据情况，通过**域对象**来实现

   request 的作用域：

   使用方法：

   ```java
   void setAttribute(String name,Object obj);  // 存储数据
   Object getAttitude(String name);  // 通过键获取值
   void removeAttribute(String name);  // 通过键移除键值对
   ```

   


## Response 对象

下面列举一些常用使用方法

**设置状态码：**

```java
void setStatus(int sc) 
```

**设置响应头：**

```java
void setHeader(String name, String value) 
```

**设置响应体：**

这里要先获取到输出流，然后使用输出流把数据输出到客户端浏览器

- 字符输出流：

  ```java
  PrintWriter getWriter()
  ```

- 字节输出流：

  ```java
  ServletOutputStream getOutputStream()
  ```



### 重定向

重定向是通过设置状态码，然后设置头来实现的：

```java
response.setStatus(302);
response.setHeader("location", path);
```

response 对象给我们提供了一个更简单的办法：

```java
response.sendRedirect(path)
```



### 路径

路径分为相对路径和绝对路径

- 相对路径：以 "." 开头，如 ./index.html
- 绝对路径：以 "/" 开头，如 /response/demo



路径在使用中分为 ”给服务器用“ 和给 “浏览器用” 两种情况

- 给浏览器用：需要添加项目 context path （如 \<a> 和 \<form> 标签，重定向）
- 给服务器用：不需要添加 context path （如 forward）



动态获取 context path：

```java
request.getContextPath();
```



### 输出字符注意事项

因为输出中文可能会乱码，就一次性配置好，告诉浏览器用 UTF-8 解码，让 Writer 用 UTF-8 编码。

response.getWriter() 获取到的流编码默认为 ISO-8859-1。

**以下要在获取流之前设置！**

```java
// 非简化版
response.setCharacterEncoding("utf-8");
response.setHeader("content-type", "text/html;charset=utf-8");

// 简化版
response.setContentType("text/html;charset=utf-8");

response.getWriter();
```



### 验证码简单原理

这块简单的说一下就行，因为以后真的用到验证码的时候，可以调用其他平台的 API，或者更成熟好用的现成的代码。

在 Java 这面做一个简易的验证码生成，要借助一个叫 ```BufferedImage``` 的类，然后借助里面的 ```getGraphics ``` 来得到一个画笔对象，去绘制这个验证码。大概的思路和真实的人去手写验证码的思路差不多，放一段简易代码：

```java
protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    int width = 100;
    int height = 50;
    // 1、创建对象，在内存中的图片
    BufferedImage image = new BufferedImage(width, height, BufferedImage.TYPE_INT_RGB);
    
    // 2、美化图片
    
    // 2.1 填充背景色
    Graphics g = image.getGraphics();
    g.setColor(Color.PINK);
    g.fillRect(0, 0, width, height);
    // 2.2 画边框
    g.setColor(Color.BLUE);
    g.drawRect(0, 0, width - 1, height - 1);
    
    // 2.3 写验证码
    String str = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789";
    Random r = new Random();

    for (int i = 1; i <= 4; i++) {
        int index = r.nextInt(str.length());
        char ch = str.charAt(index);
        g.drawString(ch+"", width/5*i, height/2);
    }

    // 2.4 画干扰线
    g.setColor(Color.GREEN);
    for (int i = 0; i < 5; i++) {
        int x1 = r.nextInt(width);
        int x2 = r.nextInt(width);
        int y1 = r.nextInt(height);
        int y2 = r.nextInt(height);
        g.drawLine(x1, y1, x2, y2);
    }

    ImageIO.write(image, "jpg", response.getOutputStream());
}
```



验证码想时间点击更换，只需要在前端用 JS 给这个 \<img> 标签绑定一个监听事件，每次点击更换 ```src``` 属性即可。需要注意的是，新的 src 路径需要后面跟一个时间戳作为参数，伪造一次新的请求，否则它会走本地的缓存。

```javascript
window.onload = function() {
    // 1. 获取图像
    var img = document.getElementById("checkCode");
    // 2. 绑定单击事件
    img.onclick = function() {
        // 3. 添加时间戳保证不重复
        var date = new Date().getTime
        img.src = "/reponse?" + date;
    }
}
```





## ServletContext 对象

其代表整个 web 应用，可以和程序的容器进行通信

获取 ServletContext 对象有两个方式

- 通过 ```request.getServletContext();``` 获取
- 通过 ```this.getServletContext();``` 获取

这两个方法获取到的是同一个对象

### 获取 MIME 类型

MIME 类型是在互联网通信中定义的一种数据类型，格式为：```大类型/小类型```

比如：

- text/html
- image/jpg

调用 context 对象中的 getMimeType(String file) 的方法，下面是源码实现：

```java
// org.apache.catalina.core.ApplicationContext;

public String getMimeType(String file) {
    if (file == null) {
        return null;
    } else {
        int period = file.lastIndexOf(46);
        if (period < 0) {
            return null;
        } else {
            String extension = file.substring(period + 1);
            return extension.length() < 1 ? null : this.context.findMimeMapping(extension);
        }
    }
}
```

1. 先判断输入的文件名是不是 null，是 null 则无类型，否则判断是否有  ```.``` 
2. ```.``` 的位置若为负数，则说明无 ```.``` ，依旧无类型
3. 确定有 ```.```  之后的，判断 ```.``` 后面 extension 的长度，若为 0，则依旧无类型
4. 最后它会去一个 HashMap 里去找



### 共享数据

这和 request 对象用法一样，**但是作用域不一样**，request 那面是传给了哪个  Servlet 哪个可以用，这个是所有的 Servlet 都可以访问。

```java
void setAttribute(String name,Object obj);
Object getAttitude(String name);
void removeAttribute(String name);
```





### 获取文件的真实路径

部署的项目通常有两份，一个是在工作空间的，一个是部署到容器里的。这也就造成了部署之后的某个文件的路径会发生改变，我们需要动态的获取到被部署后的文件的所在额真实路径。

获取不同位置的文件大概分为三种情况：

- 在工作空间项目中的 WEB目录 或者在 WEB 中的目录下：

  ```java
  ServletContext context = this.getServletContext();
  String path1 = context.getRealPath("/a.txt");
  String path2 = context.getRealPapth("/WEB-INF/b.txt")
  ```

- 放在工作空间工程文件的 src 文件夹中：

  ```java
  ServletContext context = this.getServletContext();
  String path = context.getRealPath("/WEB-INF/classes/c.txt");
  ```



### 下载文件

当直接把文件的路径放在 \<a> 标签内的 href 中，那个文件如果可以被解析的话，会直接展示在浏览器中。

如果我们想实现点击即下载的话，需要填写 Servlet 的路径地址，然后通过判断请求文件类型，编码，流输出的方式来实现下载。

**下面例子没法处理中文文件名问题，需要一个判断浏览器然后再编码的工具类来实现（也可以自己写）**

```java
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        request.setCharacterEncoding("utf-8");
        response.setCharacterEncoding("utf-8");
        // 1. 获取到文件名称
        String filename = request.getParameter("filename");
        
        // 2. 找到文件在服务器内的路径
        ServletContext context = this.getServletContext();
        String path = context.getRealPath("/img/" + filename);

        // 3. 设置 response header（这里的 filename 即下载时显示的文件名）
        String type = context.getMimeType(filename);
        response.setContentType("img/jpeg;");
        response.setHeader("content-disposition", "attachment;filename=" + filename);

        // 4. 将输入流数据写到输出流中
        FileInputStream fis = new FileInputStream(path);
        ServletOutputStream os = response.getOutputStream();

        byte[] buffer = new byte[128 * 8];
        int len = 0;
        while ((len = fis.read(buffer)) > 0) {
            os.write(buffer, 0, len);
        }
        fis.close();
    }
```







# Tomcat

Tomcat 是由 Apache 软件基金会属下 Jakarta 项目开发的 Servlet 容器，按照 Sun Microsystems 提供的技术规范，实现了对 Servlet 和 JavaServer Page 的支持，并提供了作为 Web 服务器的一些特有功能，如 Tomcat 管理和控制平台、安全局管理和 Tomcat 阀等。



## 部署项目

部署项目有多种方式

1. 直接把项目的文件夹放在 webapps 目录下，其文件夹名就为虚拟目录名

2. 把项目打包成一个 war 包放在 webapps 目录下，当 Tomcat 启动之后，丢进去的 war 包会自动解压；当删除 war 包的时候，解压出来的文件夹也会跟着自动删除

3. 配置 conf/server.xml 文件，在 \<Host> 标签体中添加 

   ```
   <Context docBase="" path="/" />
   docBase 为项目存放目录
   path 为虚拟目录
   ```

   server.xml 是全局配置，有可能因为修改而导致其他的项目运行失败，一般不会这么用

4.  在 conf/Catalina/localhost 下创建一个任意名字的 xml 文件，并编写内容

   ```
   <Context docBase="" />
   ```

   这样部署的方式，虚拟路径变成自己创建的 xml 文件的名字。这种方式比较推荐，其本身是一种热部署的方式；如果不想展示出部署的项目，可以把 xml 文件做下划线注释处理



Java 项目的常见项目目录结构：

```
-- 项目的根目录
	-- WEB-INF目录：
        -- web.xml：web项目的核心配置文件
        -- classes目录：放置字节码文件的目录
        -- lib目录：放置依赖的jar包
```



## IDEA 控制台乱码解决

为了解决乱码问题，需要清楚一点就是，要让 IDEA 日志显示控制台编码和 Tomcat 的日志编码匹配，所以首先第一件事是先去看 Tomcat 的 log properties

1. 在 Tomcat 的的 conf 目录下找到 ```logging.properties``` 然后打开找到：

   ```
   java.util.logging.ConsoleHandler.encoding = UTF-8
   ```

   确定设置成了 ```UTF-8```

2. 在 IDEA 的 help --> Edic Custom VN Option 中，添加 ```-Dfile.encoding=UTF-8```

   （Windoes 默认用 GBK 编码）
   
3. Tomcat 项目配置中，添加 ```-Dfile.encoding=UTF-8``` 参数

**千万注意：不要改 Tomcat 的 logging.properties 为 GBK，否则会导致调试时 get/post 参数乱码**



## 和 IDEA 的配置

1. IDEA会为每一个 Tomcat 部署的项目单独建立一份配置文件，位置在启动项目的时候的控制台中 ```CATALINA_BASE``` 后面显示。
2. 工作空间的项目 和 Tomcat  部署的项目是两个东西，Tomcat 访问的目录实际为 ```CATALINA_BASE``` 下 ```localhost``` 中 xml 文件的 docBase 路径，所访问的内容对应工作空间项目的 web 目录下的所有资源
3. ```WEB-INF``` 目录下的资源不能被浏览器直接访问，需要其他技术，里面就是 Java 编译好的字节码文件

