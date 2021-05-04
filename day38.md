## 一、昨日内容回顾

1. 用户相关操作
   1. 查看当前用户是谁? select user();
   2. 给当前用户设置密码 set password = password('123');
   3. 创建用户 create user '用户名'@'主机的ip/主机域名' identified by '密码'
   4. 授权 grant select on 数据库名.* to '用户名'@'主机的ip/主机域名' identified by '密码'
   5. 授权并创建用户 grant select on 数据库名.* to '用户名'@'主机的ip/主机域名'
2. 基础的库\表\数据操作
   1. 库 - 文件夹
      1. 创建库  ：         create database 数据库名;
      2. 切换到这个库下:     use 库名
      3. 查看所有库  :       show databases;
      4. 删除: drop database 库名;
   2. 表 - 文件
      1. 查看这个库下的所有表  show tables;
      2. 创建表  :          create table 表名(字段名 数据类型(长度),字段名 数据类型(长度),..);
      3. 删除表  :              drop table 表名;
      4. 查看表结构   :         desc 表名;  （describe 表名;）
   3. 数据(记录) - 文件中的内容
      1. 增 : insert into 表 values (一行数据),(一行数据),(一行数据);
      2. 删 : delete from 表 where 条件;
      3. 改 : update 表 set 字段名=值,字段2=值2 where 条件;
      4. 查 : select 字段 from 表;

## 二、今日内容

1. 数据库：你可以理解为 数据库 是一个可以在一台机器上独立工作的，并且可以给我们提供高效、便捷的方式对数据进行增删改查的一种工具。

2. 数据库的优势：

   1. 程序稳定性 ：这样任意一台服务所在的机器崩溃了都不会影响数据和另外的服务。
   2. 数据一致性 ：所有的数据都存储在一起，所有的程序操作的数据都是统一的，就不会出现数据不一致的现象
   3. 并发 ：数据库可以良好的支持并发，所有的程序操作数据库都是通过网络，而数据库本身支持并发的网络操作，不需要我们自己写socket
   4. 效率 ：使用数据库对数据进行增删改查的效率要高出我们自己处理文件很多

3. 什么是数据（Data）

   描述事物的符号记录称为数据，描述事物的符号既可以是数字，也可以是文字、图片，图像、声音、语言等，数据由多种表现形式，它们都可以经过数字化后存入计算机

4. 什么是数据库（DataBase，简称DB）

   数据库即存放数据的仓库，只不过这个仓库是在计算机存储设备上，而且数据是按一定的格式存放的

   过去人们将数据存放在文件柜里，现在数据量庞大，已经不再适用

   数据库是长期存放在计算机内、有组织、可共享的数据集合。

   数据库中的数据按一定的数据模型组织、描述和储存，具有较小的冗余度、较高的数据独立性和易扩展性，并可为各种 用户共享

5. 什么是数据库管理系统（DataBase Management System 简称DBMS）

   数据库管理系统，如MySQL、Oracle、SQLite、Access、MS SQL Server

   mysql主要用于大型门户，例如搜狗、新浪等，它主要的优势就是开放源代码，因为开放源代码这个数据库是免费的，他现在是甲骨文公司的产品。
   *oracle主要用于银行、铁路、飞机场等。该数据库功能强大，软件费用高。也是甲骨文公司的产品。*
   sql server是微软公司的产品，主要应用于大中型企业，如联想、方正等

6. 数据库管理员 DBA（Database Administrator）

7. ##### 数据库服务器、数据管理系统、数据库、表与记录的关系（重点）

   记录：1 朱葛 13234567890 22（多个字段的信息组成一条记录，即文件中的一行内容）

   表：userinfo,studentinfo,courseinfo（即文件）

   数据库：db（即文件夹）

   数据库管理系统：如mysql（是一个软件）

   数据库服务器：一台计算机（对内存要求比较高）

   总结：

   数据库服务器-：运行数据库管理软件

   数据库管理软件：管理-数据库

   数据库：即文件夹，用来组织文件/表

   表：即文件，用来存放多行内容/多条记录

8. 数据库管理软件分类

   管理数据的工具有很多种，不止mysql一个。关于分类其实可以从各个纬度来进行划分，但是我们最常使用的分类还是根据他们存取数据的特点来划分的，主要分为关系型和非关系型。

   　　可以简单的理解为，关系型数据库需要有表结构，非关系型数据库是key-value存储的，没有表结构

   ```python
   关系型：如sqllite，db2，oracle，access，sql server，MySQL，注意：sql语句通用
   非关系型：mongodb，redis，memcache
   ```

9. 小结

   1. 数据库 : DB
      1. 所有的数据存放的仓库
      2. 每一个文件夹也是一个数据库
   2. 数据库管理系统  -- 软件 DBMS
      1. 关系型数据库 : mysql oracle sqllite sql server db2 access
      2. 非关系型数据库 : redis mongodb memcache
   3. 数据库管理员  DBA：管理数据库软件
   4. 数据库服务器   一台跑着一个数据库管理软件的机器
   5. 表 : 文件,一张存储了数据的表
   6. 数据/记录 : 表中的信息,一行就是一条记录

10. #### 存储引擎 -- 存储数据的方式

    1. mysql支持的存储引擎包括InnoDB、MyISAM、MEMORY、CSV、BLACKHOLE、FEDERATED、MRG_MYISAM、ARCHIVE、PERFORMANCE_SCHEMA。其中NDB和InnoDB提供事务安全表，其他存储引擎都是非事务安全表。

    2. 一张表的结构

       1. 数据
       2. 表的结构
       3. 索引(查询的时候使用的一个目录结构)

    3. Innodb存储引擎（mysql5.6之后的默认的存储引擎） ：  用于事务处理应用程序，支持外键和行级锁。如果应用对事物的完整性有比较高的要求，在并发条件下要求数据的一致性，数据操作除了插入和查询之外，还包括很多更新和删除操作，那么InnoDB存储引擎是比较合适的。InnoDB除了有效的降低由删除和更新导致的锁定，还可以确保事务的完整提交和回滚，对于类似计费系统或者财务系统等对数据准确要求性比较高的系统都是合适的选择。

       1. 数据和索引存储在一起   2个文件>>>数据索引\表结构

       2. 特点

          1. 数据持久化
          2. 支持事务   : 为了保证数据的完整性,将多个操作变成原子性操作   : 保持数据安全
          3. 支持行级锁 : 修改的行少的时候使用                          : 修改数据频繁的操作
          4. 支持表级锁 : 批量修改多行的时候使用                        : 对于大量数据的同时修改
          5. 支持外键   : 约束两张表中的关联字段不能随意的添加\删除      : 能够降低数据增删改的出错率

       3. Myisam存储引擎（mysql5.5之前的默认的存储引擎）：如果应用是以读操作和插入操作为主，只有很少的更新和删除操作，并且对事务的完整性、并发性要求不高，那么可以选择这个存储引擎。    

          1. 数据和索引不存储在一起   3个文件>>>数据\索引\表结构
          2. 数据持久化，只支持表锁

       4. Memory存储引擎：将所有的数据保存在内存中，在需要快速定位记录和其他类似数据的环境下，可以提供极快的访问。Memory的缺陷是对表的大小有限制，虽然数据库因为异常终止的话数据可以正常恢复，但是一旦数据库关闭，存储在内存中的数据都会丢失。

          1. 数据存储在内存中, 1个文件>>>表结构
          2. 数据断电消失

       5. 面试题:你的项目用了什么存储引擎,为什么?

          innodb，1、多个用户操作的过程中对同一张表的数据同时做修改，innodb支持行级锁,所以我们使用了这个存储引擎；2、为了适应程序未来的扩展性,扩展新功能的时候可能会用到...,涉及到要维护数据的完整性；3、项目中有一两张xx xx表,之间的外键关系是什么,一张表的修改或者删除比较频繁,怕出错所以做了外键约束
        
       6. ```
          查看当前的默认存储引擎:
          mysql> show variables like "default_storage_engine";
          
          查询当前数据库支持的存储引擎
          mysql> show engines \G;
          
          指定存储引擎建表
          mysql> create table ai(id bigint(12),name varchar(200)) ENGINE=MyISAM; 
          
          mysql> create table country(id int(4),cname varchar(50)) ENGINE=InnoDB;
          ```
    
