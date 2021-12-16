---
title: Java Note P5 (黑马程序员)
date: 2020-05-21 19:57:31

categories: 
- Java
tags: 
- Java
- 后端
---

>笔记内容全部来源于 https://www.bilibili.com/video/BV17W411a764 



# I/O流



先介绍了 FileReader，大概的使用方法和 FileInputStream 那面基本是一样的，越是要写路径，也有 read 方法。字符流之所异能读到中文，是有GBK码表的支持。中文的第一个字节，一定是负数，那么在读的时候，如果碰到负，则一次读两个字节，这样保证中文的读取不会发生字节流那面缺码的情况。FileReader 在读到每一个字符的字节之后，可以通过 char 强转来得到内容。

FileReader 注意一定一定注意闭流，因为最上层的父类在读字节的时候也是用一个 byte 数组实现的，如果不 close 的话，内容写不出来。一般字符流更多的使用在只读或者只写的情况中，因为如果是拷贝的话，效率不是很快，虽然可以这么操作但是不推荐。

字符流只能拷贝纯文本，音频图片这样的文件格式是不能拿来用的，因为在读的时候，虽然能读到每一个字节，但是字节不一定能去码表中找到对应的字符，在写出去的时候就会出现数据缺失导致文件损坏的情况。



### 小数组拷贝

这里这个小数组的用法和字节流那面一样，多写一遍加深印象



```java
public static void main(String[] args) {
    FileReader fr = new FileReader("xxx.txt");
    FileWriter fw = new FileWriter("yyy.txt");
    
    char[] arr = new char[1024];
    int len; // 这个 len 表示读到的这个 char 数组里的有效字节数
    while ((len = fr.read(arr)) != -1) {
        fw.write(arr, 0, len);  // 从第 0 位读 len 个
    }
    
    fw.close();
    fr.close();
}
```



### BufferedReader / Writer

（和字节流那面基本用法是一样的，byte 数组大小 8192）

这里有两个比较好用的方法，一个是 readLine() 一个是 newLine()，具体用法：

**（读）readLine()**

```java
public static void main(String[] args) {
    BufferedReader br = new BufferedReader(new FileReader("zzz.txt"));
    String line;
    
    while ((line = br.readLine()) != null) {
        System.out.println(line); // 这里如果不加 ln 则全部一整行输出
    }
    br.close();
}
```

readLine() 方法读一整行，直到碰到 '\n' 或者 '\r' 为止，且  '\n' 和 '\r' 不会读过来，所以再输出的时候如果不加 ln 的话就会全都输出在一行。

 

**（写）newLine()**

这是个适配全平台的方法，不同平台下的换行操作符不一样。

```java
public static void main(String[] args) {
    BufferedReader br = new BufferedReader(new FileReader("zzz.txt"));
    BufferedWriter bw = new BufferedWriter(new FileWriter("copy.txt"));
    String line;
    
    while ((line = br.readLine()) != null) {
        bw.write(line);
        bw.newLine();  // 这里会在结尾处写上一个换行操作符
    }
    br.close();
}
```



**在用流的时候注意事项：尽可能的做到 ”晚开早关“ ，什么时候用什么时候快，用完尽快关。**



### LineNumberReader

它继承了 BufferedReader，同时加了一个行号功能，可以知道目前读到了哪一行，也可以规定从规定的某一行开始往下读。

```java
public static void main(String[] args) {
    LineNumberReader lnr = new LineNumberReader(new FileReader("zzz.txt"));
    String line;
    lnr.setLineNumber(100);
    while((line = lnr.readLine()) != null) {
         System.out.println(lnr.getLineNymber() + " : " + line);
    }
    lnr.close();
}
```



### 使用指定码表读写

```java
BufferedReader br = 
    new BufferedReader(new InputStreamReader(new FileInputStream("utf-8.txt", "utf-8")));
BufferedWriter bw = 
    new BufferedWriter(new OutStreamReader(new FileOutStream("gbk.txt", "gbk")));
```



图解：

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20200523025505.png)



### 获取文本中每一个字符出现的次数

```java
public static void main(String[] args) {
    // 1. 创建带缓冲的输入流对象
    BufferedReader br = new BufferedReader(new FileReader("zzz.txt"));
    // 2. 创建双列集合对象
    TreeMap<Character, Integer> tm = new TreeMap<>();
    // 3. 将读到的存到双列集合中并且根据情况进行判断
    int ch;  // 这个 ch 得到的是每一个字节对应的码
    while ((ch = br.read()) != -1) {
        char c = (char)ch;  // 这一步把刚才得到的 ch 根据码表进行强转
        tm.put(c, !tm.containsKey(c) ? 1 : tm.get(c) + 1);
    }
    // 关流
    br.close();
    
    for (Character key : tm.keySet()) {
        switch (key) {
            case '\t':
                bw.write("\\t" + " = " + tm.get(key));
                break;
            case '\n':
                bw.write("\\n" + " = " + tm.get(key));
                break;
            case '\r':
                bw.write("\\r" + " = " + tm.get(key));
                break;
            default:
                bw.write(key + " = " + tm.get(key));
                break;
        }
        bw.newLine();
    }
}
```



### 序列流（SequenceInputStream）

它可以把两个 InputStream 合在一起，先从第一个流开始读，之后读第二个，在调用 close 方法的时候会把两个流正常关闭，不用担心这个问题。

```java
public static void main(String[] args) {
    FileInputStream fis1 = new FileInputStream("a.txt");
    FileInputStream fis2 = new FileInputStream("b.txt");
    SequenceInputStream sis = new SequenceInputStream(fis1, fis2);
    FileOutputStream fos = new FileOutputStream("c.txt");
    
    int b;
    while ((b = sis.read()) != -1) {
        fos.write(b);
    }
    
    sis.close();
    fos.close();
}
```



