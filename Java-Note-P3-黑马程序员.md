---
title: Java Note P3 (黑马程序员)
date: 2019-11-18 12:43:23
categories: 
- Java
tags: 
- Java
- 后端
---

> 笔记内容全部来源于 https://www.bilibili.com/video/av16566857 

# Java 常见对象



## P1~P8快捷键

- Ctrl + N : 新建
- Ctrl + Shift + F : 格式化
- Ctrl + Shift + O : 整理包
- Alt + ↑ / ↓ : 移动代码
- Ctrl + Alt + ↑ / ↓ : 把当前行的代码复制一行
- Ctrl + Shift + T : 搜索具体的类
- Ctrl + 1 : IDE给建议
- Ctrl + D : 删除代
- Alt + Shift + M : 抽取方法
- Alt + Shift + R : 集体改名
- Alt + Shift + S : 
  1. 加C创建空参构造
  2. 加O创建有参构造
  3. 加R创建Set / Get方法
  4. 加V重写方法
  5. 加S创建 toString() 方法



## P9 Jar文件

右键工程文件，选择导出，再选择Jar File后，把工程中的东西都打包在一起。

在使用的时候，直接复制Jar包，在当前的工程中直接Ctrl + V粘贴过来；右键Jar抱后，选择Build Path → Add to Build Path才算完成（出现奶瓶）

在移除的时候右键选择奶瓶，选择Build Path → Remove即可。

**注意：**在实际开发中不只是复制过来这么简单，一般会创建一个lib文件夹，把这些Jar包放在一起统一管理。

## P13 传参问题

这里用Debug的方式简单说了一下传参的问题。

当传递一些基础数据类型的时候，只是简单的传值；比如一个 change(int a, int b) 方法，传进去的只是值而已，并不会对方法歪的a和b进行修改。

而如果传进去的是一个非基础数据类型的话，传进去的是地址值，比如传一个数组进去。这样经过一系列操作，是会对这个数组进行改变的。

## P14 Object类

Object是最最最顶层的类，所有的类都继承了Object的类，它自己有带一些方法。

### hashCode()

哈希表这个东西，形象一点形容，它就像是经过一个公式来得到的一串数字（官方文档有写），这个数字表明了某个对象具体被存放的位置；有时候会出现不同对象共用一个哈希值，遇到这种情况，大多数情况都会用一个LinkedList来解决，系统找到这个哈希值之后，在去具体寻找想用的对象。

### getClass()

getClass返回的是一个Class类（注意这是大写的C），Class类是用来形容类的类。

```java
Student s = new Student("张三", 23);
Class cls = s.getClass();
String name = cls.getName();
```

### toString

这不说了...很熟悉了

多说一句：如果直接父类或者间接父类重写的话，它也可以直接继承下来。

### equals

这个源码是判断两个对象的地址值，一般来说是需要改写的；比如比较两个人：

```java
Person s1 = new Person("Jack", 23);
Person s2 = new Person("Jack", 23);
```

这里s1和s2是一个人，但是地址值不同，很明显这不符合我们判断两个人是否一致的标准。名字都是Jack，年龄都是23，这样有一样的特征属性的时候，他们就是一个人。所以需要改写 equals() 方法，在这个方法中比较姓名和年龄

" == " 和 "equals" 的对比：

共同点：都可以做比较，返回值都是boolean类型

区别：
  		1. "==" 是比较运算符，可以比较基本数据类型和引			用数据类型；基本数据类型比较值，引用数据类型比较地址值。
                		2. equals 方法在没重写之前，比较地址值，底层依赖是 " == "，但是比较地址值无意义，需要重写，比较对象中的属性值。equals 方法只能比较引用数据类型。

## P21 Scanner

用Scanner输入整数之后在输入字符串的话，会出现跳过输入字符串的情况：

```java
Scanner sc = new Scanner(System.in);
System.out.println("请输入第一个整数:");
int i = sc.nextInt();
System.out.println("请输入第二个字符串:");
String line = sc.nextLine();
System.out.println("i: " + i + ", line: " + line);

```

这是因为，键盘输入默认会跟着 \r\n ，而nextInt只取了10，后面的 \r\n 留下了； 之后 nextLine 是越到 \r\n 就停止本行的扫描，所以没有再次要求用户输入下一行的内容

解决方案：全都用 nextLine 解决，然后用Inegeter.parseInt来进行转换。（用到if来判断类别）

## P23 String类

### 简单概述

字符串字面值 "abc" 是一个字符串对象；而且字符串是常量，一旦被赋值，就不能改变。

```java
String str = "abc";  // 创建一个 "abc"对象，str指向 "abc" 的地址
str = "def";  // 创建 "def" 对象，重新指向
// 这里创建的字符串是在常量池里

```