11. ### 表和数据的基础操作   `*******`

    ```python
    #创建表
    #语法：
    create table 表名(
    字段名1 类型[(宽度) 约束条件],
    字段名2 类型[(宽度) 约束条件],
    字段名3 类型[(宽度) 约束条件]
    );
    
    #注意：
    1. 在同一张表中，字段名是不能相同
    2. 宽度和约束条件可选
    3. 字段名和类型是必须的
    
    create table 表名(
    id int,
    name char(18),
    字段名3 类型[(宽度) 约束条件]
    );
    
    # 放在中括号里的内容可以不写
    
    # 写入数据的方式
    # insert into 表 values (值1,值2,值3);
        # 这张表有多少的字段,就需要按照字段的顺序写入多少个值
    # insert into 表 values (值1,值2,值3),(值1,值2,值3),(值1,值2,值3);
        # 一次性写入多条数据
    # insert into 表 (字段1,字段3 ) values (值1,值3);
        # 指定字段名写入,可以任意的选择表中你需要写入的字段进行
    
    # 查表中的数据
        # select * from 表
    
    # 查看表结构
        # desc 表名;
            # 能够查看到有多少个字段\类型\长度,看不到表编码,引擎,具体的约束信息只能看到一部分
        # show create table 表名;
            # 能查看字段\类型\长度\编码\引擎\约束
    
    #建表示例：
    mysql> create database staff;
    Query OK, 1 row affected (0.00 sec)
    
    mysql> use staff;
    Database changed
    mysql> create table staff_info (id int,name varchar(50),age int(3),sex enum('male','female'),phone bigint(11),job varchar(11));
    Query OK, 0 rows affected (0.02 sec)
    
    
    mysql> show tables;
    +-----------------+
    | Tables_in_staff |
    +-----------------+
    | staff_info      |
    +-----------------+
    1 row in set (0.00 sec)
    
    mysql> desc staff_info;
    +-------+-----------------------+------+-----+---------+-------+
    | Field | Type                  | Null | Key | Default | Extra |
    +-------+-----------------------+------+-----+---------+-------+
    | id    | int(11)               | YES  |     | NULL    |       |
    | name  | varchar(50)           | YES  |     | NULL    |       |
    | age   | int(3)                | YES  |     | NULL    |       |
    | sex   | enum('male','female') | YES  |     | NULL    |       |
    | phone | bigint(11)            | YES  |     | NULL    |       |
    | job   | varchar(11)           | YES  |     | NULL    |       |
    +-------+-----------------------+------+-----+---------+-------+
    6 rows in set (0.00 sec)
    
    mysql> select id,name,sex from staff_info;
    Empty set (0.00 sec)
    
    mysql> select * from staff_info;
    Empty set (0.00 sec)
    
    #插入数据
    mysql> insert into staff_info (id,name,age,sex,phone,job) values (1,'Alex',83,'female',13651054608,'IT');
    Query OK, 1 row affected (0.00 sec)
    
    mysql> insert into staff_info values (2,'Egon',26,'male',13304320533,'Teacher');
    Query OK, 1 row affected (0.00 sec)
    
    mysql> insert into staff_info values (3,'nezha',25,'male',13332353222,'IT'),(4,'boss_jin',40,'male',13332353333,'IT');
    Query OK, 2 rows affected (0.00 sec)
    Records: 2  Duplicates: 0  Warnings: 0
    
    mysql> select * from staff_info;
    +------+----------+------+--------+-------------+---------+
    | id   | name     | age  | sex    | phone       | job     |
    +------+----------+------+--------+-------------+---------+
    |    1 | Alex     |   83 | female | 13651054608 | IT      |
    |    2 | Egon     |   26 | male   | 13304320533 | Teacher |
    |    3 | nezha    |   25 | male   | 13332353222 | IT      |
    |    4 | boss_jin |   40 | male   | 13332353333 | IT      |
    +------+----------+------+--------+-------------+---------+
    4 rows in set (0.00 sec)
    
    mysql> describe staff_info;
    +-------+-----------------------+------+-----+---------+-------+
    | Field | Type                  | Null | Key | Default | Extra |
    +-------+-----------------------+------+-----+---------+-------+
    | id    | int(11)               | YES  |     | NULL    |       |
    | name  | varchar(50)           | YES  |     | NULL    |       |
    | age   | int(3)                | YES  |     | NULL    |       |
    | sex   | enum('male','female') | YES  |     | NULL    |       |
    | phone | bigint(11)            | YES  |     | NULL    |       |
    | job   | varchar(11)           | YES  |     | NULL    |       |
    +-------+-----------------------+------+-----+---------+-------+
    6 rows in set (0.00 sec)
    
    mysql> desc staff_info;
    +-------+-----------------------+------+-----+---------+-------+
    | Field | Type                  | Null | Key | Default | Extra |
    +-------+-----------------------+------+-----+---------+-------+
    | id    | int(11)               | YES  |     | NULL    |       |
    | name  | varchar(50)           | YES  |     | NULL    |       |
    | age   | int(3)                | YES  |     | NULL    |       |
    | sex   | enum('male','female') | YES  |     | NULL    |       |
    | phone | bigint(11)            | YES  |     | NULL    |       |
    | job   | varchar(11)           | YES  |     | NULL    |       |
    +-------+-----------------------+------+-----+---------+-------+
    6 rows in set (0.00 sec)
    
    mysql> show create table staff_info\G;
    *************************** 1. row ***************************
           Table: staff_info
    Create Table: CREATE TABLE `staff_info` (
      `id` int(11) DEFAULT NULL,
      `name` varchar(50) DEFAULT NULL,
      `age` int(3) DEFAULT NULL,
      `sex` enum('male','female') DEFAULT NULL,
      `phone` bigint(11) DEFAULT NULL,
      `job` varchar(11) DEFAULT NULL
    ) ENGINE=InnoDB DEFAULT CHARSET=utf8
    1 row in set (0.01 sec)
    
    ERROR: 
    No query specified
    ```

    

12. 数据类型—数字

    ```python
    # int 不约束长度,最多表示10位数
    # float(m,n)
        # m 一共多少位,
        # n 小数部分多少位
    
    #可直接复制到数据库运行    
    create table t1(
        id int,               # 默认是有符号的
        age tinyint unsigned  # 如果需要定义无符号的使用unsigned
    );
    
    create table t2(
      f1 float(5,2),   # 保留2位小数 并四舍五入
      f2 float,
      f3 double(5,2),
      f4 double
    )
    
    insert into t2(f2,f4) values(5.1783682169875975,5.1783682169875975179);
    
    create table t3(
      f1 float,   # 保留2位小数 并四舍五入
      d1 double,
      d2 decimal(30,20),
      d3 decimal
    );
    insert into t3 values(5.1783682169875975179,5.1783682169875975179,
                          5.1783682169875975179,5.1783682169875975179);   
    
    # 创建表一个是默认宽度的int，一个是指定宽度的int(5)
    mysql> create table t1 (id1 int,id2 int(5));
    Query OK, 0 rows affected (0.02 sec)
    
    # 像t1中插入数据1，1
    mysql> insert into t1 values (1,1);
    Query OK, 1 row affected (0.01 sec)
    
    # 可以看出结果上并没有异常
    mysql> select * from t1;
    +------+------+
    | id1  | id2  |
    +------+------+
    |    1 |    1 |
    +------+------+
    1 row in set (0.00 sec)
    
    # 那么当我们插入了比宽度更大的值，会不会发生报错呢？
    mysql> insert into t1 values (111111,111111);
    Query OK, 1 row affected (0.00 sec)
    
    # 答案是否定的，id2仍然显示了正确的数值，没有受到宽度限制的影响
    mysql> select * from t1;
    +------------+--------+
    | id1        | id2    |
    +------------+--------+
    | 0000000001 |  00001 |
    | 0000111111 | 111111 |
    +------------+--------+
    2 rows in set (0.00 sec)
    
    # 修改id1字段 给字段添加一个unsigned表示无符号
    mysql> alter table t1 modify id1 int unsigned;
    Query OK, 0 rows affected (0.01 sec)
    Records: 0  Duplicates: 0  Warnings: 0
    
    mysql> desc t1;
    +-------+------------------+------+-----+---------+-------+
    | Field | Type             | Null | Key | Default | Extra |
    +-------+------------------+------+-----+---------+-------+
    | id1   | int(10) unsigned | YES  |     | NULL    |       |
    | id2   | int(5)           | YES  |     | NULL    |       |
    +-------+------------------+------+-----+---------+-------+
    2 rows in set (0.01 sec)
    
    # 当给id1添加的数据大于214748364时，可以顺利插入
    mysql> insert into t1 values (2147483648,2147483647);
    Query OK, 1 row affected (0.00 sec)
    
    # 当给id2添加的数据大于214748364时，会报错
    mysql> insert into t1 values (2147483647,2147483648);
    ERROR 1264 (22003): Out of range value for column 'id2' at row 1
        
    
    # 创建表的三个字段分别为float，double和decimal参数表示一共显示5位，小数部分占2位
    mysql> create table t2 (id1 float(5,2),id2 double(5,2),id3 decimal(5,2));
    Query OK, 0 rows affected (0.02 sec)
    
    # 向表中插入1.23，结果正常
    mysql> insert into t2 values (1.23,1.23,1.23);
    Query OK, 1 row affected (0.00 sec)
    
    mysql> select * from t2;
    +------+------+------+
    | id1  | id2  | id3  |
    +------+------+------+
    | 1.23 | 1.23 | 1.23 |
    +------+------+------+
    1 row in set (0.00 sec)
    
    # 向表中插入1.234，会发现4都被截断了
    mysql> insert into t2 values (1.234,1.234,1.234);
    Query OK, 1 row affected, 1 warning (0.00 sec)
    
    mysql> select * from t2;
    +------+------+------+
    | id1  | id2  | id3  |
    +------+------+------+
    | 1.23 | 1.23 | 1.23 |
    | 1.23 | 1.23 | 1.23 |
    +------+------+------+
    2 rows in set (0.00 sec)
    
    # 向表中插入1.235发现数据虽然被截断，但是遵循了四舍五入的规则
    mysql> insert into t2 values (1.235,1.235,1.235);
    Query OK, 1 row affected, 1 warning (0.00 sec)
    
    mysql> select * from t2;
    +------+------+------+
    | id1  | id2  | id3  |
    +------+------+------+
    | 1.23 | 1.23 | 1.23 |
    | 1.23 | 1.23 | 1.23 |
    | 1.24 | 1.24 | 1.24 |
    +------+------+------+
    3 rows in set (0.00 sec)
    
    # 建新表去掉参数约束
    mysql> create table t3 (id1 float,id2 double,id3 decimal);
    Query OK, 0 rows affected (0.02 sec)
    
    # 分别插入1.234
    mysql> insert into t3 values (1.234,1.234,1.234);
    Query OK, 1 row affected, 1 warning (0.00 sec)
    
    # 发现decimal默认值是(10,0)的整数
    mysql> select * from t3;
    +-------+-------+------+
    | id1   | id2   | id3  |
    +-------+-------+------+
    | 1.234 | 1.234 |    1 |
    +-------+-------+------+
    1 row in set (0.00 sec)
    
    # 当对小数位没有约束的时候，输入超长的小数，会发现float和double的区别
    mysql> insert into t3 values (1.2355555555555555555,1.2355555555555555555,1.2355555555555555555555);
    Query OK, 1 row affected, 1 warning (0.00 sec)
    
    mysql> select * from t3;
    +---------+--------------------+------+
    | id1     | id2                | id3  |
    +---------+--------------------+------+
    |   1.234 |              1.234 |    1 |
    | 1.23556 | 1.2355555555555555 |    1 |
    +---------+--------------------+------+
    2 rows in set (0.00 sec)    
    ```

