---
title: Java Note P2 (黑马程序员）
date: 2019-10-15 23:39:14
categories: 
- Java
tags: 
- Java
- 后端
---


> 笔记内容全部来源于https://www.bilibili.com/video/av16284875

# P2 面向对象笔记

## — 2019/10/15 —

## P7 面向对象简易内存图解

之前只提到了内存中的栈和堆，讲到这里就出现第三块需要了解的区域：方法区

方法区也可以理解为代码仓库，.class文件会被加载到方法区。

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20191016002615.png)

大概步骤：

1. 先是第一个.class文件进入方法区
2. 调用mai()方法，main()方法压栈
3. 之后遇到Car类，加载Car.class进入方法区
4. 创建Car c1实例对象在堆(heap)中，初始化color和num，前者为null，后者为0
5. 之后给color和num赋值
6. 调用run()，run()方法压栈，输出color和num之后弹栈



## P10 成员变量和局部变量

```java
class Person {
    String name;  //成员变量
    int num;
    public void speak() {
        int num = 10;  //局部变量
    }
}
```

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20191017012506.png)

懒得打字了，打这些浪费时间

## P12 匿名对象

```java
class Myclass {
    public static void main(String[] args) {
        Car c1 = new Car();  //有名字的对象
        c1.run();
        
        new Car().run();  //匿名对象
    }
}

class Car {
    String color;
    int num;
    public void run() {
        System.out.println("车运行");
    }
}
```

一般来说，这样的匿名对象不会用来创造实际的对象出来，因为new出来之后并没有任何标签去引用，就会被垃圾回收机制回收。

那么匿名对象用来干锤子的？

可以提高代码的复用性

```java
public static void creatCar(Car cc) {
    cc.color = "red";
    cc.num = 8;
    cc.run();
}

//直接调用以下代码
creatCar(new Car());
```

## P18 Constructor（构造函数）

目前讲的已经变成常识了

需要注意一点的是：

**建议Constructor永远自己写，即使是空参Constructor，另外它也支持重载（同名不同参）**

---

创造一个对象的时候的内存图

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20191019221002.png)



## P25 static修饰词

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20191020172253.png)

在每一个类被传进方法区的时候，都会存在一个静态区，所有被static修饰的东西（方法，变量）都会进入到这个区域；每一个实例对象都会共享这一个static，以图中例子为例，两个人都是日本国籍，事先设定好country变量的话，那么以后无论生成多少个实例对象，他们的country的值都是日本。

### 简单介绍

1. 随着类的加载而加载
2. 优先于对象存在
3. 被类的所有对象共享
   - 举例：班级同学应该共用 一个班级编号
   - 饮水机（静态修饰）
   - 水杯（不能用静态修饰）
4. 共性用静态，特性用非静态
5. 可以通过类名调用
   - 推荐使用类名调用
   - 静态修饰的内容一半称为：类成员，类变量，类方法

### 注意事项

1. 静态方法中是没有this关键词 （写了直接报错）
   - 静态是跟随类的加载而加载的，this是随着对象的创建而存在的
   - 静态优先于对象
2. 静态方法只能访问静态的成员变量和静态的成员方法
3. 非静态方法就通用一点，静不静态都无所谓，要求不是很严格

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20191020212100.png)



## P30 工具类 （ArrayTool举例）

工具类，顾名思义是为了提供某种快捷的办法而创造出来的类。在使用工具类的时候大多数情况都是把当前工作类中的方法全部变成静态，且私有化构造方法，目的是阻止创建工具类的实例对象。直接调用此类会方便一点，具体见以下代码：

```java
//类文件

class ArrayTool {
    private ArrayTool() {}

    //得到一个数组中的最大值
    public static int getMax(int[] arr){
        int max = arr[0];
        for (int i = 1; i < arr.length; i++) {
            if (max < arr[i]) {
                max = arr[i];
            }
        }
        return max;
    }

    //遍历数组中的全部元素
    public static void print(int[] arr) {
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i] + " ");
        }
    }

    //反转数组
    public static void revArray(int[] arr) {
        for (int i = 0; i < arr.length / 2; i++) {
            int temp = arr[i];
            arr[i] = arr[arr.length - 1 - i];
            arr[arr.length - 1 - i]  =temp;
        }
    }
}
```

```java
//main函数

class Test {
    public static void main(String[] args) {
        int[] arr = {-1, -15, -28, -48, -21, -9, -5};
        ArrayTool.print(arr);  // 输出arr
        ArrayTool.revArray(arr);  // 对arr数组进行反转操作
        System.out.println();  // 换行
        ArrayTool.print(arr);  // 查看反转后的结果
        System.out.println();  // 换行
        System.out.println(ArrayTool.getMax(arr));  // 输出最大值
        //注意一点：最后 getMax() 是有返回值的，所以直接输出出来就完事
    }
}
```

