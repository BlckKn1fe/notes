---
title: MySQL基础
date: 2020-06-19 15:04:54
categories: 
- MySQL
tags: 
- MySQL
- 后端
---

笔记全部内容来自B站狂神说Java系列和C语言中文网，链接如下

> https://www.bilibili.com/video/BV1NJ411J79W
>
> http://c.biancheng.net/mysql/

Navicat 可以去这个网站自行下载

> https://macwk.com/



# MySQL

## MySQL的四大组成部分

### 数据定义语言（DDL）

Data Definition Language, 用来创建或删除数据库以及表等对象，主要包含以下几种命令：

- DROP：删除数据库和表等对象
- CREATE：创建数据库和表等对象
- ALTER：修改数据库和表等对象的结构

### 数据操作语言（DML）

Data Manipulation Language, 用来变更表中的记录，主要包含以下几种命令：

- SELECT：查询表中的数据
- INSERT：向表中插入新数据
- UPDATE：更新表中的数据
- DELETE：删除表中的数据

### 数据查询语言（DQL）

Data Query Language, 用来查询表中的记录，主要包含 SELECT 命令，来查询表中的数据。

### 数据控制语言（DCL）

Data Control Language, 用来确认或者取消对数据库中的数据进行的变更。除此之外，还可以对数据库中的用户设定权限。主要包含以下几种命令：

- GRANT：赋予用户操作权限
- REVOKE：取消用户的操作权限
- COMMIT：确认对数据库中的数据进行的变更
- ROLLBACK：取消对数据库中的数据进行的变更

## 数据库的列类型

**数值**

- tinyint，十分小的数据，1个字节
- smallint，较小的数据，2个字节
- mediumint，中等大小的数据，3个字节
- **int，4个字节**
- bigint，较大的数据，8个字节
- float，4个字节
- double，8个字节
- decimal，字符串形式的浮点数（高精度要求）

> 提示：显示宽度和数据类型的取值范围是无关的。显示宽度只是指明 MySQL 最大可能显示的数字个数，数值的位数小于指定的宽度时会由空格填充。如果插入了大于显示宽度的值，只要该值不超过该类型整数的取值范围，数值依然可以插入，而且能够显示出来。例如，year 字段插入 19999，当使用 SELECT 查询该列值的时候，MySQL 显示的将是完整的带有 5 位数字的 19999，而不是 4 位数字的值。

**字符串**

- char，固定大小的字符串，0-255
- **varchar，最常用的可变字符串，0-65535**，**对标 Java String**
- tinytext，微型文本，2^8 - 1
- text，文本串，2^16 - 1



**时间日期**

- date，YYYY-MM-DD，日期格式
- time，HH:mm:ss，时间格式
- **datetime，YYYY-MM-DD HH:mm:ss，最常用的** 
- **timestamp，时间戳，1970年1月1日至今的毫秒数**
- year，年份



## 命名规范

以下为阿里开发手册中的强制规范：

>   表名、字段名必须使用小写字母或数字，禁止出现数字开头，禁止两个下划线中间只出现数字。数据库字段名的修改代价很大，因为无法进行预发布，所以字段名称需要慎重考虑。

## 数据库字段属性

**Unsign**

- 无符号整数
- 声明该列不能为负



**Zerofill**

- 数据变成 0 填充
- 意思是，不足的位数用 0 在前面进行填充
- 声明 int 长度为 8，若赋值为5，则 5 --> 00000005



**Autoincrement**

- 自动在上一条的基础上加一
- 同上用来设置唯一的主键 --> index，类型必须为整数
- 可以自定义自增的初始值和步长



**Null / Not Null**

- 勾选后，必须给值，否则报错



## 库和表的基本操作（DDL）

Data Define Language

### 查看数据库

```mysql
SHOW DATABASES [LIKE 'XXX'];
```

括号内的内容为可选项，而且可以用 ```%``` 来进行部分匹配，如下

```mysql
SHOW DATABASES LIKE '%test%'; -- 包含 test 关键字的数据库
mysql> SHOW DATABASES LIKE 'db%'; -- 以 db 开头的数据库
mysql> SHOW DATABASES LIKE '%db'; -- 以 db 结尾的数据库
```



### 创建数据库

在创建数据库的时候也有一些可选项

```mysql
CREATE DATABASE [IF NOT EXISTS] <数据库名>
[[DEFAULT] CHARACTER SET <字符集名>] 
[[DEFAULT] COLLATE <校对规则名>];
```

MySQL 的字符集（CHARACTER）和校对规则（COLLATION）是两个不同的概念。字符集是用来定义 MySQL 存储字符串的方式，校对规则定义了比较字符串的方式。

```mysql
CREATE DATABASE IF NOT EXISTS test_db_char
-> DEFAULT CHARACTER SET utf8
-> DEFAULT COLLATE utf8_general_ci;
```

注意上面 database 是单数，exist 加 s



### 创建表

可以创建表头，和一些特定的属性，可是操作起来感觉好麻烦

```mysql
CREATE TABLE IF NOT EXISTS `student` (
  `id` INT(4) NOT NULL AUTO_INCREMENT COMMENT '学号',
  `nanme` VARCHAR(30) NOT NULL DEFAULT '匿名' COMMENT '姓名',
  `pwd` VARCHAR(20) NOT NULL DEFAULT '123456' COMMENT '密码',
  `birthday` DATETIME DEFAULT NULL COMMENT '出生日期',
  PRIMARY KEY(`id`)
)ENGINE = INNODB DEFAULT CHARSET = utf8
```

固定格式：

```mysql
CREATE TABLE [IF NOT EXISTS] `表名`(
  `字段名` 类型 [属性] [索引] [注释],
  ...
  ...
)[表类型] [字符集设置] [注释]
```



### 修改表 / 表字段

**注意CREATE，DROP，ALERT都是表级别的操作**

修改表名：

```mysql
ALTER TABLE `表名` RENAME AS `新表名`; -- 这个反引号可以省略
```

添加字段：

```mysql
ALTER TABLE `表名` ADD `字段名` 数据类型 [其他属性]; -- 反引号还是可以省略
```

