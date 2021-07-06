---
title: Java上传文件案例
date: 2020-06-13 09:54:25
categories: 
- 计网
tags: 
- Java
- TCP
- 网络协议
---

用 Java 实现了一个极其简陋的 TCP文件上传，其服务端使用多线程，内容全部来源于 **Java Note P5 黑马程序员中的教程**，此文档主要用于保存案例代码

**客户端：**

```java
package com.heima.tcp;

import java.io.*;
import java.net.Socket;
import java.util.Scanner;

public class MyClient {
    public static void main(String[] args) throws Exception {
        // 1. 提示要上传路径，验证
        File file = getFile();
        // 2. 发送文件名到服务端
        Socket socket = new Socket("127.0.0.1", 9999);
        BufferedReader br = new BufferedReader(new InputStreamReader(socket.getInputStream()));
        PrintStream ps = new PrintStream(socket.getOutputStream());
        ps.println(file.getName());
        // 6. 接收结果，如果存在给予提示，程序直接退出
        String result = br.readLine();
        if ("存在".equals(result)){
            System.out.println("您上传的文件已经存在，请不要重复上传");
        }
        else {
        // 7. 如果不存在，定义 FileInputStream 读取文件，写出到服务器
            FileInputStream fis = new FileInputStream(file);
            int len;
            byte[] arr = new byte[8192];
            while ((len = fis.read(arr)) != -1) {
                ps.write(arr, 0, len);
            }
            fis.close();
        }
        socket.close();
    }

    private static File getFile() {
        Scanner sc = new Scanner(System.in);
        System.out.println("请输入一个文件路径: ");
        File file = new File(sc.nextLine());
        while(true) {
            if (!file.exists()) {
                System.out.println("目标文件不存在，请重新输入：");
                file = new File(sc.nextLine());
            } else if (file.isDirectory()) {
                System.out.println("请不要输入文件夹路径，重新输入：");
                file = new File(sc.nextLine());
            } else {
                return file;
            }
        }
    }
}
```



**服务端：**

```java
package com.heima.tcp;

import java.io.*;
import java.net.ServerSocket;
import java.net.Socket;

public class MyServer {
    public static void main(String[] args) throws Exception{
        // 3. 建立多线程服务器
        ServerSocket server = new ServerSocket(9999);
        System.out.println("服务器启动成功，绑定9999端口");
        // 4. 读取文件名
        while (true) {
            final Socket socket = server.accept();
            new Thread() {
                public void run() {
                    try {
                        InputStream is = socket.getInputStream();
                        BufferedReader br = new BufferedReader(new InputStreamReader(is));
                        PrintStream ps = new PrintStream(socket.getOutputStream());

                        String fileName = br.readLine();
                        File dir = new File("upload");
                        dir.mkdir();

                        File file = new File(dir, fileName);
                        if (file.exists()) {
                            ps.println("存在");
                            socket.close();
                        }
                        else {
                            ps.println("不存在");
                            // 8. 定义 FileOutPutStream，读取数据，写到本地
                            FileOutputStream fos = new FileOutputStream(file);
                            byte[] arr = new byte[8192];
                            int len;
                            while ((len = is.read(arr)) != -1) {
                                fos.write(arr, 0, len);
                            }
                            fos.close();
                            socket.close();
                        }

                    } catch(IOException e) {
                        e.printStackTrace();
                    }
                }
            }.start();
        }
    }
}

```