**这一章节实际还讲了Java Doc的事，这里就不多说了，VSCode直接就会显示出来，而且 "@" 之后也会有各种需要说明的关键词pop出来。**

Java文档可以通过 javadoc 指令来生成一个网页版高大上的说明书。

**注意，类要用public修饰才可以读取到，才能创造Java Doc**

```
javadoc -d [路径] [需要显示的内容] [类名]
```

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20191020221459.png)

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20191020221607.png)

## P33 简单的Random介绍

这里使用了Java.lang中的Math类，直接使用就可以

```java
System.out.println(Math.random());
//输出一个0.00000 - 9.99999的数

(int)(Math.random() * 100)
//通过强转忽略小数点后的数，得到0 - 99的随机数
```

## — 2019/10/21 —

## P35 代码块讲解

### 1. 局部代码块

写在方法中的代码块

```java
{
    int x = 10;
    System.out.println(x);
}
System.out.println(x);
```

以上，x出了当前的这个代码块之后，内存被释放，第一个输出语句正常执行，第二个输出语句报错。

### 2. 构造代码块

```java
class Student {
    private String name;
    private int age;
    
    public Student() {}
    
    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    public void setName(String name) {
        this.name = name
    }
    
    public String getName() {
        return name;
    }
    
    {
        System.out.println("构造代码块");
    }
    
    static {
        System.out.println("静态代码块")
    }
}
```

**解释：**

**以上代码，当创造Student的实例对象的时候，无论有参还是无参创造，构造代码块中的内容都会优先于构造函数先执行。**

可以通过这一点来增加代码的复用性，比如有一些共有的特性或者输出等情况，可以先在结构代码块中优先执行。（很少用！！！）

### 3. 静态代码块

还是以以上代码为例，在第一次加载该类的时候，该类会先进入到方法区，然后读取这个类；static修饰的代码块会进入到静态区，被读取之后会直接执行，如果方法区没有开启垃圾回收机制（GC）的话，不会发生二次加载该类，所以当第二次创造该类实例对象的时候，也就不会将静态代码块执行两次了。**注意：静态代码块基本来说是最高优先级输出了，一个类进方法区后，读取静态区就会执行；main() 方法也不例外**

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20191021014933.png)

（这里学生学习实际就是构造代码块，空参和有参结构是在构造函数里输出的。以上是创造了两个Student的实例对象。）

**作用：用来给类进行初始化，比如可以用来加载驱动**



*同步代码块（多线程再讲）*

## P37 继承

### 1. 作用和使用

1. 可以高效的提高代码**复用性**和**维护性**
2. 可以让类与类之间产生关系，是**多态**的前提
3. 耦合性增强（说白了就是爹变儿子也变）

**开发时注意的原则：高内聚，低耦合**

- 内聚：自己完成某件事情的能力
- 耦合：类与类之间的关系

**继承关键字extend**，继承使用方法：

```java
class Dog extends Animal {}
```

见得一句话概括就是：子承父类，子可改变

### 2. 继承的特点

- Java只支持单继承，不支持多继承

  ```java
  class Test1 {
      int num = 1;
  }
  
  class Test2 {
      int num = 0;
  }
  
  class Test3 extends Test1, Test2 {}
  
  ```

  以上这种情况，因为两个父类中都有num成员变量，所以会发生冲突，在调用的时候无法判断应该调用哪一个来使用。

- Java支持多层继承，以上代码举例：Test3可以继承Test2，Test2又可以继承Test1。如此一来Test3继承了1和2的特点，2继承了1的。

### 3. 注意事项以及何时使用

- 子类**只能继承**父类所有**非私有**的成员（方法和变量）

- 子类不能继承父类的构造方法，因为构造方法的结构是：

  ```
  public [类名] {}
  ```

  子类此处倘若父类的话，这个类名也会继承过来，所以这样的操作不太行。（后续会讲super用法）

- 不要为了部分功能就去继承，举个例子：

  项目经理：姓名  工号  工资  奖金

  程序员：    姓名  工号  工资

  不能用项目经理去继承程序员，不合适。如果强行想继承的话，可以单独在开设一个大类叫员工，无论是项目经理还是程序员都是属于员工这个大的父类的。

  到底可不可以用继承可以如此判断：

  两个类A，和B，只有当他们复合A是B的一种，或B是A的一种的时候，就可以考虑是否使用继承。

### 4. this和super