当多个流合并的时候，可以用 Vector 来存每一个输入流对象，然后调用 Vector 里的 element 方法会返回一个枚举对象，把这个对象传给序列流作参，就可以实现多个流拼接。



### ByteArrayOutputStream

这个其实就是把读过来的一些东西，暂时写在内存里的一小块缓冲区里，在 write 的时候不是向硬盘中写出数据，而是存在自己的一块缓冲区里，且这个缓冲区满了之后会自己扩容。

```java
public static void main(String[] args) {
    FileInputStream fis = new FileInputStream("a.txt");
    ByteArrayOutputStream baos = new ByteArrayOutputStream();
    
    int b;
    while ((b = fis.read()) != -1) {
        baos.write(b);  // 这里是向缓冲区里写入内容
    }
    
    byte[] arr = baos.toByteArray();  // 这个可以改变码表
    System.out.println(new String(arr));
    
    System.out.println(baos.toString());  // 这里使用的是平台默认码表
    fis.close();
    
    // 注意: bos.close() 可以不写，因为根本没有流通道到硬盘上；内存会自动释放
    
}
```



### 随机流（RandomAccessFile）

这东西可以规定在特定的某一个位置去读和写，在关联某一个文件的时候，要再第二个参数设置 mode，分别有 "r", "rw", "rws", 和 "rwd" 四个模式。

```java
public static void main(String[] args) {
    RandomAccessFile raf = new RandomAccessFile("a.txt", "rw");
    raf.write(97);
    int x = raf.read();
    raf.seek(10);  // 把指针指到index为 10 的位置
    raf.write(98);
    raf.close();
}
```

这个可以应用在多线程下载上，在一个文件的不同位置开启新的线程。



### ObjectOutput/InputStream

这里先引入一个概念叫序列化和反序列化，序列化就好比玩游戏的时候存档这样的操作，是按照某一种规则的写入。而反序列化，则是按照某一种规则读出。在 ObjectOutputStream 中，会把对象序列化，存储在一个文件中，方便下次读取该对象的信息。

```java
public static void main(String[] args) {
    Person p = new Person("张三", 23);
    
    ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("a.txt"));
    oos.writeObject(p);
    oos.close();
}
```

这里注意，要被序列化的对象的类，需要实现 Serializable 接口，该接口内无任何 method，它只是起到一个标签化的功能。切记一点，序列化的是一个对象当时的状态，当时的状态，当时的状态，所以牢记存档这个概念。它不是把类级别的内容序列化了。

实现了 Serializable 接口的类注意要加入一个叫 serialVersionUID 的 field，这个东西主要是用来记录改类每次改变之后的版本号的。比如定义了一个Person 类，然后一开始只有 name 和 age 属性，version 为 1L；后来加了一个 gender 属性，version 就为 2L。

```java
private static final long serialVersionUDI = 1L;
```



在读回对象的时候，注意要强转会所需的类型，而且要注意一个问题，如果只是单纯写好几个对象的话，在读的时候是没办法确定到底写了多少个对象的。所以我们可以考虑用一个容器把我们想写的对象都装进去，然后把这个容器对象写出去。

```java
public static void main(String[] args) {
    Person p1 = new Person("张三", 23);
    Person p2 = new Person("李四", 24);
    ArrayList<Person> arr = new ArrayList<>();
    arr.add(p1);
    arr.add(p2);
    
    ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("a.txt"));
    oos.writeObject(arr);
    oos.close();
    
    ObjectInputStream ois = new ObjectInputStream(new FileInputStream("a.txt"));
    ArrayList<Person> result = (ArrayList<Person>)ois.readObject();
    ois.close();
}
```

以上，就可以读取到装 Person 对象的 ArrayList 容器，只需要对其遍历即可。



### 数据输入输出流

DataInput/OutputStream 是可以一次写更多的字节，比如在 FileOutputStream 中写一个 fos.write(997) 的时候，他会把 997 转成二进制形式，然后用补足到 32 位，往外写的之后只取最后八位，所以就会额外砍掉两个，导致读回来的时候不是 997 的这个数了。这时候就可以用 DateInput/OutputStream 来实现这个

```java
dos.writeInt(997);
dis.readInt();
```



### 打印流

这里说两个，一个是 PrintStream，一个是 PrintWriter。 PrintStream 其实就是 System.out 这个东西，实在很对控制台而言的，不仅有 println 等方法，也有 write 的方法；println 等一些列方法打印出去的是字符串，而 write 实际是去查表，然后把内容显示出来，PrintStream 打印的是字节楼。PrintWriter 的话，也是把一些信息 print 到一个文件当中，比如可以调用其 println 方法和 write 方法；PrintWriter 在构造函数中，其中一个可以传入一个叫 autoFlush 的参数，这个参数仅仅是针对 println 方法而言的，如果是 true 的话，每调用一次 println，那么就会把缓冲区的内容写出去并清空，调用其他的方法不起作用，所以还是要记得关流!!!

**这两个都是往外输出的一个过程，只操作数据目的。**

**PrintStream：**

```java
public static void main(String[] args) {
    PrintStream ps = System.out;
    ps.println(97);  // 底层通过 Integer.toString() 把97以字符串形式打印出来
    ps.write(97);  // 查表了
    
    Person p = new Person("Johnathon", 22);
    ps.println(p);  // 判断是不是 null，是则输出 null，否则默认调用对象的 toString（）方法
    ps.close();
}
```