修改字段属性：

```mysql
ALTER TABLE `表名` MODIFY `要改字段名` 新的数据类型;  -- 反引可省
```

修改字段名字（和属性）:

```mysql
ALTER TABLE `表名` CHANGE `旧字段名` `新字段名` [新数据类型] [其他属性] -- 反引可省
```

删除字段:

```mysql
ALTER TABEL `表名` DROP `要删除的字段名`; -- 反引可省
```

删除表：

```mysql
DROP TABLE IF EXISTS `表名`; -- 反引可省
```



## 数据的操作（DML重点）

Data Management Language

### 插入数据

```mysql
INSERT INTO `表名`(`字段1`[,`字段2`,...]) VALUES('值1'[,'值2',...])[,('值1'[,'值2',...])];
-- 注意事项：
-- 1. 字段和值之间，要用逗号隔开，注意值有的用单引号，数值不需要
-- 2. 可以同时插入多行数据，VALUES 后面用多个括号扩起来，且也是逗号隔开
-- 3. 注意字段和值的一一对应
-- (插入数据的时候，可以不加表名，那么就需要每一个字段都要一一对应)
-- 反引可省略
INSERT INTO `student`(`name`, `hobby`, `location`, `tel`)  
-> VALUES('test5', 'dance', 'China', 123456), 
-> ('test6', 'gaming', 'CA', 666666); 
```

### 修改数据

修改数据的时候，要知道修改谁，所以需要一个给定的条件，**如果条件语句没有，则修改全部该字段的诗句**

```mysql
UPDATE `表名` SET 
-> `字段1` = `值1`, 
-> `字段2` = `值2`,
... WHERE 条件语句;
/***** 注意逗号隔开 *****/
UPDATE `student` SET 
-> `name` = 'Jack', 
-> `tel` = 626777, 
-> `hobby` = 'see' 
-> WHERE `id` >=3 AND `id` <= 5;  
```

特殊的条件语句：（大于小于啥的就不写了）

| 操作符              | 含义                 |
| ------------------- | -------------------- |
| <> 或 !=            | 不等于               |
| BETWEEN ... and ... | 闭合区间 (inclusive) |
| AND                 | 就是 &&              |
| OR                  | 就是 \|\|            |
| NOT                 | 非                   |



### 删除数据

清空表

```mysql
TRUNCATE [TABLE] `表名` [WHERE 条件语句];
DELETE FROM `表名`;
-- 相同点：都能删除数据，都不会删除表结构
-- 不同：
--   1. TRUNCATE 重新设置自增列，计数器会归零
--   2. TRUNCATE 不会影响事务
```

**了解:**

DELETE 虽然不重置自增，但是在不同引擎之间，会有不同效果

InnoDB，关闭 MySQL 后重启，自增还是从 1 开始，因为数据存在内存中，断电即失

MyISAM，重启 MySQL 会继续从上一个自增量开始，因为数据存在文件中



## 数据库查询（重点）

所有的查询都会用到 Select 语句，简单的查询和复杂的查询都能实现，数据库中最核心的语句，最重要的语句，使用频率最高。下面是固定一个模板，中括号中的内容为可选，而大括号内的内容为必选。其先后顺序固定，不可改变。

```mysql
SELECT [ALL | DISTINCT] 
{* | table.* | [table.field1[as alias1][,table.field2[as alias2]][,...]]} 
-> FROM table_name [as table_alias]
-> [left | right | inner join table_name2] -- 联合查询 
-> [WHERE ...] -- 指定结果需满足的条件 
-> [GROUP BY ...] -- 指定结果按照哪几个字段来分组 
-> [HAVING] -- 过滤分组的记录必须满足的次要条件 
-> [ORDER BY ...] -- 指定查询记录按一个或多个条件排序 
-> [LIMIT {[offset,]row_count | row_countOFFSET offset}]; -- 指定查询的记录从哪条至哪条
```



### 查询指定字段

```mysql
SELECT `字段1` [AS '别名'] [, `字段2` [AS '别名'], ...] FROM `表名`;
-- 如果字段部分用 * 的话表示查询所有数据
SELECT `name` AS '姓名' FROM student;
```



### 内容拼接

在每一个字段显示的内容中，可以拼接一些其他的说明

```mysql
SELECT CONCAT('拼接内容', `字段1`, '拼接内容') [AS '别名'] 
-> [,CONCAT('拼接内容', `字段2`, '拼接内容') [AS '别名']] 
-> FROM `表名`;
-- 注意前后都可以进行拼接
SELECT CONCAT('姓名:', `name`,'test') AS '姓名' FROM student; 
```



### 去重

在查某一览的时候可能会出现很多重复的不需要的数据，比如在查看一个 student 的表里，每个 student 有自己的 ID，对应可以有很多个成绩，所以这时候就会在一栏里出现很多重复的 id，这时候就可以去重。

```mysql
SELECT DISTINCT `字段` FROM `表名`;
```



### SELECT 表达式

SELECT 语句后面不仅仅可以选择字段，还可以在字段的基础上增加一些操作

```mysql
SELECT `字段` + n FROM `表名`;
```



### 模糊查询

模糊查询利用 like 和 in 关键词，同时有 ```%```, 和 ```_``` 符号，分别表示 0至任意个字符和 1 个字符。

比如想要查询班级里姓刘的同学可以用 like 关键字

```mysql
SELECT `StudentNo`, `StudentName` FROM `student`
-> WHERE StudentName LIKE `刘%`;
-- 这里表示只要姓刘都显示出来
-- 如果后面的 % 换成一个 _ 则表示刘后面跟一个字符的姓刘的，再加一个 _ 则为跟两个字符
```

还可以用 in 来查找一个范围内的数据

```mysql
SELECT `StudentNo`, `StudentName` FROM `student`
-> WHERE StudentNo IN (1001, 1002, 1003);
-- IN 后面要跟的是具体的一个或多个值，不可以 % 或 _
```



### 联表查询

联表查询意思其实就是关联另一个表，同时根据特定条件进行查询；它有7中 Join 方式

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20200622011633.png)



