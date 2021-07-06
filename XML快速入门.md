---
title: XML快速入门
date: 2020-11-01 21:43:36
categories: 
- 后端
- 前端
tags:
- 后端
- XML
---



W3C 官网：

>https://www.w3schools.com/xml/default.asp

# 介绍

XML 和 HTML 都起源于 W3C，但是由于各种原因，XML 的定位和 HTML 不再相同

* XML功能
	* 存储数据
		1. 配置文件
		2. 在网络中传输
* XML 与 HTML 的区别
	1. XML 标签都是自定义的，HTML 标签是预定义。
	2. XML 的语法严格，HTML 语法松散
	3. XML 是存储数据的，HTML 是展示数据





# 快速开始

XML 的第一行**必须定义为文档声明**

```xml
<?xml version='1.0'?>
```

XML 中有且仅有一个根标签，属性值必须用用引号括起来

```xml
<?xml version='1.0' ?>

<!-- 这个 users 就是根标签 -->
<users>
    <user id='1'>
        <name>张三</name>
        <age>23</age>
        <gender>Male</gender>
    </user>
    <user id='2'>
        <name>李四</name>
        <age>24</age>
        <gender>Female</gender>
    </user>
</users>
```

**XML 标签名区分大小写**



# 文档声明

格式：

```xml
<?xml 属性 ?>
```

| 属性值                                           | 说明     |
| ------------------------------------------------ | -------- |
| version（必须有）                                | 版本号   |
| encoding                                         | 编码方式 |
| （告诉解析引擎当前文档的编码，默认：ISO-8859-1） |          |
| standalone                                       | 是否独立 |



# 属性和文本

对于标签的属性来说，id 这个属性唯一，由约束来限制的

文本有一注意事项，特殊符号没法直接显示出来，需要放在 CDATA 区中，格式：

```xml
<![CDATA[ 数据 ]]>

<![CDATA[
	if (a == b && a > c) {}
]]>
```



# 约束



## 介绍

因为 XML 是一个随意自定义标签的标记语言，那么当 XML 作为配置文件出现的时候，用户（程序员）需要知道这个配置文件的创建规则，换句话说就是要知道这个 XML 配置文件中都有那些标签，可以填写什么值。这样的约束一般会由软件的开发者来提供一份约束文档，告诉用户要怎么创建 XML 配置文件。

![image-20201101225127845](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/image-20201101225127845.png)



作为使用者来说：

- 能够在 XML 中引入约束文档
- 简单读懂约束文档



## DTD 约束

DTD 约束分为：

- 内部 DTD：将约束定义在 XML 文件中
- 外部 DTD：单独用一个 DTD 文件定义约束
  1. 本地：```<!DOCTYPE 根标签 SYSTEM "dtd 文件路径">```
  2. 网络：```<!DOCTYPE 根标签 PUBLIC "dtd 文件名字" "dtd 文件 URL">```



举例说明：

```xml-dtd
<!ELEMENT students (student*) >
<!ELEMENT student (name,age,sex)>
<!ELEMENT name (#PCDATA)>
<!ELEMENT age (#PCDATA)>
<!ELEMENT sex (#PCDATA)>
<!ATTLIST student number ID #REQUIRED>
```

第一行定义了根标签，小括号中规定其字标签可以出现的次数

第二行也是如此，name、age 和 sex 没有写任何数量符号，表示只出现一次

第三行到第五行和第二行的字段进行匹配，并且规定了值得类型（#PCDATA 为字符串）

第六行比较特殊，添加了属性值，并且必须要求填写





## XSD 约束

这个很复杂... 结合代码慢慢说

```xml
<?xml version="1.0"?>
<xsd:schema xmlns="http://url/xml"
        xmlns:xsd="http://www.w3.org/2001/XMLSchema"
        targetNamespace="http://url/xml" elementFormDefault="qualified">
	<!-- 注解 1 -->
    <xsd:element name="students" type="studentsType"/>
	<!-- 注解 2 -->
    <xsd:complexType name="studentsType">
        <!-- 注解 3 -->
        <xsd:sequence>
            <xsd:element name="student" type="studentType" minOccurs="0" maxOccurs="unbounded"/>
        </xsd:sequence>
    </xsd:complexType>
	
    <!-- 注解 4 -->
    <xsd:complexType name="studentType">
        <xsd:sequence>
            <xsd:element name="name" type="xsd:string"/>
            <xsd:element name="age" type="ageType" />
            <xsd:element name="sex" type="sexType" />
        </xsd:sequence>
        <!-- 注解 5 -->
        <xsd:attribute name="number" type="numberType" use="required"/>
    </xsd:complexType>

	<!-- 注解 6 -->
    <xsd:simpleType name="sexType">
        <xsd:restriction base="xsd:string">
            <xsd:enumeration value="male"/>
            <xsd:enumeration value="female"/>
        </xsd:restriction>
    </xsd:simpleType>
    
    <xsd:simpleType name="ageType">
        <xsd:restriction base="xsd:integer">
            <xsd:minInclusive value="0"/>
            <xsd:maxInclusive value="256"/>
        </xsd:restriction>
    </xsd:simpleType>
    <xsd:simpleType name="numberType">
        <xsd:restriction base="xsd:string">
            <xsd:pattern value="empID_\d{4}"/>
        </xsd:restriction>
    </xsd:simpleType>
</xsd:schema> 

```



注解 1：

此处定义的是根标签，相当于写 Java 中创建一个 Class 文件这个过程，而 type 中的值就好比定义该标签属于什么类

注解 2：

此处是对根标签 type 的定义，所有 complexType 定义的，都是大的标签内容，换句话说就是它下面还有其他的标签

注解 3：