**PrintWriter：**

```java
public static void main(String[] args) {
    PrintWriter pw = new PrintWriter(new FileOutputStream("a.txt"), true);
    pw.print(97);  // 这次 print 方法只是把内容写到了缓冲区中
    pw.println(98);  // 因为 autoFlush 是true，在把内容写进缓冲区后进行 flush
    pw.close();  // 记得关流!!!
}
```



### 设置标准输入输出流

可以更改 System.in 和 System.out

```java
public static void main(String[] args) throws IOException {
        System.setIn(new FileInputStream("xxx.txt"));
        System.setOut(new PrintStream("copy.txt"));

        InputStream is = System.in;
        PrintStream ps = System.out;

        int b;
        while ((b = is.read()) != -1) {
            ps.write(b);  // 这里也可以用 println 方法，但是要注意 char 强转
        }

        is.close();
        ps.close();

    }
```



### Properties

它是个双列集合，一般都是用来存储配置文件的，没有对 key 和 value 的泛型规定，一般都是存 String的；继承了 HashTable 所以也可以当成一个 map 来使用，可以用 put 和 get 方法。

Properties有它一些特有的方法，比如 setProperty 和 getProperty (其实效果和 put，get一样)，在便利的时候可以调用 propertyNames 的方法得到一个 Enumeration 的对象，然后对其遍历。

```java
public static void main(String[] args) {
    Properties prop = new Properties();
    prop.setProperty("name", "张三");
    prop.setProperty("tel", "12345678");
    
    Enumeration<String> en = (Enumeration<String>) prop.propertyNames();
    while (en.hasMoreElements()) {
        String key = en.nextElement();
        String value = prop.getProperty(key);
        System.out.println(key + " = " + value);
    }
}
```



Properties 有 load 方法和 store 方法，读取一个文件中的配置信息，在通过 setProperty 的修改之后还可以再储存回去。

```java
public static void main(String[] args) {
    Properties prop = new Properties();
    prop.load(new FileInputStream("config.properties"));
    
    prop.setProperty("tel", "911");
    
    prop.store(new FileOutputStream("config.properties"), "注释信息");
}
```



### 复制文件夹练习

```java
import java.io.*;
import java.util.*;
public class Test {
    public static void main(String[] args) throws IOException {
        File src = getDir();
        File dest = getDir();
        if (src.equals(dest)) {
            System.out.println("The destination path is the subfile of the source file!");
        }
        else {
            copy(src, dest);
        }
    }

    public static File getDir() {
        Scanner sc = new Scanner(System.in);
        
        while(true) {
            File  f = new File(sc.nextLine());
            if (!f.exists()) {
                System.out.println("该路径不存在");
            }
            else if (f.isFile()) {
                System.out.println("这是一个文件");
            }
            else {
                sc.close();
                return f;
            }
        }
    }

    public static void copy(File src, File dest) throws IOException {
        File newDir = new File(dest, src.getName());
        newDir.mkdir();
        File[] subFiles = src.listFiles();

        for (File subFile : subFiles) {
            if (subFile.isFile()) {
                BufferedInputStream bis = new BufferedInputStream(new FileInputStream(subFile));
                BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream(new File(newDir, subFile.getName())));
                int b;
                while ((b = bis.read()) != -1) {
                    bos.write(b);
                }
                bis.close();
                bos.close();
            }
            else {
                copy(subFile, newDir);
            }
        }
    }
}

```



# 线程

一个线程就是程序执行的一条路径，一个进程中可以包含多条线程。多线程并发执行可以提高程序的效率，比如一个单核 CPU 去执行多个应用程序，并发就会发生。以下图为例，CPU 会在三者之间快速的切换，由于其运算速度执行效率之快，使用中是无法觉察出来的，这就是一种并发。

![image-20200606160326349](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/image-20200606160326349.png)

和并发有一个很相近的词叫并行，举个例子，比如左手用一个电脑和 A 聊天，右手也操作一个电脑和 B 聊天，这就是并行；并发的话就是在一个电脑上，一会和 A 聊，一会和 B 聊。

![image-20200611130336019](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/image-20200611130336019.png)



## 实现

**方法一：**继承 Thread 类，然后重写 run 方法，调用 start 方法来开启新的线程。

```java
public static void main(String[] args) {
    MyThread mt = new MyThread();
    mt.start();
}

class MyThread extends Thread {
    public void run() {
        // code...
    }
}
```



**方法二：**创造一个自定义类，实现 Runnable 接口，重写 run 方法，然后在创建 Thread 实例对象的时候，该自定义类实例对象作为参数传给 Thread 的构造函数。Thread 类中有一个成员变量，是用 Runnable 接的，叫 target，这个对象最后就赋给那个成员变量了。

```java
public static void main(String[] args) {
    Thread t = new Thread(new MyRunnable());
    t.start();
}

class MyRunnable implements Runnable {
    @Override
    public void run() {
        // code...
    }
}
```



这俩方法各有利弊，一个简单一点一个复杂一点。前者在调用 start 之后是直接去找子类中的 run 方法，而后者需要判断该对象是不是为 null。第一个方法有一个弊端，若该类已经有父类的情况下，则不能继承 Thread 了，所以在这种情况下才考虑第二个方法。第一个方法可以直接调用到 Thread 中的方法，第二种的话虽然扩展性好，但是代码复杂度会提高。

当用 Runnable 来实现的时候，还想调用该线程的方法的时候，可以通过  ```Thread.currentThread()``` 来获取当前的线程；该方法可以用在每一个线程当中来获取当前线程。



## 匿名内部类实现