```java
class Father {
    int num1 = 10;
    int num2 = 30;
}

class son extends Father {
    int num2 = 20;
    
    public void print() {
        System.out.println(this.num1);  //继承后this调用父类num1（本类无的情况）
        System.out.println(this.num2);  //就近原则
        System.out.println(super.num1);  //super直接调用父类的num1
    }
}
```

1. 成员变量：
   - this.成员变量：调用本类的成员变量，也可以调用父类的成员变量
   - super.成员变量：只可以调用父类的成员变量



### 5. 构造方法之间的关系

this()：调用本类中的构造方法

super()：调用父类中的构造方法

如果两个里面按格式传参，那就是调用该类中的有参构造方法；另外子类必定至少访问父类中的一个构造方法

子类中所有的构造方法默认都会访问父类中空参的构造方法

```java
class Father {
    public Father() {
        System.out.println("父类的构造方法")；
    }
}

class Son extends Father {
    public Son() {
        super()  //这里一条语句，如果不写系统默认加上，用来访问父类空参构造方法
        System.out.println("子类的构造方法")；
    }
}
```

**每一个构造方法的第一条语句默认都是 super() 用来访问父类空参构造方法**

**Object类是最顶层的父类**



### 6. 构造方法的注意事项

**友情提示：以下内容非常啰嗦非常长，想看测试过程的可以看看，否则直接跳过看结论**

**这里得多bb几句**

**Case 1： 父类成员变量非私有化且存在空参构造函数**

*这里只讨论传参的情况，不考虑空参构造，因为如果父类中的空参构造方法不存在的话，想要后面子类正确执行构造函数，那必须就要改写super为有参，所以还讨论个锤子的空参。*

这种情况下还要分三个小的情况

1. **若子类中成员变量不声明，那么成员变量的变动是适用于其子类和父类的**

   比如：

   ```java
   class Father {
       String name;
       int age;
       
       public Father() {
           System.out.println("父类的空参构造方法");
       }
       public Father(String name, int age) {
           this.name = name;
           this.age = age;
           System.out.println("父类的有参构造方法");
           System.out.println("父类中的name：" + name);
           System.out.println("父类中的age:" + age);
       }
       
   }
   
   class Son extends Father {
       
       public Son() {}
       public Son(String name, int age) {
           super(name, age + 5);
           System.out.println("子类的有参构造方法");
           super.age = 20;  //注意这里是通过super修改父类的成员变量，子类也会跟着变
           System.out.println("子类中的成员变量：");
           System.out.println(this.name);
           System.out.println(this.age);
       }
   }
   ```

   这种情况，name和age是一起变的，通过在main函数中调用get方法可以验证：

   ![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20191021193407.png)

2. **若子类中声明了成员变量且非私有：**

   ```java
   class Father {
       String name;
       int age;
       
       public Father() {}
       public Father(String name, int age) {
           this.name = name;  //给父类的初始化对象赋值，栈中无标签指向
           this.age = age;  //同上
           System.out.println("父类的有参构造方法");  //注释①
           System.out.println("父类中的name：" + name);
           System.out.println("父类中的age:" + age);
       }
   }
   
   class Son extends Father {
       String name;
       int age;
       
       public son() {}
       public Son(String name, int age) {
           super(name, age + 5);
           this.name = name;
           this.age = age;
           System.out.println("子类的有参构造方法");
           //super.age = 20;
           System.out.println("子类中的成员变量：");
           System.out.println(this.name);
           System.out.println(this.age);
           System.out.println("再次查看父类中的成员变量：");  //注释②
           System.out.println(super.name);
           System.out.println(super.age);
       }
   }
   ```

   ![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20191021195049.png)

3. **子类成员变量私有化**，和以上效果一样，不过私有化之后也就是提高了安全性，必须要用set和get方法操作，直接用 s.name 或者 s.age 没办法访问到



**以上总结：**

1. 当父类成员变量非私有化时，**子类若不重新声明**，直接继承的话，**当子类对成员变量进行修改的时候，实际上就是直接改的 super 区域里的值**。
2. 当父类成员变量非私有化时，**子类若重新声明成员变量**，无论是私有也好还是非私有，**新声明的变量是只属于子类的**。**想要访问到重新声明的成员变量的话，还需要重写 set/get 方法**。否则，直接调用 **s.getName()** 会返回**父类的初始化对象的name值**。



**Case 2：**父类的成员变量为私有化的时候