13. 数据类型—时间

    ```python
    #表示时间值的日期和时间类型为DATETIME、DATE、TIMESTAMP、TIME和YEAR。
    # date  20190620   写的时候注意把0补上
    # time  121953
    # datetime 20190620121900
    
    create table t4(
        dt datetime,
        y year,
        d date,
        t time,
        ts timestamp
    );
    
    #把 datetime 的默认属性改成 timestamp 一样的
    mysql> create table t5(
        -> id int,
        -> dt datetime NOT NULL                        # 不能为空
                      DEFAULT CURRENT_TIMESTAMP        # 默认是当前时间
                      ON UPDATE CURRENT_TIMESTAMP);    # 在更新的时候使用当前时间更新字段
    
    #date/time/datetime示例
    mysql> create table t4 (d date,t time,dt datetime);
    Query OK, 0 rows affected (0.02 sec)
    
    mysql> desc t4;
    +-------+----------+------+-----+---------+-------+
    | Field | Type     | Null | Key | Default | Extra |
    +-------+----------+------+-----+---------+-------+
    | d     | date     | YES  |     | NULL    |       |
    | t     | time     | YES  |     | NULL    |       |
    | dt    | datetime | YES  |     | NULL    |       |
    +-------+----------+------+-----+---------+-------+
    3 rows in set (0.01 sec)
    
    mysql> insert into t4 values (now(),now(),now());
    Query OK, 1 row affected, 1 warning (0.01 sec)
    
    mysql> select * from t4;
    +------------+----------+---------------------+
    | d          | t        | dt                  |
    +------------+----------+---------------------+
    | 2018-09-21 | 14:51:51 | 2018-09-21 14:51:51 |
    +------------+----------+---------------------+
    1 row in set (0.00 sec)
    
    mysql> insert into t4 values (null,null,null);
    Query OK, 1 row affected (0.01 sec)
    
    mysql> select * from t4;
    +------------+----------+---------------------+
    | d          | t        | dt                  |
    +------------+----------+---------------------+
    | 2018-09-21 | 14:51:51 | 2018-09-21 14:51:51 |
    | NULL       | NULL     | NULL                |
    +------------+----------+---------------------+
    2 rows in set (0.00 sec)
    
    #timestamp示例
    mysql> create table t5 (id1 timestamp);
    Query OK, 0 rows affected (0.02 sec)
    
    mysql> desc t5;
    +-------+-----------+------+-----+-------------------+-----------------------------+
    | Field | Type      | Null | Key | Default           | Extra                       |
    +-------+-----------+------+-----+-------------------+-----------------------------+
    | id1   | timestamp | NO   |     | CURRENT_TIMESTAMP | on update CURRENT_TIMESTAMP |
    +-------+-----------+------+-----+-------------------+-----------------------------+
    1 row in set (0.00 sec)
    
    # 插入数据null，会自动插入当前时间的时间
    mysql> insert into t5 values (null);
    Query OK, 1 row affected (0.00 sec)
    
    mysql> select * from t5;
    +---------------------+
    | id1                 |
    +---------------------+
    | 2018-09-21 14:56:50 |
    +---------------------+
    1 row in set (0.00 sec)
    
    #添加一列 默认值是'0000-00-00 00:00:00'
    mysql> alter table t5 add id2 timestamp;
    Query OK, 0 rows affected (0.02 sec)
    Records: 0  Duplicates: 0  Warnings: 0
    
    mysql> show create table t5 \G;
    *************************** 1. row ***************************
           Table: t5
    Create Table: CREATE TABLE `t5` (
      `id1` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
      `id2` timestamp NOT NULL DEFAULT '0000-00-00 00:00:00'
    ) ENGINE=InnoDB DEFAULT CHARSET=utf8
    1 row in set (0.00 sec)
    
    ERROR: 
    No query specified
    
    # 手动修改新的列默认值为当前时间
    mysql> alter table t5 modify id2 timestamp default current_timestamp;
    Query OK, 0 rows affected (0.02 sec)
    Records: 0  Duplicates: 0  Warnings: 0
    
    mysql> show create table t5 \G;
    *************************** 1. row ***************************
           Table: t5
    Create Table: CREATE TABLE `t5` (
      `id1` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
      `id2` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP
    ) ENGINE=InnoDB DEFAULT CHARSET=utf8
    1 row in set (0.00 sec)
    
    ERROR: 
    No query specified
    
    mysql> insert into t5 values (null,null);
    Query OK, 1 row affected (0.01 sec)
    
    mysql> select * from t5;
    +---------------------+---------------------+
    | id1                 | id2                 |
    +---------------------+---------------------+
    | 2018-09-21 14:56:50 | 0000-00-00 00:00:00 |
    | 2018-09-21 14:59:31 | 2018-09-21 14:59:31 |
    +---------------------+---------------------+
    2 rows in set (0.00 sec)
    
    #timestamp示例2
    mysql> create table t6 (t1 timestamp);
    Query OK, 0 rows affected (0.02 sec)
    
    mysql> desc t6;
    +-------+-----------+------+-----+-------------------+-----------------------------+
    | Field | Type      | Null | Key | Default           | Extra                       |
    +-------+-----------+------+-----+-------------------+-----------------------------+
    | t1    | timestamp | NO   |     | CURRENT_TIMESTAMP | on update CURRENT_TIMESTAMP |
    +-------+-----------+------+-----+-------------------+-----------------------------+
    1 row in set (0.01 sec)
    
    mysql> insert into t6 values (19700101080001);
    Query OK, 1 row affected (0.00 sec)
    
    mysql> select * from t6;
    +---------------------+
    | t1                  |
    +---------------------+
    | 1970-01-01 08:00:01 |
    +---------------------+
    1 row in set (0.00 sec)
    # timestamp时间的下限是19700101080001
    mysql> insert into t6 values (19700101080000);
    ERROR 1292 (22007): Incorrect datetime value: '19700101080000' for column 't1' at row 1
    
    mysql> insert into t6 values ('2038-01-19 11:14:07');
    Query OK, 1 row affected (0.00 sec)
    # timestamp时间的上限是2038-01-19 11:14:07
    mysql> insert into t6 values ('2038-01-19 11:14:08');
    ERROR 1292 (22007): Incorrect datetime value: '2038-01-19 11:14:08' for column 't1' at row 1
    mysql> 
    
    #year示例
    mysql> create table t7 (y year);
    Query OK, 0 rows affected (0.02 sec)
    
    mysql> insert into t7 values (2018);
    Query OK, 1 row affected (0.00 sec)
    
    mysql> select * from t7;
    +------+
    | y    |
    +------+
    | 2018 |
    +------+
    1 row in set (0.00 sec)
    
    #datetime示例
    mysql> create table t8 (dt datetime);
    Query OK, 0 rows affected (0.01 sec)
    
    mysql> insert into t8 values ('2018-9-26 12:20:10');
    Query OK, 1 row affected (0.01 sec)
    
    mysql> insert into t8 values ('2018/9/26 12+20+10');
    Query OK, 1 row affected (0.00 sec)
    
    mysql> insert into t8 values ('20180926122010');
    Query OK, 1 row affected (0.00 sec)
    
    mysql> insert into t8 values (20180926122010);
    Query OK, 1 row affected (0.00 sec)
    
    mysql> select * from t8;
    +---------------------+
    | dt                  |
    +---------------------+
    | 2018-09-26 12:20:10 |
    | 2018-09-26 12:20:10 |
    | 2018-09-26 12:20:10 |
    | 2018-09-26 12:20:10 |
    +---------------------+
    4 rows in set (0.00 sec)
    
    #进阶
    <span style="font-size:14px;">interval的说明：
    
    1、当函数使用时，即interval(),为比较函数，如：interval(10,1,3,5,7); 结果为4；
           
       原理：10为被比较数，后面1,3,5,7为比较数，将后面四个依次与10比较，看后面数字组有多少个少于10，则返回其个数。前提是后面数字组为从小到大排列，否则返回结果0。
    	   
    2、当关键词使用时，表示为设置时间间隔，常用在date_add()与date_sub()函数里，如：interval 1 day ，解释为将时间间隔设置为1天。
    </span>
    
    # 本月第一天
    select date_add(curdate(), interval - day(curdate()) + 1 day);
    # 本月最后一天
    select last_day(curdate());
    # 上月第一天
    select date_add(curdate()-day(curdate())+1,interval -1 month);
    # 上月最后一天
    select last_day(date_sub(now(),interval 1 month));
    # 下月第一天
    select date_add(curdate()-day(curdate())+1,interval 1 month);
    # 下月最后一天
    select last_day(date_sub(now(),interval -1 month));
    # 本月天数
    select day(last_day(curdate()));
    # 上月今天的当前日期
    select date_sub(curdate(), interval 1 month);
    # 上月今天的当前时间（时间戳）
    select unix_timestamp(date_sub(now(),interval 1 month));
    # 获取当前时间与上个月之间的天数
    select datediff(curdate(), date_sub(curdate(), interval 1 month));
    ```