作为了解知道即可

```java
public static void main(String[] args) {
    new Thread() {
        public void run() {
            // code...
        }
    }

    new Thread(new Runnable() {
        public void run() {
            //code...
        }
    }).start();
}
```



## 睡眠

sleep 方法是 Thread 类中的一个 static 方法，可以直接通过类名.调用。一般都用毫秒来作为参数，去规定这条线程睡眠多久，也可以传第二个参数，其为纳秒，但是 Win 对其支持的不好。1秒 = 1000 毫秒。

```java
public static void main(String[] args) throws InterruptedException {
    for (int i = 20; i >= 0; i--) {
        Thread.sleep(1000);
        System.out.println("倒计时第 " + i + " 秒");
    }
}
```



##  守护线程

守护线程 daemon，也可以叫做后台程序。其会随着非守护线程的结束而结束，它 ”守护“ 的线程都挂掉了，它还守护个锤子，自然就也会终止。

```java
public static void main(String[] args) throws InterruptedException {
    Thread t1 = new Thread() {
        public void run() {
            // code..
        }
    };

    Thread t2 = new Thread() {
        public void run() {
            // code..
        }
    };

    t2.setDaemon(true);
    t1.start();
    t2.start();
}
```



## 加入线程

简单来说就是插队，通过 join 方法来插队执行，而且可以设置插队多久。

```java
public static void main(String[] args) throws InterruptedException {
    Thread t1 = new Thread() {
        public void run() {
            for (int i = 0; i < 10; i++) {
                System.out.println(getName() + "...aa");
            }
        }
    };

    Thread t2 = new Thread() {
        public void run() {
            for (int i = 0; i < 10; i++) {
                if (i == 2) {
                    try {
                        t1.join();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
                System.out.println(getName() + "...bb");
            }

        }
    };

    t1.start();
    t2.start();
}
```



## 礼让线程

这东西没啥效果，主要就是告诉 scheduler 这条线程愿意去 yield，给出占用的 cpu 资源，但是让不让不好说，看脸，作为了解知道就可以。（以后有深入应用或者了解再来补）



##  线程优先级

线程的默认优先级是0，最小为1，最大为10，优先级越高的，优先处理，反之慢一点处理，效果一般明显。

```java
public static void main(String[] args) throws InterruptedException {
    Thread t1 = new Thread() {
        public void run() {
            for (int i = 0; i < 100; i++) {
                System.out.println(getName() + "...aa");
            }
        }
    };

    Thread t2 = new Thread() {
        public void run() {
            for (int i = 0; i < 100; i++) {
                System.out.println(getName() + "...bb");
            }

        }
    };

    t2.setPriority(Thread.MAX_PRIORITY);
    t1.setPriority(Thread.MIN_PRIORITY);

    t1.start();
    t2.start();
}
```



## 同步

先上代码在加说明：

```java
public static void main(String[] args) throws InterruptedException {
    Printer p = new Printer();
    // 第一个线程无限调用 p.print1()
    Thread t1 = new Thread() {
        public void run() {
            while (true) {
                p.print1();
            }
        }
    };
    // 第二个线程无限调用 p.print2()
    Thread t2 = new Thread() {
        public void run() {
            while (true) {
                p.print2();
            }
        }
    };

    t1.start();
    t2.start();
}

class Printer {
    public void print1() {
        System.out.print("黑");
        System.out.print("马");
        System.out.print("程");
        System.out.print("序");
        System.out.print("员");
        System.out.print("\r\n");
    }

    public void print2() {
        System.out.print("传");
        System.out.print("智");
        System.out.print("播");
        System.out.print("客");
        System.out.print("\r\n");
    }
}
```



![image-20200607062208759](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/image-20200607062208759.png)

输出结果会出现这样的错位的情况，这是因为在并发的过程中，会出现资源抢夺的情况，比如以上的输出中，输出 ```马``` 字之后，资源就被第二条线程抢过去用了，然后在输出完第一个 ```传``` 字之后，又被第一条线程抢过去了。所以避免这种代码无法执行完整的情况发生，需要使用同步代码块，引入 ```synchronized``` 关键字

```java
public static void main(String[] args) throws InterruptedException {
    Object o = new Object();
    synchronized(o) {
        // code...
    }
}
```

使用 ```synchronized``` 要注意，它需要统一一把锁，比如在两块代码块之间来回切换的过程中，需要指定同一把锁，让 A 和 B呈现互斥关系。

同步还可以在方法上进行 ```synchronized``` 关键字修饰，可以给非静态方法和静态方法添加该关键字；当方法为非静态方法的时候，锁对象为该对象，则为 ```this``` ；而当静态方法被该关键字修饰的时候，锁对象为字节码对象，比如该类名为 ```Demo``` ，那么该锁对象就是 ```Demo.class``` 

**非静态方法：**

```java
class Printer {
    Object o = new Object();

    public synchronized void  print1() {
        // code...
    }

    public void print2() {
        synchronized (this) {
            // code...
        }

    }
}
```



**静态方法：**

```java
class Printer {
    Object o = new Object();

    public static synchronized void  print1() {
        // code...
    }

    public void print2() {
        synchronized (Printer.class) {
            // code...
        }

    }
}
```



## 火车票举例

线程安全和线程不安全，如果对同一个数据进行操作，就可能出现线程上的不安全，比如卖票，一共就100张，规定在一个条件下售空，但是如果是多线程来操作的话，很可能就会出现条件越界，比如其中一个线程在输出语句的时候另一个输出语句对票数进行了修改，那么一开始这个线程去回去判断条件的时候就可能会出现不 break 的情况，导致超卖，下面是一段正确的代码