### Constructor

| Constructor                                 | Comment              |
| ------------------------------------------- | -------------------- |
| String()                                    | 空参构造             |
| String(byte[] bytes)                        | 把字节数组转成字符串 |
| String(byte[] bytes, int index, int length) | 转一部分             |
| String(char[] value)                        | 把字符数组转成字符串 |
| String(char[] value, int index, int count)  | 转一部分             |
| String(String original)                     |                      |

这是一个转码的过程，数组里如果放的是数字的话，会根据系统本地的编码进行转换，很多编码都实现了ASCII，所以97是a，98是b ...

上面转一部分的，第二个参数 index 代表从第几个开始，inclusive，然后length代表从起始点开始，取多少个，也就是想要得到的长度。

### 常量池以及堆内存

常量池中没有字符串对象的话，会创建一个，有的话，直接拿来用。

```java
String s1 = "abc";
String s2 = "abc";
// S1 和 S2 指向同一个地址值
```

使用 new 创建字符串对象的时候，会创建两个对象；堆内存中创建的对象，可以当做是一种副本的存在方式。常量池中的字符串和堆内存中的对象地址值不相同。

```java
String s1 = new String("abc");
s2 = "abc";
System.out.println(s1 === s2);  // false
System.out.println(s1.equals(s2));  // true
```

Java中自带常量优化机制，"a", "b", "c", 三个相加，编译的时候就会在常量池中创建 "abc" 对象

```java
String s1 = "a" + "b" + "c";
String s2 = "abc";
System.out.println(s1 === s2);  // true
System.out.println(s1.equals(s2));  // true
```

还有一种特殊情况说明，对于String的 " + " 的；加号底层是通过StringBuilder或StringBuffer以及他们的append功能实现的，然后toString输出出来。具体实现入下图：

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20191223131229.png)

此时如果执行以下代码

```java
String s3 = s1 + "c";
System.out.println(s3 === s2);  // false
System.out.println(s3.equals(s2));  // true
```

因为s3指的是堆内存中对象的地址，而s2指向的是常量池中的地址，所以 " == " 返回的是false

### 常用方法

1. equals(Object o) : 比较相不相同

2. equalsIgnoreCase(String str) : 不区分大小写进行比较内容

3. contians(String str) : 问包不包含某个什么什么

4. startsWith(String str) : 判断是否以某段开头

5. endsWith(String str) : 判断是否以某段结束

6. isEmpty() : 判断是否为空（不是判断null的）

7. length() : 获得字符串长度

8. charAt(int index) : 得到指定索引位置的字符

   **所有indexOf返回 -1 的话代表没找到**

9. indexOf(int ch) : 这里ch实际是ASCII，比如97就是找第一个a的位置，参数写char类型也可以，注意使用单引号

10. indexOf(String str) : 返回该字符第一次出现的位置

11. indexOf(int ch, int fromIndex) : 返回该字符在指定位置之后第一次出现的位置

12. indexOf(String str, int fromIndex) : 返回该字符串在指定位置之后第一次出现的位置

13. lastIndexof : 这个和上面用法差不多，只是是从后向前找，索引还是从左往右，不展开写了

14. substring(int start) : 截取字符串用的，默认是从start的索引开始到结尾

15. substring(int start, int end) : 规定截取的详细范围，左闭右开

16. getBytes() : 把一串字符串变成一个字节数组（根据本地码表）

17. toCharArray() : 把一串字符变成一个字符字符字符数组!!!

18. valueOf(char[] chs) : 这是个静态方法!! 把一串字符数组转成字符串，底层实际是由 String 类的 Constructor 实现的

19. valueOf(int i) : 把一个数变成字符串

    valueIOf 可以把任意数据类型转换字符串，具体看IDE自带的补齐

20. toLowerCase() : 没什么讲的，注意不是对本身进行大小写改变

21. toUpperCase() : 同上

22. concat(String str) : 连接两个字符串，不过没 " + " 强大

23. replace(char old, char new) : 注意这是字符

24. replace(String old, String new) : 这是字符串

25. trim() : 去掉两端空格

## P38 StringBuffer类（更安全）

### 构造方法

1. StringBuffer() : 不带字符的buffer，初始容量16个字符
2. StringBuffer(int capacity) : 初始容量为capacity的buffer

### 常用方法

1. append() : 没啥说的
2. insert(int offset, String str) : 在指定索引的地方添加内容，注意不要越界
3. deleteCharAt(int index) : 根据索引删除指定位置的字符
4. delete(int start, int end) : 删除指定范围内的字符，注意，左闭右开
5. replace(int start, int end, String str) : 替换字符，还是左闭右开
6. reverse() : 反转
7.  StringBuffer也有截取功能，也是substring，用法一样，需要注意的是返回类型是String，需要用String来接受；同时原来的StringBuffer内容是不变的
8. String转StringBuffer可以通过StringBuffer的构造方法或者append；反之StringBuffer转String的话可以直接丢进String构造方法中、toString方法或者substring截取