**Inner Join**

```mysql
SELECT s.studentNo, studentName, SubjectNo, StudentResult
-> FROM student AS s
-> INNER JOIN result AS r
-> ON s.studentNO = r.studentNO
-- 注意事项：
-- 1. 联表需要为其声明一个别名，这样可以确定要表的哪一个字段的数据
-- 2. WHERE 后填写的是一个特殊字段的条件，这里两表都有 studentNO 的字段
-- 3. SELECT 语句后面那个，用 s. 避免使用数据的模棱两可 (因为两表都有这个字段)
```

**( Left Join 和 Right Join 略 )**

这里区分一下三个 Join 的区别，Inner Join 会找两个表中，共有的字段，当两个表都有同样的字段且数据数量能匹配上的话，两表会完美的联合；如果出现数据的数量不一致，那么只会取其共有的部分。这样就可以 SELECT 两表中任意字段的数据了。

而 Right Join 和 Left 的 Join 的侧重点不太一样，用以下代码来说明

```mysql
SELECT s.studentNo, studentName, SubjectName, StudentResult
-> FROM student AS s
-> RIGHT JOIN result AS r  -- 注释1
-> ON s.studentNo = r.studentNo
-> INNER JOIN `subject` AS sub -- 注释2
-> ON r.subjectNo = sub.subjectNo
-- 说明：
/* 注释1：这里用 RIGHT JOIN 举例，RIGHT JOIN 会以右表为主要依据，把所有需要的数据给展示出来
 * 如果没有的话则显示为 NULL，比如一个班级有 50 个人，45 人参加考试，那么在 student 的表里有50个人
 * 而在 result 里只有 45 个人，那么 RIGHT JOIN 只会把这 45个人找出来，然后把这 45 个人的 ID 去和
 * student 表里的信息做匹配，然后返回出想要 SELECT 的字段内容，所以它只会显示考过试的
 * 注释2：这里用 INNER JOIN 就是取同一个字段里重叠的内容，比如我有 10 个学科，那就会把这 10 个
 * 对应的学科的名字都显示出来，如果没有，则 NULL 表示。
 */
```



## 外键（Foreign Key）

这个外键，其实就是类似 Java 里的引用，让表中的某一列引用到其他的表中，同时这个准备引用其他表的列的表，需要声明一个外键，这个外键的作用就是有点接收引用的意思，键命名通常以 ```FK_``` 开头。

添加外键格式:

```mysql
ADD CONSTRAINT `FK_外键名` FOREIGN KEY(`作为外键的字段`)
REFERENCES `被引用的表名`(`被引用的字段名`);

```

**以上操作都是物理外键，数据库级别的外键，不建议使用。**避免数据库过多，带来不必要的麻烦

最佳方案：

- 数据库只用来数据，行就代表数据，列代表字段
- 想使用多张表，在应用层实现



## 自连接（Recursion）

这块可是看了两遍才看明白咋回事，其实就是表内多个层级，用数字表示，从大类别（高等级）慢慢细分到小类别（低等级），把一张表的内容拆分成两个

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20200623233425.png)

以这张表为例，```pid``` 为 Parent ID，1作为最高层级出现，所以 ```pid``` 对应为 1 的软件开发、美术设计和信息技术为三个大类，其他的都围绕这三个进行展开。然后通过其他的 ```pid``` 如2，3，去找 ```pid``` 对应的相同的 ```categoryid```，比如**数据库**的 ```pid``` 为3，需要找它上一级 ```categoryid``` 为 3 的内容，其为**软件开发**；同理，**办公信息**的 ```pid``` 为 2，则应该找到**信息技术**，因为**信息技术**的 ```categoryid``` 为 2

| categoryid | categoryName |
| :--------: | :----------: |
|     2      |   信息技术   |
|     3      |   软件开发   |
|     5      |   美术设计   |

以上表格，表中这三个类别的 ```categoryid```，就是下一级（子类）的 ```pid```

| pid  | categoryid | categoryName |
| :--: | :--------: | :----------: |
|  3   |     4      |    数据库    |
|  2   |     8      |   办公信息   |
|  3   |     6      |   web开发    |
|  5   |     7      |   美术设计   |

查询父类对应的子类的关系：

|        父类        |        子类        |
| :----------------: | :----------------: |
| 信息技术（1）（3） | （2）（8）办公信息 |
| 软件开发（1）（3） |  （3）（4）数据库  |
| 软件开发（1）（3） | （3）（6）web开发  |
| 美术设计（1）（5） |  （5）（7）ps技术  |



代码实现：

```mysql
SELECT f.`categoryName` AS `父栏目`, c.`categoryName` AS `子栏目`
-> FROM `categoty` AS f, `categoty` AS c
-> WHERE a.`categoryid` = b.`pid`;
```



## 分页和排序

分页关键字 ```limit``` ，排序的关键字是 ```order by```

排序可以用 ```ASC (Ascend)``` 关键字来实现；降序则用 ```DESC (Descend)``` 来实现

```mysql
ORDER BY field1 [ASC [DESC][默认 ASC]], [field2...] [ASC [DESC][默认 ASC]]
-- 这里如果 field1 相等的话，会用 fields2 去继续排
-- ORDER BY 放在很后面的位置
```

分页可以降低数据库的压力，也可以给人更好的使用体验。这里多说一嘴瀑布流，瀑布流就是像百度图片那样，往下拉进度条它会不断的加载，好像无穷无尽一样。

```mysql
LIMIT 初始数据下标, 一页显示的数据个数
```



## 子查询（Subquery）

个人看来，子查询就是通过类似调用一个外部函数，然后利用其返回值进行判断

```mysql
select s.`StudentNo`, `StudentName`, `StudentResult` 
-> from `student` AS s INNER JOIN `result` AS r -- 联表获取到成绩
-> ON s.`StudentNo` = r.`StudentNo` -- 这个条件千万不能省
-> where r.`SubjectNo` = ( 
->   select `SubjectNo` from `subject` where `SubjectName` = '数据库结构-1' 
-> ); 
-- 这里通过子查询得到数据库结构-1对应的课号，然后将其返回给 r.`SubjectNo`
-- 这样在筛选学生成绩的时候，只会把数据库结构-1的成绩筛选出来
```