此处 sequence 表示该标签后面还会连续出现的其他的元素，此处会声明其他元素的 name，type，和出现的次数。name 表示的在写 xml 标签的时候所用到的名字，而 type 更多的是用来写约束用的

注解 4：

此处就是对 student 这个对象，这个标签进行详细的定义，以及它的类型，其中 xsd:String 已经属于基础数据类型

注解 5：

为该标签规定属性，此处规定的 number 是来记录 student# 的一个属性值，且必须填写

注解 6：

在注解 4 那一段中，除了基础数据类型以外，还有一些自定义的类型，比如 sexType，而在注解 6 这里就会对这个类型进行详细的限制；其中 sexType 的限制中，基础数据类型为 xsd:String，限定的内容为枚举，只能填写 male 或者 female；ageType 类型的约束的基本数据类型为 xsd:integer，数字范围为 0 到 256；最后对 numberType 的约束就更厉害了，基础数据类型为 xsd:String，内容需要满足一个 pattern



XSD 的引入方式也很麻烦：

```xml
 <students xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
 		xmlns="http://url/xml" 
 		xsi:schemaLocation="http://url/xml
                               http://url/xmlstudent.xsd"
 		    >
 </students>
```

上面的 xmlns 为引入约束的开头，好像... 其中 w3c 那个必须引用，而且似乎都会以 xsi 为前缀，然后继续使用 xmlns 来引入自己全部的约束所在文件夹路径，最后通过 xsi:schemaLocation 的方式来找到自己写好的约束文件的位置，下面放一张 Spring 框架的 XML 例子

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	xmlns:context="http://www.springframework.org/schema/context"
    xmlns:mvc="http://www.springframework.org/schema/mvc"
    xsi:schemaLocation="
        http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context 
        http://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/mvc
        http://www.springframework.org/schema/mvc/spring-mvc.xsd">
```

还有注意一点的是，xmlns 后面引入之后，如果不加前缀的话，则为空前缀；如果加了的话，那在对应的标签前面，需要加上前缀



# 解析

## DOM/SAX

解析：操作xml文档，将文档中的数据读取到内存中

​	操作xml文档
​		1. 解析(读取)：将文档中的数据读取到内存中
​		2. 写入：将内存中的数据保存到xml文档中。持久化的存储

* 解析xml的方式：
  1. DOM：将标记语言文档一次性加载进内存，在内存中形成一颗dom树（一般服务端会用这个方式）
    - 优点：操作方便，可以对文档进行CRUD的所有操作

    * 缺点：占内存
  2. SAX：逐行读取，基于事件驱动的（一般客户端会用这个方式，尤其是移动端）
  	* 优点：不占内存。
  	* 缺点：只能读取，不能增删改



## Jsoup

Jsoup 是个工具类，方法都是静态的，可以直接使用：

| 方法                               | 描述                     |
| ---------------------------------- | ------------------------ |
| parse(File in, String charsetName) | 解析 xml 或者 html 文件  |
| parse(String html)                 | 解析 xml 或 html 字符串  |
| parse(URL url, int timeoutMillis)  | 在线获取指定 xml 或 html |

（第二个的话，就是把 HTML 的内容或者 XML 的全部内容，直接拷贝成字符串的形式，再传入）

快速使用：

```java
public class JsoupDemo {
    public static void main(String[] args) throws Exception {
        String url = JsoupDemo.class.getClassLoader().getResource("student.xml").getPath();
        Document doc = Jsoup.parse(new File(url), "utf-8");
        Elements name = doc.getElementsByTag("name");

        for(Element e : name) {
            System.out.println(e.text());
        }
    }
}
```



## Document 对象

里面有很多 get 方法，使用起来和 DOM 操作几乎一样

通过标签名获取：

```java
document.getElementsByTag(String tagName);
```

通过属性获取：

```java
document.getElementsByAttribute(String key);
```

通过属性和属性值获取：

```java
document.getElementsByAttributeValue(String key, String value);
```

通过 ID 获取：（这个必须要求其中属性名为 ID）

```java
document.getElementsById(String id);
```

其他的还有很多方法，具体查文档或者源码



## Element

Element 是一个元素对象，它下面还可以有其他的元素对象，所以上述的一些 get 方法它也可以用

Element 对象可以获取属性值：

```java
String attr(String key);  // 根据属性名获取属性值 
```

获取文本内容：

```java
String text();  // 获取文本内容
String html();  // 获取的是 innerHTML（获取标签体内所有内容）
```



## 选择器

document 对象中可以调用 ```select``` 方法，其中的参数为 css query，基本和 CSS 的选择器的使用一毛一样

```java
Elements el = document.select("name");  // 获取全部 name 标签
Elements el = document.select("#red");  // 获取 ID 为 red 的标签
Elements el = document.select("student[number='0001'] > age");  // 获取学号为 0001 的学生的 age 标签
```

更多的使用语法和使用例子，可以参考 Java 文档





## Xpath

这个也是来选择特定元素内容的，有点像路径选择一样，所以有个”oath“这个名字。使用 Xpath 需要单独的一个 Xpath 的 jar 包，配合 Jsoup 来使用。

项目官方网站：

> https://github.com/zhegexiaohuozi/JsoupXpath

```//``` 有点像标签选择器，会把全局内的那个标签的元素全部选择到：

```java
List<JXNode> jxNodes = jxDocument.selN("//student");
```

```/``` 标签就是选择下面子级的标签内容：

```java
List<JXNode> jxNodes = jxDocument.selN("//student/name");
```

```@``` 来选择带有特定属性的标签内容：（在这后面还可以用 ```='value'``` 的方式来选择带特定值的属性值）

```java
List<JXNode> jxNodes = jxDocument.selN("//student/name[@id]");
```

更多的使用方法可以查阅 W3C 官网：

>https://www.w3schools.com/xml/xpath_intro.asp