### 传参

**先说String**

```java
public static void main(String[] args) {
    String s = "heima";
    System.out.println(s);  // heima
    change(s);
    System.out.println(s);  // heima
}

public static void change(String s) {
    s += "itcast";
}
```

基本数据类型的值传递，不改变起值

引用数据类型的值传递，改变其值（其实传禁区的）

String虽然是引用数据类型，但是他所谓参数传递时，和基本数据类型一样

**再说StringBuffer**

```java
public static void main(String[] args) {
    StringBuffer sb = new StringBuffer("heima");
    System.out.println(sb);  // heima
    change(sb);
    System.out.println(s);  // heimaitcast
}

public static void change(StringBuffer sb) {
    sb.append("itcast");
}
```





### 小节

这玩应还挺好用的，对String的操作上更灵活；可以先把需要操作的String丢过来，操作完之后再转回去

另外StringBuilder和StringBuffer用法基本一样，只不过StringBuilding线程不安全，但是效率高；反之StringBuffer线程安全，效率低。

## P57 Integer（Wraplcass）

### 自动装箱 / 拆箱

这里提一嘴自动装箱和自动拆箱

```java
/* 创造Integer对象的时候，可以直接给值，这是自动装箱的过程
 * 直接用Integer对象和一个基本数据类型 int 相加的时候
 * 底层自动会调用 i1.intValue() 和 int 相加
 * 要注意，当对象为null的时候，会有空指针异常的问题
 */
Integer i1 =  100;
int x = i1 + 200;
```



### 面试题

```java
Integer i1 = new Integer(97);
Integer i2 = new Integer(97);
System.out.println(i1 == i2);      // false
System.out.println(i1.equals(i2);  // true
                   
```

创建两个Integer对象，地址值当然不一样，" == " 比较地址值，而equals

被重写，比较属性值

```java
Integer i3 = 127;
Integer i4 = 127;
System.out.println(i3 == i4);      // true
System.out.println(i3.equals(i4);  // true
                   
Integer i5 = 128;    // false
Integer i6 = 128;    // false
                   
```

当这个数值在 -128 ~ 127 之间的时候，它会从常量池中直接找到这个数，它就会去创造一个对象，-128 ~ 127 刚好是byte的取值范围，下面放一下源码。

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20200107221544.png)

如果这个数字在这个范围内的话，会在一个列表里直接找到这个数（准确点说是 Integer 对象，这个方法返回类型就是 Integer），否则创建对象。



## 正则表达式 (Regular Expression)

String自带的一个匹配正则的方法

```java
"5449872".matches.(regex);
```

### 字符类

|     格式     |                 作用                  |
| :----------: | :-----------------------------------: |
|    [abc]     |        表示abc三个**单个字符**        |
|    [^abc]    | 除abc外任意**单个字符** *（10 不算）* |
|   [a-zA-Z]   |       a到z，A到Z，**单个字符**        |
|  [a-d[m-p]]  |      a到d，或m到p，**单个字符**       |
| [a-z&&[def]] |   结果为d、e或f，交集，**单个字符**   |
| [a-z&&[ ^cd]] | a到z，除了c和d，**单个字符** |
| [a-z&&[ ^c-f]] | a到z，除了c到f，**单个字符** |



举例：

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20200207225558.png)



### 预定义字符类

| 格式 |             作用             |
| :--: | :--------------------------: |
|  .   | 任意字符，**一个点一个字符** |
|  \d  |       **单个**数字字符       |
|  \D  |        单个非数字字符        |
|  \s  |  空白字符，[ \t\n\x0B\f\r]   |
|  \S  |          非空白字符          |
|  \w  |   a到z，A到Z，0到9，下划线   |
|  \W  |      除了单词字符以外的      |

**以上要注意反斜转义!!!**

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20200207225945.png)

### 数量词

|  格式  |            作用            |
| :----: | :------------------------: |
|   X?   |       出现一次或0次        |
|   X*   |       出现零次或多次       |
|   X+   |       出现一次或多次       |
|  X{n}  |        恰好出现n次         |
| X{n,}  |        至少出现n次         |
| X{n,m} | 至少出现n次，但是不超过m次 |

（例子说明用法）

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20200207230843.png)



### 简单案例

这里的案例就是给一个字符串的数组，然后通过split把这个字符串数组变成一个int数组，用Arrays工具类排序之后输出。在输出这一个环节上出现一个有关内存优化的小细节：

