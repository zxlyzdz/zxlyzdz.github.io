layout: post
title: MySQL
date: 2021-03-30
Author: 龙龙
categories: 
tags: [MySQL]
comments: true

### MySQL

JavaEE：企业级Java开发 web

前端（页面：展示数据）

后台（连接点：连接数据库，连接前端（控制，控制视图跳转，和给前端传递数据））

数据库（存数据）

> 学好数据库
>
> 操作系统，数据结构与算法！
>
> 离散数学，数字电路，体系结构，编译原理，实战经验

### 1、初识MySQL

#### 1.1、为什么要学习数据库

1、岗位需求

2、现在的世界，大数据时代，得数据者得天下

3、被迫需求：存数据

**4、数据库是所有软件体系中最核心的存在**

#### 1.2、什么是数据库

数据库：DB，database

概念：数据仓库。软件，安装在操作系统上！SQL，可以存储大量的数据，500W！

作用：存储数据，管理数据

#### 1.3、数据库分类

* 关系型数据库（SQL）
  * MySQL，Oracle，SQL server，SQlite
  * 通过表和表之间、行和列之间的关系进行数据的存储
* 非关系型数据库（NoSQL）
  * Redis，MongoDB
  * 非关系型数据库，对象存储，通过对象的自身的属性来决定
* **DBMS：数据库管理系统**
  * 数据库的管理软件，科学有效的管理数据，易于维护和管理数据
  * MySQL，本质上就是一个数据库管理系统！

#### 1.4、MySQL简介

MySQL是一个**关系型数据库管理系统**，

MySQL是最好的 RDBMS（Rational Database Management System，关系型数据库管理系统）应用软件之一

开源的数据库软件

体积小、速度快、总体拥有成本低、招人成本比较低，所有人必须会

中小型网站、或者大型网站都在用

官网：https://www.mysql.com

版本：5.7 稳定 

安装建议：

1、尽量不要使用 exe

2、尽可能使用压缩包安装

#### 1.5、连接数据库

命令行连接

连接数据库

```sql
mysql -u root -p
```

**连接了数据之后，所有的SQL语句都要以 ; 结尾**

显示所有的数据库

```sql
show databases;
```

创建数据库

```sql
create database school;
```

使用数据库

```sql
use school; --use 数据库名
```

显示该数据库中所有的表

```sql
show tables;
```

显示数据中指定表的信息

```sql
describe student;
```

退出连接

```sql
exit;
```

注释

```sql
-- 单行注释
/*
多行注释
多行注释
多行注释
*/
```



* DDL：数据库定义语言
* DML：数据库操作语言
* DQL：数据库查询语言
* DCL：数据库控制语言

### 2、操作数据库

操作数据库 >  操作数据库中的表 > 操作数据库中表的数据

#### 2.1、操作数据库

1、创建数据库

```sql
CREATE DATABASE IF NOT EXISTS school;
```

2、删除数据库

```sql
DROP DATABASE IF EXISTS school; 
```

3、使用数据库

```sql
-- 如果表名或者字段名是一个特殊字符，就需要带``
USE `school`;
```

4、查询数据库

```sql
-- 查询所有数据库
SHOW DATABASES;
```

对比：SQLyog的可视化操作

**学习思路：**

* 对照sqlyog可视化历史记录查看sql
* 固定的语法或关键字必要强行记住

#### 2.2、数据库的列类型

>数值：

* tinyint：十分小的数据，占 1 个字节
* smallint：较小的数据，占 2 个字节
* mediumint：中等大小的数据，占 3 个字节
* **int：标准的整数，占 4 个字节**
* bigint：较大的数据，占 8 个字节

* float：单精度浮点数，占 4 个字节
* double：双精度浮点数，占 8 个字节
* decimal：字符串形式的浮点数，金融计算的时候，一般是使用 decimal

>字符串

* char：固定大小的字符串， 0~255
* **varchar：可变字符串，0~65535，常用**
* tinytext：微型文本，2 ^ 8 - 1
* **text：文本串，2 ^ 16 - 1，保存大文本**

> 时间日期

* date：YYYY-MM-DD 日期
* time：HH:mm:ss 时间
* **datetime：YYYY-MM-DD HH:mm:ss 最常用的时间格式**
* **timestrap：时间戳，1970.01.01 00：00：00 到现在的毫秒数，也较为常用**
* year：年份表示

> null

* 没有值，未知
* **注意：不要使用 null 进行运算**

#### 2.3、数据库的字段属性（重点）

**Unsigned**

* 无符号整数
* 声明了该列不能声明为负数

**zerofill**

* 0 填充
* 不足的位数使用 0 填充，int(3)，5 ... 005

**自增**

* 通常理解为自增，自动在上一条记录的基础上 + 1（默认）
* 通常用来设计唯一的主键~index，必须是整数类型
* 可以自定义设计主键自增的起始值和步长 

**非空** NULL, NOT NULL

* 假设设置为 not null，如果不给它赋值，就会报错！
* NULL，如果不填写值，默认就是 NULL

**默认**

* 设置默认的值
* sex，默认值为男，如果不指定该列的值，则会有默认的值！

**拓展**

```sql
/* 每一个表，都必须存在以下五个字段！未来做项目用的，表示一个记录存在的意义。
id 主键
'version' 乐观锁
is_delete 伪删除
gmt_create 创建时间
gmt_update 修改时间
*/
```

#### 2.4、创建数据库表

```sql
-- 目标：创建一个 school 数据库
-- 创建学生表（列，字段） 使用 SQL 创建
-- 学号 int 登陆密码 varchar(20) 姓名 varchar(20) 性别 varchar(2) 出生日期 datetime 家庭地址 varchar(50) 邮箱 email
-- 注意点：使用英文()，表的名称和字段尽量使用 `` 括起来
-- AUTO_INCREMENT 自增
-- 字符串 使用 '' 括起来
-- 所有的语句后面加 , (英文)，最后一个字段不用加
-- PRIMARY KEY 主键，一般一个表只有一个主键
CREATE TABLE IF NOT EXISTS `student` (
    `id` INT(4) NOT NULL AUTO_INCREMENT COMMENT '学号',
    `name` VARCHAR(20) NOT NULL DEFAULT '匿名' COMMENT '姓名',
    `password` VARCHAR(20) NOT NULL DEFAULT '123456' COMMENT '密码',
    `sex` VARCHAR(2) NOT NULL DEFAULT '女' COMMENT '性别',
    `birthday` DATETIME DEFAULT NULL COMMENT '出生日期',
    `address` VARCHAR(100) DEFAULT NULL COMMENT '家庭住址',
    `email` VARCHAR(50) DEFAULT NULL COMMENT '邮箱',
    PRIMARY KEY(`id`)
) ENGINE INNODB DEFAULT CHARSET=utf8
```

**格式：**

```sql
CREATE TABLE IF NOT EXISTS `表明`(
	`字段名` 列类型 [属性] [索引] [注释],
    `字段名` 列类型 [属性] [索引] [注释],
    ...
    `字段名` 列类型 [属性] [索引] [注释]
)[表类型][字符集设置][注释]
```

**常用命令**

```sql
SHOW CREATE DATABASE school -- 查看创建数据库语句
SHOW CREATE TABLE `student` -- 查看创建表语句
DESC `student` -- 查看表结构
```

#### 2.5、数据表的类型

```sql
-- 关于数据库引擎
/*
INNODB：默认使用
MYISAM：早些年使用的
*/
```



|            | MYISAM | INNODB                 |
| ---------- | ------ | ---------------------- |
| 事务支持   | 不支持 | 支持                   |
| 数据行锁定 | 不支持 | 支持                   |
| 外键约束   | 不支持 | 支持                   |
| 全文索引   | 支持   | 不支持                 |
| 表空间大小 | 较小   | 较大，约为MYISAM的两倍 |

**常规使用操作**

* MYISAM：节约空间，速度较快
* INNODB：安全性高，支持事务处理，多表多用户操作

> 在物理空间存在的位置

* 所有的数据库文件都存在 data 目录下，一个文件夹对应一个数据库
* 本质还是文件的存储！

MySQL 引擎在物理文件上的区别

* InnoDB 在数据库表中只有一个 *.frm 文件，以及上级目录下的 ibdata 文件
* MYISAM 对应文件
  * *.frm   表结构的定义文件
  * *.MYD  数据文件（data）
  * *.MYI   索引文件（index）

> 设置数据库表的字符集编码

```sql
CHARSET=utf8
```

不设置的话，会是 MySQL 默认的字符集编码~ (不支持中文)

MySQL 的默认编码是 Latin1，不支持中文

#### 2.6、修改和删除表

> 修改

 ```sql
-- 修改表名 ALTER TABLE 旧名表 RENAME AS 新表名
ALTER TABLE `teacher` RENAME AS `teacher1`
-- 增加表的字段 ALTER TABLE 表名 ADD 字段名 列属性
ALTER TABLE `teacher1` ADD age INT(11)
-- 修改表的字段（重命名，修改约束！）
-- ALTER TABLE 表名 MODIFY 字段名 列属性[]
ALTER TABLE `teacher1` MODIFY age VARCHAR(11)  -- 修改约束
-- ALTER TABLE 表名 CHANGE 旧名字 新名字 列属性[] 
ALTER TABLE `teacher1` CHANGE age age1 INT(11) -- 字段重命名

-- 删除表字段
-- ALTER TABLE 表名 DROP 字段名
ALTER TABLE `teacher1` DROP age1
 ```

> 删除

```sql
-- 删除表（如果表存在再删除）
DROP TABLE IF EXISTS `teacher1` 
```

**所有的创建和删除操作尽量加上判断，以免报错**

**注意点**

* `` 字段名，表名使用这个括起来
* 注释 -- /**/
* sql 关键字大小写不敏感，建议小写
* 所有的符号要用英文

### 3、MySQL数据管理

#### 3.1、外键（了解即可）

> 方式一、在创建表的时候，增加约束（麻烦，比较复杂）