14. 数据类型—字符串

    ```python
    # char
    # varchar
    #CHAR 和 VARCHAR 类型类似，但它们保存和检索的方式不同。它们的最大长度和是否尾部空格被保留等方面也不同。在存储或检索过程中不进行大小写转换。
    
    #CHAR列的长度固定为创建表是声明的长度,范围(0-255);而VARCHAR的值是可变长字符串范围(0-65535)。
    # char(18)    最多只能表示255个字符,括号里填的
        # 定长存储,浪费空间,节省时间
        # 'alex'   'alex                 '
    # varchar(18) 最多能表示65535个字符 ，括号里填的
        # 变长存储,节省空间,存取速度慢
        # 'alex'   'alex4'
    
    # 适合使用char
        # 身份证号
        # 手机号码
        # qq号
        # username 12-18
        # password 32
        # 银行卡号
    # 适合使用varchar
        # 评论
        # 朋友圈
        # 微博
    
    create table t6(c1 char,v1 varchar(1),c2 char(8),v2 varchar(8));
    
    #char/varchar示例
    mysql> create table t9 (v varchar(4),c char(4));
    Query OK, 0 rows affected (0.01 sec)
    
    mysql> insert into t9 values ('ab  ','ab  ');
    Query OK, 1 row affected (0.00 sec)
    
    # 在检索的时候char数据类型会去掉空格
    mysql> select * from t9;
    +------+------+
    | v    | c    |
    +------+------+
    | ab   | ab   |
    +------+------+
    1 row in set (0.00 sec)
    
    # 来看看对查询结果计算的长度
    mysql> select length(v),length(c) from t9;
    +-----------+-----------+
    | length(v) | length(c) |
    +-----------+-----------+
    |         4 |         2 |
    +-----------+-----------+
    1 row in set (0.00 sec)
    
    # 给结果拼上一个加号会更清楚
    mysql> select concat(v,'+'),concat(c,'+') from t9;
    +---------------+---------------+
    | concat(v,'+') | concat(c,'+') |
    +---------------+---------------+
    | ab  +         | ab+           |
    +---------------+---------------+
    1 row in set (0.00 sec)
    
    # 当存储的长度超出定义的长度，会截断
    mysql> insert into t9 values ('abcd  ','abcd  ');
    Query OK, 1 row affected, 1 warning (0.01 sec)
    
    mysql> select * from t9;
    +------+------+
    | v    | c    |
    +------+------+
    | ab   | ab   |
    | abcd | abcd |
    +------+------+
    2 rows in set (0.00 sec)
    
    ```

15. 数据类型—enum和set

    ```python
    # enum 单选
    # set 多选
    #ENUM中文名称叫枚举类型，它的值范围需要在创建表时通过枚举方式显示。ENUM只允许从值集合中选取单个值，而不能一次取多个值。
    
    #SET和ENUM非常相似，也是一个字符串对象，里面可以包含0-64个成员。根据成员的不同，存储上也有所不同。set类型可以允许值集合中任意选择1或多个元素进行组合。对超出范围的内容将不允许注入，而对重复的值将进行自动去重。
    
    create table t8(
        id int,
        name char(18),
        gender enum('male','female')
    )
    
    create table t9(
        id int,
        name char(18),
        hobby set('抽烟','喝酒','烫头','洗脚','按摩')
    );
    insert into t9 values (1,'太白','烫头,抽烟,喝酒,按摩');
    insert into t9 values (1,'大壮','洗脚,洗脚,洗脚,按摩,打游戏');
    
    #set/enum示例
    mysql> create table t10 (name char(20),gender enum('female','male'));
    Query OK, 0 rows affected (0.01 sec)
    
    # 选择enum('female','male')中的一项作为gender的值，可以正常插入
    mysql> insert into t10 values ('nezha','male');
    Query OK, 1 row affected (0.00 sec)
    
    # 不能同时插入'male,female'两个值，也不能插入不属于'male,female'的值
    mysql> insert into t10 values ('nezha','male,female');
    ERROR 1265 (01000): Data truncated for column 'gender' at row 1
    
    mysql> create table t11 (name char(20),hobby set('抽烟','喝酒','烫头','翻车'));
    Query OK, 0 rows affected (0.01 sec)
    
    # 可以任意选择set('抽烟','喝酒','烫头','翻车')中的项，并自带去重功能
    mysql> insert into t11 values ('yuan','烫头,喝酒,烫头');
    Query OK, 1 row affected (0.01 sec)
    
    mysql> select * from t11;
    +------+---------------+
    | name | hobby        |
    +------+---------------+
    | yuan | 喝酒,烫头     |
    +------+---------------+
    1 row in set (0.00 sec)
    
    # 不能选择不属于set('抽烟','喝酒','烫头','翻车')中的项，
    mysql> insert into t11 values ('alex','烫头,翻车,看妹子');
    ERROR 1265 (01000): Data truncated for column 'hobby' at row 1
    ```

