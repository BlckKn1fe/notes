

服务器系统：Ubuntu Server 18.04.1 LTS 64位



首先更新源：

```
sudo apt update
sudo apt upgrade
```



# 安装 Apache2

```
sudo apt install apache2
```

安装之后查看安装状态以及版本：

```
sudo systemctl status apache2
apache2 -version
```

修改 UFW 配置：

```
sudo ufw allow 'Apache'
```



# 安装 MySQL

安装：

```
sudo apt-get install mysql-server
```

查看用户名密码：

```
sudo cat /etc/mysql/debian.cnf
```

修改用户密码验证插件

```mysql
mysql> alter user 'root'@'localhost' identified with mysql_native_password by 'root';
Query OK, 0 rows affected (0.45 sec)
```

修改密码

```mysql
alter user 'root'@'localhost' identified by '123456';
```

赋权

```mysql
grant all privileges on *.* to 'root'@'localhost' with grant option;
```

修改 host

```mysql
use mysql;
update user set host='%'  where user='root';
```

刷新！

```mysql
flush privileges;
```

由于数据库只做本地使用，所以这里不设置外部链接的内容



# 安装 PHP

安装：

```
sudo apt-get install php
```

安装 php-mysql 扩展：

```
sudo apt-get install php-mysql
```

安装 php-curl 扩展：

```
sudo apt-get install php-curl
```

安装 php-mbstring 扩展：

```
sudo apt-get install php-mbstring
```



# 最后

重启 Apache

```
sudo /etc/init.d/apache2 restart
```



# 安装

下载安装包后，解压到

```
/var/www/html
```

使用 tar

```
tar -xzvf 
```

把 build 文件夹中的全部内容放在 html 目录下，然后打开网页按照步骤安装，值得注意的是需要先在 MySQL 中创建一个叫 typecho 的数据库



# 备份

主要就是把原服务器 MySQL 中的所有内容备份出来

```mysql
mysqldump -u root -p typecho > typecho.sql
```

**这里备份出来的数据包括 Handsome 主题相关的数据内容！**



# 还原

还原这一步一定注意，要先恢复 MySQL 数据库的内容，然后在拷贝主题文件和插件，再去恢复主题相关数据