![image-20200207231608445](C:\Users\lding\AppData\Roaming\Typora\typora-user-images\image-20200207231608445.png)

如果用 str += 的方法的话，每一次这个 str 都会指向一个新的内存地址，这样之前的字符串就会变成垃圾，如果这个数组很长的话会造成内存资源的浪费。所以这里可以使用 **StringBuilder** 来做这件事，它就在堆内存里创建一个对象，同时本身是一个可变的字符序列，占用更少的内存空间。



### replaceAll 用法

```java
public String replaceAll(String regex, String replacement)
```

这里系统会一个个去遍历这个字符串，然后看每一个字符串是不是符合正则表达式的要求，所以在案例中，无论多少个数字都会被替换掉。

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20200207232314.png)

### 正则表达式分组

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20200207232504.png)

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20200207235322.png)

这里先用

```java
String s2 = s.replaceAll("\\.", "");
```

把所有的点都替换掉，然后在下面第二次替换的时候，先利用

```java
"(.)\\1+"
```

把重复的字符全都获取到，比如获取到 "我我我我我我"，然后利用 $1 把这一堆 "我" 替换掉。这里的 $1 是指的第一组里的内容，也就是 " (.) " 里的内容。组数是由左向右来判断的，一个括号一个组。



### Pattern 和 Matcher

简单的用法介绍：

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20200208000135.png)



下面用一个简单的截取电话号的小案例来说明：

```java
String s = "我的手机号是18988888888，曾经用过18666668888，还用过18812345678";
String regex = "1[3578]\\d{9}"
Pattern p = Pattern.compile(regex);
Matcher m = p.matcher(s);

while(m.find()) {
    System.out.println(m.group());
}
```

regex中：

1 代表第一个数是 1

[3578] 代表第二位是3578中的一个

后面的 \\\d{9} 表示任意出现9个数字

之后用 Pattern 规定格式，并生成一个 Matcher

把这一串字符串丢给Matcher，然后用内置方法 find() 去寻找是否存在，如果存在的话，就会有指针指向第一段符合正则的字符串，然后存在一个地方，用 group() 就可以获取到那一串，再调用 find() 还会找到下一个符合规正则的字符串，直到最后没有了为止，会返回false



## Math类

 这有啥讲的... 记得有这个东西就行

有取PI常量、取绝对值、ceil/floor、二选一个最大值、平方、随机数、四舍五入、开根等功能。

## Random类

在这多说两句废话，这里的Random创造的一个对象生成的随机数，也是伪随机数，底层是先给构造函数一个seed，根据这个seed来生成随机数的。空参构造里是取 nanoTime 来作为 seed的。

Random里有一个方法很好用，可以很方便的随机取一个范围内的数：

```java
Random r = new Random();
r.nextInt(50);
```

这就会取得一个 0~49 之间的随机数，想取 20 ~ 59 之间的随机数的话

```java
int a = 20 + r.nextInt(40);
```

即可



## System类

几个可能常用的成员方法：

```java
public static void gc();
public static void exit(int status);
public static long currentTimeMillis();
public static void arraycopy(Object src, int srcPos, Object dest, int destPos, int length);

```



## BigInteger类

主要是用来装非常大的数的，可以实现加减乘除，取div和mod，好像也没什么说的，作为了解知道就可以了。

## BigDecimal类

同上，为了更高的精度，记得传进去的是字符串

```java
BigDecimal bd1 = BigDecimal.valueOf(2.0);
BigDecimal bd2 = new BigDecimal("1.1");
System.out.println(bd1.subtract(bd2));  // 0.9
```



## SimpleDateFormat 类

它是继承了DateFormat，DateFormat是个抽象类。

```java
Date d = new Date();
SimpleDateFormat sdf = new SimpleDateFormat();
System.out.println(sdf.format(d));  //yy-mm-dd AM/PM HH:mm
```

这里时间的显示的格式，用户是可以自定义的

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20200208231329.png)

```java
SimpleDateFormat sdf = new SimpleDateFormat("yyyy年MM月dd日 HH:mm:ss");

```

这里的四个y可以写成一个y，两个M可以写成一个M，其他同理，但是开发中还是建议写成四个/两个的形式。

然后它还可以把按照规则写的时间字符串转换成Date对象（Date类之前根本就没写，用的时候现查吧，感觉用到的不会很多）

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20200208232035.png)



## Calendar类

Date类过时了，用来替换Date的

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20200209000351.png)

这里其实是父类引用指向子类对象，实际产生的对象是 GregorianCalendar

Calender其他的一些方法：

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20200209000918.png)

---

## 可算结束了

哇，这一章可算结束了，这一月份生了病耽误了好多进程。太难顶了，要抓紧时间追进度了。