```sql
-- 定义一个年级表
CREATE TABLE IF NOT EXISTS `grade` (
    `gradeId` INT(10) NOT NULL AUTO_INCREMENT COMMENT '年级id',
    `gradename` VARCHAR(20) NOT NULL COMMENT '年级名称',
    PRIMARY KEY(`gradeId`)
) ENGINE=INNODB DEFAULT CHARSET=utf8

-- 学生表的 gradeId 字段，要去引用年级表的 gradeId
-- 定义外键 key
-- 给这个外键添加约束（执行引用）  references 引用
CREATE TABLE IF NOT EXISTS `student` (
    `id` INT(4) NOT NULL AUTO_INCREMENT COMMENT '学号',
    `name` VARCHAR(20) NOT NULL DEFAULT '匿名' COMMENT '姓名',
    `password` VARCHAR(20) NOT NULL DEFAULT '123456' COMMENT '密码',
    `sex` VARCHAR(2) NOT NULL DEFAULT '女' COMMENT '性别',
    `birthday` DATETIME DEFAULT NULL COMMENT '出生日期',
    `gradeId` INT(10) NOT NULL COMMENT '学生的年级',
    `address` VARCHAR(100) DEFAULT NULL COMMENT '家庭住址',
    `email` VARCHAR(50) DEFAULT NULL COMMENT '邮箱',
    PRIMARY KEY(`id`),
    KEY `FK_gradeId` (gradeId), 
    CONSTRAINT `FK_gradeId` FOREIGN KEY (`gradeId`) REFERENCES `grade`(`gradeId`)
) ENGINE=INNODB DEFAULT CHARSET=utf8
```

**删除有外键关系的表的时候，必须要先删除引用别人的表（从表），再删除被引用的表（主表）**

> 方式二：创建表成功后，添加外键约束

```sql
-- 定义一个年级表
CREATE TABLE IF NOT EXISTS `grade` (
    `gradeId` INT(10) NOT NULL AUTO_INCREMENT COMMENT '年级id',
    `gradename` VARCHAR(20) NOT NULL COMMENT '年级名称',
    PRIMARY KEY(`gradeId`)
) ENGINE=INNODB DEFAULT CHARSET=utf8


-- 学生表的 gradeId 字段，要去引用年级表的 gradeId
-- 定义外键 key
-- 给这个外键添加约束（执行引用）  references 引用
CREATE TABLE IF NOT EXISTS `student` (
    `id` INT(4) NOT NULL AUTO_INCREMENT COMMENT '学号',
    `name` VARCHAR(20) NOT NULL DEFAULT '匿名' COMMENT '姓名',
    `password` VARCHAR(20) NOT NULL DEFAULT '123456' COMMENT '密码',
    `sex` VARCHAR(2) NOT NULL DEFAULT '女' COMMENT '性别',
    `birthday` DATETIME DEFAULT NULL COMMENT '出生日期',
    `gradeId` INT(10) NOT NULL COMMENT '学生的年级',
    `address` VARCHAR(100) DEFAULT NULL COMMENT '家庭住址',
    `email` VARCHAR(50) DEFAULT NULL COMMENT '邮箱',
    PRIMARY KEY(`id`)
) ENGINE=INNODB DEFAULT CHARSET=utf8

-- 创建表的时候没有外键关系
-- ALTER TABLE 表名 ADD CONSTRAINT 约束名 FOREIGN KEY(作为外键的列) REFERENCES 被接外键的那个表(对应的字段)
ALTER TABLE `student` 
ADD CONSTRAINT `FK_gradeId` FOREIGN KEY(`gradeId`) REFERENCES `grade`(`gradeId`);
```

以上的操作都是物理外键，数据库级别的外键，不建议使用！（避免数据库过多造成过多困扰，了解即可）

**最佳实现**

* 数据库就是单纯的表，只用来存数据，只有行（数据）和列（字段）
* 我们想使用多张表的数据，想使用外键（用程序去实现）

#### 3.2、DML语言（全部记住）

数据库存在意义：存储数据，管理数据

DML语言：数据操作语言

* insert：插入
* update：修改
* delete：删除

#### 3.3、添加

> insert

```sql
-- 插入数据(SQL语句)
-- insert into 表名 ([字段名1],[字段名2]) values ('值1')，(‘值2’)
INSERT INTO `grade`(`gradename`) VALUES ('大三')

-- 由于主键自增我们可以省略（如果不写表的字段，它会一一匹配 ）
-- 一般写插入语句，我们一定要保证字段和数据一一对应

-- 插入多个值
INSERT INTO `grade`(`gradename`) 
VALUES ('大二'), ('大一')

-- 插入多个字段的多个值
INSERT INTO `student`(`name`, `password`, `sex`) 
VALUES ('张三', '123456', '男'), ('李四', '123456', '男'), ('王五', '123456', '男')
```

语法：`insert into 表名 ([字段名1],[字段名2]) values ('值1', '值2'), ('值3', '值4')`

注意事项：

1. 字段和字段之间要用英文的逗号隔开
2. 字段是可以省略的，但是后面的值要一一对应
3. 可以同时插入多条数据，VALUES 后面的多个值之间要用逗号隔开 `VALUES(...), (...).....`

#### 3.4、修改

> update 修改谁 {条件}  set 原来的值 = 新值

```sql
-- 修改学生名字，带了 where 条件
UPDATE `student` SET `name`='我是你大哥' WHERE id = 1

-- 不指定条件的情况下，会改动所有表
UPDATE `student` SET `name`='长江七号'

-- 修改多个属性，逗号隔开
UPDATE `student` SET `name`='我是你大哥', `email`='122345661@qq.com' WHERE id = 1

-- 语法：
-- UPDATE 表名 SET column_name = value [, column_name = value ......] WHERE [条件]
```

条件：where 子句 运算符  id 等于某个值，大于某个值，在某个区间内修改

操作符会返回布尔值

| 操作符              | 含义         | 范围            | 结果  |
| ------------------- | ------------ | --------------- | ----- |
| =                   | 等于         | 5 = 6           | false |
| <> 或 !=            | 不等于       | 5 <> 6          | true  |
| >                   | 大于         | 5 > 4           | true  |
| <                   | 小于         | 5 < 4           | false |
| >=                  | 大于或等于   | 3 >= 3          | true  |
| <=                  | 小于或等于   | 3 <= 2          | false |
| BETWEEN ... AND ... | 在某个范围内 | [2, 5]          | true  |
| AND                 | 与 &&        | 5 > 1 AND 1 > 2 | false |
| OR                  | 或  \|\|     | 5 > 1 OR 1 > 2  | true  |

```sql
-- 通过多个条件定位数据
UPDATE `student` SET `name`='叫我一声大哥' WHERE `name` = '长江七号' AND `email` = '122345661@qq.com'
```

语法：`UPDATE 表名 SET column_name = value [, column_name = value ......] WHERE [条件]`

注意：

* column_name 是数据库的列，尽量带上 ``

* 条件，筛选的条件，如果没有指定，则会修改所有的列

* value，是一个具体的值，也可以是一个变量

  ```sql
  UPDATE `student` SET `birthday`= CURRENT_TIME WHERE `name` = '长江七号' AND `email` = '122345661@qq.com'
  ```

  

#### 3.5、删除

> delete 命令

语法：`delete from 表名 where [条件]`

```sql
-- 删除所有数据
DELETE FROM `student`


-- 删除指定数据
DELETE FROM `student` WHERE `id` = 1;
```

> truncate 命令

作用：完全清空一个数据库表，表的结构和索引约束不会改变

```sql
-- 清空表
TRUNCATE `student`
```

> delete 和 truncate 区别

* 相同点：都能删除数据，都不会删除表结
* 不同点：
  * truncate：重新设置自增列，计数器会归 0
  * truncate：不会影响事务

```sql
CREATE TABLE `test` (
    `id` INT(4) NOT NULL AUTO_INCREMENT,
    `coll` VARCHAR(30) NOT NULL,
    PRIMARY KEY(`id`)
)ENGINE=INNODB DEFAULT CHARSET=utf8


INSERT INTO `test`(`coll`) VALUES ('1'), ('2'), ('3')

-- 不会影响自增
DELETE FROM `test` 

-- 自增归零
TRUNCATE TABLE `test`
```

了解即可：`delete 删除的问题`，重启数据库，现象

* InnoDB：自增列会从 1 开始（存在内存中，断电即失）
* MyISAM：继续从上一个自增量开始（存在文件中的，不会消失）

### 4、DQL查询数据(重点)

#### 4.1、DQL

（Data Query Language：数据查询语言）

* 所有的查询操作都用它 select
* 简单的查询，复杂的查询它都能做
* **数据库最核心的语言，最重要的语言**
* 使用频率最高的语句

**Select 完整语法**

```sql
SELECT [ALL|DISTINCT] <目标列表达式> [AS 列名]
[,<目标列表达式> [AS 列名] ...] 
FROM <表名> [，<表名>…]
[WHERE <条件表达式> [AND|OR <条件表达式>...]
[GROUP BY 列名 
[HAVING <条件表达式>
[ORDER BY 列名 [ASC | DESC>
[LIMIT ...
/*
解释：[ALL|DISTINCT] ALL：全部； DISTINCT：不包括重复行
<目标列表达式> 对字段可使用AVG、COUNT、SUM、MIN、MAX、运算符等
<条件表达式>
查询条件 谓词
比较 =、>,<,>=,<=,!=,<>,
确定范围 BETWEEN AND、NOT BETWEEN AND
确定集合 IN、NOT IN
字符匹配 LIKE（“%”匹配任何长度，“_”匹配一个字符）、NOT LIKE
空值 IS NULL、IS NOT NULL
子查询 ANY、ALL、EXISTS
集合查询 UNION（并）、INTERSECT（交）、MINUS（差）
多重条件 AND、OR、NOT
<GROUP BY 列名> 对查询结果分组
[HAVING <条件表达式>] 分组筛选条件
[ORDER BY 列名 [ASC | DESC> 对查询结果排序；ASC：升序 DESC：降序
*/
```

**sql测试数据**