1. 子类中不声明成员变量

   ```java
   class Father {
       private String name;
       private int age;
   
       public Father() {
           System.out.println("父类的空参构造方法");
       }
       
       public Father(String name, int age) {
           this.name = name;
           this.age = age;
           System.out.println("父类的有参构造方法");
           System.out.println("父类中的name：" + name);
           System.out.println("父类中的age:" + age);
       }
   }
   
   class Son extends Father {
   
       public Son() {
           super();
           System.out.println("子类的空参构造方法");
       }
       
       public Son(String name, int age) {
           super(name, age + 5);
       }
   }
   ```

   此处有点特殊，因为Son类并没有继承Father类中的name和age，也没有自己声明。在执行super(name, age + 5)之后，会给Father的初始化对象中的成员变量赋值，然后通过 s.getName() 调用Father中的方法，以此得到Father初始化对象中的name值。(其实就是把父类成员变量私有化了，不给访问而已，还存在继承体系的 super 区里)

   

2. 子类声明变量

   ```java
   class Father {
       private String name;
       private int age;
   
       public Father() {
           System.out.println("父类的空参构造方法");
       }
       
       public Father(String name, int age) {
           this.name = name;
           this.age = age;
           System.out.println("父类的有参构造方法");
           System.out.println("父类中的name：" + name);
           System.out.println("父类中的age:" + age);
       }
   }
   
   class Son extends Father {
   	String name;
       String age;
       public Son() {
           super();
           System.out.println("子类的空参构造方法");
       }
       
       public Son(String name, int age) {
           super(name, age + 5);
       }
   }
   ```

   ![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20191021212252.png)



**两种情况结合在一起的结论：最重要需要记住一点就是，如果子类没有重写的话，所有的都随着父类来。**

**重点：**执行 Son s = new Son(name, age)的时候，实际上name和age两个参数是传给了堆内存中**super区**，此时在栈内存中**标签s**并不是指向堆内存中的**Father初始化对象**（这里没修改，做个纪念...hhh，当时自己学这块的时候，自己猜到了好像有这么个super区），Son类只是继承了Father中的方法，由于Son中并没有这些方法，所以去到父类找这些方法。再然后Father中的成员变量如果是非私有的，那么子承父类，成员变量之间是互通的，如果想单独让子类自己有自己的变量，需要重写，而且想要获取到的话要重写get/set方法。如果Father中成员变量是私有的，那子类怎么调用方法，得到的值都是父类的那个初始化对象中的变量值，**除非重写！除非重写！除非重写！除非重写！除非重写！**



### 7. 代码块练习题

```java
public class FatherClass {
    public static String staticVariable = "我是父类静态变量";
    static {
        System.out.println(staticVariable);
        System.out.println("我是父类静态代码块");
    }

    public String memberVariable = "我是父类成员变量";
    {
        System.out.println(memberVariable);
        System.out.println("我是父类普通代码块");
    }

    public FatherClass() {
        System.out.println("我是父类构造函数");
    }
}
}


public class SonClass extends FatherClass {
    public static String staticVariable = "我是子类静态变量";
    static {
        System.out.println(staticVariable);
        System.out.println("我是子类静态代码块");
    }

    public String memberVariable = "我是子类成员变量";
    {
        System.out.println(memberVariable);
        System.out.println("我是子类普通代码块");
    }

    public SonClass() {
        System.out.println("我是子类构造函数");
    }
}
```

**输出结果如下：**

```
我是父类静态变量
我是父类静态代码块
我是子类静态变量
我是子类静态代码块
我是父类成员变量
我是父类普通代码块
我是父类构造函数
我是子类成员变量
我是子类普通代码块
我是子类构造函数
```

**执行顺序：**

父类静态变量、父类静态代码块（加载顺序同声明的先后顺序） -> 子类静态变量、子类静态代码块（加载顺序同声明的先后顺序） -> 父类成员变量、父类普通代码块（加载顺序同声明的先后顺序） -> 父类构造方法 -> 子类成员变量、子类普通代码块（加载顺序同声明的先后顺序） -> 子类构造方法

（静态的内容，只随着第一次创建对象的时候加载进静态区，第二次创建的对象的时候就不会再执行了）

### 8. 重写

- 重写：子父类出现了一模一样的方法（注意：返回值类型可以是子父类，在后面的笔记还会重提）
- 应用：当子类需要父类的功能，而功能主体子类有自己特有的内容时，可以重写父类中的方法。这样，既沿袭了父类的功能，又定义了子类特有的内容。
- 作用：一般可以来做功能的升级，维护等



### 9. 重写注意事项

- 父类中私有方法不能被重写（因为都没继承过来）但是你在子类中写了就变成子类特有的了，即使名字一样

- 子类重写父类方法时，访问权限不能更低（最好一致，都用public就完事了，特殊情况除外）

- 父类的静态方法，子类也必须通过静态方法重写

  *\* 其实这里也不能算是重写，但是现象，表现出来的是这么回事，至于为什么不能算重写，多态的时候会再提*