还有嵌套式写法，这里用其他的例子

```mysql
SELECT `StudentNo`, `StudentName` FROM `student` WHERE `StudentNo` IN (
->   SELECT `StudentNo` FROM `result` WHERE `StudentResult` >= 80 AND `SubjectNo` =  (
->     SELECT `SubjectNo` FROM `subject` WHERE `SubjectName` = '高等数学-2'
->   )
-> );

-- 以上先通过第一次嵌套，获取第一个条件是“成绩大于80”
-- 第二次嵌套的时候限定了具体某一个科目
-- 所以最后得到的是所有高等数学-2分数大于80的学生
-- **注意：执行顺序是由里到外的**
```



## 函数

### 常用函数（不常用）

这太多了... 用到再查吧... 引用 “菜鸟教程” 的网站

> https://www.runoob.com/mysql/mysql-functions.htm



### 聚合函数

**\*\*注意：WHERE 条件语句不能用聚合函数\*\*** 这面用 ```HAVING```



| 函数    | 描述     |
| ------- | -------- |
| COUNT() | 计数     |
| SUM()   | 求和     |
| AVG()   | 求平均值 |
| MAX()   | 最大值   |
| MIN()   | 最小值   |
| ...     | ...      |



COUNT():

```mysql
SELECT COUNT(`字段名`) FROM `表名`;  -- 只会根据选择的字段去数，忽略 NULL
SELECT COUNT(*)  FROM `表名`;  -- 这个和 1 都是查多少行，不会忽略 NULL
SELECT COUNT(1)  FROM `表名`;
```

其他几个:

```mysql
SELECT SUM(`字段名`) AS 别名 FROM `表名`;  -- 其他几个都是这个用法
...
...
```

代码示例:

```mysql
SELECT `SubjectName` AS subject, AVG(`StudentResult`), MAX(`StudentResult`), Min(`StudentResult`) -- 选取平均值，最大值和最小值，算是给了一个计算筛选规则
-> FROM `result` AS r 
-> INNER JOIN `subject` AS sub 
-> ON r.`SubjectNo` = sub.`SubjectNo`
-> GROUP BY r.`SubjectNo` -- GROUP BY 语句必须有，否则报错
-> HAVING Average >= 80; -- 可以用别名
-- 以上代码是查询每一科目的平均分，最高分和最低分
```

这里虽然狂神没对```GROUP BY``` 关键字进行过多解释，凭个人理解来记录一下自己的想法。当想显示一个表的某一字段的最大值，最小值或者平均值的时候，是很容易的，它只需要考虑自己那一列就完事了。但是如果出现联表的话，就会出现显示矛盾的情况。比如，数据库结构-1 这门课中，有一个学生得了100分，其他所有科目中都没有得100份的，那么这个 MAX 就是100，然后PS技术这们，有个同学得了1分，全年级全科目最低分，这时候MIN 就是1。此时如果只是单独从 result 表里去计算，得到结果是被允许的，但是一旦加入科目名，牵扯到另一个表的时候，就没法显示了。比如MAX 明明是数据库结构-1，而最低分是PS技术，由于最大和最小，是只能有一个的，这时候却出现了两个 SubjectName，它违背了中 “最大” 和 “最小” 的概念（一行只能显示一个最大值和一个最小值），所以此时需要牵扯到 GROUP BY。代码示例中用的是```GROUP BY r.`SubjectNo` ``` ，恰好迎合“分组”的概念，即以学科分组：求每一科目的平均值，最大值，和最小值。



## 事务

### 概述

事务英文 transaction，意表一个变化的完成；在计算机这面事务是代表一系列事情，他们都正常的完成，就叫做一个事务。其包含四个特点：

- 原子性（Atomicity）

  这个意思是要么所有任务都成功完成，要么全都失败

- 一致性（Consistency）

  事务前后的数据要保持完整，比如转账这个例子，总共就那么多钱

- 持久性（Durability）

  事务只要有提交的动作，就不可逆，持久到数据库里

- 隔离性（Isolation）

  其实这说的就是并发，数据库为每一个用户开启的事务，不能因为其他事务操作数据而受干扰



由于隔离带来的问题：

- 脏读

  一个事务读取了另一个事务的未提交的数据

- 不可重复读

  在一个事务中读取某一行的数据，多次读取的结果不同（不一定就是错的）

- 虚读（幻读）

  在一个事务中，读取到了其他事务插入的数据，导致前后读取的结果不一样



### READ UNCOMMITTED

在这个隔离级别下，会发生脏读，不可重复读，和幻读

当对一个表进行修改，且没有 Commit 的情况下，另外一个用户读到了这个 uncommitted data

![image-20201120085254964](R:/mdImages/image-20201120085254964.png)

如果此时，左面进行 rollback，那么右面再查的话，这个 500 又会变回去。

**上面同样发生了不可重复读**，右侧在同一事务中，查询同样的数据，但是返回了不同的结果。



### READ COMMITTED

在这种隔离级别下，不会读到 uncommitted 数据，但是还是会发生不可重复读

![image-20201120090033035](R:/mdImages/image-20201120090033035.png)

上图中，在提交之前，读到的 balance 还是两个 1000，在提交之后才显示出正确的值，但是两次查询的值不一样。



### REPEATABLE READ

![image-20201120090333987](R:/mdImages/image-20201120090333987.png)

在这个隔离级别中，右侧的一次事务中读取到的值都是一样的，即使左面提交了。只有当右面结束了本次事务，再去执行查询，才能看到左侧提交过的新的结果。



### SERIALIZABLE

这其实用锁了，如果一个事务开启了，去对一个表进行修改等操作，那么其他的事务想要操作这个表，或者查询这个表，是需要等之前的那个事务完成。

![image-20201120090843774](R:/mdImages/image-20201120090843774.png)