```sql
-- 创建数据库
CREATE DATABASE IF NOT EXISTS `school`;

-- 使用数据库
USE `school`;

DROP TABLE IF EXISTS `grade`;
-- 创建 grade 表
CREATE TABLE `grade` (
     `gradeid` INT(11) NOT NULL AUTO_INCREMENT COMMENT '年级编号',
     `gradename` VARCHAR(50) NOT NULL COMMENT '年级名称',
     PRIMARY KEY (`gradeid`)
) ENGINE=INNODB AUTO_INCREMENT=6 DEFAULT CHARSET=utf8;

-- 插入数据
INSERT INTO `grade` (`gradeid`, `gradename`) VALUES (1, '大一'), (2, '大二'), (3, '大三'), (4, '大四'), (5, '预科班');

DROP TABLE IF EXISTS `result`;
-- 创建 result 表
CREATE TABLE `result`(
    `studentno` INT(4) NOT NULL COMMENT '学号',
    `subjectno` INT(4) NOT NULL COMMENT '课程编号',
    `examdate` DATETIME NOT NULL COMMENT '考试日期',
    `studentresult` INT (4) NOT NULL COMMENT '考试成绩',
    KEY `subjectno` (`subjectno`)
)ENGINE = INNODB DEFAULT CHARSET = utf8;

-- 插入数据
INSERT INTO `result`(`studentno`,`subjectno`,`examdate`,`studentresult`)
VALUES
(1000,1,'2013-11-11 16:00:00',85),
(1000,2,'2013-11-12 16:00:00',70),
(1000,3,'2013-11-11 09:00:00',68),
(1000,4,'2013-11-13 16:00:00',98),
(1000,5,'2013-11-14 16:00:00',58);

DROP TABLE IF EXISTS `student`;
-- 创建 student 表
CREATE TABLE `student`(
	`studentno` INT(4) NOT NULL COMMENT '学号',
    `loginpwd` VARCHAR(20) DEFAULT NULL,
    `studentname` VARCHAR(20) DEFAULT NULL COMMENT '学生姓名',
    `sex` TINYINT(1) DEFAULT NULL COMMENT '性别，0或1',
    `gradeid` INT(11) DEFAULT NULL COMMENT '年级编号',
    `phone` VARCHAR(50) NOT NULL COMMENT '联系电话，允许为空',
    `address` VARCHAR(255) NOT NULL COMMENT '地址，允许为空',
    `borndate` DATETIME DEFAULT NULL COMMENT '出生时间',
    `email` VARCHAR (50) NOT NULL COMMENT '邮箱账号允许为空',
    `identitycard` VARCHAR(18) DEFAULT NULL COMMENT '身份证号',
    PRIMARY KEY (`studentno`),
    UNIQUE KEY `identitycard`(`identitycard`),
    KEY `email` (`email`)
)ENGINE=MYISAM DEFAULT CHARSET=utf8;

-- 插入数据
INSERT INTO `student` (`studentno`,`loginpwd`,`studentname`,`sex`,`gradeid`,`phone`,`address`,`borndate`,`email`,`identitycard`)
VALUES
(1000,'123456','张伟',0,2,'13800001234','北京朝阳','1980-1-1','text123@qq.com','123456198001011234'),
(1001,'123456','赵强',1,3,'13800002222','广东深圳','1990-1-1','text111@qq.com','123456199001011233');

DROP TABLE IF EXISTS `subject`;
-- 创建 subject 表
CREATE TABLE `subject`(
    `subjectno`INT(11) NOT NULL AUTO_INCREMENT COMMENT '课程编号',
    `subjectname` VARCHAR(50) DEFAULT NULL COMMENT '课程名称',
    `classhour` INT(4) DEFAULT NULL COMMENT '学时',
    `gradeid` INT(4) DEFAULT NULL COMMENT '年级编号',
    PRIMARY KEY (`subjectno`)
)ENGINE = INNODB AUTO_INCREMENT = 19 DEFAULT CHARSET = utf8;

-- 插入数据
INSERT INTO `subject`(`subjectno`,`subjectname`,`classhour`,`gradeid`)VALUES
(1,'高等数学-1',110,1),
(2,'高等数学-2',110,2),
(3,'高等数学-3',100,3),
(4,'高等数学-4',130,4),
(5,'C语言-1',110,1),
(6,'C语言-2',110,2),
(7,'C语言-3',100,3),
(8,'C语言-4',130,4),
(9,'Java程序设计-1',110,1),
(10,'Java程序设计-2',110,2),
(11,'Java程序设计-3',100,3),
(12,'Java程序设计-4',130,4),
(13,'数据库结构-1',110,1),
(14,'数据库结构-2',110,2),
(15,'数据库结构-3',100,3),
(16,'数据库结构-4',130,4),
(17,'C#基础',130,1);
```

#### 4.2、指定查询所有字段

```sql
-- 查询表中全部数据
SELECT * FROM `student`

-- 查询指定字段
SELECT `studentno`, `studentname` FROM `student`

-- 使用别名，给结果起一个名字 AS
-- 可以给字段起别名，也可以给表起别名
SELECT `studentno` AS 学号, `studentname` AS 学生姓名 FROM `student` AS s

-- 函数
-- 拼接字符串 Concat(a, b)
SELECT CONCAT('姓名: ', `studentname`) AS 新名字 FROM `student`
```

语法：`select 字段，... from 表`

> 有的时候，列名字不是那么的见名知意。我们可以起别名

语法：`select 字段1 as 别名1，... from 表 as 表别名`

> 去重：distinct

作用：去除 select 查询出来的结果中重复的数据，只显示一条

```sql
-- 查询有哪些同学参加了考试
SELECT * FROM `result`  -- 查询全部的成绩

SELECT `studentno` FROM `result` -- 查询有哪些同学参加了考试

SELECT DISTINCT `studentno` FROM `result` -- 发现重复数据，去重
```

> 数据库的列（表达式）

```sql
-- 查看系统的版本 （函数）
SELECT VERSION()
-- 用来计算（表达式）
SELECT 100*3-1 AS 计算结果
-- 查询自增的步长（变量）
SELECT @@auto_increment_increment

-- 学院考试成绩 + 1 分
SELECT `studentno`, `studentresult`+1 AS '提分后' FROM `result`
```

**数据库中的表达式：文本值，列，null，函数，计算表达式，系统变量...**

`select 表达式 from 表`

#### 4.3、where 条件子句

作用：检索数据中`符合条件`的值

搜索的条件由一个或者多个表达式构成！结果为布尔值

> 逻辑运算符

| 运算符  | 语法             | 描述                           |
| ------- | ---------------- | ------------------------------ |
| and &&  | a and b a && b   | 逻辑与，两个都为真，结果为真   |
| or \|\| | a or b  a \|\| b | 逻辑或，只要一个为真，结果为真 |
| not !   | !a               | 逻辑非，取反                   |

```sql
-- ======================= where ================================
-- 查询了所有的成绩
SELECT `studentno`, `studentresult` FROM `result`

-- 查询考试成绩是80 - 100 分之间的
SELECT `studentno`, `studentresult` FROM `result`
WHERE `studentresult`>=80 AND `studentresult`<=100

-- 模糊查询（区间）
SELECT `studentno`, `studentresult` FROM `result`
WHERE `studentresult` BETWEEN 80 AND 100

-- 查询考试成绩不是80 - 100 分之间的
SELECT studentno, studentresult FROM `result`
WHERE NOT studentresult BETWEEN 80 AND 100
```

> 模糊查询：比较运算符

| 运算符              | 语法                | 描述                                               |
| ------------------- | ------------------- | -------------------------------------------------- |
| is null             | a is  null          | 如果 a 为 null，结果为真                           |
| is not null         | a is  not null      | 如果 a 为 not null，结果为真                       |
| betwwen ... and ... | a between b and c   | 若 a 在 b 和 c 之间，结果为真                      |
| **Like**            | a like b            | SQL匹配，如果 a 匹配 b，结果为真                   |
| **in**              | a in (a1, a2, ....) | 假设 a1 是 (a1, a2, ....) 其中的某一个值，结果为真 |

```sql
-- ======================= 模糊查询 ================================
-- 查询姓赵的同学
-- like 集合 % (代表0到任意个字符） _(一个字符)
-- 查询姓赵的同学，名字后面有任意个字符的
SELECT `studentno`, `studentname` FROM `student` 
WHERE `studentname` LIKE '赵%'

-- 查询姓赵的同学，名字后面只有一个字的
SELECT `studentno`, `studentname` FROM `student` 
WHERE `studentname` LIKE '赵_'

-- 查询名字中带 伟 的同学
SELECT `studentno`, `studentname` FROM `student` 
WHERE `studentname` LIKE '%伟%'

-- ======== in(具体的一个或者多个值) ========
-- 查询 1001, 1002, 1003 号学生
SELECT `studentno`, `studentname` FROM `student` 
WHERE studentno IN (1001, 1002, 1003)

-- ========   null 和 not null==================
-- 查询地址为空的学生 null
SELECT `studentno`, `studentname` FROM `student` 
WHERE `address`='' OR `address`IS NULL

-- 查询有出生日期的同学 not null
SELECT `studentno`, `studentname` FROM `student` 
WHERE `borndate` IS NOT NULL
```

#### 4.4、联表查询

> JOIN

```sql
-- =================  联表查询 join    ====================

-- 查询参加了考试的同学（学号，姓名，科目编号，分数）
/*
思路：
1. 分析需求，分析查询的字段来自哪些表
2. 确定使用哪种连接查询？
确定交叉点（这两个表中哪些数据时相同的）
*/
SELECT s.studentno, studentname, subjectno, studentresult
FROM `student` AS s
INNER JOIN `result` AS r
ON s.studentno = r.studentno

-- Right join
SELECT s.studentno, studentname, subjectno, studentresult
FROM `student` AS s
RIGHT JOIN `result` AS r
ON s.studentno = r.studentno


-- Left join
SELECT s.studentno, studentname, subjectno, studentresult
FROM `student` AS s
LEFT JOIN `result` AS r
ON s.studentno = r.studentno
```

| 操作       | 描述                                         |
| ---------- | -------------------------------------------- |
| inner join | 如果表中至少有一个匹配，就返回行             |
| left join  | 即使右表中没有匹配，也会从左表中返回所有的值 |
| right join | 即使左表中没有匹配，也会从右表中返回所有的值 |

```sql
-- =================  联表查询 join    ====================

-- 查询参加了考试的同学（学号，姓名，科目编号，分数）
/*
思路：
1. 分析需求，分析查询的字段来自哪些表
2. 确定使用哪种连接查询？
确定交叉点（这两个表中哪些数据时相同的）
*/

-- join （连接的表）on （条件） 连接查询
-- where 等值查询

SELECT s.studentno, studentname, subjectno, studentresult
FROM `student` AS s
INNER JOIN `result` AS r
ON s.studentno = r.studentno

-- Right join
SELECT s.studentno, studentname, subjectno, studentresult
FROM `student` AS s
RIGHT JOIN `result` AS r
ON s.studentno = r.studentno


-- Left join
SELECT s.studentno, studentname, subjectno, studentresult
FROM `student` AS s
LEFT JOIN `result` AS r
ON s.studentno = r.studentno


-- 思考题（查询了参加考试的同学信息：学号，姓名，科目名，分数）
SELECT s.studentno, studentname, sub.subjectname, studentresult 
FROM `student` AS s
RIGHT JOIN `result` AS r
ON s.studentno = r.studentno	
INNER JOIN `subject` AS sub
ON r.subjectno = sub.subjectno

-- 查询哪些数据 select ...
-- 从哪几张表中查 from 表 xxx join 连接的表 on 交叉条件
-- 假设存在一种多表查询，慢慢来，先查询两张表然后再慢慢增加
-- From a left join b（以左表为基准）
-- From a right join b（以右表为基准）
```