```java
public class Test {
    public static void main(String[] args) {
        MyTicket mt = new MyTicket();
        new Thread(mt).start();
        new Thread(mt).start();
        new Thread(mt).start();
        new Thread(mt).start();
    }
}

class MyTicket implements Runnable {
    private int tickets = 100;

    @Override
    public void run() {
        while (true) {
            synchronized (this) {
                if (tickets <= 0) {
                    break;
                }
                try {
                    Thread.sleep(10);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println(Thread.currentThread().getName() + "...这是第 " + tickets-- + " 号票");
            }
        }
    }
}
```

以上，这个锁对象是可以用 this 的，因为用 Thread 开启线程的时候，传入的都是同一个对象。如果不用 Runnable 接口实现，通过继承来实现的话，那么需要注意这个锁对象要么是字节码对象，要么就是静态成员对象。

​       

## 死锁

死锁出现的情况主要出现在 ```synchronized``` 嵌套中，当两个线程执行一个 ```synchronized``` 嵌套的代码块的时候，会出现其中一个线程占用一把锁，另一个线程也占有一个，然后在想要继续往下执行的时候，都被卡死了

```java
public static void main(String[] args) {
    new Thread() {
        public void run() {
            while (true) {
                synchronized (s1) {
                    System.out.println(getName() + "...获取" + s1 + "等待" + s2);
                    synchronized (s2) {
                        System.out.println(getName() + "...获取" + s2 + "等待" + s1);
                    }
                }
            }
        }
    }.start();

    new Thread() {
        public void run() {
            while (true) {
                synchronized (s2) {
                    System.out.println(getName() + "...获取" + s1 + "等待" + s2);
                    synchronized (s1) {
                        System.out.println(getName() + "...获取" + s2 + "等待" + s1);
                    }
                }
            }
        }
    }.start();
}
```

上面这段代码就会出现死锁的情况，在第一个线程在执行到第二个锁的时候，如果被第二个线程抢过去了，那么第二个线程就会开始执行，s2被占用，注意此时s1是被第一个线程占用的，所以执行一条之后也会卡主，这时候想回第一个线程继续执行也不行，因为s2被占用。



## 线程安全/不安全的类

通过源码可以知道，Vector，StringBuffer，HashTable 都是线程安全的，而相对应的 ArrayList，StringBuilder，HashMap 是线程不安全的；前三者在进行添加操作的时候都有 ```synchronized``` 关键字修饰，后三者没有。

还有一个办法可以让后面三个线程不安全的变成线程安全的，通过 Collections 中的 ```Collections.synchronizedXXX()``` 来实现线程安全。



## Runtime类

这个类就是用单例设计模式做的，暂时作为了解，其中一个方法可以直接执行本地终端。



## Timer

这个类有点像闹钟的感觉，它可以计划在具体某个时间里执行什么任务。这里还要提到一个 TimerTask 类，这个就是用来创建具体要执行什么任务用的，先创造自己的任务类，继承 TimerTask，然后重写 run 方法，在用 Timer schedule 任务的时候，最为参数传入。

```java
public static void main(String[] args) throws InterruptedException {
    Timer t = new Timer();
    t.schedule(new MyTimerTask(), new Date(188, 6, 14, 20, 30));
    
    while (true) {
        Thread.sleep(1000);
        System.out.println(new Date());
    }
}

class MyTimerTask extends TimerTask {
    @Override
    public void run() {
        System.out.println("背单词了！");
    }
}
```



## 两个/多个线程的通讯

通过 wait 和 notify 方法，可以让两个线程之间实现某种程度上的 ”互动“，wait 在没有接收到 notify 之前，线程会一直处于暂停的状态，下面给一个案例：

```java
class Printer {
    private int flag = 1;
    public void print1() throws InterruptedException {
        synchronized(this) {
            if (flag != 1) {
                this.wait();
            }
            System.out.println("线程1");
            flag = 2;
            this.notify();
        }
    }
    
    public void print2() throws InterruptedException {
        synchronized(this) {
            if (flag != 2) {
                this.wait();
            }
            System.out.println("线程2");
            flag = 1;
            this.notify();
        }
    }
}
```

在通过 flag 来限定特定的线程运行，其他的线程暂停。在这里注意一下，一旦线程的运行权回来之后，它会继 if 语句之后继续执行，而不是再次判断这个 flag 符不符合条件，这个只能用在两个线程的通信上。还有一个方法叫 notifyAll 的，可以用这个实现多线程之间的通信，需要修改的第一个地方就是 if 的判断改成 while 的判断，因为当出现多个线程的时候，一旦调用 notifyAll 之后，有一些线程会被反复的唤醒，然后判断，这个就需要用 while 一遍遍的判断，直到满足条件才可以继续运行

```java
class Printer {
    private int flag = 1;
    public void print1() throws InterruptedException {
        synchronized(this) {
            while (flag != 1) {
                this.wait();
            }
            System.out.println("线程1");
            flag = 2;
            this.notifyAll();
        }
    }
    
    public void print2() throws InterruptedException {
        synchronized(this) {
            while (flag != 2) {
                this.wait();
            }
            System.out.println("线程2");
            flag = 3;
            this.notifyAll();
        }
    }
    
    public void print3() throws InterruptedException {
        synchronized(this) {
            while (flag != 3) {
                this.wait();
            }
            System.out.println("线程3");
            flag = 1;
            this.notifyAll();
        }
    }
}
```



## 线程通信注意事项