## —2019/10/22—

## P53 final关键字/方法和变量

final简单介绍：

- final修饰过的类不能被继承
- final修饰过的变量会变成常量，只能被赋值一次
- final修饰过的方法不能被重写

注意事项：

- final修饰的变量名字全大写，下划线连接
- final一般都会和static搭配在一起使用



```java
//假设有Person类且有set/get方法
public static void main(String[] args) {
    final int num = 10;
    
    final Person p = new Person("张三", 23);
    p.setName("李四");
    p.setAge(24);
}
```

以上，final修饰**基本数据类型**的时候，是**值不可以被 改变**；当修饰**引用数据类型**的时候，是**地址值不能被改变**，换句话说上面如果 p = new xxxx 这样的语句出现就会报错。

```java
class Demo {
    final int num;
    public Demo() {
        num = 10;
    }
    public void print(){
        System.out.println(num);
    }
}
```

final修饰变量的初始化时机，这里需要注意两点：

1. final修饰之后的变量必须要给值，显示初始化
2. 如果没给的话，需要在构造方法结束之前完成，换句话说可以在构造方法中实现（如上所示）

## P56 多态

### 1. 简单介绍

多态表示事物存在的多重形态，比如猫既可以说成是猫，也可以说成是动物。**多态的存在是有前提的：**

1. 要有继承关系
2. 要有方法重写
3. 要有父类引用指向子类对象

```java
class Animal {
    public void eat() {
        System.out.println("动物吃饭");
    }
}

class Cat extends Animal {
    public void eat() {
        System.out.println("猫吃鱼")
    }
}

public static void main(String[] args) {
    Cat c = new Cat();  //猫是一只猫
    c.eat();
    
    Animal a = new Cat();  //猫是一只动物
    //父类引用指向子类对象
    //这里a是猫，调用的方法都是从Animal继承来的
    //eat() 方法的具体内容要看有没有重写
}
```

### 2. 成员访问特点：变量

先上个图和代码再解释...

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20191022214049.png)

```java
class Father {
    int num = 10;
}

class Son extends Father {
    int num = 20;
}

Father f = new Son();
System.out.println (f.num);  //输出结果为 10
Son s = new Son();
System.out.println(s.num);  //输出结果为20
```

破案了...

如果说一开始新建一个实例对象的时候，若最开始用父类声明，那么成员变量走的是父类的；相反，用子类声明的那么成员变量走的是子类的。

super区域区域就是之前自己猜想的和子类实例对象绑定的一块区域...

### 3. 成员访问特点：方法 

还是先写一个一段代码来解释...

```java
class Father {
    int num = 10;
    public void print("father");
}

class Son extends Father {
    int num = 20;
    public void print("son");
}

Father f = new Son();
f.print();  //此处输出为son

```

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20191022222004.png)

这里的流程是先检查父类中存不存在该方法（print()），保证编译通过，然后去子类找该方法。



### 4. 成员访问特点：静态方法（总结）

直接给答案不废话：

成员变量

编译看左边（父类），运行看左边（父类）

成员方法

编译看左边（父类），运行看右边（子类），这也是动态绑定

静态方法

编译看左边（父类），运行看左边（父类）

（静态和类相关，算不上重写，静态方法写在哪个类中就属于哪个类，自己的就是自己的；只有非静态的成员方法，编译看左边，运行看右边）

这里的编译也就是刚开始调用，系统去**检查这个变量/方法是否在父类中**的这个过程。

### 5. 超人的故事

```java
class Person {
    String name = "John";
    public void 谈生意() {
        System.out.println("谈生意");
    }
}

class SuperMan extends Person {
    String name = "SuperMan";
    public void 谈生意() {
        System.out.println("谈几个亿的大单");
    }
    public void fly() {
        System.out.println("飞出去救人");
        //这里犹豫父类没有fly方法，会用到转型后面会讲
    }
}

Person p = new Superman();
System.out.println(p.name);
p.谈生意();
//p.fly();转型
```

超人这个例子很好的解释了多态的应用：

比如这里有一个人去谈生意，互相自我介绍，对方告诉我，他的名字叫John，然后开始和我谈生意。谈着谈着就突然有个人要跳楼，他就和我说他是个超人，现在要飞出去救人。

**注意：**这里直接调用p.fly()报错，因为父类中没有该方法。

### 6. 向上向下转型

```java
Person p = new SuperMan();//  向上转型
System.out.println(p.name);  //输出结果为John
p.谈生意();  //输出结果为“谈几个亿的大单子”
SuperMan sm = (superMan)p;  //向下转型
sm.fly();
```

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20191030003327.png)

### 7. 转型的好处和弊端