> 自连接（了解）

自己的表和自己的表连接，核心：**一张表拆为两张一样的表即可**

父类

| categoryid | categoryname |
| ---------- | ------------ |
| 2          | 信息技术     |
| 3          | 软件开发     |
| 5          | 美术设计     |

子类

| pid  | categoryid | categoryname |
| ---- | ---------- | ------------ |
| 3    | 4          | 数据库       |
| 2    | 8          | 办公信息     |
| 3    | 6          | web 开发     |
| 5    | 7          | ps 技术      |

操作：查询父类对应的子类关系

| 父类     | 子类     |
| -------- | -------- |
| 信息技术 | 办公信息 |
| 软件开发 | 数据库   |
| 软件开发 | web 开发 |
| 美术设计 | ps 技术  |

```sql
CREATE TABLE `category`( 
    `categoryid` INT(3) NOT NULL COMMENT 'id', 
    `pid` INT(3) NOT NULL COMMENT '父id 没有父则为1', 
    `categoryname` VARCHAR(10) NOT NULL COMMENT '种类名字', 
    PRIMARY KEY (`categoryid`) 
) ENGINE=INNODB AUTO_INCREMENT=9 CHARSET=utf8;

INSERT INTO `category` (`categoryid`, `pid`, `categoryname`) VALUES 
('2', '1', '信息技术'),
('3', '1', '软件开发'),
('5', '1', '美术设计'),
('4', '3', '数据库'),
('8', '2', '办公信息'),
('6', '3', 'web开发'),
('7', '5', 'ps技术');


-- 查询父子信息
SELECT a.categoryname AS '父栏目', b.categoryname AS '子栏目'
FROM `category` AS a, `category` AS b
WHERE a.categoryid = b.pid

-- 查询学员所属的年级（学号，学生的姓名，年级名称）
SELECT studentno, studentname, gradename
FROM `student` AS s
INNER JOIN `grade` AS g
ON s.gradeid = g.gradeid


-- 查询科目所属的年级（科目名称，年级名称）
SELECT subjectname, gradename
FROM `subject` AS sub
INNER JOIN `grade` AS g
ON sub.gradeid = g.gradeid

-- 查询了参加 数据库结构-1 考试的同学信息：学号，姓名，科目名，分数
SELECT s.studentno, studentname, subjectname, studentresult
FROM `student` AS s
INNER JOIN `result` AS r
ON s.studentno = r.studentno
INNER JOIN `subject` AS sub
ON r.subjectno = sub.subjectno
WHERE sub.subjectname = '数据库结构-1'
```

#### 4.5、分页和排序

> 排序

```sql
-- order by
-- order by 通过哪个字段排序
-- 排序：升序 ASC，降序 DESC
SELECT s.studentno, studentname, subjectname, studentresult
FROM `student` AS s
INNER JOIN `result` AS r
ON s.studentno = r.studentno
INNER JOIN `subject` AS sub
ON r.subjectno = sub.subjectno
WHERE sub.subjectname = '数据库结构-1'
ORDER BY studentresult ASC
```

> 分页

```sql
-- 为什么要分页？
-- 缓解数据库压力，给人的体验更好，瀑布流

-- 分页，每页只显示 1 条数据
-- 语法：limit 起始值，页的大小
SELECT s.studentno, studentname, subjectname, studentresult
FROM `student` AS s
INNER JOIN `result` AS r
ON s.studentno = r.studentno
INNER JOIN `subject` AS sub
ON r.subjectno = sub.subjectno
WHERE sub.subjectname = '数据库结构-1'
ORDER BY studentresult ASC
LIMIT 0, 5


-- 第一页 limit 0, 5
-- 第二页 limit 5, 5
-- 第三页 limit 10, 5
-- 第四页 limit 15, 5
-- 第五页 limit 20, 5
-- 第n页 limit (n-1)*pagesize, pagesize
-- pagesize:页面大小
-- (n-1)*pagesize:起始值 
-- n 为当前页
-- 数据总数/页面大小 = 总页数
```

语法：`limit (查询起始下标，pagesize)`

#### 4.6、子查询

where (这个值时计算出来的)

本质：`在 where 语句中嵌套一个子查询语句`

```sql
-- =================== where =========================
-- 1. 查询数据库结构-1的考试结果（学号，科目编号，成绩），降序排列
-- 方式一: 使用连接查询
SELECT studentno, r.subjectno, studentresult
FROM `result` AS r
INNER JOIN `subject` AS sub
ON r.subjectno = sub.subjectno
WHERE subjectname = '数据库结构-1'
ORDER BY studentresult DESC

-- 方式二：使用子查询（由里及外）
SELECT studentno, subjectno, studentresult
FROM `result`
WHERE subjectno = (
    SELECT subjectno
    FROM `subject`
    WHERE subjectname = '数据库结构-1'
)
ORDER BY studentresult DESC

-- 分数不小于80分的学生的学号和姓名
SELECT DISTINCT s.studentno, studentname
FROM `student` AS s
INNER JOIN `result` AS r 
ON r.studentno = s.studentno
WHERE studentresult >= 80


-- 数据库结构-1的分数不小于80分的学生的学号和姓名
-- 子查询
SELECT DISTINCT s.studentno, studentname
FROM `student` AS s
INNER JOIN `result` AS r 
ON r.studentno = s.studentno
WHERE studentresult >= 80 AND subjectno=(
    SELECT subjectno FROM `subject` 
    WHERE subjectname = '数据库结构-1'
)

-- 联表查询
SELECT DISTINCT s.studentno, studentname
FROM `student` AS s
INNER JOIN `result` AS r
ON r.studentno = s.studentno
INNER JOIN `subject` AS sub
ON r.subjectno = sub.subjectno
WHERE sub.subjectname = '数据库结构-1' AND studentresult >= 80


-- 嵌套查询
SELECT studentno, studentname FROM `student` WHERE studentno IN (
    SELECT studentno FROM `result` WHERE studentresult >= 80 AND subjectno = (
         SELECT subjectno FROM `subject` WHERE subjectname = '数据库结构-1'    
    )
)

-- 练习：查询 C语言-1 前 5 名同学的成绩的信息（学号，姓名，分数）
SELECT s.studentno, studentname, studentresult
FROM `student` AS s
INNER JOIN `result` AS r
ON s.studentno = r.studentno
WHERE r.subjectno = (
    SELECT subjectno FROM `subject`
    WHERE subjectname = 'C语言-1'
)
ORDER BY studentresult DESC
LIMIT 0, 5
```

#### 4.7、分组和过滤

```sql
-- 查询不同课程的平均分，最高分，最低分
-- 核心：根据不同的课程分组
SELECT subjectname, AVG(studentresult), MAX(studentresult), MIN(studentresult)
FROM `result` AS r
INNER JOIN `subject` AS sub
ON r.subjectno = sub.subjectno
GROUP BY r.subjectno  -- 通过什么字段分组
HAVING AVG(studentresult) >= 80
```

#### 4.8、select 小结

```sql
select distinct 要查询的字段 from 表（注意：表和字段可以取别名）
xxx join 要连接的表 on 等值判断
where (具体的值 或者是 子查询语句)
group by (通过哪个字段来分组)
having (过滤分组后的信息，条件和 where 一致)
order by (通过哪个字段排序) asc or desc
limit startindex, pagesize
```

### 5、MySQL函数

#### 5.1、常用函数

```sql
-- ============== 常用函数 ================================

-- 数学运算
SELECT ABS(-5)  -- 绝对值
SELECT CEILING(9.1)  -- 向上取整
SELECT FLOOR(9.2) -- 向下取整
SELECT RAND() -- 返回一个(0, 1)之间的随机数
SELECT SIGN(-5) -- 判断一个数的符号 0-0  符数返回 -1 整数返回 1

-- 字符串函数
SELECT CHAR_LENGTH('即使再小的帆也能远航') -- 字符串长度
SELECT CONCAT('我', '爱', '学习')  -- 字符串拼接
SELECT INSERT('我爱编程helloworld', 1, 2, '超级热爱') -- 字符串替换，从某个位置开始替换某个长度的字符串
SELECT LOWER('Woshinidage') -- 转换成小写
SELECT UPPER('Woshinidage') -- 转换成大写
SELECT INSTR('我时一个爱学习的孩子', '爱学习') -- 寻找子字符串在主子符串中第一次出现的位置
SELECT REPLACE('坚持就能成功', '坚持', '努力') -- 替换出现的指定字符串
SELECT SUBSTR('龙龙说坚持就能成功', 4, 5) -- 返回指定的字符串(原字符串，截取的位置，截取的长度)
SELECT REVERSE('坚持就能成功') -- 反转字符串


-- 查询姓赵的同学改为姓刘
SELECT REPLACE(studentname, '赵', '刘') FROM `student`
WHERE studentname LIKE '赵%'

-- 时间和日期函数（基础）
SELECT CURRENT_DATE()  -- 获取当前日期
SELECT CURDATE()  -- 获取当前日期
SELECT NOW() -- 获取当前时间
SELECT LOCALTIME() -- 获取本地时间
SELECT SYSDATE() -- 系统时间
SELECT YEAR(NOW()) -- 年
SELECT MONTH(NOW()) -- 月
SELECT DAY(NOW()) -- 日
SELECT HOUR(NOW()) -- 时
SELECT MINUTE(NOW()) -- 分
SELECT SECOND(NOW()) -- 秒

-- 系统
SELECT USER() -- 系统用户
SELECT SYSTEM_USER() -- 系统用户
SELECT VERSION() -- 数据库版本
```

#### 5.2、聚合函数（常用）

| 函数名称 | 描述   |
| -------- | ------ |
| COUNT()  | 计数   |
| SUM()    | 求和   |
| AVG()    | 平均值 |
| MAX()    | 最大值 |
| MIN()    | 最小值 |
| ...      | ...    |