1. 同步代码块中，调用 wait 方法的是锁对象
2. 因为锁可以为任意对象，所以 Object 类中有 wait 和 notify 的方法
3. sleep 只是短暂的休眠，其并不会释放锁；而 wait 方法是直接进入等待状态，让出锁。wait 方法可以传参，如果传参的话其使用和 sleep 相似，到时间之后如果锁没有被其他线程占用，则会继续执行，一般用的多的是无参的用法。



## ReentrantLock 和 Condition

ReetrantLock 为 JDK 1.5 引入的新特性，其用法和 synchronized 语句有点像，也可以实现代码块的同步，通过 lock 和 unlock 方法来实现。Condition 的话有点像信号发射器一样，由 ReentrantLock 示例调用 newCondition 来得到。通过 Condition 可以单一定向决定哪个线程等待，哪个线程继续运行。

```java
public static void main(String[] args) {
    Printer p = new Printer();
    new Thread() {
        public void run() {
            try {
                while (true) {
                    p.print1();
                }
            } catch (InterruptedException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
        }
    }.start();

    new Thread() {
        public void run() {
            try {
                while (true) {
                    p.print2();
                }
            } catch (InterruptedException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
        }
    }.start();

    new Thread() {
        public void run() {
            try {
                while (true) {
                    p.print3();
                }
            } catch (InterruptedException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
        }
    }.start();
}


class Printer {
    private ReentrantLock r = new ReentrantLock();
    Condition c1 = r.newCondition();
    Condition c2 = r.newCondition();
    Condition c3 = r.newCondition();
    private int flag = 1;

    public void print1() throws InterruptedException {
        r.lock();
        if (flag != 1) {
            c1.await();
        }
        System.out.println("线程1");
        flag = 2;
        c2.signal();
        r.unlock();
    }

    public void print2() throws InterruptedException {
        r.lock();
        if (flag != 2) {
            c2.await();
        }
        System.out.println("线程2");
        flag = 3;
        c3.signal();
        r.unlock();
    }

    public void print3() throws InterruptedException {
        r.lock();
        if (flag != 3) {
            c3.await();
        }
        System.out.println("线程3");
        flag = 1;
        c1.signal();
        r.unlock();
    }
}
```



## 线程组

线程组就像是个容器一样，只不过装的都是线程，可以利用它来做一些批量化的操作。

```java
public static void main(String[] args) {
    ThreadGroup tg = new ThreadGroup("新线程组");
    MyRunnable mr = new MyRunnable();  // 此类实现 Runnable 接口，不单独敲了
    
    Thread t1 = new Thread(tg, mr, "张三");  // 这个操作是把 t1 加到新的线程组里
    Thread t2 = new Thread(tg, mr, "李四");
}
```



### 线程池

线程池也可以看做一个容器，但是呢，在线程池里的线程，在执行结束之后，不会死掉，可以反复利用。因为开一个新的线程的成本太高了，放在线程池里这样的缓存操作可以节省资源，当需要某一条线程的时候直接调用就可以，其实现需要 Executor 和 ExecutorService 来实现。Executor 有一个静态方法叫 newFixedThreadPool 来获取线程池

```java
public static void main(String[] args) {
    ExecutorService pool = Executor.newFixedThreadPool(2);  // 规定池子装多少线程
    pool.submit(new MyRunnable());
    pool.submit(new MyRunnable());
    
    pool.shutdown();  // 此方法可以关闭线程池
}
```



# GUI

GUI 这块用的很少，作为了解就可以了

## 监听

比如我们点一个窗口的关闭按钮或者点最小化按钮，希望它做出一些反应，就需要对其进行监听

```java
public static void main(String[] args) {
    Frame f = new Frame("第一窗口");
    f.setSize(400, 600);  // 设置窗口大小
    f.setLocation(500, 50);  // 设置窗口初始位置
    f.setIconImage(Toolkit.getDefaultToolkit().creatImage("xxx.pnh"));
    Button b1 = new Button("按钮");
    f.add(b1);
    f.setLayout(new FlowLayout());  // 还有很多布局，下面说
    f.addWindowListener(new WindowAdapter() {  // 利用匿名内部类实现
        @Override
        public void windowClosing(WindowEvent e) {
            System.exit(0);
        }
    });
    f.setVisible(true);  // 直接 frame 出来的那个对象是不可见的
}
```

这里通过抽象类 WindowAdapter 来进行方法的重写，当一个事件发生的时候，应该做出什么操作。这个 WindowAdapter 里所有的方法都是空的，我们需要用到哪个就重写哪个就可以了。

另外布局方面还有很多其他的布局：

![image-20200611204306927](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/image-20200611204306927.png)



还有就是鼠标监听，键盘监听，和窗口监听基本一样，键盘监听的话需要借助 KeyEvent 来判断按哪个按键。 还有一个是动作监听，可以同时监听鼠标和键盘的一些操作。



# 网络编程

网络传输中有 UDP（User Datagram Protocol） 协议和 TCP（Transmission Control Protocol）协议。其中 UDP 协议考虑面向无连接，数据传输不安全，但是速度快，它不区分客户端和服务端；TCP 协议有三次握手，客户端向服务端发送请求，服务端响应，传输数据，其数据通讯相对安全，速度略低，区分服务端和客户端。



## UDP传输

这里就是一个 Sender 做定向的发送，然后有一个 Receiver 等着，规定好 URL 和 端口，就可以单纯的发和接收了。（以下内容较视频做了一些修改）

**Sender 代码：**