左面开启事务之后，进行修改，右面也开启，但是查询语句会暂停在那里，等待左面事务的结束。





### 使用

MySQL 默认开启 Auto Commit，开启和关闭的指令:

```mysql
SET AUTOCOMMIT = 0; -- 关闭
SET AUTOCOMMIT = 1; -- 开启
```

使用事务（其实感觉就是Java的Synchronized代码块）需要先关闭自动提交功能。

```mysql
SET AUTOCOMMIT = 0 -- 关闭自动提交
-> START TRANSACTION -- 开启事务
-> ... -- 操作
-> ... -- 操作
-> COMMIT -- 成功则提交，持久化
-> ROLLBACK -- 失败则回到开始事务前
-> SET AUTOCOMMIT = 1; -- 事务结束
```

查询隔离级别

```mysql
select @@tx_isolation;
```



设置隔离级别

```mysql
set global transaction isolation level 级别字符串;
```







## 索引

MySQL官方对索引的定义为：索引（index）是 帮助MySQL高效获取数据的数据结构。提取句子主干，就可以得到索引的本质。

### 索引分类

主键索引 (Primary Key) ：唯一的标识，主键不可重复，只能有一个列作为主键

唯一索引 (Unique) ：它与前面的普通索引类似，不同的就是，索引列的值必须唯一，但允许有空值。

常规索引 (Index) ：默认的，不设置的话就是这个，KEY 关键字设置

全文索引 (FullText)：在特定的数据库引擎下才有，快速定位数据



### 添加索引

添加索引有两个方式

```mysql
CREATE 索引类型 索引名 ON 表名(字段名(length)); 
-- 如果是CHAR，VARCHAR类型，length可以小于字段实际长度；如果是BLOB和TEXT类型，必须指定 length
ALTER TABLE 表名 ADD 索引类型 索引名(字段名字);
```



### 创造百万数据量

```mysql
DELIMITER $$
CREATE FUNCTION mock_data()
RETURNS INT
BEGIN
	DECLARE num INT DEFAULT 1000000; -- 声明变量 num
	DECLARE i INT DEFAULT 0; -- 声明变量 i
	WHILE i < num DO -- 开始循环
		INSERT INTO `user`(`name`, `gender`, `phone`)
		VALUES(
			CONCAT('user',i),
			FLOOR(RAND() * 2),
			CONCAT('18', FLOOR(RAND() * 899999999) + 100000000)
		);
		SET i = i + 1;
	END WHILE;
	RETURN i;
END

SELECT mock_data();
```

以上内容，在创建完之后，如果直接调用以下指令，耗时会比较长，系统找到这条数据之前，查询很多不必要的数据。

```mysql
SELECT * FROM `user` WHERE `name` = 'user9999';
```

此时添加普通索引

```mysql
CREATE INDEX id_user_name ON `user`(`name`);
```

之后的查询速度会非常快，其原理就是会为数据库创建一个索引文件，使其 index 一一对应每个数据的位置，这样的话想找哪一个数据就可以一次性定位到。

**添加索引前：**

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20200625170741.png)

**添加索引之后：**

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20200625170848.png)

**查看加入索引之后的 explain 结果：**

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20200625171106.png)

可见其只查询了一行

### 索引原则

- 索引不是越多越好
- 不要对经常变动的数据加索引
- 小数据量的表建议不要加索引
- 索引一般应加在查找条件的字段



## 权限

### 基本操作

创建用户：

```mysql
CREATE USER 用户名 IDENTIFIED BY '密码';
```

修改当前用户密码：

```mysql
SET PASSWORD = PASSWORD('密码');
```

修改其他用户密码：

```mysql
SET PASSWORD FOR 用户名 = PASSWORD('密码');
```

重命名：

```mysql
RENAME USER 原用户名 TO 新用户名;
```

用户授权：

```mysql
GRANT ALL PRIVILEGES ON *.* TO 用户名;
-- 这个赋予了除 GRANT 权限外的所有权限
```

查询权限：

```mysql
SHOW GRANTS FOR 用户名[@localhost];
-- 查看该用户权限
```

撤销权限：

```mysql
REVOKE ALL PRIVILEGES ON *.* FROM 用户名;
```

删除用户：

```mysql
DROP USER 用户名;
```



## 备份

数据备份有很多方式，可以备份一整个数据库，可备份单个或者多个表

1. 物理拷贝，直接把本地的数据文件拷贝走
2. 使用可视化工具可以直接右键导出
3. 使用 ```mysqldump``` 命令

```
mysqldump -h host -u username -p database [table1 table2 ...] > PATH/xxx.sql
/*
 * -h后接数据库地址
 * database 为数据库名
 * table 之间用空格隔开！
 * PATH 就是路径，文件要以 .sql 结尾
 */
```

用命令行的话，也可以导入 .sql 文件

```mysql
USE 数据库;
SOURCE 路径;
```



## 数据库设计

**糟糕的数据库设计：**

- 数据冗余
- 存储空间浪费
- 数据更新和插入的异常
- 程序性能差

**良好的设计库设计：**

- 节省数据的存储空间
- 能够保证数据的完整性
- 方便进行数据库应用系统的开发

**设计一个数据库要注意的：**

- 需求分析阶段: 分析客户的业务和数据处理需求 
- 概要设计阶段:设计数据库的E-R模型图 , 确认需求信息的正确和完整

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20200625220840.png)

这块偷懒截图了...

## 规范

### 三大范式

**第一范式 (1st NF)**

第一范式的目标是确保每列的原子性，说的通俗点就是这一列不能在细分成其他的类别了

**第二范式(2nd NF)**

前提：满足第一范式

第二范式要求每个表只描述一件事情

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20200625230319.png)

以这张表举例，在其中一个订单里，卖一个产品，然后对应的卖出的金额，这实际上是两回事，一个意在详细注重描述卖出的产品的相关信息，一个是在描述订单的相关的信息，所以尽可能的细分就尽可能细分。

**第三范式(3rd NF)**

如果一个关系满足第二范式,并且除了主键以外的其他列都不传递依赖于主键列,则满足第三范式.