```sql
-- ============= 聚合函数 ====================

-- count()
-- 都能统计表中的数据（想查询一个表中有多少个数据，就是用这个count()）
SELECT COUNT(studentname) FROM `student`  -- count(指定列)，会忽略所有的 null 值
SELECT COUNT(*) FROM `student` -- count(*)，不会忽略 null 值, 本质 计算行数
SELECT COUNT(1) FROM `student` -- count(1)，不会忽略 null 值，本质 计算行数

-- 求和
SELECT SUM(studentresult) FROM `result`
-- 平均分
SELECT AVG(studentresult) FROM `result`
-- 最高分
SELECT MAX(studentresult) FROM `result`
-- 最低分
SELECT MIN(studentresult) FROM `result`
```

#### 5.3、数据库级别的MD5加密（扩展）

什么是 MD5 ?

主要增强算法复杂度和不可逆性。

MD5 不可逆，具体的值的 MD5 值是一样的

MD5 破解网站的原理，背后有一个字典，MD5 加密后的值，加密前的值

```sql
-- ============= 测试 MD5 加密 ================
CREATE TABLE `testmd5`(
    `id` INT(4) NOT NULL,
    `name` VARCHAR(20) NOT NULL, 
    `pwd` VARCHAR(50) NOT NULL,
    PRIMARY KEY(`id`)
) ENGINE=INNODB DEFAULT CHARSET=utf8

-- 明文密码
INSERT INTO `testmd5` VALUES (1, '张三', '123456'), (2, '李四', '123456'), (3, '王五', '123456');

-- 加密
UPDATE `testmd5` SET pwd=MD5(pwd) WHERE id=1 -- 加密指定的密码

UPDATE `testmd5` SET pwd=MD5(pwd) -- 加密全部的密码

-- 插入的时候加密
INSERT INTO `testmd5` VALUES (4, 'xiaoming', MD5('123456'))

-- 如何校验：将用户传递进来的密码，进行md5加密，然后对比加密后的值
SELECT * FROM `testmd5` WHERE `name` = 'xiaoming' AND `pwd`=MD5('123456')
```

### 6、事务

#### 6.1、什么是事务

**要么都成功，要么都失败**

——————————

1、SQL执行 A 给 B 转账 A 1000  --> 200  B 200

2、SQL执行 B 收到 A 转账 A 800 -> B 400

———————————

将一组SQL放在一个批次中去执行

> 事务原则：ACID 原则 原子性，一致性，隔离性，持久性（脏读，不可重复读，幻读）

1. 原子性

   一个事务要么全部提交成功，要么全部失败

2. 一致性

   事务前后的数据完整性要保持一致

3. 隔离性

   一个事务的执行不会能被其他事务干扰，即一个事务内部的操作以及使用的数据对并发的其他事务是隔离的，并发执行的各个事务之间不能互相干扰

4. 持久性

   事务一旦提交则不可逆，被持久化到数据库中

> 隔离所导致的问题

**脏读：**

是指一个事务读取了另一个事务未提交的数据

**不可重复读：**

在一个事务内读取表中的某一行数据，多次读取结果不同。（这个不一定是错误，只是某些场合不对）

**虚度（幻读）：**

是指在一个事务内读取到了别的事物插入的数据，导致前后读取不一致

```sql

-- ===================== 事务 ===========================
-- MySQL 是默认开启事务自动提交的

SET autocommit = 0 -- 关闭
SET autocommit = 1 -- 开启（默认的）

-- 手动处理事务
SET autocommit = 0 -- 关闭自动提交

-- 事务开启
START TRANSACTION -- 标记一个事务的开始，从这个之后的 sql 都在同一个事务内

INSERT xxx
INSERT xxx

-- 提交：持久化（成功）

-- 回滚：回到原来的样子（失败，rollback）


-- 事务结束
SET autocommit = 1 -- 开启自动提交

-- 了解
SAVEPOINT 保存点名 -- 设置一个事务的保存点
ROLLBACK TO SAVEPOINT 保存点名-- 回滚到保存点
RELEASE SAVEPOINT 保存点名-- 撤销指定的保存点
```

> 模拟场景-转账

```sql
-- 转账
CREATE DATABASE shop CHARACTER SET utf8 COLLATE utf8_general_ci

USE shop

CREATE TABLE `account`(
    `id` INT(3) NOT NULL AUTO_INCREMENT,
    `name` VARCHAR(30) NOT NULL,
    `money` DECIMAL(9, 2) NOT NULL,
    PRIMARY KEY(`id`)
) ENGINE=INNODB DEFAULT CHARSET=utf8

INSERT INTO `account` (`name`, `money`) VALUES
('A', 2000.00), ('B', 10000.00)

-- 转账模拟，事务
SET autocommit = 0 -- 关闭自动提交
START TRANSACTION -- 开启一个事务（一组事务）

UPDATE account SET money = money - 500 WHERE `name` = 'A' -- A 减500
UPDATE account SET money = money + 500 WHERE `name` = 'B' -- B 加500

COMMIT -- 提交事务
ROLLBACK -- 回滚

SET autocommit = 1 -- 恢复默认值
```

### 7、索引

> MySQL官方对索引的定义为：**索引（index）是帮助MySQL高效获取数据的数据结构。**
>
> 提取句子主干，就可以得到索引的本质：索引是数据结构。

#### 7.1、索引的分类

> 在一个表中，主键索引只能有一个，唯一索引可以有多个

* 主键索引（PRIMARY KEY）
  * 唯一的标识，主键不可重复，只能有一个列作为主键
* 唯一索引（UNIQUE KEY）
  * 避免重复的列出现，唯一索引可以重复，多个列都可以表示为唯一索引
* 常规索引（KEY / INDEX）
  * 默认的，index / key 关键字来设置
* 全文索引（FullText）
  * 在特定的数据库引擎下才有，MyISAM
  * 快速定位数据

基础语法

```sql
-- ===================== 索引的使用================================
-- 1. 在创建表的时候给字段增加索引
-- 2. 创建完毕后，添加索引

-- 显示所有的索引信息
SHOW INDEX FROM `student`

-- 增加一个索引  索引名（列名）
ALTER TABLE school.student ADD FULLTEXT INDEX `studentname`(`studentname`)


-- EXPLAIN 分析 sql 执行的状况
EXPLAIN SELECT * FROM `student`; -- 非全文索引


EXPLAIN SELECT * FROM `student` WHERE MATCH(`studentname`) AGAINST ('赵')
```

#### 7.2、测试索引

```sql
CREATE TABLE `app_user` (
    `id` BIGINT(20) UNSIGNED NOT NULL AUTO_INCREMENT,
    `name` VARCHAR(50) DEFAULT '',
    `eamil` VARCHAR(50) NOT NULL,
    `phone` VARCHAR(20) DEFAULT '',
    `gender` TINYINT(4) UNSIGNED DEFAULT '0',
    `password` VARCHAR(100) NOT NULL DEFAULT '',
    `age` TINYINT(4) DEFAULT NULL,
    `create_time` DATETIME DEFAULT CURRENT_TIMESTAMP,
    `update_time` TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
PRIMARY KEY (`id`)
) ENGINE=INNODB DEFAULT CHARSET=utf8


-- 插入 100 万数据
DELIMITER $$ -- 写函数之前必须要写，标志
CREATE FUNCTION mock_data()
RETURNS INT 
BEGIN
    DECLARE num INT DEFAULT 1000000;
    DECLARE i INT DEFAULT 0;
    WHILE i < num DO
        -- 插入语句
        INSERT INTO `app_user`(`name`, `eamil`, `phone`, `gender`, `password`, `age`)
	VALUES (
	    CONCAT('用户-', i), 
	    '123456789@qq.com', 
	    CONCAT('18', FLOOR(RAND()*((999999999-100000000)+100000000))),
	    FLOOR(RAND()*2),
	    UUID(),
	    FLOOR(RAND()*100)
	);    
        SET i = i + 1;
    END WHILE;
    RETURN i;
END;

SELECT mock_data();

-- 查询指定数据
SELECT * FROM `app_user` WHERE `name`='用户-9999' -- 0.547 sec
SELECT * FROM `app_user` WHERE `name`='用户-9999' -- 0.552 sec

-- 分析
EXPLAIN SELECT * FROM `app_user` WHERE `name`='用户-9999'


-- 创建索引
-- CREATE INDEX 索引名 ON 表(字段)
CREATE INDEX id_app_user_name ON app_user(`name`)

-- 分析
EXPLAIN SELECT * FROM `app_user` WHERE `name`='用户-9999' -- 0.001 sec
```

**索引在小数据量的时候，用处不大，但是在大数据的时候，区别十分明显**

#### 7.3、索引原则

* 索引不是越多越好
* 不要对经常变动的数据添加索引
* 小数据量的表不需要添加索引
* 索引一般加在常用来查询的字段上

> 索引的数据结构

Hash 类型的索引

Btree：InnoDB 的默认数据结构

阅读：http://blog.codinglabs.org/articles/theory-of-mysql-index.html

### 8、权限管理和备份

#### 8.1、用户管理

> SQL yog 可视化管理

> SQL 命令操作

用户表：mysql.user

本质：对这张表进行增删改查

```sql
-- 创建用户  CREATE USER 用户名 IDENTIFIED BY '密码'
CREATE USER zxl IDENTIFIED BY '123456'

-- 修改密码（修改当前用户密码）
SET PASSWORD = PASSWORD('123456')

-- 修改密码（修改指定用户密码）
SET PASSWORD FOR zxl = PASSWORD('123456')

-- 用户重命名 RENAME USER 原名字 TO 新名字
RENAME USER zxl TO zxlyzdz

-- 用户授权 GRANT ALL PRIVILEGES 全部权限 on 库.表 to 用户名
-- ALL PRIVILEGES 除了给别人授权，其他都能干
GRANT ALL PRIVILEGES ON *.* TO zxlyzdz

-- 查看权限
SHOW GRANTS FOR zxlyzdz -- 查看指定用户的权限
SHOW GRANTS FOR root@localhost

-- root 用户权限
-- GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost' WITH GRANT OPTION

-- 撤销权限 REVOKE 哪些权限，在哪个库撤销，给谁撤销
REVOKE ALL PRIVILEGES ON *.* FROM zxlyzdz

-- 删除用户
DROP USER zxl
```

#### 8.2、数据库备份

> 为什么要备份？

* 保证重要的数据不丢失
* 数据转移 A --> B

> MySQL数据库备份的方式