16. 完整性约束

    ```python
    #来自博客
    #为了防止不符合规范的数据进入数据库，在用户对数据进行插入、修改、删除等操作时，DBMS自动按照一定的约束条件对数据进行监测，使不符合规范的数据不能进入数据库，以确保数据库中存储的数据正确、有效、相容。
    # NOT NULL ：非空约束，指定某列不能为空； 
    # UNIQUE : 唯一约束，指定某列或者几列组合不能重复
    # PRIMARY KEY ：主键，指定该列的值可以唯一地标识该列记录
    # FOREIGN KEY ：外键，指定该行记录从属于主表中的一条记录，主要用于参照完整性
    
    #1、NOT NULL
    #是否可空，null表示空，非字符串  ，not null - 不可空 ，null - 可空 
    mysql> create table t12 (id int not null);
    Query OK, 0 rows affected (0.02 sec)
    
    mysql> select * from t12;
    Empty set (0.00 sec)
    
    mysql> desc t12;
    +-------+---------+------+-----+---------+-------+
    | Field | Type    | Null | Key | Default | Extra |
    +-------+---------+------+-----+---------+-------+
    | id    | int(11) | NO   |     | NULL    |       |
    +-------+---------+------+-----+---------+-------+
    1 row in set (0.00 sec)
    
    #不能向id列插入空元素。 
    mysql> insert into t12 values (null);
    ERROR 1048 (23000): Column 'id' cannot be null
    
    mysql> insert into t12 values (1);
    Query OK, 1 row affected (0.01 sec)
    
    #我们约束某一列不为空，如果这一列中经常有重复的内容，就需要我们频繁的插入，这样会给我们的操作带来新的负担，于是就出现了默认值的概念。
    #默认值，创建列时可以指定默认值，当插入数据时如果未主动设置，则自动添加默认值
    mysql> create table t13 (id1 int not null,id2 int not null default 222);
    Query OK, 0 rows affected (0.01 sec)
    
    mysql> desc t13;
    +-------+---------+------+-----+---------+-------+
    | Field | Type    | Null | Key | Default | Extra |
    +-------+---------+------+-----+---------+-------+
    | id1   | int(11) | NO   |     | NULL    |       |
    | id2   | int(11) | NO   |     | 222     |       |
    +-------+---------+------+-----+---------+-------+
    2 rows in set (0.01 sec)
    
    # 只向id1字段添加值，会发现id2字段会使用默认值填充
    mysql> insert into t13 (id1) values (111);
    Query OK, 1 row affected (0.00 sec)
    
    mysql> select * from t13;
    +-----+-----+
    | id1 | id2 |
    +-----+-----+
    | 111 | 222 |
    +-----+-----+
    1 row in set (0.00 sec)
    
    # id1字段不能为空，所以不能单独向id2字段填充值；
    mysql> insert into t13 (id2) values (223);
    ERROR 1364 (HY000): Field 'id1' doesn't have a default value
    
    # 向id1，id2中分别填充数据，id2的填充数据会覆盖默认值
    mysql> insert into t13 (id1,id2) values (112,223);
    Query OK, 1 row affected (0.00 sec)
    
    mysql> select * from t13;
    +-----+-----+
    | id1 | id2 |
    +-----+-----+
    | 111 | 222 |
    | 112 | 223 |
    +-----+-----+
    2 rows in set (0.00 sec)
    
    #not null不生效
    设置严格模式：
        不支持对not null字段插入null值
        不支持对自增长字段插入”值
        不支持text字段有默认值
    
    直接在mysql中生效(重启失效):
    mysql>set sql_mode="STRICT_TRANS_TABLES,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION";
    
    配置文件添加(永久失效)： #用pycharm打开 my.ini 文件 复制这句话 保存就行
    sql-mode="STRICT_TRANS_TABLES,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION"
    
    #2、unique:唯一约束，指定某列或者几列组合不能重复
    #unique示例
    方法一：
    create table department1(
    id int,
    name varchar(20) unique,
    comment varchar(100)
    );
    
    
    方法二：
    create table department2(
    id int,
    name varchar(20),
    comment varchar(100),
    unique(name)
    );
    
    
    mysql> insert into department1 values(1,'IT','技术');
    Query OK, 1 row affected (0.00 sec)
    mysql> insert into department1 values(1,'IT','技术');
    ERROR 1062 (23000): Duplicate entry 'IT' for key 'name'
        
    #not null 和unique的结合
    mysql> create table t1(id int not null unique);
    Query OK, 0 rows affected (0.02 sec)
    
    mysql> desc t1;
    +-------+---------+------+-----+---------+-------+
    | Field | Type    | Null | Key | Default | Extra |
    +-------+---------+------+-----+---------+-------+
    | id    | int(11) | NO   | PRI | NULL    |       |
    +-------+---------+------+-----+---------+-------+
    1 row in set (0.00 sec)
    
    #联合唯一
    create table service(
    id int primary key auto_increment,
    name varchar(20),
    host varchar(15) not null,
    port int not null,
    unique(host,port) #联合唯一
    );
    
    mysql> insert into service values
        -> (1,'nginx','192.168.0.10',80),
        -> (2,'haproxy','192.168.0.20',80),
        -> (3,'mysql','192.168.0.30',3306)
        -> ;
    Query OK, 3 rows affected (0.01 sec)
    Records: 3  Duplicates: 0  Warnings: 0
    
    mysql> insert into service(name,host,port) values('nginx','192.168.0.10',80);
    ERROR 1062 (23000): Duplicate entry '192.168.0.10-80' for key 'host'
        
    #3、primary key:主键为了保证表中的每一条数据的该字段都是表格中的唯一值。换言之，它是用来独一无二地确认一个表格中的每一行数据。
    #主键可以包含一个字段或多个字段。当主键包含多个栏位时，称为组合键 (Composite Key),也可以叫联合主键。
    #主键可以在建置新表格时设定 (运用 CREATE TABLE 语句)，或是以改变现有的表格架构方式设定 (运用 ALTER TABLE)。
    #主键必须唯一，主键值非空；可以是单一字段，也可以是多字段组合。
    
    #单字段主键
    #方法一：not null+unique
    create table department1(
    id int not null unique, #主键
    name varchar(20) not null unique,
    comment varchar(100)
    );
    
    mysql> desc department1;
    +---------+--------------+------+-----+---------+-------+
    | Field   | Type         | Null | Key | Default | Extra |
    +---------+--------------+------+-----+---------+-------+
    | id      | int(11)      | NO   | PRI | NULL    |       |
    | name    | varchar(20)  | NO   | UNI | NULL    |       |
    | comment | varchar(100) | YES  |     | NULL    |       |
    +---------+--------------+------+-----+---------+-------+
    rows in set (0.01 sec)
    
    #方法二：在某一个字段后用primary key
    create table department2(
    id int primary key, #主键
    name varchar(20),
    comment varchar(100)
    );
    
    mysql> desc department2;
    +---------+--------------+------+-----+---------+-------+
    | Field   | Type         | Null | Key | Default | Extra |
    +---------+--------------+------+-----+---------+-------+
    | id      | int(11)      | NO   | PRI | NULL    |       |
    | name    | varchar(20)  | YES  |     | NULL    |       |
    | comment | varchar(100) | YES  |     | NULL    |       |
    +---------+--------------+------+-----+---------+-------+
    rows in set (0.00 sec)
    
    #方法三：在所有字段后单独定义primary key
    create table department3(
    id int,
    name varchar(20),
    comment varchar(100),
    primary key(id); #创建主键并为其命名pk_name
    
    mysql> desc department3;
    +---------+--------------+------+-----+---------+-------+
    | Field   | Type         | Null | Key | Default | Extra |
    +---------+--------------+------+-----+---------+-------+
    | id      | int(11)      | NO   | PRI | NULL    |       |
    | name    | varchar(20)  | YES  |     | NULL    |       |
    | comment | varchar(100) | YES  |     | NULL    |       |
    +---------+--------------+------+-----+---------+-------+
    rows in set (0.01 sec)
    
    # 方法四：给已经建成的表添加主键约束
    mysql> create table department4(
        -> id int,
        -> name varchar(20),
        -> comment varchar(100));
    Query OK, 0 rows affected (0.01 sec)
    
    mysql> desc department4;
    +---------+--------------+------+-----+---------+-------+
    | Field   | Type         | Null | Key | Default | Extra |
    +---------+--------------+------+-----+---------+-------+
    | id      | int(11)      | YES  |     | NULL    |       |
    | name    | varchar(20)  | YES  |     | NULL    |       |
    | comment | varchar(100) | YES  |     | NULL    |       |
    +---------+--------------+------+-----+---------+-------+
    3 rows in set (0.01 sec)
    
    mysql> alter table department4 modify id int primary key;
    Query OK, 0 rows affected (0.02 sec)
    Records: 0  Duplicates: 0  Warnings: 0
    
    mysql> desc department4;
    +---------+--------------+------+-----+---------+-------+
    | Field   | Type         | Null | Key | Default | Extra |
    +---------+--------------+------+-----+---------+-------+
    | id      | int(11)      | NO   | PRI | NULL    |       |
    | name    | varchar(20)  | YES  |     | NULL    |       |
    | comment | varchar(100) | YES  |     | NULL    |       |
    +---------+--------------+------+-----+---------+-------+
    3 rows in set (0.01 sec)
        
    #多字段主键
    create table service(
    ip varchar(15),
    port char(5),
    service_name varchar(10) not null,
    primary key(ip,port)
    );
    
    
    mysql> desc service;
    +--------------+-------------+------+-----+---------+-------+
    | Field        | Type        | Null | Key | Default | Extra |
    +--------------+-------------+------+-----+---------+-------+
    | ip           | varchar(15) | NO   | PRI | NULL    |       |
    | port         | char(5)     | NO   | PRI | NULL    |       |
    | service_name | varchar(10) | NO   |     | NULL    |       |
    +--------------+-------------+------+-----+---------+-------+
    3 rows in set (0.00 sec)
    
    mysql> insert into service values
        -> ('172.16.45.10','3306','mysqld'),
        -> ('172.16.45.11','3306','mariadb')
        -> ;
    Query OK, 2 rows affected (0.00 sec)
    Records: 2  Duplicates: 0  Warnings: 0
    
    mysql> insert into service values ('172.16.45.10','3306','nginx');
    ERROR 1062 (23000): Duplicate entry '172.16.45.10-3306' for key 'PRIMARY'
        
    #4、auto_increment： 约束字段为自动增长，被约束的字段必须同时被key约束
    #不指定id，则自动增长
    create table student(
    id int primary key auto_increment,
    name varchar(20),
    sex enum('male','female') default 'male'
    );
    
    mysql> desc student;
    +-------+-----------------------+------+-----+---------+----------------+
    | Field | Type                  | Null | Key | Default | Extra          |
    +-------+-----------------------+------+-----+---------+----------------+
    | id    | int(11)               | NO   | PRI | NULL    | auto_increment |
    | name  | varchar(20)           | YES  |     | NULL    |                |
    | sex   | enum('male','female') | YES  |     | male    |                |
    +-------+-----------------------+------+-----+---------+----------------+
    mysql> insert into student(name) values
        -> ('egon'),
        -> ('alex')
        -> ;
    
    mysql> select * from student;
    +----+------+------+
    | id | name | sex  |
    +----+------+------+
    |  1 | egon | male |
    |  2 | alex | male |
    +----+------+------+
    
    
    #也可以指定id
    mysql> insert into student values(4,'asb','female');
    Query OK, 1 row affected (0.00 sec)
    
    mysql> insert into student values(7,'wsb','female');
    Query OK, 1 row affected (0.00 sec)
    
    mysql> select * from student;
    +----+------+--------+
    | id | name | sex    |
    +----+------+--------+
    |  1 | egon | male   |
    |  2 | alex | male   |
    |  4 | asb  | female |
    |  7 | wsb  | female |
    +----+------+--------+
    
    
    #对于自增的字段，在用delete删除后，再插入值，该字段仍按照删除前的位置继续增长
    mysql> delete from student;
    Query OK, 4 rows affected (0.00 sec)
    
    mysql> select * from student;
    Empty set (0.00 sec)
    
    mysql> insert into student(name) values('ysb');
    mysql> select * from student;
    +----+------+------+
    | id | name | sex  |
    +----+------+------+
    |  8 | ysb  | male |
    +----+------+------+
    
    #应该用truncate清空表，比起delete一条一条地删除记录，truncate是直接清空表，在删除大表时用它
    mysql> truncate student;
    Query OK, 0 rows affected (0.01 sec)
    
    mysql> insert into student(name) values('egon');
    Query OK, 1 row affected (0.01 sec)
    
    mysql> select * from student;
    +----+------+------+
    | id | name | sex  |
    +----+------+------+
    |  1 | egon | male |
    +----+------+------+
    row in set (0.00 sec)
        
    #5、外键 Foreign key
     '''
     多表 ：
    假设我们要描述所有公司的员工，需要描述的属性有这些 ： 工号 姓名 部门
    公司有3个部门，但是有1个亿的员工，那意味着部门这个字段需要重复存储，部门名字越长，越浪费
    解决方法： 我们完全可以定义一个部门表 然后让员工信息表关联该表，如何关联，即foreign key
     '''   
    #创造外键的条件
    mysql> create table departments (dep_id int(4),dep_name varchar(11));
    Query OK, 0 rows affected (0.02 sec)
    
    mysql> desc departments;
    +----------+-------------+------+-----+---------+-------+
    | Field    | Type        | Null | Key | Default | Extra |
    +----------+-------------+------+-----+---------+-------+
    | dep_id   | int(4)      | YES  |     | NULL    |       |
    | dep_name | varchar(11) | YES  |     | NULL    |       |
    +----------+-------------+------+-----+---------+-------+
    2 rows in set (0.00 sec)
    
    # 创建外键不成功
    mysql> create table staff_info (s_id int,name varchar(20),dep_id int,foreign key(dep_id) references departments(dep_id));
    ERROR 1215 (HY000): Cannot add foreign key 
    
    # 设置dep_id非空，仍然不能成功创建外键
    mysql> alter table departments modify dep_id int(4) not null;
    Query OK, 0 rows affected (0.02 sec)
    Records: 0  Duplicates: 0  Warnings: 0
    
    mysql> desc departments;
    +----------+-------------+------+-----+---------+-------+
    | Field    | Type        | Null | Key | Default | Extra |
    +----------+-------------+------+-----+---------+-------+
    | dep_id   | int(4)      | NO   |     | NULL    |       |
    | dep_name | varchar(11) | YES  |     | NULL    |       |
    +----------+-------------+------+-----+---------+-------+
    2 rows in set (0.00 sec)
    
    mysql> create table staff_info (s_id int,name varchar(20),dep_id int,foreign key(dep_id) references departments(dep_id));
    ERROR 1215 (HY000): Cannot add foreign key constraint
    
    # 当设置字段为unique唯一字段时，设置该字段为外键成功
    mysql> alter table departments modify dep_id int(4) unique;
    Query OK, 0 rows affected (0.01 sec)
    Records: 0  Duplicates: 0  Warnings: 0
    
    mysql> desc departments;                                                                                                       +----------+-------------+------+-----+---------+-------+
    | Field    | Type        | Null | Key | Default | Extra |
    +----------+-------------+------+-----+---------+-------+
    | dep_id   | int(4)      | YES  | UNI | NULL    |       |
    | dep_name | varchar(11) | YES  |     | NULL    |       |
    +----------+-------------+------+-----+---------+-------+
    2 rows in set (0.01 sec)
    
    mysql> create table staff_info (s_id int,name varchar(20),dep_id int,foreign key(dep_id) references departments(dep_id));
    Query OK, 0 rows affected (0.02 sec)
    #外键操作示例
    #表类型必须是innodb存储引擎，且被关联的字段，即references指定的另外一个表的字段，必须保证唯一
    create table department(
    id int primary key,
    name varchar(20) not null
    )engine=innodb;
    
    #dpt_id外键，关联父表（department主键id），同步更新，同步删除
    create table employee(
    id int primary key,
    name varchar(20) not null,
    dpt_id int,
    foreign key(dpt_id)
    references department(id)
    on delete cascade  # 级连删除
    on update cascade # 级连更新
    )engine=innodb;
    
    
    #先往父表department中插入记录
    insert into department values
    (1,'教质部'),
    (2,'技术部'),
    (3,'人力资源部');
    
    
    #再往子表employee中插入记录
    insert into employee values
    (1,'yuan',1),
    (2,'nezha',2),
    (3,'egon',2),
    (4,'alex',2),
    (5,'wusir',3),
    (6,'李沁洋',3),
    (7,'皮卡丘',3),
    (8,'程咬金',3),
    (9,'程咬银',3)
    ;
    
    
    #删父表department，子表employee中对应的记录跟着删
    mysql> delete from department where id=2;
    Query OK, 1 row affected (0.00 sec)
    
    mysql> select * from employee;
    +----+-----------+--------+
    | id | name      | dpt_id |
    +----+-----------+--------+
    |  1 | yuan      |      1 |
    |  5 | wusir     |      3 |
    |  6 | 李沁洋    |      3 |
    |  7 | 皮卡丘    |      3 |
    |  8 | 程咬金    |      3 |
    |  9 | 程咬银    |      3 |
    +----+-----------+--------+
    6 rows in set (0.00 sec)
    
    
    #更新父表department，子表employee中对应的记录跟着改
    mysql> update department set id=2 where id=3;
    Query OK, 1 row affected (0.01 sec)
    Rows matched: 1  Changed: 1  Warnings: 0
    
    mysql> select * from employee;
    +----+-----------+--------+
    | id | name      | dpt_id |
    +----+-----------+--------+
    |  1 | yuan      |      1 |
    |  5 | wusir     |      2 |
    |  6 | 李沁洋    |      2 |
    |  7 | 皮卡丘    |      2 |
    |  8 | 程咬金    |      2 |
    |  9 | 程咬银    |      2 |
    +----+-----------+--------+
    6 rows in set (0.00 sec)    
    
    #on delete(了解)
    '''
    . cascade方式
    在父表上update/delete记录时，同步update/delete掉子表的匹配记录 
    
       . set null方式
    在父表上update/delete记录时，将子表上匹配记录的列设为null
    要注意子表的外键列不能为not null  
    
       . No action方式
    如果子表中有匹配的记录,则不允许对父表对应候选键进行update/delete操作  
    
       . Restrict方式
    同no action, 都是立即检查外键约束
    
       . Set default方式
    父表有变更时,子表将外键列设置成一个默认的值 但Innodb不能识别    
    '''
    ```

    ```python
    #来自课堂
    # 约束某一个字段
    # 无符号的 int unsigned
    # 不能为空 not null
    # 默认值  default
    # 唯一约束 unique
        # 联合唯一 unique(字段1,字段2)
    # 自增 auto_increment
        # 只能对数字有效.自带非空约束
        # 至少是unique的约束之后才能使用auto_increment
    # 主键 primary key
        # 一张表只能有一个
        # 如果不指定主键,默认是第一个非空+唯一
        # 联合主键 primary key(字段1,字段2)
    # 外键 Foreign key
        # Foreign key(自己的字段) references 外表(外表字段)
        # 外表字段必须至少是"唯一"的
        
    create table t10(
      id int unsigned
    );
    
    create table t11(
      id int unsigned not null,
      name char(18) not null
    );
    
    create table t12(
      id int unsigned not null,
      name char(18) not null,
      male enum('male','female') not null default 'male's
    );
    
    # 不能重复  unique   值不能重复,但是null可以写入多个
    create table t13(
      id1 int unique,
      id2 int
    )
    
    # 联合唯一 unique
    create table t14(
        id int,
        server_name char(12),
        ip char(15),
        port char(5),
        unique(ip,port)
    );
    
    # 非空 + 唯一约束
    # 第一个被定义为非空+唯一的那一列会成为这张表的primary key
    # 一张表只能定义一个主键
    create table t15(
        id int not null unique,
        username char(18) not null unique
    );
    create table t16(
        username char(18) not null unique,
        id int not null unique
    );
    create table t17(
        username char(18) not null unique,
        id int primary key
    );
    
    # 联合主键
    create table t18(
        id int,
        server_name char(12),
        ip char(15) default '',
        port char(5) default '',
        primary key(ip,port)
    );
    
    create table t19(
        id int primary key,
        server_name char(12),
        ip char(15) not null,
        port char(5) not null,
        unique(ip,port)
    );
    
    # 自增
    create table t20(
        id int primary key auto_increment,
        name char(12)
    );
    insert into t20(name) values('alex');
    
    # 外键
    # 班级表
    create table class(
        cid int primary key auto_increment,
        cname char(12) not null,
        startd date
    )
    # 学生表
    create table stu(
        id int primary key auto_increment,
        name char(12) not null,
        gender enum('male','female') default 'male',
        class_id int,
        foreign key(class_id) references class(cid)
    )
    
    
    create table stu2(
        id int primary key auto_increment,
        name char(12) not null,
        gender enum('male','female') default 'male',
        class_id int,
        foreign key(class_id) references class(cid)
        on update cascade
        on delete cascade  # 尽量不用
    )
    ```