```java
 public static void main(String[] args) throws Exception {
     Scanner sc = new Scanner(System.in);
     DatagramSocket socket = new DatagramSocket();  // 创建 socket，相当于创建码头
     String content;
     DatagramPacket packet;
     while (true) {
         content = sc.nextLine();  // 获取 Sender 每次发送的内容
         packet = new DatagramPacket(content.getBytes(),   // 获取 byte 数组
                                     content.getBytes().length,  // 内容长度
                                     InetAddress.getByName("127.0.0.1"), 
                                     6666);
         socket.send(packet);
         if (content.equals("quit")) {
             break;
         }
     }
     sc.close();
     socket.close();  // socket 本质也是 IO 流传输，需要关流

 }
```

**Receiver 代码：**

```java
public static void main(String[] args) throws Exception {
    DatagramSocket socket = new DatagramSocket(6666);
    DatagramPacket packet = new DatagramPacket(new byte[1024], 1024);
    
    while (true) {  
        socket.receive(packet);  // 先让它开始等待接收
        byte[] arr = packet.getData();
        int len = packet.getLength();
        String ip = packet.getAddress().getHostAddress();
        int port = packet.getPort();
		// 如果内容是设定的特殊事件，则停止接收，且也不会输出这个特殊事件内容
        if (new String(arr, 0, len).equals("quit")) {
            break;
        }

        System.out.println(ip + " : " + port + " : " + new String(arr, 0, len));
    }
    socket.close();
}
```

上面就算是勉强实现 Receiver 持续等待 Sender 发送数据并且接收，一直等到一个特殊的内容在 Sender 这面出现，才会两面都断开连接。



## TCP传输

TCP 传输相比较 UDP 传输来说，其实就多了一个三次握手的过程，说白了就是一个验证的过程。

**服务端：**

```java
public class MyClient {
    public static void main(String[] args) throws Exception {
        Socket socket = new Socket("127.0.0.1", 9999);

        InputStream is = socket.getInputStream();
        OutputStream os = socket.getOutputStream();

        os.write("客户端发送".getBytes());

        byte[] arr = new byte[1024];
        int len = is.read(arr);
        System.out.println(new String(arr, 0, len));

        //os.write("客户端发送".getBytes());

        socket.close();

    }
}
```

**客户端：**

```java
public class MyClient {
    public static void main(String[] args) throws Exception {
        Socket socket = new Socket("127.0.0.1", 9999);

        InputStream is = socket.getInputStream();
        OutputStream os = socket.getOutputStream();

        os.write("客户端发送".getBytes());

        byte[] arr = new byte[1024];
        int len = is.read(arr);
        System.out.println(new String(arr, 0, len));

        //os.write("客户端发送".getBytes());

        socket.close();

    }
}
```

**服务端：**

```java
public class MyServer {
    public static void main(String[] args) throws Exception{
        ServerSocket server = new ServerSocket(9999);
        Socket socket = server.accept();

        InputStream is = socket.getInputStream();
        OutputStream os = socket.getOutputStream();

        //os.write("服务端发送".getBytes());

        byte[] arr = new byte[1024];
        int len = is.read(arr);
        System.out.println(new String(arr, 0, len));

        os.write("服务端发送".getBytes());
    }
}
```



---

上述代码优化

**服务端：**

```java
public class MyServer {
    public static void main(String[] args) throws Exception{
        ServerSocket server = new ServerSocket(9999);
        Socket socket = server.accept();

        BufferedReader br = new BufferedReader(new InputStreamReader(socket.getInputStream()));
        PrintStream ps = new PrintStream(socket.getOutputStream());  // 有写出换号的方法

        System.out.println(br.readLine());
        ps.println("服务器成功接收请求 - 1");

        socket.close();
    }
}
```

**客户端：**

```java
public class MyClient {
    public static void main(String[] args) throws Exception {
        Socket socket = new Socket("127.0.0.1", 9999);
        BufferedReader br = new BufferedReader(new InputStreamReader(socket.getInputStream()));
        PrintStream ps = new PrintStream(socket.getOutputStream());  // 有写出换号的方法

        ps.println("客户端发送请求 - 1");
        System.out.println(br.readLine());

        socket.close();
    }
}
```



## TCP多线程

这个就是服务端那面用 while 写死，然后开启多线程，这样可以同时接受多个请求

```java
public class MyServer {
    public static void main(String[] args) throws Exception{
        ServerSocket server = new ServerSocket(9999);
        while (true) {
            final Socket socket = server.accept();
            new Thread() {
                public void run() {
                    try {
                        BufferedReader br = new BufferedReader(
                            new InputStreamReader(
                            socket.getInputStream()));
                        PrintStream ps = new PrintStream(socket.getOutputStream());  // 有写出换行的方法
                        System.out.println(br.readLine());
                        ps.println("服务器成功接收请求");
                        socket.close();
                    } catch(IOException e) {
                        e.printStackTrace();
                    }
                }
            }.start();
        }
    }
}
```



# 反射

## 字节码对象

获取字节码对象的方法有三种，通过 Class 类中的 forName 方法，通过 ```类名.class``` 来获得，还可以通过 ```对象.getClass()``` 方法获得。Java 的反射机制是说在运行状态中，对于任意一个类，都能够知道这个类所有的属性和方法；对于任意一个对象都能调用它的任意一个方法和属性。

以下三种方式获取：

```java
public class MyClass {
    public static void main(String[] args) throws Exception {
        Class c1 = Class.forName("Person");
        Class c2 = Person.class;
        Person p = new Person();
        Class c3 = p.getClass();
    }
}
```



## 通过字节码创建对象

Class 类中有 ```getConstructor(s)``` 方法，其中 ```getConstructors``` 会得到一个数组，显示所有的构造方法，当没有空参构造的时候可以通过这样获取；```getConstructor()``` 可以得到一个特定的构造方法，需要传参，规定传入的类型，然后可以通过 Constructor 来创造实例