第三范式需要确保数据表中的每一列数据都和主键直接相关，而不能间接相关。

![image-20200625230828319](/Users/guanyuding/Library/Application Support/typora-user-images/image-20200625230828319.png)

第三范式稍微好理解一点，比如我第一个就是想描述学生表，那在学生表中没必要把班主任的其他信息给写出来，我们只需要保留班主任姓名来作为关联部分，再去创造一个班主任表就可以了。



### 规范性和性能之间

- 为满足某种商业目标 , 数据库性能比规范化数据库更重要
- 在数据规范化的同时 , 要综合考虑数据库的性能
- 有时候故意增加冗余字段，避免联表查询，节省时间提高性能
- 通过在给定的表中插入计算列,以方便查询





## ResultSet

ResultSet：结果集对象，封装查询结果
		* boolean next(): 游标向下移动一行，判断当前行是否是最后一行末尾(是否有数据)，如果是，则返回false，如果不是则返回true
		* getXXX(参数):获取数据
			* 参数：
				1. int：代表列的编号,从1开始   如： getString(1)
				2. String：代表列名称。 如： getDouble("balance")
		

```java
while(rs.next()){
    //获取数据
    //6.2 获取数据
    int id = rs.getInt(1);
    String name = rs.getString("name");
    double balance = rs.getDouble(3);

    System.out.println(id + "---" + name + "---" + balance);
}
```
### PrepareStatement

PrepareStatement 可以防止 SQL 注入，效率更高。SQL 注入的本质就是依靠非法字符串的拼接，来得到一个 True 的返回结果，然后骗过服务器验证，这样就能获取到服务器里的数据。

PrepareStatement 实际就是把 sql 语句中所有的要传的值都变成了问号 ```?```， 然后通过 PrepareStatement 自身的 SetXX 的方法来对这些 ```?``` 赋值

```java
String sql = "SELECT * FROM student WHERE id = ?";
PrepareStatement pst = conn.prepareStatement(sql);
pst.setInt(1, 5);
// 其他的 set 方法也一样，第一个参数表第几个问号，从 1 开始，然后第二个参数就是要传的值
// 其余使用方法一致，在 execute 的时候不需要再传参数了
```



### JDBC操作事务

这随便提一嘴，想手动操作事务要通过 Connection 对象 setAutoCommit 为 false 然后就可以正常写自己的代码了，在最后执行一下 commit 语句，然后在 finally 里协商 rollBack ，这个 rollBack 其实不写也可以，出错默认回滚。



### DBCP/C3P0

连接池基本思想就是通过 DataSource 接口来实现，每次需要取得 Connection 对象的时候，来这个 pool 里取；这个 pool 和线程池一个意思，可以避免多次建立连接又断开连接，节省资源。各项设置呢，都有一个配置文件，比如 DBCP 用 .properties，C3P0 用 .xml 文件。这些配置文件都会配置好要用哪个数据库驱动，协议是什么，软口，host，username，password，等等一些列参数，都会在配置文件里写好。然后直接通过一个 getConnetion 的方法，获取到数据库对象，然后再通过 Statement 对象来编译 sql 语句。

DBCP怎么都跑不成功... 先暂时不写... 

C3P0 感觉还不错，还带日志的，直接贴代码了。
```xml
<?xml version="1.0" encoding="UTF-8"?>

<c3p0-config>
    <!-- C3P0的缺省(默认)配置，
    如果在代码中“ComboPooledDataSource ds = new ComboPooledDataSource();”
    这样写 就表示使用的是C3P0的缺省(默认)配置信息来创建数据源
    -->
    <default-config>
        <property name="driverClass">com.mysql.jdbc.Driver</property>
        <property name="jdbcUrl">jdbc:mysql://localhost:3306/jdbcStudy? useUnicode=true&amp;characterEncoding=utf8&amp;useSSL=true</property>
        <property name="user">root</property>
        <property name="password">123456</property>
        <property name="acquireIncrement">5</property>
        <property name="initialPoolSize">10</property>
        <property name="minPoolSize">5</property>
        <property name="maxPoolSize">20</property>
    </default-config>

    <!--C3P0的命名配置，如果在代码中“ComboPooledDataSource ds =
    new ComboPooledDataSource("MySQL");”
    这样写就表示使用的是name是MySQL的配置信息来创建数据 源
    -->
    <named-config name="MySQL">
        <property name="driverClass">com.mysql.jdbc.Driver</property>
        <property name="jdbcUrl">jdbc:mysql://localhost:3306/school?useUnicode=true&amp;characterEncoding=utf8&amp;useSSL=false</property>
        <property name="user">root</property>
        <property name="password">111111</property>
        <property name="acquireIncrement">5</property>
        <property name="initialPoolSize">10</property>
        <property name="minPoolSize">5</property>
        <property name="maxPoolSize">20</property>
    </named-config>

</c3p0-config>
```

Utils文件代码：

```java
package com.ding.pool;

import com.mchange.v2.c3p0.ComboPooledDataSource;
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

public class JdbcUtils_C3P0 {
        private static ComboPooledDataSource ds = null;

        static {
            try {
                ds = new ComboPooledDataSource("MySQL");
            } catch (Exception e) {
                e.printStackTrace();
            }
        }

        public static Connection getConnection() throws SQLException {
            return ds.getConnection();
        }

    public static void release(Connection conn, Statement st, ResultSet rs) {
       // Code.. 省略，上面有
    }
}

```

测试代码：

```java
package com.ding.pool;

import java.sql.*;

public class TestC3P0 {
    public static void main(String[] args) throws SQLException {
        Connection conn = null;
        PreparedStatement st = null;
        ResultSet rs = null;
        try {
            conn = JdbcUtils_C3P0.getConnection();
            String sql = "SELECT StudentName, StudentNo FROM student WHERE StudentNo = ?";
            st = conn.prepareStatement(sql);
            st.setInt(1, 1003);
            rs = st.executeQuery();
            while (rs.next()) {
                System.out.println(rs.getString("StudentName"));
                System.out.println(rs.getString("StudentNo"));
                System.out.println("===========");
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
        finally {
            JdbcUtils_C3P0.release(conn, st, rs);
        }
    }
}
```