向上转型的弊端：无法使用子类特有的属性和行为

好处：

1. 提高代码的维护性
2. 提高代码扩展性

```java
public static void main(String[] args) {
    // Animal a = new Cat(); 
    /* 开发的时候很少在创建对象的时候赢用父类引用指向子类对象，
     * 直接创建子类对象更方便，可以直接使用子类中特有的方法和属性。
     * 一般 只有当做参数的时候用多态会多一点，因为扩展性强
     */
    //Cat c = new Cat();
    //c1.eat();
    method(new Cat());
    method(new Dog());
}

public static void method(Animal a) {
    /* 这里parameter如果是写Cat类或者Dog类的话
     * 就需要定义很多个method
     * 通过父类引用指向子类对象来实现这个parameter
     * 可以传所有继承过Animal的类
    */
    a.eat();
}
```

```java
//定义三个类
class Animal {
    public void eat() {
        System.out.println("动物吃饭")；
    }
}

class Cat extends Animal {
    public void eat() {
        System.out.println("猫吃鱼");
    }
    
    public void catchMouse() {
        System.out.println("抓老鼠")
    }
}

class Dog extends Animal {
    public void eat() {
        System.out.println("狗吃肉");
    }
    
    public void lookHome() {
        System.out.println("看家");
    }
}
```

这样的写法其实也是有弊端的，父类中只有一些非常通用的行为还好，比如“吃饭”，“睡觉”之类的，但是如果父类中在把“抓老鼠”这样的行为加进去就很不合适。比如我创建一个猪的类，一旦继承的话，猪岂不是就要去抓老鼠了？

但是想要通过method方法把新生成的实例对象的行为展现出来，还可以在method里用instanceof判断传进去的具体是什么类，详细如下

```java
public static void method(Animal a) {
    if (a instanceof Cat) {
        Cat c = (cat)a;
        c.eat();
        c.catchMouse;
    }
    else if (a instanceof Dog) {
        Dog d = (dog)d;
        d.eat();
        d.lookHome();
    }
    else {
        a.eat();
    }
}
```

一般来说比较少的这样使用，开发中父类中写的方法一般子类中都有；如果非要想用子类中特有的方法的话，可以通过强转来实现。

### 8. 题目分析

直接上代码

```java
class A {
    public void show() {
         show2();
    }
    public void show2(){
        System.out.println("我")；
    }
}

class B extends A {
    public void show2(){
        System.out.println("爱")；
    }
}

class C extends B {
    public void show() {
         super.show();
    }
    public void show2(){
        System.out.println("你")；
    }
}
```



第一种情况分析：

```java
A a = new B();
a.show();
```

注意，这里会在先检查A中有没有show方法，然后如果有的话去B里看，B里没有，所以直接调用A中的show()；注意了，A中show()调用了show2()，是调用给了B中的show2()。

换个思路思考这个问题：继承本来就是子承父类，那B把show()继承下来了，自然也就是调用B中的show2()了，暂且先不去考虑A中有没有show()方法。



第二种情况分析：

```java
B b = new C();
b.show();
```

一开始程序会选去B找show()方法，发现没有去A中找，找到之后确定父类中有这个方法之后，去检查C中有没有重写，发现重写之后调用super.show()，调用B中的show()，B继承了A的show()方法，然后继续调用show2()，最后输出结果为“你”

## P64 抽象类

### 1. 抽象类的特点

- 抽象类和抽象方法都必须有abstract关键字修饰

```java
abstract class A {}
```

```java
public abstract void eat();
```

- 抽象类中不一定有抽象方法，但是有抽象方法的一定事在抽象类中，或者是接口(interface)中

- 抽象类不能实例化，但是可以通过父类引用指向子类对象（多态）来实现

  ```java
  abstract class Animal {
      public abstract void eat();
  }
  
  class Cat extends Animal {
      public void eat() {
          System.out.println("猫吃鱼");
      }
  }
  ```

  这里需要注意一点：一旦需要继承抽象类的话，必须要重写抽象类中所有的抽象方法。



### 2. 老师实例

```java
abstract class Teacher {
    private String name;
    private int age;
    
    public Teacher() {}
    public Teacher (String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    public void setName(String name) {
        this.name = name;
    }
    
    public void setAge(int age) {
        this.age = age;
    }
    
    public String getName() {
        return this.name;
    }
    
    public int getAge() {
        return this.age;
    }
    
    public abstract void teach();
}

class BaseTeacher extends Teacher {
    public BaseTeacher() {}
    public BaseTeacher(String name, int age) {
        super(name, age);
    }
    
    public void teach() {
        System.out.println("姓名:" + name ",年龄:" + age + "讲的内容是Java基础");
    }
}
```

