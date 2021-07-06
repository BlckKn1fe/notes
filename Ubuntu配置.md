---
title: Ubuntu配置
date: 2020-06-14 10:52:25
categories: 
- Linux
tags: 
- Linux
- MySQL
---



## 安装 Ubuntu

安装 Ubuntu 的时候切记不要选择推荐项目

![image-20200614105453648](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/image-20200614105453648.png)

主要是这一步，如果选择了第二项的话，它会在安装之前让你设置账户密码什么的，如果填写的话，Ubuntu 进去就直接到这个界面，你么得选择，一路下一步，一直在可以自定义配置的地方，在光驱那选择 ISO 镜像文件

![image-20200614105639801](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/image-20200614105639801.png)

点 SKIP 就完事了



解压：

```
tar zxvf FileName.tar.gz
```





再然后就是修改一下镜像源，可以通过选择 ```系统设置 -> 软件和更新 选择下载服务器 -> "mirrors.aliyun.com"``` ，也可以手动修改。

打开 ```/etc/apt/sources.list``` 然后全部代替为（以下内容直接拷贝来的，放心使用）

**ubuntu 20.4 的配置**：（其他自行查阅 https://developer.aliyun.com/mirror/ubuntu?spm=a2c6h.13651102.0.0.4ee41b11IEUSBW）

```
deb http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ focal-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-security main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ focal-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-updates main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ focal-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-proposed main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse
```



## 设置 Git 用户名，密码，秘钥

执行下面两条命令

```
git config --global user.name "xxx"

git config --global user.email "xxx@xxx.com"
```

之后生成 SSH 秘钥

```
ssh-keygen -t rsa -C "xxx@xxx.com"
```

安装 SSH 服务端和客户端

```
sudo apt-get install openssh-client 
sudo apt-get install openssh-server
```

然后要修改一个配置文件，这样可以从外部来连接，修改 ```PermitRootLogin``` 和 ```PasswordAuthentication```

```
sudo nano /etc/ssh/sshd_config
```

**还要注意一点，如果要准备去连接的电脑上有之前的记录，记得把记录删除。**





## 安装 MySQL

### Ubuntu

> 转载自：https://www.cnblogs.com/zh-xiaoyuan/p/13384524.html



1、安装mysql

```
sudo apt-get install mysql-server
```

此外还能安装mysql-workbench

```
sudo apt-get install mysql-workbench
```

2、安装之后查看默认的用户名和密码

```
sudo cat /etc/mysql/debian.cnf
```

![img](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/1853614-20200727105622486-204989601.png)

3、用上述用户名和密码登录mysql

```
 mysql -u debian-sys-maint -p
```

然后回车，输入上述密码进行登录

![img](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/1853614-20200727110723745-119467851.png)



4、修改用户密码验证插件

```mysql
mysql> alter user 'root'@'localhost' identified with mysql_native_password by 'root';
Query OK, 0 rows affected (0.45 sec)
```



5、修改密码

```mysql
alter user 'root'@'localhost' identified by '111111';
```



6、赋权

```mysql
grant all privileges on *.* to 'root'@'localhost' with grant option;
```



7、修改host

```mysql
update user set host='%'  where user='root';
```



8、刷新！

```mysql
flush privileges;
```



9、修改 mysql 配置文件

```
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
```



10、重启服务

```
sudo service mysql restart
```



### Windows

啥也不说了，直接上正解，注意全称要用管路员模式！

> https://www.bilibili.com/video/BV1VE411x7fZ

第一步，下载安装包，解压缩，创建 my.ini 文件

```
[mysqld]
# 设置3306端口
port=3306
# 设置mysql的安装目录
basedir=D:\\Mysql\\mysql-8.0.19-winx64
# 设置mysql数据库的数据的存放目录
datadir=D:\\Mysql\\mysql-8.0.19-winx64\\data
# 允许最大连接数
max_connections=200
# 允许连接失败的次数。这是为了防止有人从该主机试图攻击数据库系统
max_connect_errors=10
# 服务端使用的字符集默认为UTF8
character-set-server=utf8
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB
# 默认使用“mysql_native_password”插件认证
default_authentication_plugin=mysql_native_password
[mysql]
# 设置mysql客户端默认字符集
default-character-set=utf8
[client]
# 设置mysql客户端连接服务端时默认使用的端口
port=3306
default-character-set=utf8
```



第二步，设置环境变量，提前创好文件夹，执行 ```mysqld --intialize --console``` 和 ```mysqld -install mysql``` 命令，初始化+添加服务

第三步，启动服务 ```net start mysql```

第四步，用随机给的密码进入之后，修改密码 

```ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_nativa_password BY '111111';```

最后 ```flush privileges;``` 结束



报错：

1. 执行 mysqld --initialize --console 时候的报错：
   针对这里的报错有几种可能：A. 你的 my.ini 文件中，复制粘贴过去之后存在格式上的问题，同时要注意修改一下括号，把 【】 换成 []，这两个是不一样的 B. 去你Data目录下，创造好最里层的那个文件夹， 比如我自己的配置文件写的是 datadir=C:\\ProgramData\\MySQL\\MySQL Server 8.0 那么我就回去 C:\ProgramData\MySQL 路径下创造好 MySQL Server 8.0，这里不创造这个文件夹也可能造成报错 C. 没用管理员身份

2. 在添加到服务这块，也就是执行 mysqld --install mysql 这句，这句出错就是没用管理员身份，没权限，这块没理由有其他方面的错误。
3. 输入密码这给个贴士，建议复制粘贴出给的随机密码到聊天框里，看的清楚一些，有时候有特殊符号。
4. 第四项注意很重要！！UP忘记说了，在执行完 ALTER 语句改密码之后，要执行一下 flush privileges; 语句，目的是刷新。



# 配置 C 语言环境

无脑执行就对了

```
sudo apt-get update
sudo apt-get install gcc
sudo apt-get install g++
sudo apt-get install gdb
```

另外对于 VScode 的配置，点左面 debug，然后点启动，添加配置，自动生成一个 launch.json 文件，然后修改内容如下：

```json
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "(gdb) Launch",
            "type": "cppdbg",
            "request": "launch",
            "program": "${workspaceFolder}/${fileBasenameNoExtension}.out",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}",
            "environment": [],
            "externalConsole": true,
            "MIMode": "gdb",
            "preLaunchTask": "build",
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ]
        }
    ]
}
```

再然后ctrl+shift+p打开命令行，输入`Tasks: Run task`，会出现如下提示：

```
No task to run found. configure tasks...
```

添加一个 task.json 文件

```json
{
    // See https://go.microsoft.com/fwlink/?LinkId=733558
    // for the documentation about the tasks.json format
    "version": "2.0.0",
    "tasks": [
        {
            "label": "build",
            "type": "shell",
            "command": "g++",
            "args": [
                "-g",
                "${file}",
                "-std=c++11",
                "-o",
                "${fileBasenameNoExtension}.out"
            ],
            "option": {
                "cwd": "${workspaceFolder}"
            },
            "problemMatcher": [
                "$gcc"
            ],
            "group": "build"
        }
     ]
}
```

