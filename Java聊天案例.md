---
title: Java聊天案例
date: 2020-06-12 16:16:04
categories: 
- 计网
tags: 
- Java
- UDP
---

用 Java 实现了一个极其简陋的 UDP 通讯，内容全部来源于 **Java Note P5 黑马程序员中的教程**，此文档主要用于保存案例代码

```java
import java.awt.*;
import java.awt.event.*;
import java.io.*;
import java.net.*;
import java.text.SimpleDateFormat;
import java.util.*;

class MyFrame extends Frame {
    int receivingPort;
    int targetPort;
    private TextField tf;
    private Button send;
    private Button log;
    private Button clear;
    private Button shake;
    private TextArea viewText;
    private TextArea sendText;
    private DatagramSocket socket;
    private BufferedWriter bw;
    private String logName;

    public MyFrame(int rPort, int tPort) {
        this.receivingPort = rPort;
        this.targetPort = tPort;
        southPanel();
        centerPanel();
        logName = tf.getText() + targetPort + ".txt";
        init();
        event();
    }

    public void centerPanel() {
        Panel center = new Panel();
        viewText = new TextArea();
        sendText = new TextArea(5, 1);
        center.setLayout(new BorderLayout());
        center.add(sendText, BorderLayout.SOUTH);
        center.add(viewText, BorderLayout.CENTER);
        viewText.setEditable(false);
        viewText.setBackground(Color.WHITE);
        sendText.setFont(new Font("Microsoft Yahei", Font.PLAIN, 15));
        sendText.setFont(new Font("Microsoft Yahei", Font.PLAIN, 15));
        this.add(center, BorderLayout.CENTER);
    }

    public void event() {
        this.addWindowListener(new WindowAdapter() {
            @Override
            public void windowClosing(WindowEvent e) {
                try {
                    bw.write("------------------------------");
                    socket.close();
                    bw.close();
                } catch (IOException e1) {
                    e1.printStackTrace();
                }
                System.exit(0);
            }
        });

        send.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                try {
                    send();
                } catch (IOException ioException) {
                    ioException.printStackTrace();
                }
            }
        });

        log.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                try {
                    logFile();
                } catch (IOException ioException) {
                    ioException.printStackTrace();
                }

            }
        });

        clear.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                viewText.setText("");
            }
        });

        sendText.addKeyListener(new KeyAdapter() {
            @Override
            public void keyReleased(KeyEvent e) {
                if (e.getKeyCode() == KeyEvent.VK_ENTER) {
                    try {
                        send();
                    } catch (IOException ioException) {
                        ioException.printStackTrace();
                    }
                }
            }
        });

        shake.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                try {
                    send(new byte[]{-1}, tf.getText());
                } catch (IOException ioException) {
                    ioException.printStackTrace();
                }
            }
        });
    }

    private void shake() {
        int x = this.getLocation().x;
        int y = this.getLocation().y;
        for (int i = 0; i < 5; i++) {
            try {
                this.setLocation(x - 5, y + 5);
                Thread.sleep(20);
                this.setLocation(x + 5, y - 5);
                Thread.sleep(20);
                this.setLocation(x + 5, y - 5);
                Thread.sleep(20);
                this.setLocation(x - 5, y + 5);
                Thread.sleep(20);
                this.setLocation(x,y);
            } catch (Exception e) {
                e.printStackTrace();
            }
        }

        this.setLocation(x, y);
    }

    private void logFile() throws IOException {
        bw.flush();
        FileInputStream fis = new FileInputStream(logName);
        ByteArrayOutputStream baos = new ByteArrayOutputStream();
        int len;
        byte[] arr = new byte[8192];
        while ((len = fis.read(arr)) != -1) {
            baos.write(arr, 0, len);
        }
        String message = baos.toString();
        viewText.setText(message);
        fis.close();
    }

    private void send(byte[] arr, String ip) throws IOException {
        DatagramPacket packet = new DatagramPacket(arr, arr.length,
                InetAddress.getByName(ip), targetPort);  // 这个 9999 是指的发到对方的9999端口
        socket.send(packet);
    }

    private void send() throws IOException {
        String message = sendText.getText();  // 获取输入框内的内容
        String ip = tf.getText();  // 获取 IP 地址
        // 随机端口
        send(message.getBytes(), ip);
        String time = getCurrentTime();
        String logMessage = time + " 我对: " + ip + ":" + targetPort + " 说\r\n" + message + "\r\n\r\n";
        viewText.append(logMessage);
        sendText.setText("");
        try {
            bw.write(logMessage);
        } catch (IOException e) {
            System.out.println("这里出错了");
            e.printStackTrace();
        }
    }

    private String getCurrentTime() {
        Date d = new Date();
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy年MM月dd日 HH:mm:ss");
        return  sdf.format(d);  // 格式化时间
    }

    public void southPanel() {
        Panel south = new Panel();  // 创建中间的 Panel
        tf = new TextField(15);
        tf.setText("127.0.0.1");
        send = new Button("Send");
        log = new Button("Log");
        clear = new Button("Clean");
        shake = new Button("Shake");
        south.add(tf);  // 添加各个控件到 Panel 上
        south.add(send);
        south.add(log);
        south.add(clear);
        south.add(shake);
        this.add(south, BorderLayout.SOUTH);
    }

    public void init() {
        this.setLocation(500, 50);
        this.setSize(400, 600);
        new Receive().start();
        try {
            socket = new DatagramSocket();
            bw = new BufferedWriter(new FileWriter(logName, true));  // 每次尾部追加
        } catch (Exception e) {
            e.printStackTrace();
        }
        this.setVisible(true);
    }

    private class Receive extends Thread {
        public void run() {
            try {
                DatagramSocket socket = new DatagramSocket(receivingPort);
                DatagramPacket packet = new DatagramPacket(new byte[8192], 8192);
                while (true) {
                    socket.receive(packet);
                    byte[] arr = packet.getData();
                    int len = packet.getLength();
                    if (arr[0] == -1 && len == 1) {
                        viewText.append(tf.getText() + " 发送了一个窗口抖动\r\n");
                        shake();
                        continue;
                    }
                    String message = new String(arr, 0, len);
                    String time = getCurrentTime();
                    String ip = packet.getAddress().getHostAddress();  // 获取发送者的 IP
                    String logMessage = time + " " + ip + ":" + packet.getPort() + " 对我说：\r\n" + message + "\r\n\r\n";
                    viewText.append(logMessage);
                    bw.write(logMessage);

                }
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }

    public static void main(String[] args) {
        MyFrame chat1 = new MyFrame(9999, 6666);
        MyFrame chat2 = new MyFrame(6666, 9999);
    }
}

```