这里就是规范了Teacher的很多共性的东西，然后因为基础班老师和就业班老师讲得内容不一样，所以teach要抽象化，其他继承Teacher的类都需要重写teach方法。

### 3. 两道面试题

**面试题1：**一个抽象类如果没有抽象方法，可不可以定义为抽象类呢？如果可以，有什么意义？

A1：可以，这么做的目的只有一个，就是不让其他的类创造本类的实例对象，想要创造需要交给子类完成。

**面试2：**abstract不能和哪些关键字共存？

**abstract和static：**

被abstract修饰的方法没有方法体

被static此时的可以用“类名.”调用，但是，“类名.”调用抽象方法是没有意义的



**abstract和final：**

被abstract修饰的方法强制子类重写

被final修饰的不让子类重写，所以他们两个矛盾



**abstract和private：**

被abstract修饰的是为了让子类看到并且强制重写

被private修饰的不让子类访问，所以他们是矛盾的



## P71 Interface

接口是通过implements关键词实现的

```java
class 类名 implements 接口名 {}
```

（其实这也是一种多态的表现形式）

接口的子类可以是抽象类，但是没什么实际意义。

大多数都是创建实体类，然后重写接口中的所有抽象方法。接口中的方法全都是抽象方法！如果没写abstract关键词的话，系统会默认给出。

接口中没有构造方法，所以当一个类implements一个接口之后，这个类若不继承其他的父类，会默认继承最高级父类：Object类

**接口中定义的成员变量都是常量，final修饰的**

**接口中定义的成员变量也都被static修饰，换句话说可以用“接口名.”来调用到**

**接口中定义的成员变量是要被访问到的，所有还默认有public修饰**

（以上三个修饰词可以交换顺序，建议初学者写出来）

### 1. 类，接口，关系

类与类之间是继承关系，**只能单继承，可以多层继承。**

类与接口之间是实现关系，**可以单实现，也可以多实现。**

接口与接口是继承关系，可以**单继承，也可以多继承。**



### 2. 接口和抽象类

**抽象类：**

​		成员变量：可以变量，也可以常量

​		构造方法：有

​		成员方法：可以抽象，也可以是非抽象

**接口：**

​		成员变量：只可以是常量

​		成员方法：只可以是抽象方法

**设计理念区别：**

​		抽象类：被继承体现的是“is a”的关系，抽象类中定义的是该继承体系的共性功能

​		接口：被实现体现的是“like a”的关系。接口中定义的是该继承体系的扩展功能



### 3. 实际案例

**定义猫类**

```java
class Cat extends Animal {
    
    public Cat() {}
    public Car() {String name, int age} {
        super(name, age);
    }
    
    public void eat() {
        System.out.println("猫吃鱼");
    }
    
    public void sleep() {
        System.out.println("侧着睡");
    }
}
```

**定义一个特殊的猫类**

```java
class JumpCat extends Cat implements Jumping {
    
    public JumpCat() {}
    public JumpCat (String name, int age) {
        super(name, age);
    }
    public void jump() {
        System.out.println("猫跳高");
    }
}
```

**定义向上抽取的大类**

```java
abstract class Animal {
    private String name;
    private int age;
    
    public Animal() {}
    public Animal (String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    public void setName(String name) {
        this.name = name;
    }
    
    public void setAge(int age) {
        this.age = age;
    }
    
    public String getName() {
        return this.name;
    }
    
    public int getAge() {
        return this.age;
    }
    
    public abstract void eat();
    
    public abstract void sleep();
}

interface Jumping {
    public void jump();
}
```

**构造方法不继承！！**

**构造方法不继承！！**

**构造方法不继承！！**



## P76 Package

### 1. 包的简单概述

也没啥概述的啊，直接上个图

简单易懂，不多bb

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20191107064143.png)



### 2. 定义包的格式

\*	package 报名;

\*	多级包用 “.” 分开即可 



package语句必须是程序的第一条可执行的代码

package预计在一个java文件中只能有一个



### 3. 创建包

```java
package come.heima;
class Demo1_Package {
    public static void main(String args[]) {
        System.out.println("Hello World!");
    }
}
```

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20191107065923.png)

这个带package的文件编译之后，生成的.class文件必须要在这个路径下。如果一开始的时候这个路径不存在的话，通过编译，系统会自动给你创建这个路径，然后这个类也会在这个路径下。

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20191107070302.png)

以后用eclipse就会比较方便了

### 4. 包与包之间的访问

第一个包中的类：

```java
package com.heima;
class Demo1_Package {
    public static void main(String[] args) {
        come.baidu.Person p = new com.baidu.Person("张三", 23);
    }
}
```