17. 修改表结构(一般比较少)

    ```python
    '''
    语法：
    1. 修改表名
          ALTER TABLE 表名 
                          RENAME 新表名;
    
    2. 增加字段
          ALTER TABLE 表名
                          ADD 字段名  数据类型 [完整性约束条件…],
                          ADD 字段名  数据类型 [完整性约束条件…];
                                
    3. 删除字段
          ALTER TABLE 表名 
                          DROP 字段名;
    
    4. 修改字段
          ALTER TABLE 表名 
                          MODIFY  字段名 数据类型 [完整性约束条件…];
          ALTER TABLE 表名 
                          CHANGE 旧字段名 新字段名 旧数据类型 [完整性约束条件…];
          ALTER TABLE 表名 
                          CHANGE 旧字段名 新字段名 新数据类型 [完整性约束条件…];
    
    5.修改字段排列顺序/在增加的时候指定字段位置
        ALTER TABLE 表名
                         ADD 字段名  数据类型 [完整性约束条件…]  FIRST;
        ALTER TABLE 表名
                         ADD 字段名  数据类型 [完整性约束条件…]  AFTER 字段名;
        ALTER TABLE 表名
                         CHANGE 字段名  旧字段名 新字段名 新数据类型 [完整性约束条件…]  FIRST;
        ALTER TABLE 表名
                         MODIFY 字段名  数据类型 [完整性约束条件…]  AFTER 字段名;
    '''
    #alter操作非空和唯一（了解）
    create table t(id int unique,name char(10) not null);
    #去掉null约束
    alter table t modify name char(10) null;
    # 添加null约束
    alter table t modify name char(10) not null;
    # 去掉unique约束
    alter table t drop index id;
    # 添加unique约束
    alter table t modify id int unique;
    # 添加联合唯一
    alter table t add unique index(aa,bb);
    
    #alter操作主键（了解）
    '''
    1、首先创建一个数据表table_test：
    create table table_test(
    `id` varchar(100) NOT NULL,
    `name` varchar(100) NOT NULL,
    PRIMARY KEY (`name`)
    ); 
    2、如果发现主键设置错了,应该是id是主键,但如今表里已经有好多数据了,不能删除表再重建了，仅仅能在这基础上改动表结构。
    先删除主键
    alter table table_test drop primary key;
    然后再增加主键
    alter table table_test add primary key(id);
    注:在增加主键之前,必须先把反复的id删除掉。
    '''
    #为表添加外键（了解）
    '''
    创建press表
    CREATE TABLE `press` (
      `id` int(11) NOT NULL,
      `name` char(10) DEFAULT NULL,
      PRIMARY KEY (`id`)
    ) ；
    
    创建book表
    CREATE TABLE `book` (
      `id` int(11) DEFAULT NULL,
      `bk_name` char(12) DEFAULT NULL,
      `press_id` int(11) NOT NULL,
      KEY `press_id` (`press_id`)
    ) ；
    
    为book表添加外键
    alter table book add constraint fk_id foreign key(press_id) references press(id);
    
    删除外键
    alter table book drop foreign key fk_id;
    '''
    
    #示例
    mysql> desc staff_info;
    +-------+-----------------------+------+-----+---------+-------+
    | Field | Type                  | Null | Key | Default | Extra |
    +-------+-----------------------+------+-----+---------+-------+
    | id    | int(11)               | YES  |     | NULL    |       |
    | name  | varchar(50)           | YES  |     | NULL    |       |
    | age   | int(3)                | YES  |     | NULL    |       |
    | sex   | enum('male','female') | YES  |     | NULL    |       |
    | phone | bigint(11)            | YES  |     | NULL    |       |
    | job   | varchar(11)           | YES  |     | NULL    |       |
    +-------+-----------------------+------+-----+---------+-------+
    6 rows in set (0.00 sec)
    
    # 表重命名
    mysql> alter table staff_info rename staff;
    Query OK, 0 rows affected (0.00 sec)
    
    mysql> desc staff;
    +-------+-----------------------+------+-----+---------+-------+
    | Field | Type                  | Null | Key | Default | Extra |
    +-------+-----------------------+------+-----+---------+-------+
    | id    | int(11)               | YES  |     | NULL    |       |
    | name  | varchar(50)           | YES  |     | NULL    |       |
    | age   | int(3)                | YES  |     | NULL    |       |
    | sex   | enum('male','female') | YES  |     | NULL    |       |
    | phone | bigint(11)            | YES  |     | NULL    |       |
    | job   | varchar(11)           | YES  |     | NULL    |       |
    +-------+-----------------------+------+-----+---------+-------+
    6 rows in set (0.00 sec)
    
    # 删除sex列
    mysql> alter table staff drop sex;
    Query OK, 0 rows affected (0.02 sec)
    Records: 0  Duplicates: 0  Warnings: 0
    
    mysql> desc staff;
    +-------+-------------+------+-----+---------+-------+
    | Field | Type        | Null | Key | Default | Extra |
    +-------+-------------+------+-----+---------+-------+
    | id    | int(11)     | YES  |     | NULL    |       |
    | name  | varchar(50) | YES  |     | NULL    |       |
    | age   | int(3)      | YES  |     | NULL    |       |
    | phone | bigint(11)  | YES  |     | NULL    |       |
    | job   | varchar(11) | YES  |     | NULL    |       |
    +-------+-------------+------+-----+---------+-------+
    5 rows in set (0.01 sec)
    
    # 添加列
    mysql> alter table staff add sex enum('male','female');
    Query OK, 0 rows affected (0.03 sec)
    Records: 0  Duplicates: 0  Warnings: 0
    
    # 修改id的宽度
    mysql> alter table staff modify id int(4);
    Query OK, 0 rows affected (0.02 sec)
    Records: 0  Duplicates: 0  Warnings: 0
    
    mysql> desc staff;
    +-------+-----------------------+------+-----+---------+-------+
    | Field | Type                  | Null | Key | Default | Extra |
    +-------+-----------------------+------+-----+---------+-------+
    | id    | int(4)                | YES  |     | NULL    |       |
    | name  | varchar(50)           | YES  |     | NULL    |       |
    | age   | int(3)                | YES  |     | NULL    |       |
    | phone | bigint(11)            | YES  |     | NULL    |       |
    | job   | varchar(11)           | YES  |     | NULL    |       |
    | sex   | enum('male','female') | YES  |     | NULL    |       |
    +-------+-----------------------+------+-----+---------+-------+
    6 rows in set (0.01 sec)
    
    # 修改name列的字段名
    mysql> alter table staff change name sname varchar(20);
    Query OK, 4 rows affected (0.03 sec)
    Records: 4  Duplicates: 0  Warnings: 0
    
    mysql> desc staff;
    +-------+-----------------------+------+-----+---------+-------+
    | Field | Type                  | Null | Key | Default | Extra |
    +-------+-----------------------+------+-----+---------+-------+
    | id    | int(4)                | YES  |     | NULL    |       |
    | sname | varchar(20)           | YES  |     | NULL    |       |
    | age   | int(3)                | YES  |     | NULL    |       |
    | phone | bigint(11)            | YES  |     | NULL    |       |
    | job   | varchar(11)           | YES  |     | NULL    |       |
    | sex   | enum('male','female') | YES  |     | NULL    |       |
    +-------+-----------------------+------+-----+---------+-------+
    6 rows in set (0.00 sec)
    
    # 修改sex列的位置
    mysql> alter table staff modify sex enum('male','female') after sname;
    Query OK, 0 rows affected (0.02 sec)
    Records: 0  Duplicates: 0  Warnings: 0
    
    mysql> desc staff;
    +-------+-----------------------+------+-----+---------+-------+
    | Field | Type                  | Null | Key | Default | Extra |
    +-------+-----------------------+------+-----+---------+-------+
    | id    | int(4)                | YES  |     | NULL    |       |
    | sname | varchar(20)           | YES  |     | NULL    |       |
    | sex   | enum('male','female') | YES  |     | NULL    |       |
    | age   | int(3)                | YES  |     | NULL    |       |
    | phone | bigint(11)            | YES  |     | NULL    |       |
    | job   | varchar(11)           | YES  |     | NULL    |       |
    +-------+-----------------------+------+-----+---------+-------+
    6 rows in set (0.00 sec)
    
    # 创建自增id主键
    mysql> alter table staff modify id int(4) primary key auto_increment;
    Query OK, 4 rows affected (0.02 sec)
    Records: 4  Duplicates: 0  Warnings: 0
    
    mysql> desc staff;
    +-------+-----------------------+------+-----+---------+----------------+
    | Field | Type                  | Null | Key | Default | Extra          |
    +-------+-----------------------+------+-----+---------+----------------+
    | id    | int(4)                | NO   | PRI | NULL    | auto_increment |
    | sname | varchar(20)           | YES  |     | NULL    |                |
    | sex   | enum('male','female') | YES  |     | NULL    |                |
    | age   | int(3)                | YES  |     | NULL    |                |
    | phone | bigint(11)            | YES  |     | NULL    |                |
    | job   | varchar(11)           | YES  |     | NULL    |                |
    +-------+-----------------------+------+-----+---------+----------------+
    6 rows in set (0.00 sec)
    
    # 删除主键，可以看到删除一个自增主键会报错
    mysql> alter table staff drop primary key;
    ERROR 1075 (42000): Incorrect table definition; there can be only one auto column and it must be defined as a key
    
    # 需要先去掉主键的自增约束，然后再删除主键约束
    mysql> alter table staff modify id int(11);
    Query OK, 4 rows affected (0.02 sec)
    Records: 4  Duplicates: 0  Warnings: 0
    
    mysql> desc staff;
    +-------+-----------------------+------+-----+---------+-------+
    | Field | Type                  | Null | Key | Default | Extra |
    +-------+-----------------------+------+-----+---------+-------+
    | id    | int(11)               | NO   | PRI | 0       |       |
    | sname | varchar(20)           | YES  |     | NULL    |       |
    | sex   | enum('male','female') | YES  |     | NULL    |       |
    | age   | int(3)                | YES  |     | NULL    |       |
    | phone | bigint(11)            | YES  |     | NULL    |       |
    | job   | varchar(11)           | YES  |     | NULL    |       |
    +-------+-----------------------+------+-----+---------+-------+
    6 rows in set (0.01 sec)
    
    mysql> alter table staff drop primary key;
    Query OK, 4 rows affected (0.06 sec)
    Records: 4  Duplicates: 0  Warnings: 0
    
    # 添加联合主键
    mysql> alter table staff add primary key (sname,age);
    Query OK, 0 rows affected (0.02 sec)
    Records: 0  Duplicates: 0  Warnings: 0
    
    # 删除主键
    mysql> alter table staff drop primary key;
    Query OK, 4 rows affected (0.02 sec)
    Records: 4  Duplicates: 0  Warnings: 0
    
    # 创建主键id
    mysql> alter table staff add primary key (id);
    Query OK, 0 rows affected (0.02 sec)
    Records: 0  Duplicates: 0  Warnings: 0
    
    mysql> desc staff;
    +-------+-----------------------+------+-----+---------+-------+
    | Field | Type                  | Null | Key | Default | Extra |
    +-------+-----------------------+------+-----+---------+-------+
    | id    | int(11)               | NO   | PRI | 0       |       |
    | sname | varchar(20)           | NO   |     |         |       |
    | sex   | enum('male','female') | YES  |     | NULL    |       |
    | age   | int(3)                | NO   |     | 0       |       |
    | phone | bigint(11)            | YES  |     | NULL    |       |
    | job   | varchar(11)           | YES  |     | NULL    |       |
    +-------+-----------------------+------+-----+---------+-------+
    6 rows in set (0.00 sec)
    
    # 为主键添加自增属性
    mysql> alter table staff modify id int(4) auto_increment;
    Query OK, 4 rows affected (0.02 sec)
    Records: 4  Duplicates: 0  Warnings: 0
    
    mysql> desc staff;
    +-------+-----------------------+------+-----+---------+----------------+
    | Field | Type                  | Null | Key | Default | Extra          |
    +-------+-----------------------+------+-----+---------+----------------+
    | id    | int(4)                | NO   | PRI | NULL    | auto_increment |
    | sname | varchar(20)           | NO   |     |         |                |
    | sex   | enum('male','female') | YES  |     | NULL    |                |
    | age   | int(3)                | NO   |     | 0       |                |
    | phone | bigint(11)            | YES  |     | NULL    |                |
    | job   | varchar(11)           | YES  |     | NULL    |                |
    +-------+-----------------------+------+-----+---------+----------------+
    6 rows in set (0.00 sec)
        
    ```