* 直接拷贝物理文件
* 在SQLyog这种可视化工具中手动导出
* 使用命令行到处 mysqldump 命令行使用

```bash
# mysqldump -h 主机 -u 用户名 -p 密码 数据库 表名 > 物理磁盘位置/文件名
mysqldump -hlocalhost -uroot -p123456 school student >D:/a.sql

# mysqldump -h 主机 -u 用户名 -p 密码 数据库> 物理磁盘位置/文件名
mysqldump -hlocalhost -uroot -p123456 school >D:/b.sql

# mysqldump -h 主机 -u 用户名 -p 密码 数据库 表名1 表名2 表名3 > 物理磁盘位置/文件名
mysqldump -hlocalhost -uroot -p123456 school student grade >D:/a.sql

# 导入
# 登录用户的情况下，切换到指定的数据库
# source 备份文件
source d:/a.sql

```

* 假设你要备份数据库，防止数据库丢失。
* 把数据库数据分享出去，sql 发送给别人即可!

### 9、规范数据库设计

#### 9.1、为什么要数据库设计

**当数据库比较复杂的时候，我们就需要设计了**

**糟糕的数据库设计：**

* 数据冗余，浪费空间
* 数据库插入和删除都会麻烦、异常【屏蔽使用物理外键】
* 程序的性能差

**良好的数据库设计：**

* 节省内存空间
* 保证数据的完整性
* 方便我们开发系统

**软件开发中，关于数据库的设计**

* 分析需求：分析业务和需要处理的数据库的需求
* 概要设计：设计关系图 E-R 图

**设计数据库的步骤：（个人博客）**

* 收集信息，分析需求
  * 用户表（用户登陆注销，用户的个人信息，写博客，创建分类）
  * 分类表（文章分类，谁创建的）
  * 文章表（文章的信息）
  * 友链表（友链信息）
  * 评论表（评论）
  * 自定义表（系统信息，某个关键的字，或者一些主字段）
  * 说说表（发表心情，id...content）

* 标识实体（把需求落地到每个字段）
* 标识实体之间的关系
  * 写博客：user -> blog
  * 创建分类：user -> categor
  * 关注：user -> user
  * 友链：links
  * 评论：user -> user -> blog

#### 9.2、三大范式

**为什么需要数据规范化？**

* 信息重复
* 更新异常
* 插入异常
  * 无法正常显示信息
* 删除异常
  * 丢失有效的信息

> 三大范式（了解）

**第一范式**

要求数据库的每一列都是不可分割的原子数据项

原子性：保证每一列不可再分

**第二范式**

前提：满足第一范式

每张表只描述一件事情

**第三范式**

前提：满足第一范式和第二范式

第三范式需要确保数据表中的每一列数据都和主键直接相关，而不能间接相关

**目的：规范数据库的设计**

**规范性和性能的问题**

关联查询的表不得超过三张表

* 考虑商业化的需求和目标（成本，用户体验），数据库的性能更加重要
* 在规范性能的问题的时候，需要适当的考虑一下规范性
* 有时候要故意给某些表增加一些冗余的字段（从多表查询变为单表查询）
* 有时候故意增加一个计算列（从大数据量降低为小数据量的查询，索引）

### 10、JDBC（重点）

#### 10.1、数据库驱动

驱动：声卡，显卡，数据库