第二个包中的类：

```java
package com.baidu;
public class Person {
    private String name;
    private int age;
    
    public Person() {}
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
}
```



**注意事项：**

1. 在com.heima的包中引用Person的时候，一定要写全类名：com.baidu.Person

2. com.baidu.Person这个类前要有public修饰才有权限引用

3. 同理Person中的Constructor也需要public修饰，否则在new的时候没办法调用Constructor一样没权限

4. 如果使用了import语句：

   ```java
   import com.baidu.Person;
   ```

   就可以不使用全类名，在创建实例对象的时候，可以去掉com.baidu.

**以上，其实体现了封装的一个特性，很明显了。**



## P81 4种权限修饰符

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20191109223923.png)



public权限最大，被public修饰的外部不论在不在这个包下的，都可以通过导包之后直接用。

protected其次，比如子类继承父类（是前提），无论这个子类是不是在同一个包下，都可以继承到父类的方法并且使用；但是如果在其他的包中，没有继承该父类的一个无关类，导包之后想调用是不允许的。

默认什么都不加的话，只要是一个包内，什么都行，不在同一个包内，什么都不行。

private只能在本类中使用。

**放一个总结表：**

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20191110011823.png)



## P83 内部类

### 1. 简单概述

\- 1 内部类可以直接访问外部类成员，包括私有

\- 2 外部类要访问内部类的成员必须创造对象

\- 3 外部类名.内类名 对象名 = 外部类对象.内部类对象

```java
class Outer {
    private int num = 10;
    class Inner {
        public void method() {
            System.out.println("Hello World!");
        }
    }
}
```

**测试类：**

```java
public static void main(String[] args) {
    Outer.Inner oi = new Outer().new Inner();
    oi.method;
}
```



### 2. 私有内部类

```java
class Outer {
    private int num = 10;
    private class Inner {
        public void method() {
            System.out.println(num);
        }
    }
    
    public void print() {
        Inner i = new Inner();
        i.method;
    }
}
```

当内部类被私有的时候，想要创建内部类对象的话，无法通过Outer.Inner oi = new Outer().new Inner()来实现，需要在外部类中做操作

创建一个print方法，在print中创建一个Inner的对象，之后在调用method方法。



### 3. 内部类访问外部类成员变量

```java
class Outer {
    public int num = 10;
    class Inner {
        public int num = 20;
        public void show() {
            int num = 30;
            System.out.println(num);  // 输出30
            System.out.println(this.num);  // 输出20
            System.out.println(Outer.this.num);  // 输出10
        }
    }
}
```

这没啥讲的，就一个Outer.this就能访问到了

### 4. 局部内部类

```java
class Outer {
    public void method() {
        class Inner {
            public void print() {
                System.out.println("Hello World!");
            }
        }
        
        Inner i = new Inner();
        i.print();
    }
}
```

局部内部类，**只能通过**内部类所在的方法中创造对象然后调用内部类的方法。

以上代码，如果在method里添加一个 final num = 10;

然后在print方法里输出这个num，是有一些讲究的。

这个num必须要用final来修饰，让num变成常量，这样num就会进入到方法区中的常量池，可以延迟声明周期。放一个内存图

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20191110204131.png)

引用视频中的话：

因为当调用这个方法的时候，局部变量如果没有用final修饰，他的生命周期和方法的生命周期是一样的，当方法弹栈，这个局部变量也会消失。那么如果局部内部类对象还没有马上消失而且想用这个局部变量的时候，就没有了，如果用final修饰，在类加载的时候这个常量会进入常量池，即使方法弹栈，常量池的常量还在，还可以继续使用。

（1.8取消了，不加final也可以）

### 5. 匿名内部类

```java
interface Inter {
    public void print();
}

class Outer {
    class Inner implements Inter {
        public void print() {
            System.out.println("print");
        }
    }
    
    public void method() {
        //Inner i = new Inner;
        //i.print();
        
        new Inter() {
            public void print() {
                System.out.print("print");
            }
        }.print();
    }
}
```

new Inter() {} 直接创造的是子类对象，大括号里的一坨看成一个整体；然后直接调用print()方法。

感觉就是和匿名对象的感觉一样，也都是 new 类名() 然后啥啥啥的，只不过这里是一个抽象的接口，需要重写，所以就出现了格式上视觉的差别。

匿名内部类也可以重写多个方法，但是强烈不建议这么写，每次的重写都要写一堆然后调用，虽然可以通过

```
Inter i = new Inter() {}
```

这样的语句来让它看着好看一点，但是实际还是不方便。开发中不会这样操作的。开发中主要用匿名内部类传参使用。