```java
public static void main(String[] args) throws Exception {
    Class c = Class.forName("Person");
    Constructor cc = c.getConstructor(String.class);
    Person p = (Person)cc.newInstance("DDD");
    System.out.println(p);
}
```

不仅仅可以获取到构造方法，其中的成员变量也可以获取到；公有的直接通过 ```getField(s)``` 就可以获取，私有的话需要用 ```getDeclaredField()``` 和 ```setAccessible()``` 来暴力获取到。Method 和成员变量用法近似。



## 无视泛型添加

泛型是可以通过反射来强制无视泛型规则的，比如给一个 ArrayList 规定了只能装整数，但是也可以通过反射来得到该方法，然后强行添加一个字符串进去

```java
public void main(String[] args) {
    ArrayList<Integer> list = new ArrayLisy<>();
    list.add(111);
    list.add(222);
    
    Class c = Class.forName("java.util.ArrayList");
    Method m = c.getMethod("add", Object.class);
    
    System.out.println(list);
}
```



## 动态代理

这个动态代理，个人感觉是在一个对象的基本功能的基础上，雇佣另外一个类在封装此对象的基础上，进行其他功能的扩展，而且，代理的对象，只能使用接口中的方法，但是操作的是同一个对象，只是限制了一些方法的使用，具体说明在代码注释里写

**Person Interface：**

```java
import java.util.ArrayList;

interface Person {
    ArrayList<String> mHobby = new ArrayList<>();  // 创建一个容器来装东西，验证后面的猜想用的
    public void eat();  // 创建了无参的方法
    public void sleep();
    public void addHobby(String h);  // 创建了有参方法
    public void showHobbies();
}
```



**Person Implementation：**

```java
public class PersonImp implements Person {
    @Override
    public void eat() {
        System.out.println("eat");
    }

    @Override
    public void sleep() {
        System.out.println("sleep");
    }

    @Override
    public void addHobby(String h) {
        mHobby.add(h);
    }

    @Override
    public void showHobbies() {
        System.out.println(mHobby);
    }

    public void sing() {
        System.out.println("唱歌");
    }
}
```



**MyInvocationHandler：**

```java
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;

public class MyInvocationHandler implements InvocationHandler {
    private Object o;  // 对想要进行代理的对象进行封装
    public MyInvocationHandler(Object o) {
        this.o = o;
    }
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        System.out.println("验证身份");  // 添加额外的需要代理的功能
        method.invoke(o, args);  // 这里传参不用改，它底层有封装，调用代理对象的方法的时候，如果有参则会自己传过来
        System.out.println("记录");  // 添加额外的需要代理的功能
        return null;
    }
}
```



**Main：**

```java
import java.lang.reflect.Proxy;

public class MyClass {
    public static void main(String[] args) throws Exception {
        PersonImp pi = new PersonImp();
        MyInvocationHandler m = new MyInvocationHandler(pi);
        Person p = (Person) Proxy.newProxyInstance(pi.getClass().getClassLoader(),
                pi.getClass().getInterfaces(), m);
        p.eat();  // 用代理对象调用，会有验证身份和记录的功能
        p.sleep();
        p.addHobby("唱歌");  // 会添加到该对象的 mHobby 中
        p.showHobbies();
        pi.addHobby("跳舞");  // 虽然一个是代理对象一个是对象本身，添加到的都是同一个 mHobby
        pi.addHobby("打篮球");
        pi.showHobbies();  // [唱歌, 跳舞, 打篮球]
        pi.sing();  // 对象独有的方法只能通过对象本身调用

    }
}
```



# 枚举

枚举其实就是在单例中规定好有多少个单例

## 自己实现枚举

**无参构造：**

```java
public class Week {
    public static final Week MON = new Week();
    public static final Week TUE = new Week();
    public static final Week WED = new Week();
    
    private Week(){}
}
```



**有参构造：**

```java
public class Week {
    private String name;
    public static final Week MON = new Week("星期一");
    public static final Week TUE = new Week("星期二");
    public static final Week WED = new Week("星期三");

    private Week(String s){
        this.name = s;
    }

    public String getName() {
        return name;
    }
}
```



**匿名内部类：**（了解）

```java
public abstract class Week {
    private String name;
    public static final Week MON = new Week("星期一") {
        public void show() {
            System.out.println("星期一");
        }
    };
    public static final Week TUE = new Week("星期二") {
        public void show() {
            System.out.println("星期二");
        }
    };

    public static final Week WED = new Week("星期三") {
        public void show() {
            System.out.println("星期三");
        }
    };

    private Week(String s){
        this.name = s;
    }

    public String getName() {
        return name;
    }

    public abstract void show()
}

```





## Java自带枚举

这个之前课上使用到过，大概怎么回事还是知道的

```java
public enum MyEnum {
    MON("星期一"), TUE("星期二"), WED("星期三");
    private String name;
    private MyEnum(String name) {
        this.name = name;
    }

    public String getName() {
        return this.name;
    }
}
```

这里有一个 ```values()``` 的方法，可以得到每一个枚举项





# JDK1.7

A: 二进制字面量

```java
System.out.println(0b110);
```



B: 数字字面量可以加下划线

```java
System.out.println(100_000_000);  // 100000000
```



C: Swich 语句可以用哪个字符串

D: 泛型简化，菱形泛型

E: 异常的多个 catch 合并，每个异常用 ```|``` 来并列

F: try-with-resources 语句，1.7版标准异常处理代码