18. 表与表之间的关系

    ```python
    #如何找出两张表之间的关系 
    '''
    分析步骤：
    #1、先站在左表的角度去找
    是否左表的多条记录可以对应右表的一条记录，如果是，则证明左表的一个字段foreign key 右表一个字段（通常是id）
    
    #2、再站在右表的角度去找
    是否右表的多条记录可以对应左表的一条记录，如果是，则证明右表的一个字段foreign key 左表一个字段（通常是id）
    
    #3、总结：
    #多对一：
    如果只有步骤1成立，则是左表多对一右表
    如果只有步骤2成立，则是右表多对一左表
    
    #多对多
    如果步骤1和2同时成立，则证明这两张表时一个双向的多对一，即多对多,需要定义一个这两张表的关系表来专门存放二者的关系
    
    #一对一:
    如果1和2都不成立，而是左表的一条记录唯一对应右表的一条记录，反之亦然。这种情况很简单，就是在左表foreign key右表的基础上，将左表的外键字段设置成unique即可
    '''
    
    #建立表之间的关系
    #一对多或称为多对一
    '''
    三张表：出版社，作者信息，书
    一对多（或多对一）：一个出版社可以出版多本书
    关联方式：foreign key
    '''
    #sql示例
    create table press(
    id int primary key auto_increment,
    name varchar(20)
    );
    
    create table book(
    id int primary key auto_increment,
    name varchar(20),
    press_id int not null,
    foreign key(press_id) references press(id)
    on delete cascade
    on update cascade
    );
    
    
    insert into press(name) values
    ('北京工业地雷出版社'),
    ('人民音乐不好听出版社'),
    ('知识产权没有用出版社')
    ;
    
    insert into book(name,press_id) values
    ('九阳神功',1),
    ('九阴真经',2),
    ('九阴白骨爪',2),
    ('独孤九剑',3),
    ('降龙十巴掌',2),
    ('葵花宝典',3)
    ;
    
    ##多对多
    '''
    三张表：出版社，作者信息，书
    多对多：一个作者可以写多本书，一本书也可以有多个作者，双向的一对多，即多对多　　
    关联方式：foreign key+一张新的表
    '''
    #sql示例
    create table author(
    id int primary key auto_increment,
    name varchar(20)
    );
    
    
    #这张表就存放作者表与书表的关系，即查询二者的关系查这表就可以了
    create table author2book(
    id int not null unique auto_increment,
    author_id int not null,
    book_id int not null,
    constraint fk_author foreign key(author_id) references author(id)
    on delete cascade
    on update cascade,
    constraint fk_book foreign key(book_id) references book(id)
    on delete cascade
    on update cascade,
    primary key(author_id,book_id)
    );
    
    
    #插入四个作者，id依次排开
    insert into author(name) values('egon'),('alex'),('yuanhao'),('wpq');
    
    #每个作者与自己的代表作如下
    egon: 
    九阳神功
    九阴真经
    九阴白骨爪
    独孤九剑
    降龙十巴掌
    葵花宝典
    alex: 
    九阳神功
    葵花宝典
    yuanhao:
    独孤九剑
    降龙十巴掌
    葵花宝典
    wpq:
    九阳神功
    
    
    insert into author2book(author_id,book_id) values
    (1,1),
    (1,2),
    (1,3),
    (1,4),
    (1,5),
    (1,6),
    (2,1),
    (2,6),
    (3,4),
    (3,5),
    (3,6),
    (4,1)
    ;
    ##一对一
    '''
    两张表：学生表和客户表
    一对一：一个学生是一个客户
    关联方式：foreign key+unique
    '''
    #sql示例
    create table customer(
        -> id int primary key auto_increment,
        -> name varchar(20) not null,
        -> qq varchar(10) not null,
        -> phone char(16) not null
        -> );
    
    create table student(
        -> id int primary key auto_increment,
        -> class_name varchar(20) not null,
        -> customer_id int unique, #该字段一定要是唯一的
        -> foreign key(customer_id) references customer(id) #外键的字段一定要保证unique
        -> on delete cascade
        -> on update cascade
        -> );
    
    #增加客户
    mysql> insert into customer(name,qq,phone) values
        -> ('韩蕾','31811231',13811341220),
        -> ('杨澜','123123123',15213146809),
        -> ('翁惠天','283818181',1867141331),
        -> ('杨宗河','283818181',1851143312),
        -> ('袁承明','888818181',1861243314),
        -> ('袁清','112312312',18811431230)
    
    mysql> #增加学生
    mysql> insert into student(class_name,customer_id) values
        -> ('脱产1班',3),
        -> ('周末1期',4),
        -> ('周末1期',5)
        -> ;
    ```

    