![image-20210329161744080](https://www.zxlyzdz.com/images/mysql/jdbc_1.png)

我们的程序会通过数据库驱动，和数据库进行交互

#### 10.2、JDBC

SUN 公司为了简化开发人员的（对数据库的统一）操作，提供了一个（Java操作数据库的）规范，俗称 JDBC

这些规范的实现由具体的厂商去做

对于开发人员来说，我们只需要掌握 JDBC 接口的操作即可

![image-20210329161656149](https://www.zxlyzdz.com/images/mysql/jdbc_2.png)

java.sql

javax.sql

还需要导入一个数据库驱动包 mysql-connector-java-5.1.37-bin

#### 10.3、第一个jdbc程序

> 创建测试数据库

```sql
CREATE DATABASE jdbcStudy CHARACTER SET utf8 COLLATE utf8_general_ci;

USE jdbcStudy;

CREATE TABLE `users`(
	id INT PRIMARY KEY,
	NAME VARCHAR(40),
	PASSWORD VARCHAR(40),
	email VARCHAR(60),
	birthday DATE
);

INSERT INTO `users`(id,NAME,PASSWORD,email,birthday)
VALUES(1,'zhansan','123456','zs@sina.com','1980-12-04'),
(2,'lisi','123456','lisi@sina.com','1981-12-04'),
(3,'wangwu','123456','wangwu@sina.com','1979-12-04')
```

1、创建一个普通项目

2、导入数据库驱动

3、编写测试代码

```java
package com.zxl.demo01;

import java.sql.*;

// 第一个jdbc程序
public class JDBCFirstDemo {
    public static void main(String[] args) throws ClassNotFoundException, SQLException {
        // 1. 加载驱动
        Class.forName("com.mysql.jdbc.Driver"); // 固定写法
        // 2. 用户信息和 url
        String url = "jdbc:mysql://localhost:3306/jdbcstudy?useUnicode=true&characterEncoding=utf8&useSSL=false";
        String username = "root";
        String password = "123456";
        // 3. 连接成功，返回数据库对象 connection 数据库对象
        Connection connection = DriverManager.getConnection(url, username, password);
        // 4. 执行SQL的对象 statement 执行sql
        Statement statement = connection.createStatement();
        // 5. 执行SQL的对象 去 执行SQL
        String sql = "select * from users";
        // 返回的结果集，结果集中封装了全部的查询结果
        ResultSet resultSet = statement.executeQuery(sql);
        while(resultSet.next()) {
            System.out.print("id="+resultSet.getObject("id"));
            System.out.print(" name="+resultSet.getObject("NAME"));
            System.out.print(" password="+resultSet.getObject("PASSWORD"));
            System.out.print(" email="+resultSet.getObject("email"));
            System.out.print(" birth="+resultSet.getObject("birthday"));
            System.out.println();
        }
        // 6. 释放连接
        resultSet.close();
        statement.close();
        connection.close();


    }
}

```

步骤总结：

1. 加载驱动
2. 用户信息和 url
3. 连接成功，获取数据库对象 DiverManager.getConnection()
4. 获取执行 SQL 的对象 Statement
5. 执行SQL的对象 去执行 SQL 语句，获得结果集 RusultSet
6. 依次释放连接

> DriverManager

```java
// DriverManager.registerDriver(new com.mysql.jdbc.Driver());
Class.forName("com.mysql.jdbc.Driver"); // 固定写法

Connection connection = DriverManager.getConnection(url, username, password);
// connection 代表数据库
// 数据库设置自动提交
// 事务提交
// 事务回滚
connection.rollback(); // 回滚
connection.commit(); // 提交
connection.setAutoCommit(false); // 设置不自动提交
```

> URL

```java
String url = "jdbc:mysql://localhost:3306/jdbcstudy?useUnicode=true&characterEncoding=utf8&useSSL=false";


// mysql 端口 3306
// jdgc:mysql：//主机地址:端口号/数据库名?参数1&参数2&参数3

// oracle 端口 1521
// jdbc:oracle:thin:@localhost:1521:sid
```

> Statement 执行SQL的对象 PreparedStatement

```java
// 4. 获取执行SQL的对象 statement 执行sql
Statement statement = connection.createStatement();

// 编写SQL语句
String sql = "select * from users";

statement.executeQuery(sql); // 查询操作 返回 ResultSet 结果集
statement.execute(); // 执行任何SQL
statement.executeUpdate();  // 更新，插入，删除 返回一个受影响的行数
```

> ResultSet 查询的结果集：封装了所有的查询结果

```java
// 在不知道数据类型的时候使用
resultSet.getObject();
// 如果知道列的类型就是用指定的类型
resultSet.getString();
resultSet.getInt();
resultSet.getFloat();
resultSet.getDouble();
resultSet.getDate();
```

遍历：指针

```java
resultSet.beforeFirst(); // 移动到最前面
resultSet.afterLast(); // 移动到最后面
resultSet.next(); // 移动到下一行
resultSet.previous(); // 移动到前一行
resultSet.absolute(row); // 移动到指定行
```

> 释放资源

```java
// 6. 释放连接
resultSet.close();
statement.close();
connection.close(); // 十分消耗资源
```

#### 10.4、statement对象

jdbc 中的 `statement`对象用于向数据库发送SQL语句，想完成对数据库的增删改查，只需要通过这个对象向数据库发送增删改查语句即可。

Statement 对象的 `executeUpdate()` 方法，用于向数据库发送增、删、改的 sql 语句，`executeUpdate() `执行完之后，将会返回一个整数（即增删改语句导致了数据库几行数据发生了变化）

`Statement.executeQuery()` 方法用于向数据库发送查询语句，`executeQuery()` 执行完之后会返回一个代表查询结果的 ResultSet 对象。

> CURD操作-create

使用`executeUpdate(String sql)`方法完成数据添加操作，实例操作：

```java
Statement statement = connection.createStatement();
String sql = "insert into user(...) values(...)";
int num = statement.executeUpdate(sql);
if (num > 0) {
    System.out.println("插入成功！！");
}
```

> CURD操作-删除

使用`executeUpdate(String sql)`方法完成数据删除操作，实例操作：

```java
Statement statement = connection.createStatement();
String sql = "delete from user where id = 1";
int num = statement.executeUpdate(sql);
if (num > 0) {
    System.out.println("删除成功！！");
}
```

> CURD操作-修改

使用`executeUpdate(String sql)`方法完成数据修改操作，实例操作：

```java
Statement statement = connection.createStatement();
String sql = "update user set name='' where id = 1";
int num = statement.executeUpdate(sql);
if (num > 0) {
    System.out.println("修改成功！！");
}
```

> CURD操作-查询

使用`executeQuery(String sql)`方法完成数据修改操作，实例操作：

```java
Statement statement = connection.createStatement();
String sql = "select * from user where id = 1";
ResultSet resultSet = statement.executeQuery(sql);
while(resultSet.next()) {
    // 根据获取列的数据类型，分别调用 resultSet 的相应方法映射到java对象中
}
```

> 代码实现

1、提取工具类

```java
package com.zxl.demo02.utils;

import java.io.IOException;
import java.io.InputStream;
import java.sql.*;
import java.util.Properties;

public class JdbcUtils {
    private static String driver = null;
    private static String url = null;
    private static String username = null;
    private static String password = null;
    static {
        try {
            InputStream in = JdbcUtils.class.getClassLoader().getResourceAsStream("jdbc.properties");
            Properties properties = new Properties();
            properties.load(in);

            driver = properties.getProperty("driver");
            url = properties.getProperty("url");
            username = properties.getProperty("username");
            password = properties.getProperty("password");

            // 1. 驱动只用加载一次
            Class.forName(driver);


        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    // 获取连接
    public static Connection getConnection() throws SQLException {
        Connection connection = DriverManager.getConnection(url, username, password);
        return connection;
    }


    // 释放连接资源
    public static void release(Connection connection, Statement statement, ResultSet resultSet) {
        if (resultSet != null) {
            try {
                resultSet.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if (statement != null) {
            try {
                statement.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if (connection != null) {
            try {
                connection.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }

}

```

2、编写增删改的方法，`executeUpdate()`

新增

```java
package com.zxl.demo02;

import com.zxl.demo02.utils.JdbcUtils;

import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

public class TestInsert {
    public static void main(String[] args) {
        Connection connection = null;
        Statement statement = null;
        ResultSet resultSet = null;

        try {
            connection = JdbcUtils.getConnection(); // 获取数据库连接
            statement = connection.createStatement();
            String sql = "INSERT INTO users(`id`, `NAME`, `PASSWORD`, `email`, `birthday`)" +
                    "VALUES (4, 'zxl', '123456', '123456@qq.com', '2020-01-01')";
            int num = statement.executeUpdate(sql);
            if (num > 0) {
                System.out.println("插入成功");
            }
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            JdbcUtils.release(connection, statement, resultSet);
        }
    }
}
```

删除

```java
package com.zxl.demo02;

import com.zxl.demo02.utils.JdbcUtils;

import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

public class TestDelete {
    public static void main(String[] args) {
        Connection connection = null;
        Statement statement = null;
        ResultSet resultSet = null;

        try {
            connection = JdbcUtils.getConnection(); // 获取数据库连接
            statement = connection.createStatement();
            String sql = "DELETE FROM users WHERE id = 4";
            int num = statement.executeUpdate(sql);
            if (num > 0) {
                System.out.println("删除成功");
            }
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            JdbcUtils.release(connection, statement, resultSet);
        }
    }
}
```

修改

```java
package com.zxl.demo02;

import com.zxl.demo02.utils.JdbcUtils;

import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

public class TestUpdate {
    public static void main(String[] args) {
        Connection connection = null;
        Statement statement = null;
        ResultSet resultSet = null;

        try {
            connection = JdbcUtils.getConnection(); // 获取数据库连接
            statement = connection.createStatement();
            String sql = "UPDATE users SET NAME = 'zxlyzdz' WHERE id = 4";
            int num = statement.executeUpdate(sql);
            if (num > 0) {
                System.out.println("更新成功");
            }
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            JdbcUtils.release(connection, statement, resultSet);
        }
    }
}
```

3、查询 `executeQuery()`

```java
package com.zxl.demo02;

import com.zxl.demo02.utils.JdbcUtils;

import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

public class TestQuery {
    public static void main(String[] args) {
        // 创建对象
        Connection connection = null;
        Statement statement = null;
        ResultSet resultSet = null;

        // 获取链接
        try {
            connection = JdbcUtils.getConnection();
            statement = connection.createStatement();
            String sql = "select * from users where id = 1";
            resultSet = statement.executeQuery(sql);
            while (resultSet.next()) {
                System.out.println("id="+resultSet.getObject("id"));
                System.out.println("name="+resultSet.getObject("NAME"));
                System.out.println("password="+resultSet.getObject("PASSWORD"));
                System.out.println("email="+resultSet.getObject("email"));
                System.out.println("birth="+resultSet.getObject("birthday"));
            }
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            JdbcUtils.release(connection, statement, resultSet);
        }
    }
}
```

> SQL 注入的问题

SQL 存在漏洞，会被攻击导致数据泄露，`SQL 会被拼接`

SQL注入攻击是通过操作输入来修改SQL语句，用以达到执行代码对[WEB服务器](https://baike.baidu.com/item/WEB服务器/8390210)进行攻击的方法。简单的说就是在post/getweb表单、输入域名或页面请求的查询字符串中插入SQL命令，最终使web服务器执行恶意命令的过程。

```java
package com.zxl.demo02;

import com.zxl.demo02.utils.JdbcUtils;

import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

// SQL 注入问题
public class SQLInjection {
    public static void main(String[] args) {
        // 正常登录
        // login("zxlyzdz or 1 = 1", "123456");
        login("'or '1=1", "123456"); 
    }

    // 登陆业务
    public static void login(String username, String password) {
        // 创建对象
        Connection connection = null;
        Statement statement = null;
        ResultSet resultSet = null;

        // 获取链接
        try {
            connection = JdbcUtils.getConnection();
            statement = connection.createStatement();
            // SELECT * FROM users WHERE `name` = 'zxlyzdz' AND `PASSWORD` = '123456'
            String sql = "select * from users where `name` = '" + username + "' and `PASSWORD` = '" + password+"'";
            resultSet = statement.executeQuery(sql);
            while (resultSet.next()) {
                System.out.println("id="+resultSet.getObject("id"));
                System.out.println("name="+resultSet.getObject("NAME"));
                System.out.println("password="+resultSet.getObject("PASSWORD"));
                System.out.println("email="+resultSet.getObject("email"));
                System.out.println("birth="+resultSet.getObject("birthday"));
            }
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            JdbcUtils.release(connection, statement, resultSet);
        }
    }
}
```

#### 10.5、PreparedStatement对象

`PreparedStatement`可以防止 SQL 注入，效率更高

本质：

1、新增

```java
package com.zxl.demo03;

import com.zxl.demo02.utils.JdbcUtils;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.Date;

public class TestInsert {
    public static void main(String[] args) {
        // 创建对象
        Connection connection = null;
        PreparedStatement preparedStatement = null;
        ResultSet resultSet = null;

        try {
            // 获取 Connection 对象
            connection = JdbcUtils.getConnection();

            // 预编译 SQL
            // 使用 ? 占位符代替参数
            String sql = "INSERT INTO users(`id`, `NAME`, `PASSWORD`, `email`, `birthday`) VALUES (?, ?, ?, ?, ?)";

            // 获取 PreparedStatement 对象
            preparedStatement = connection.prepareStatement(sql);

            // 手动给参数赋值
            preparedStatement.setInt(1, 5);
            preparedStatement.setString(2, "zzzz");
            preparedStatement.setString(3, "123456789");
            preparedStatement.setString(4, "123456789@qq.com");
            // 注意点 sql.Date   数据库用的 java.sql.Date() 转化为数据库支持的Date
            //       util.Date   Java用的 new Date().getTime() 获得时间戳
            preparedStatement.setDate(5, new java.sql.Date(new Date().getTime()));

            // 执行
            int i = preparedStatement.executeUpdate();
            if (i > 0) {
                System.out.println("插入成功");
            }

        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            JdbcUtils.release(connection, preparedStatement, resultSet);
        }
    }
}
```

2、删除

```java
package com.zxl.demo03;

import com.zxl.demo02.utils.JdbcUtils;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.Date;

public class TestDelete {
    public static void main(String[] args) {
        // 创建对象
        Connection connection = null;
        PreparedStatement preparedStatement = null;
        ResultSet resultSet = null;

        try {
            // 获取 Connection 对象
            connection = JdbcUtils.getConnection();

            // 预编译 SQL
            // 使用 ? 占位符代替参数
            String sql = "DELETE FROM users WHERE id = ? ";

            // 获取 PreparedStatement 对象
            preparedStatement = connection.prepareStatement(sql);

            // 手动给参数赋值
            preparedStatement.setInt(1, 4);

            // 执行
            int i = preparedStatement.executeUpdate();
            if (i > 0) {
                System.out.println("删除成功");
            }

        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            JdbcUtils.release(connection, preparedStatement, resultSet);
        }
    }
}
```

3、更新

```java
package com.zxl.demo03;

import com.zxl.demo02.utils.JdbcUtils;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

public class TestUpdate {
    public static void main(String[] args) {
        // 创建对象
        Connection connection = null;
        PreparedStatement preparedStatement = null;
        ResultSet resultSet = null;

        try {
            // 获取 Connection 对象
            connection = JdbcUtils.getConnection();

            // 预编译 SQL
            // 使用 ? 占位符代替参数
            String sql = "UPDATE users SET `name` = 'zxlyzdz' WHERE id = ? ";

            // 获取 PreparedStatement 对象
            preparedStatement = connection.prepareStatement(sql);

            // 手动给参数赋值
            preparedStatement.setInt(1, 5);

            // 执行
            int i = preparedStatement.executeUpdate();
            if (i > 0) {
                System.out.println("更新成功");
            }

        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            JdbcUtils.release(connection, preparedStatement, resultSet);
        }
    }
}
```

4、修改

```java
package com.zxl.demo03;

import com.zxl.demo02.utils.JdbcUtils;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

public class TestQuery {
    public static void main(String[] args) {
        // 创建对象
        Connection connection = null;
        PreparedStatement preparedStatement = null;
        ResultSet resultSet = null;

        try {
            // 获取 Connection 对象
            connection = JdbcUtils.getConnection();

            // 预编译 SQL
            // 使用 ? 占位符代替参数
            String sql = "SELECT * FROM users WHERE `name` = ? AND `PASSWORD` = ?";

            // 获取 PreparedStatement 对象
            preparedStatement = connection.prepareStatement(sql);

            // 手动给参数赋值
            preparedStatement.setString(1, "zxlyzdz");
            preparedStatement.setString(2, "123456789");

            // 执行
            resultSet = preparedStatement.executeQuery();
            while(resultSet.next()) {
                System.out.println(resultSet.getObject("NAME"));
                System.out.println(resultSet.getObject("PASSWORD"));
            }

        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            JdbcUtils.release(connection, preparedStatement, resultSet);
        }
    }
}
```

5、防止SQL注入

```java
package com.zxl.demo03;

import com.zxl.demo02.utils.JdbcUtils;

import java.sql.*;

// SQL 注入问题
public class SQLInjection {
    public static void main(String[] args) {
        // 正常登录
        // login("zxlyzdz or 1 = 1", "123456");
        login("'or '1=1", "123456");
    }

    // 登陆业务
    public static void login(String username, String password) {
        // 创建对象
        Connection connection = null;
        PreparedStatement preparedStatement = null;
        ResultSet resultSet = null;

        // 获取链接
        try {
            connection = JdbcUtils.getConnection();
            
            // SELECT * FROM users WHERE `name` = 'zxlyzdz' AND `PASSWORD` = '123456'
            String sql = "select * from users where `NAME` = ? AND `PASSWORD` = ?";
            
            preparedStatement = connection.prepareStatement(sql);
            preparedStatement.setString(1, username);
            preparedStatement.setString(2, password);
            
            resultSet = preparedStatement.executeQuery();
            while (resultSet.next()) {
                System.out.println("id="+resultSet.getObject("id"));
                System.out.println("name="+resultSet.getObject("NAME"));
                System.out.println("password="+resultSet.getObject("PASSWORD"));
                System.out.println("email="+resultSet.getObject("email"));
                System.out.println("birth="+resultSet.getObject("birthday"));
            }
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            JdbcUtils.release(connection, preparedStatement, resultSet);
        }
    }
}
```

#### 10.6、IDEA连接数据库

![image-20210329214832376](https://www.zxlyzdz.com/images/mysql/idea_database_1.png)

![image-20210329214909603](https://www.zxlyzdz.com/images/mysql/idea_database_2.png)

![image-20210329215032111](https://www.zxlyzdz.com/images/mysql/idea_database_3.png)

连接成功后，选择数据库

![image-20210329215341130](https://www.zxlyzdz.com/images/mysql/idea_database_4.png)

双击数据库就可以查看信息了

#### 10.8、事务

**要么都成功，要么都失败**

> ACID 原则

原子性：要么都成功，要么都失败

一致性：总数不变

隔离性：事务与事务之间互不干扰

持久性：事务一旦被提交，持久化到数据库

> 隔离性的问题 脏读 不可重复读 幻读

脏读：一个事务读取到另一个没有提交的事务

不可重复读：在同一个事务内，多次读取表中数据，表数据发生了改变

虚读（幻读）：在一个事务内，读取到了别人插入了数据，导致前后读出来结果不一致

> 代码实现

1、开启事务，关闭事务自动i提交 `conn.setAutoCommit(false)`

2、一组业务执行完毕，提交事务

3、可以在 catch 语句中显示的定义 回滚 语句，但是默认失败就会回滚

```java
package com.zxl.demo04;

import com.zxl.demo02.utils.JdbcUtils;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

public class TestTransaction {
    public static void main(String[] args) {
        Connection conn = null;
        PreparedStatement st = null;
        ResultSet rs = null;
        try {
            conn = JdbcUtils.getConnection();
            // 关闭数据库自动提交功能，自动开启事务
            conn.setAutoCommit(false); // 开启事务

            String sql1 = "update account set money = money - 100 where name = 'A'";
            st = conn.prepareStatement(sql1);
            st.executeUpdate();

            // 模拟失败
            int x = 1 / 0;

            String sql2 = "update account set money = money + 100 where name = 'B'";
            st = conn.prepareStatement(sql2);
            st.executeUpdate();

            // 业务完毕，提交事务
            conn.commit();
            System.out.println("成功");

        } catch (SQLException e) {
            // 如果失败，默认回滚
            //try {
            //    conn.rollback();
            //} catch (SQLException throwables) {
            //    throwables.printStackTrace();
            //}
            e.printStackTrace();
        } finally {
            JdbcUtils.release(conn, st, rs);
        }
    }
}
```

#### 10.9、数据库连接池

数据库连接 --- 执行完毕 --- 资源释放

连接 -- 释放 十分浪费系统资源

**池化技术：准备一些预先的资源，过来就连接预先准备好的**

**编写连接池，实现一个接口 DataSource** 

> 开源数据源实现（拿来即用）

DBCP，C3P0，Druid

使用了这些数据库连接池之后，我们在项目开发中就不需要编写连接数据库的代码了

```java
package com.zxl.demo05;

import com.zxl.demo02.utils.JdbcUtils;
import org.apache.commons.dbcp.BasicDataSource;
import org.apache.commons.dbcp.BasicDataSourceFactory;

import javax.sql.DataSource;
import java.io.InputStream;
import java.sql.*;
import java.util.Properties;

public class DbcpUtils {
    private static DataSource dataSource = null;
    static {
        try {
            InputStream in = JdbcUtils.class.getClassLoader().getResourceAsStream("dbcpconfig.properties");
            Properties properties = new Properties();
            properties.load(in);

            // 创建数据源 工厂模式 -->  创建
            dataSource = BasicDataSourceFactory.createDataSource(properties);

        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    // 获取连接
    public static Connection getConnection() throws SQLException {
        return dataSource.getConnection(); // 从数据源中获取连接
    }


    // 释放连接资源
    public static void release(Connection connection, Statement statement, ResultSet resultSet) {
        if (resultSet != null) {
            try {
                resultSet.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if (statement != null) {
            try {
                statement.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if (connection != null) {
            try {
                connection.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
}

```



```java
package com.zxl.demo05;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.Date;

public class TestDbcp {
    public static void main(String[] args) {
        // 创建对象
        Connection connection = null;
        PreparedStatement preparedStatement = null;
        ResultSet resultSet = null;

        try {
            // 获取 Connection 对象
            connection = DbcpUtils.getConnection();

            // 预编译 SQL
            // 使用 ? 占位符代替参数
            String sql = "INSERT INTO users(`id`, `NAME`, `PASSWORD`, `email`, `birthday`) VALUES (?, ?, ?, ?, ?)";

            // 获取 PreparedStatement 对象
            preparedStatement = connection.prepareStatement(sql);

            // 手动给参数赋值
            preparedStatement.setInt(1, 5);
            preparedStatement.setString(2, "zzzz");
            preparedStatement.setString(3, "123456789");
            preparedStatement.setString(4, "123456789@qq.com");
            // 注意点 sql.Date   数据库用的 java.sql.Date() 转化为数据库支持的Date
            //       util.Date   Java用的 new Date().getTime() 获得时间戳
            preparedStatement.setDate(5, new java.sql.Date(new Date().getTime()));

            // 执行
            int i = preparedStatement.executeUpdate();
            if (i > 0) {
                System.out.println("插入成功");
            }

        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            DbcpUtils.release(connection, preparedStatement, resultSet);
        }
    }
}

```



> DBCP

需要用到的 jar 包

commons-dbcp-1.4.jar  commons-pool-1.6.jar

```bash
<?xml version="1.0" encoding="UTF-8"?>
<c3p0-config>
    <!--
    c3p0的缺省（默认）配置
    如果在代码中"ComboPooledDataSource ds=new ComboPooledDataSource();"这样写就表示使用的是c3p0的缺省（默认）-->
    <default-config>
        <property name="driverClass">com.mysql.jdbc.Driver</property>
        <property name="jdbcUrl">jdbc:mysql://localhost:3306/jdbcstudy?useUnicode=true&amp; characterEncoding=utf8&amp;uesSSL=true&amp;serverTimezone=UTC</property>
        <property name="user">root</property>
        <property name="password">123456</property>

        <property name="acquiredIncrement">5</property>
        <property name="initialPoolSize">10</property>
        <property name="minPoolSize">5</property>
        <property name="maxPoolSize">20</property>
    </default-config>
    <!--
          c3p0的命名配置
        如果在代码中"ComboPooledDataSource ds=new ComboPooledDataSource("MySQL");"这样写就表示使用的是mysql的缺省（默认）-->
    <named-config name="MySQL">
        <property name="driverClass">com.mysql.jdbc.Driver</property>
        <property name="jdbcUrl">jdbc:mysql://localhost:3306/jdbcstudy?useUnicode=true&amp; characterEncoding=utf8&amp;uesSSL=true&amp;serverTimezone=UTC</property>
        <property name="user">root</property>
        <property name="password">123456</property>

        <property name="acquiredIncrement">5</property>
        <property name="initialPoolSize">10</property>
        <property name="minPoolSize">5</property>
        <property name="maxPoolSize">20</property>
    </named-config>
</c3p0-config>
```



```java
package com.zxl.demo05;

import com.mchange.v2.c3p0.ComboPooledDataSource;
import com.zxl.demo02.utils.JdbcUtils;
import org.apache.commons.dbcp.BasicDataSourceFactory;

import javax.sql.DataSource;
import java.io.InputStream;
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.Properties;

public class C3p0Utils {
    private static DataSource dataSource = null;
    static {
        try {
            // 创建数据源 工厂模式 -->  创建
            dataSource = new ComboPooledDataSource("MySQL"); // 配置文件写法

        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    // 获取连接
    public static Connection getConnection() throws SQLException {
        return dataSource.getConnection(); // 从数据源中获取连接
    }


    // 释放连接资源
    public static void release(Connection connection, Statement statement, ResultSet resultSet) {
        if (resultSet != null) {
            try {
                resultSet.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if (statement != null) {
            try {
                statement.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if (connection != null) {
            try {
                connection.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
}

```

```java
package com.zxl.demo05;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.Date;

public class TestC3p0 {
    public static void main(String[] args) {
        // 创建对象
        Connection connection = null;
        PreparedStatement preparedStatement = null;
        ResultSet resultSet = null;

        try {
            // 获取 Connection 对象
            connection = C3p0Utils.getConnection();

            // 预编译 SQL
            // 使用 ? 占位符代替参数
            String sql = "INSERT INTO users(`id`, `NAME`, `PASSWORD`, `email`, `birthday`) VALUES (?, ?, ?, ?, ?)";

            // 获取 PreparedStatement 对象
            preparedStatement = connection.prepareStatement(sql);

            // 手动给参数赋值
            preparedStatement.setInt(1, 5);
            preparedStatement.setString(2, "zzzz");
            preparedStatement.setString(3, "123456789");
            preparedStatement.setString(4, "123456789@qq.com");
            // 注意点 sql.Date   数据库用的 java.sql.Date() 转化为数据库支持的Date
            //       util.Date   Java用的 new Date().getTime() 获得时间戳
            preparedStatement.setDate(5, new java.sql.Date(new Date().getTime()));

            // 执行
            int i = preparedStatement.executeUpdate();
            if (i > 0) {
                System.out.println("插入成功");
            }

        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            DbcpUtils.release(connection, preparedStatement, resultSet);
        }
    }
}

```



> 结论

无论使用什么数据源，本质还是一样的，DataSource 接口不会变，方法就不会变