## 完结啦

下一步就是去搞前端，然后复习 Java，这两样同时搞完之后去看 Java Web 开发。前端的 JS 可能会持久化学习... 考虑将其作为自己第二编程语言，2020年6月26日。



# JDBC



## 概述

Java Database Connectivity 实际上是一套规范，在用 Java 操作数据库的时候，是需要有数据库驱动才可以通过 Java 来操作数据库，而不同的数据库厂商都有自己的驱动，这就可能会出现 Java 里光导入数据库驱动就会导入一大堆，所以呢就有一个 JDBC 规范，这些厂商遵循这个规范，那开发人员只需要掌握 JDBC 这一套规范就可以操作所有的数据。

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20200625233642.png)

用 Java 操作数据库的时候需要导入三个包：

- java.sql
- javax.sql
- 数据库驱动包 mysql-connector-X.X.XX.jar



## 第一次通过JDBC操作

```java
package com.ding.sql;
import java.sql.*;

public class SQLPractice {
    public static void main(String[] args) throws ClassNotFoundException, SQLException {
        String url = "jdbc:mysql://localhost:3306/school?useUnicode=true" +  // 固定格式
                "&&characterEncoding=utf8" +  // 填写地址 "?" 后面是一些可修改的参数
                "&&useSSL=false";
        String user = "root";  // 输入用户名
        String pwd = "111111";  // 输入密码
		// 加载驱动，这个Driver里有一块静态代码块，会自己调用 DriverManager 的 Register 方法
        Class.forName("com.mysql.jdbc.Driver");
		// 建立链接，本质就是获取到数据库对象，可以做库级别的操作，比如回滚
        Connection connection = DriverManager.getConnection(url, user, pwd);  
		// 得到声明对象，实际这东西就像是 Navicat 里的查询窗口对象一样，可以输入 sql 语句
        Statement statement = connection.createStatement();
		// 要写的 sql 语句
        String sql = "SELECT StudentNo, StudentName, Phone, Address FROM student";

        ResultSet resultSet = statement.executeQuery(sql);

        while (resultSet.next()) {
            System.out.println(resultSet.getObject("StudentNo"));  // 这个字段名要写对
            System.out.println(resultSet.getObject("StudentName"));
            System.out.println(resultSet.getObject("Phone"));
            System.out.println(resultSet.getObject("Address"));
            System.out.println("=====================");
            // 这里还有 getXXX() 其他类型数据，前提是已知
            // resultSet 也有 previous 方法
        }
		// 这个关闭按照从后往前的顺序关
        resultSet.close();
        statement.close();
        connection.close();  // 最耗资源
    }
}

```



## 通过工具类来实现

由于上述代码太过复杂，而且耦合性高，可以把上述代码分成几块，比如用 properties 文件来存 driver，url，username，和 password 等需要的信息，然后通过一个 Utils 类来进行读取，而且通过这个类，可以直接获取到 Connection 对象，会让整个过程变得更清晰，耦合性也会降低一些。

先是创建 properties 文件存基本信息：

```java
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/school?useUnicode=true&characterEncoding=utf8&useSSL=false
username=root
password=111111
```

然后去写 Utils 类：（使用 Druid 连接池）

```java
import com.alibaba.druid.pool.DruidDataSourceFactory;

import javax.sql.DataSource;
import java.io.IOException;
import java.io.InputStream;
import java.sql.Connection;
import java.sql.SQLException;
import java.util.Properties;

/**
 * JDBC 工具类，用 Durid 连接池
 */
public class JDBCUtils {
    private static DataSource ds;

    static {

        try {
            // 加载配置文件
            Properties pro = new Properties();
            InputStream is = JDBCUtils.class.getClassLoader().getResourceAsStream("druid.properties");
            pro.load(is);
            // 初始化连接池对象
            ds = DruidDataSourceFactory.createDataSource(pro);

        } catch (IOException e) {
            e.printStackTrace();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    /**
     * 获取连接池对象
     */
    public static DataSource getDataSource() {
        return ds;
    }

    /**
     * 获取连接
     */
    public static Connection getConnection() throws SQLException {
        return ds.getConnection();
    }

}
```





## 补充管理用户

1. 管理用户
    1. 添加用户：

        ​	语法：CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码';

        2. 删除用户：

          ​	语法：DROP USER '用户名'@'主机名';
        3. 修改用户密码：

          ```mysql
          UPDATE USER SET PASSWORD = PASSWORD('新密码') WHERE USER = '用户名';
          UPDATE USER SET PASSWORD = PASSWORD('abc') WHERE USER = 'lisi';
          
          SET PASSWORD FOR '用户名'@'主机名' = PASSWORD('新密码');
          SET PASSWORD FOR 'root'@'localhost' = PASSWORD('123');
          ```

          * mysql 中忘记了 root 用户的密码？
            1. cmd -- > net stop mysql 停止mysql服务

              ​	需要管理员运行 cmd

            2. 使用无验证方式启动mysql服务： mysqld --skip-grant-tables
            3. 打开新的cmd窗口,直接输入mysql命令，敲回车。就可以登录成功
            4. use mysql;
            5. update user set password = password('你的新密码') where user = 'root';
            6. 关闭两个窗口
            7. 打开任务管理器，手动结束mysqld.exe 的进程
            8. 启动mysql服务
            9. 使用新密码登录。
        4. 查询用户：
            -- 1. 切换到mysql数据库
            USE myql;
            -- 2. 查询user表
            SELECT * FROM USER;

          ​	通配符： % 表示可以在任意主机使用用户登录数据库



## 补充 Spring Template

需要一共下面五个 Jar 包

![image-20201030234725942](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/image-20201030234725942.png)

其提供了很多封装查询结果的方法，简化 JDBC 开发

创建 JdbcTemplate 对象，依赖 DataSource

```java
JdbcTemplate template = new JdbcTemplate(ds);
```

调用 JdbcTemplate 的方法染成 CRUD 操作：

```java
update();
queryForMap();
queryForList();
query();  // 返回一个装有 Bean 对象的 List
queryForObject();
```

快速使用：

```java
public static void main(String[] args) {
    JdbcTemplate tm = new JdbcTemplate(JDBCUtils.getDataSource());
    String sql = "update account set balance = 500 where id = ?";
    int count = tm.update(sql, 3);  // 后面为 prepared statement 的参数，位置一一对应
    System.out.println(count;)
}
```

不需要进行资源的释放，其框架本身会执行释放资源等操作。

这里多补充一下 ```query()``` 这个方法，这个方法的第一个参数为一个 SQL 语句，第二个为一个实现了RowMapper 的对象，这个对象可以自己去用匿名内部类实现，也可以通过框架本身提供的一个方法去实现。

比如，此时我们有一个 Emp （Employee）的表，然后设计了一个 Emp 的 Class，里面的 fields 和表头一一对应，然后都写好了 Setter 和 Getter

此时如果我们要自己去实现的话，写法如下：

```java
public void test() {
    String sql = "select * from emp";
    List<Emp> result = template.query(sql, new RowMapper<Emp>() {
        
        @Override
        public Emp mapRow(ResultSet rs, int i) throws SQLExeption() {
            Emp emp = new Emp();
            int id = rs.getInt("id");
            // ... 一些列获取的语句
            
            emp.setId(id);
            // ... 一些列设置的语句
            return  emp;
        }
    })
}
```

上面这样写，还是很麻烦，没有从根本上简化代码量，还是手动设置了一大堆，所以可以框架提供的一个实现类：

```java
public void test() {
    String sql = "select * from emp";
    List<Emp> result = template.query(sql, new BeanPropertyRowMapper<Emp>(Emp.class));
}
```

这样这个 result 里存的就是所有的 Emp 对象，其实这个 Emp 对象就是对应一个 row 里的每一个值

**有个很重要的要注意的**，由于在定义 Emp Class 的时候，那些 Fields 里有基本数据类型，不能接受 null 值，所以可以将定义为引用数据类型，让其自己完成自动拆装箱的操作。个人猜测其底层实现是要求这个基本类 （Emp）的 Fields 必须严格一一对应数据库的字段，（也可能是起字段名字和 fields 的名字要一致）。



另外最后那个 ```queryForObject``` 主要用来执行一些聚合函数的，起查询结果本身是具体的一个数，或者字符串之类的。比如 ```COUNT()``` 就会返回一个 Long 类型的值

```java
public void test() {
    String sql = "select count(id) from emp";
    Long total = template.queryForObject(sql, Long.class);
    System.out.println(total);
}
```



## 补充用户登录案例

先介绍一下目录：

![image-20201201003152483](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/image-20201201003152483.png)

DAO 全称为 Data Access Object，起本身起到获取数据库中某一行数据的。

Domain 包下都是对应数据库中的表，设计对应的封装对象。

案例中使用了这些包：

![image-20201201003352962](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/image-20201201003352962.png)



### User

```java
package cn.guanyu.domain;

public class User {
    private int id;
    private String username;
    private String password;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", username='" + username + '\'' +
                ", password='" + password + '\'' +
                '}';
    }
}

```



### UserDao

```java
package cn.guanyu.dao;

import cn.guanyu.Util.JDBCUtils;
import cn.guanyu.domain.User;
import org.springframework.dao.DataAccessException;
import org.springframework.jdbc.core.BeanPropertyRowMapper;
import org.springframework.jdbc.core.JdbcTemplate;

/**
 * 操作数据库中 User 表的类
 */
public class UserDao {
    // 声明 JDBCTemplate 对象共用
    private JdbcTemplate template = new JdbcTemplate(JDBCUtils.getDataSource());

    /**
     * 登录方法
     * @param loginUser 只有用户名和密码
     * @return 包含用户的全部数据
     */
    public User login(User loginUser) {
        try {
            String sql = "select * from user where username = ? and password = ?";
            return template.queryForObject(sql,
                    new BeanPropertyRowMapper<User>(User.class),
                    loginUser.getUsername(),
                    loginUser.getPassword());
        } catch (DataAccessException e) {
            // 这里后期还会记录日志
            return null;
        }
    }
}
```



### LoginServlet

这里要单独说一下 BeanUtils 这个工具类，它提供了一个非常便利的方法来直接对 loginuser 对象进行一个初始化的操作。这里要求 User 对象中必须提供标准的 get/set 方法。

**在写 User 对象的时候，那个 ```getXXX()```  的这个 ```XXX``` 是 BeanUtils 要使用的变量，注意不要写错了**

```java
package cn.guanyu.web.servlet;

import cn.guanyu.dao.UserDao;
import cn.guanyu.domain.User;
import org.apache.commons.beanutils.BeanUtils;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.lang.reflect.InvocationTargetException;
import java.util.Map;

@WebServlet("/test-login")
public class LoginServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 1.设置编码
        request.setCharacterEncoding("utf-8");
//        // 2. 获取参数
//        String username = request.getParameter("username");
//        String password = request.getParameter("password");
//        // 3. 封装 User 对象
//        User loginuser = new User();
//        loginuser.setUsername(username);
//        loginuser.setPassword(password);

        // 2. 获取所有参数集合
        Map<String, String[]> map = request.getParameterMap();
        // 3.1 创建 User 对象
        User loginuser = new User();
        // 3.2 使用 BeanUtils 来封装
        try {
            BeanUtils.populate(loginuser, map);
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        } catch (InvocationTargetException e) {
            e.printStackTrace();
        }


        // 4. 调用 UserDao 的 login 方法，从数据库中尝试获取对应 user 对象
        UserDao dao = new UserDao();
        User user = dao.login(loginuser);

        // 5. 判断 user
        if (user == null) {
            // 登录失败
            request.getRequestDispatcher("/failServlet").forward(request, response);
        }
        else {
            // 登录成功
            // 存储数据
            request.setAttribute("user", user);
            request.getRequestDispatcher("/successServlet").forward(request, response);
        }


    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}

```

