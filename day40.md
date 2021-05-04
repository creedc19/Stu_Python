## 一、内容回顾

1. 增

   insert into 表 values (值)

   insert into 表(字段,字段2) values (值,值2)

   insert into 表(字段,字段2) select 字段1,字段2  from 表2

2. 删

   delete from 表 where 条件;

   truncate table 表名;

3. 改

   update 表 set 字段=值 where 条件;

4. 查

   select 字段 from 表

   ​	where 条件  根据条件筛选符合条件的行

   ​	group by 分组

   ​	having 过滤条件 根据分组之后的内容进行组的过滤

   ​	order by 排序

   ​	limit m,n 取从m+1开始的前n条

5. where条件中不能用select字段的重命名；order by 或者having可以使用select字段的重命名，主要是因为order by 在select语句之后才执行，having经过了mysql的特殊处理,使得它能够感知到select语句中的重命名。

6. 拓展：在执行select语句的时候,实际上是通过where,group by,having这几个语句锁定对应的行，然后循环每一行执行select语句。

## 二、今日内容

1. 连表查询

   ​	如果一个问题既可以使用连表查询解决，也可以使用子查询解决，推荐使用连表查询 , 因为效率高（重点）

   1. 建表和数据准备：

      ```python
      #建表
      create table department(
      id int,
      name varchar(20) 
      );
      
      create table employee(
      id int primary key auto_increment,
      name varchar(20),
      sex enum('male','female') not null default 'male',
      age int,
      dep_id int
      );
      
      #插入数据
      insert into department values
      (200,'技术'),
      (201,'人力资源'),
      (202,'销售'),
      (203,'运营');
      
      insert into employee(name,sex,age,dep_id) values
      ('egon','male',18,200),
      ('alex','female',48,201),
      ('wupeiqi','male',38,201),
      ('yuanhao','female',28,202),
      ('liwenzhou','male',18,200),
      ('jingliyang','female',18,204)
      ;
      
      
      #查看表结构和数据
      mysql> desc department;
      +-------+-------------+------+-----+---------+-------+
      | Field | Type | Null | Key | Default | Extra |
      +-------+-------------+------+-----+---------+-------+
      | id | int(11) | YES | | NULL | |
      | name | varchar(20) | YES | | NULL | |
      +-------+-------------+------+-----+---------+-------+
      
      mysql> desc employee;
      +--------+-----------------------+------+-----+---------+----------------+
      | Field | Type | Null | Key | Default | Extra |
      +--------+-----------------------+------+-----+---------+----------------+
      | id | int(11) | NO | PRI | NULL | auto_increment |
      | name | varchar(20) | YES | | NULL | |
      | sex | enum('male','female') | NO | | male | |
      | age | int(11) | YES | | NULL | |
      | dep_id | int(11) | YES | | NULL | |
      +--------+-----------------------+------+-----+---------+----------------+
      
      mysql> select * from department;
      +------+--------------+
      | id | name |
      +------+--------------+
      | 200 | 技术 |
      | 201 | 人力资源 |
      | 202 | 销售 |
      | 203 | 运营 |
      +------+--------------+
      
      mysql> select * from employee;
      +----+------------+--------+------+--------+
      | id | name | sex | age | dep_id |
      +----+------------+--------+------+--------+
      | 1 | egon | male | 18 | 200 |
      | 2 | alex | female | 48 | 201 |
      | 3 | wupeiqi | male | 38 | 201 |
      | 4 | yuanhao | female | 28 | 202 |
      | 5 | liwenzhou | male | 18 | 200 |
      | 6 | jingliyang | female | 18 | 204 |
      +----+------------+--------+------+--------+
      
      表department与employee
      ```

   2. 交叉连接：不适用任何匹配条件。生成笛卡尔积

      ```python
      mysql> select * from employee,department;
      +----+------------+--------+------+--------+------+--------------+
      | id | name       | sex    | age  | dep_id | id   | name         |
      +----+------------+--------+------+--------+------+--------------+
      |  1 | egon       | male   |   18 |    200 |  200 | 技术         |
      |  1 | egon       | male   |   18 |    200 |  201 | 人力资源     |
      |  1 | egon       | male   |   18 |    200 |  202 | 销售         |
      |  1 | egon       | male   |   18 |    200 |  203 | 运营         |
      |  2 | alex       | female |   48 |    201 |  200 | 技术         |
      |  2 | alex       | female |   48 |    201 |  201 | 人力资源     |
      |  2 | alex       | female |   48 |    201 |  202 | 销售         |
      |  2 | alex       | female |   48 |    201 |  203 | 运营         |
      |  3 | wupeiqi    | male   |   38 |    201 |  200 | 技术         |
      |  3 | wupeiqi    | male   |   38 |    201 |  201 | 人力资源     |
      |  3 | wupeiqi    | male   |   38 |    201 |  202 | 销售         |
      |  3 | wupeiqi    | male   |   38 |    201 |  203 | 运营         |
      |  4 | yuanhao    | female |   28 |    202 |  200 | 技术         |
      |  4 | yuanhao    | female |   28 |    202 |  201 | 人力资源     |
      |  4 | yuanhao    | female |   28 |    202 |  202 | 销售         |
      |  4 | yuanhao    | female |   28 |    202 |  203 | 运营         |
      |  5 | liwenzhou  | male   |   18 |    200 |  200 | 技术         |
      |  5 | liwenzhou  | male   |   18 |    200 |  201 | 人力资源     |
      |  5 | liwenzhou  | male   |   18 |    200 |  202 | 销售         |
      |  5 | liwenzhou  | male   |   18 |    200 |  203 | 运营         |
      |  6 | jingliyang | female |   18 |    204 |  200 | 技术         |
      |  6 | jingliyang | female |   18 |    204 |  201 | 人力资源     |
      |  6 | jingliyang | female |   18 |    204 |  202 | 销售         |
      |  6 | jingliyang | female |   18 |    204 |  203 | 运营         |
      +----+------------+--------+------+--------+------+--------------+
      ```

   3. 内连接：只连接匹配的行

      ```python
      #找两张表共有的部分，相当于利用条件从笛卡尔积结果中筛选出了正确的结果
      #department没有204这个部门，因而employee表中关于204这条员工信息没有匹配出来
      mysql> select employee.id,employee.name,employee.age,employee.sex,department.name from employee inner join department on employee.dep_id=department.id; 
      +----+-----------+------+--------+--------------+
      | id | name      | age  | sex    | name         |
      +----+-----------+------+--------+--------------+
      |  1 | egon      |   18 | male   | 技术         |
      |  2 | alex      |   48 | female | 人力资源     |
      |  3 | wupeiqi   |   38 | male   | 人力资源     |
      |  4 | yuanhao   |   28 | female | 销售         |
      |  5 | liwenzhou |   18 | male   | 技术         |
      +----+-----------+------+--------+--------------+
      
      #上述sql等同于
      mysql> select employee.id,employee.name,employee.age,employee.sex,department.name from employee,department where employee.dep_id=department.id;
      ```

   4.  外链接之左连接：优先显示左表全部记录

      ```python
      #以左表为准，即找出所有员工信息，当然包括没有部门的员工
      #本质就是：在内连接的基础上增加左边有右边没有的结果
      mysql> select employee.id,employee.name,department.name as depart_name from employee left join department on employee.dep_id=department.id;
      +----+------------+--------------+
      | id | name       | depart_name  |
      +----+------------+--------------+
      |  1 | egon       | 技术         |
      |  5 | liwenzhou  | 技术         |
      |  2 | alex       | 人力资源     |
      |  3 | wupeiqi    | 人力资源     |
      |  4 | yuanhao    | 销售         |
      |  6 | jingliyang | NULL         |
      +----+------------+--------------+
      ```

   5. 外链接之右连接：优先显示右表全部记录

      ```python
      #以右表为准，即找出所有部门信息，包括没有员工的部门
      #本质就是：在内连接的基础上增加右边有左边没有的结果
      mysql> select employee.id,employee.name,department.name as depart_name from employee right join department on employee.dep_id=department.id;
      +------+-----------+--------------+
      | id   | name      | depart_name  |
      +------+-----------+--------------+
      |    1 | egon      | 技术         |
      |    2 | alex      | 人力资源     |
      |    3 | wupeiqi   | 人力资源     |
      |    4 | yuanhao   | 销售         |
      |    5 | liwenzhou | 技术         |
      | NULL | NULL      | 运营         |
      +------+-----------+--------------+
      ```

   6. 全外连接：显示左右两个表全部记录

      ```python
      全外连接：在内连接的基础上增加左边有右边没有的和右边有左边没有的结果
      #注意：mysql不支持全外连接 full JOIN
      #强调：mysql可以使用此种方式间接实现全外连接
      select * from employee left join department on employee.dep_id = department.id
      union
      select * from employee right join department on employee.dep_id = department.id
      ;
      #查看结果
      +------+------------+--------+------+--------+------+--------------+
      | id   | name       | sex    | age  | dep_id | id   | name         |
      +------+------------+--------+------+--------+------+--------------+
      |    1 | egon       | male   |   18 |    200 |  200 | 技术         |
      |    5 | liwenzhou  | male   |   18 |    200 |  200 | 技术         |
      |    2 | alex       | female |   48 |    201 |  201 | 人力资源     |
      |    3 | wupeiqi    | male   |   38 |    201 |  201 | 人力资源     |
      |    4 | yuanhao    | female |   28 |    202 |  202 | 销售         |
      |    6 | jingliyang | female |   18 |    204 | NULL | NULL        |
      | NULL | NULL       | NULL   | NULL |   NULL |  203 | 运营         |
      +------+------------+--------+------+--------+------+--------------+
      
      #注意 union与union all的区别：union会去掉相同的纪录
      ```

   7. 符合条件连接查询

      ```Python
      #示例1：以内连接的方式查询employee和department表，并且employee表中的age字段值必须大于25,即找出年龄大于25岁的员工以及员工所在的部门
      select employee.name,department.name from employee inner join department
          on employee.dep_id = department.id
          where age > 25;
      
      #示例2：以内连接的方式查询employee和department表，并且以age字段的升序方式显示
      select employee.id,employee.name,employee.age,department.name from employee,department
          where employee.dep_id = department.id
          and age > 25
          order by age asc;
      ```

      ```python
      #来自课堂讲解
      # 所谓连表
          # 总是在连接的时候创建一张大表,里面存放的是两张表的笛卡尔积
          # 再根据条件进行筛选就可以了
      
      # 表与表之间的连接方式
          # 内连接 inner join ... on ...
              # select * from 表1,表2 where 条件;(了解)
              # select * from 表1 inner join 表2  on 条件
              select * from department inner join employee on department.id = employee.dep_id;
              select * from department as t1 inner join employee as t2 on t1.id = t2.dep_id;
          # 外连接
              # 左外连接 left join ... on ...
                  # select * from 表1 left join 表2 on 条件
                  select * from department as t1 left join employee as t2 on t1.id = t2.dep_id;
              # 右外连接 right join ... on ...
                  # select * from 表1 right join 表2 on 条件
                  select * from department as t1 right join employee as t2 on t1.id = t2.dep_id
              # 全外连接 
                  select * from department as t1 left join employee as t2 on t1.id = t2.dep_id
                  union
                  select * from department as t1 right join employee as t2 on t1.id = t2.dep_id;
      
      # 1.找到技术部的所有人的姓名
      # select * from department d inner join employee e on e.dep_id = d.id;
      # select e.name from department d inner join employee e on e.dep_id = d.id where d.name='技术';
      
      # 2.找到人力资源部的年龄大于40岁的人的姓名
      # select * from department d inner join employee e on e.dep_id = d.id
      # select * from department d inner join employee e on e.dep_id = d.id where d.name='人力资源' and age>40;
      
      # 3.找出年龄大于25岁的员工以及员工所在的部门
      # select * from department d inner join employee e on e.dep_id = d.id;
      # select e.name,d.name from department d inner join employee e on e.dep_id = d.id where age>25;
      
      # 4.以内连接的方式查询employee和department表，并且以age字段的升序方式显示
      # select * from department d inner join employee e on e.dep_id = d.id order by age;
      
      # 5.求每一个部门有多少人
      # select d.name,count(e.id) from department d left join employee e on e.dep_id = d.id group by d.name;
      # 且按照人数从高到低排序
      # select d.name,count(e.id) c from department d left join employee e on e.dep_id = d.id group by d.name order by c desc;
      
      # 所谓连表就是把两张表连接在一起之后 就变成一张大表  从from开始一直到on条件结束就看做一张表
      # 之后 where 条件 group by 分组 order by limit 都正常的使用就可以了
      ```

2. 子查询

   + 子查询是将一个查询语句嵌套在另一个查询语句中。
   + 内层查询语句的查询结果，可以为外层查询语句提供查询条件。
   + 子查询中可以包含：IN、NOT IN、ANY、ALL、EXISTS 和 NOT EXISTS等关键字
   + 还可以包含比较运算符：= 、 !=、> 、<等。重点掌握包含 in、 not in 、比较运算符的 子查询。

   1. 带IN关键字的子查询

      ```python
      #查询平均年龄在25岁以上的部门名
      select id,name from department
          where id in 
              (select dep_id from employee group by dep_id having avg(age) > 25);
      
      #查看技术部员工姓名
      	#先查询技术部的部门id
          select id from department where name = '技术';
          #再根据这个部门id找到对应的员工名
          select name from employee where dep_id in (select id from department where name = '技术'); #或者这个，两者效果一样 select name from employee where dep_id =(select id from department where name = '技术');
      
      # 查看不足1人的部门名
          # 先把所有人的部门id查出来
          select distinct dep_id from employee;
          # 然后查询部门表,把不在所有人部门id这个范围的dep_id找出来
          select name from department where id not in (select distinct dep_id from employee);
      
      ```

   2. 带比较运算符的子查询

      ```
      #比较运算符：=、!=、>、>=、<、<=、<>
      #查询大于所有人平均年龄的员工名与年龄
      	#求平均年龄
          select avg(age) from employee;
         
          select name,age from employee where age >(select avg(age) from employee);
      
      mysql> select name,age from emp where age > (select avg(age) from emp);
      +---------+------+
      | name    | age  |
      +---------+------+
      | alex    | 48   |
      | wupeiqi | 38   |
      +---------+------+
      2 rows in set (0.00 sec)
      
      
      #查询大于部门内平均年龄的员工名、年龄
      select dep_id,avg(age) from employee group by dep_id;
      
      select name,age from employee as t1 inner join (select dep_id,avg(age) avg_age from employee group by dep_id) as t2 on t1.dep_id = t2.dep_id where age>avg_age;
      
      #或者
      select t1.name,t1.age from emp t1
      inner join 
      (select dep_id,avg(age) avg_age from emp group by dep_id) t2
      on t1.dep_id = t2.dep_id
      where t1.age > t2.avg_age; 
      ```

   3. 带EXISTS关键字的子查询（了解），EXISTS关字键字表示存在。在使用EXISTS关键字时，内层查询语句不返回查询的记录。而是返回一个真假值。True或False，当返回True时，外层查询语句将进行查询；当返回值为False时，外层查询语句不进行查询。

      ```python
      #department表中存在dept_id=203，Ture
      mysql> select * from employee
          ->     where exists
          ->         (select id from department where id=200);
      +----+------------+--------+------+--------+
      | id | name       | sex    | age  | dep_id |
      +----+------------+--------+------+--------+
      |  1 | egon       | male   |   18 |    200 |
      |  2 | alex       | female |   48 |    201 |
      |  3 | wupeiqi    | male   |   38 |    201 |
      |  4 | yuanhao    | female |   28 |    202 |
      |  5 | liwenzhou  | male   |   18 |    200 |
      |  6 | jingliyang | female |   18 |    204 |
      +----+------------+--------+------+--------+
      
      #department表中存在dept_id=205，False
      mysql> select * from employee
          ->     where exists
          ->         (select id from department where id=204);
      Empty set (0.00 sec)
      ```

      练习：查询每个部门最新入职的那位员工？

      表与数据准备：

      ```python
      company.employee
          员工id      id                  int             
          姓名        emp_name            varchar
          性别        sex                 enum
          年龄        age                 int
          入职日期     hire_date           date
          岗位        post                varchar
          职位描述     post_comment        varchar
          薪水        salary              double
          办公室       office              int
          部门编号     depart_id           int
      
      
      
      #创建表
      create table employee(
      id int not null unique auto_increment,
      name varchar(20) not null,
      sex enum('male','female') not null default 'male', #大部分是男的
      age int(3) unsigned not null default 28,
      hire_date date not null,
      post varchar(50),
      post_comment varchar(100),
      salary double(15,2),
      office int, #一个部门一个屋子
      depart_id int
      );
      
      
      #查看表结构
      mysql> desc employee;
      +--------------+-----------------------+------+-----+---------+----------------+
      | Field        | Type                  | Null | Key | Default | Extra          |
      +--------------+-----------------------+------+-----+---------+----------------+
      | id           | int(11)               | NO   | PRI | NULL    | auto_increment |
      | name         | varchar(20)           | NO   |     | NULL    |                |
      | sex          | enum('male','female') | NO   |     | male    |                |
      | age          | int(3) unsigned       | NO   |     | 28      |                |
      | hire_date    | date                  | NO   |     | NULL    |                |
      | post         | varchar(50)           | YES  |     | NULL    |                |
      | post_comment | varchar(100)          | YES  |     | NULL    |                |
      | salary       | double(15,2)          | YES  |     | NULL    |                |
      | office       | int(11)               | YES  |     | NULL    |                |
      | depart_id    | int(11)               | YES  |     | NULL    |                |
      +--------------+-----------------------+------+-----+---------+----------------+
      
      #插入记录
      #三个部门：教学，销售，运营
      insert into employee(name,sex,age,hire_date,post,salary,office,depart_id) values
      ('egon','male',18,'20170301','老男孩驻沙河办事处外交大使',7300.33,401,1), #以下是教学部
      ('alex','male',78,'20150302','teacher',1000000.31,401,1),
      ('wupeiqi','male',81,'20130305','teacher',8300,401,1),
      ('yuanhao','male',73,'20140701','teacher',3500,401,1),
      ('liwenzhou','male',28,'20121101','teacher',2100,401,1),
      ('jingliyang','female',18,'20110211','teacher',9000,401,1),
      ('jinxin','male',18,'19000301','teacher',30000,401,1),
      ('成龙','male',48,'20101111','teacher',10000,401,1),
      
      ('歪歪','female',48,'20150311','sale',3000.13,402,2),#以下是销售部门
      ('丫丫','female',38,'20101101','sale',2000.35,402,2),
      ('丁丁','female',18,'20110312','sale',1000.37,402,2),
      ('星星','female',18,'20160513','sale',3000.29,402,2),
      ('格格','female',28,'20170127','sale',4000.33,402,2),
      
      ('张野','male',28,'20160311','operation',10000.13,403,3), #以下是运营部门
      ('程咬金','male',18,'19970312','operation',20000,403,3),
      ('程咬银','female',18,'20130311','operation',19000,403,3),
      ('程咬铜','male',18,'20150411','operation',18000,403,3),
      ('程咬铁','female',18,'20140512','operation',17000,403,3)
      ;
      
      #ps：如果在windows系统中，插入中文字符，select的结果为空白，可以将所有字符编码统一设置成gbk
      
      准备表和记录
      ```

      答案一（连表查询）：

      ```python
      SELECT
          *
      FROM
          emp AS t1
      INNER JOIN (
          SELECT
              post,
              max(hire_date) max_date
          FROM
              emp
          GROUP BY
              post
      ) AS t2 ON t1.post = t2.post
      WHERE
          t1.hire_date = t2.max_date;
      ```

      答案二（子查询）：

      ```python
      mysql> select (select t2.name from emp as t2 where t2.post=t1.post order by hire_date desc limit 1) from emp as t1 group by post;
      +---------------------------------------------------------------------------------------+
      | (select t2.name from emp as t2 where t2.post=t1.post order by hire_date desc limit 1) |
      +---------------------------------------------------------------------------------------+
      | 张野                                                                                  |
      | 格格                                                                                  |
      | alex                                                                                  |
      | egon                                                                                  |
      +---------------------------------------------------------------------------------------+
      rows in set (0.00 sec)
      
      mysql> select (select t2.id from emp as t2 where t2.post=t1.post order by hire_date desc limit 1) from emp as t1 group by post;
      +-------------------------------------------------------------------------------------+
      | (select t2.id from emp as t2 where t2.post=t1.post order by hire_date desc limit 1) |
      +-------------------------------------------------------------------------------------+
      |                                                                                  14 |
      |                                                                                  13 |
      |                                                                                   2 |
      |                                                                                   1 |
      +-------------------------------------------------------------------------------------+
      rows in set (0.00 sec)
      
      #正确答案
      mysql> select t3.name,t3.post,t3.hire_date from emp as t3 where id in (select (select id from emp as t2 where t2.post=t1.post order by hire_date desc limit 1) from emp as t1 group by post);
      +--------+-----------------------------------------+------------+
      | name   | post                                    | hire_date  |
      +--------+-----------------------------------------+------------+
      | egon   | 老男孩驻沙河办事处外交大使              | 2017-03-01 |
      | alex   | teacher                                 | 2015-03-02 |
      | 格格   | sale                                    | 2017-01-27 |
      | 张野   | operation                               | 2016-03-11 |
      +--------+-----------------------------------------+------------+
      rows in set (0.00 sec)
      ```

      答案一为正确答案，答案二中的limit 1有问题（每个部门可能有>1个为同一时间入职的新员工），我只是想用该例子来说明可以在select后使用子查询。

   4. 综合练习

      ```python
      '''
      init.sql 文件内容
      '''
      /*
       数据导入：
       Navicat Premium Data Transfer
      
       Source Server         : localhost
       Source Server Type    : MySQL
       Source Server Version : 50624
       Source Host           : localhost
       Source Database       : sqlexam
      
       Target Server Type    : MySQL
       Target Server Version : 50624
       File Encoding         : utf-8
      
       Date: 10/21/2016 06:46:46 AM
      */
      
      SET NAMES utf8;
      SET FOREIGN_KEY_CHECKS = 0;
      
      -- ----------------------------
      --  Table structure for `class`
      -- ----------------------------
      DROP TABLE IF EXISTS `class`;
      CREATE TABLE `class` (
        `cid` int(11) NOT NULL AUTO_INCREMENT,
        `caption` varchar(32) NOT NULL,
        PRIMARY KEY (`cid`)
      ) ENGINE=InnoDB AUTO_INCREMENT=5 DEFAULT CHARSET=utf8;
      
      -- ----------------------------
      --  Records of `class`
      -- ----------------------------
      BEGIN;
      INSERT INTO `class` VALUES ('1', '三年二班'), ('2', '三年三班'), ('3', '一年二班'), ('4', '二年九班');
      COMMIT;
      
      -- ----------------------------
      --  Table structure for `course`
      -- ----------------------------
      DROP TABLE IF EXISTS `course`;
      CREATE TABLE `course` (
        `cid` int(11) NOT NULL AUTO_INCREMENT,
        `cname` varchar(32) NOT NULL,
        `teacher_id` int(11) NOT NULL,
        PRIMARY KEY (`cid`),
        KEY `fk_course_teacher` (`teacher_id`),
        CONSTRAINT `fk_course_teacher` FOREIGN KEY (`teacher_id`) REFERENCES `teacher` (`tid`)
      ) ENGINE=InnoDB AUTO_INCREMENT=5 DEFAULT CHARSET=utf8;
      
      -- ----------------------------
      --  Records of `course`
      -- ----------------------------
      BEGIN;
      INSERT INTO `course` VALUES ('1', '生物', '1'), ('2', '物理', '2'), ('3', '体育', '3'), ('4', '美术', '2');
      COMMIT;
      
      -- ----------------------------
      --  Table structure for `score`
      -- ----------------------------
      DROP TABLE IF EXISTS `score`;
      CREATE TABLE `score` (
        `sid` int(11) NOT NULL AUTO_INCREMENT,
        `student_id` int(11) NOT NULL,
        `course_id` int(11) NOT NULL,
        `num` int(11) NOT NULL,
        PRIMARY KEY (`sid`),
        KEY `fk_score_student` (`student_id`),
        KEY `fk_score_course` (`course_id`),
        CONSTRAINT `fk_score_course` FOREIGN KEY (`course_id`) REFERENCES `course` (`cid`),
        CONSTRAINT `fk_score_student` FOREIGN KEY (`student_id`) REFERENCES `student` (`sid`)
      ) ENGINE=InnoDB AUTO_INCREMENT=53 DEFAULT CHARSET=utf8;
      
      -- ----------------------------
      --  Records of `score`
      -- ----------------------------
      BEGIN;
      INSERT INTO `score` VALUES ('1', '1', '1', '10'), ('2', '1', '2', '9'), ('5', '1', '4', '66'), ('6', '2', '1', '8'), ('8', '2', '3', '68'), ('9', '2', '4', '99'), ('10', '3', '1', '77'), ('11', '3', '2', '66'), ('12', '3', '3', '87'), ('13', '3', '4', '99'), ('14', '4', '1', '79'), ('15', '4', '2', '11'), ('16', '4', '3', '67'), ('17', '4', '4', '100'), ('18', '5', '1', '79'), ('19', '5', '2', '11'), ('20', '5', '3', '67'), ('21', '5', '4', '100'), ('22', '6', '1', '9'), ('23', '6', '2', '100'), ('24', '6', '3', '67'), ('25', '6', '4', '100'), ('26', '7', '1', '9'), ('27', '7', '2', '100'), ('28', '7', '3', '67'), ('29', '7', '4', '88'), ('30', '8', '1', '9'), ('31', '8', '2', '100'), ('32', '8', '3', '67'), ('33', '8', '4', '88'), ('34', '9', '1', '91'), ('35', '9', '2', '88'), ('36', '9', '3', '67'), ('37', '9', '4', '22'), ('38', '10', '1', '90'), ('39', '10', '2', '77'), ('40', '10', '3', '43'), ('41', '10', '4', '87'), ('42', '11', '1', '90'), ('43', '11', '2', '77'), ('44', '11', '3', '43'), ('45', '11', '4', '87'), ('46', '12', '1', '90'), ('47', '12', '2', '77'), ('48', '12', '3', '43'), ('49', '12', '4', '87'), ('52', '13', '3', '87');
      COMMIT;
      
      -- ----------------------------
      --  Table structure for `student`
      -- ----------------------------
      DROP TABLE IF EXISTS `student`;
      CREATE TABLE `student` (
        `sid` int(11) NOT NULL AUTO_INCREMENT,
        `gender` char(1) NOT NULL,
        `class_id` int(11) NOT NULL,
        `sname` varchar(32) NOT NULL,
        PRIMARY KEY (`sid`),
        KEY `fk_class` (`class_id`),
        CONSTRAINT `fk_class` FOREIGN KEY (`class_id`) REFERENCES `class` (`cid`)
      ) ENGINE=InnoDB AUTO_INCREMENT=17 DEFAULT CHARSET=utf8;
      
      -- ----------------------------
      --  Records of `student`
      -- ----------------------------
      BEGIN;
      INSERT INTO `student` VALUES ('1', '男', '1', '理解'), ('2', '女', '1', '钢蛋'), ('3', '男', '1', '张三'), ('4', '男', '1', '张一'), ('5', '女', '1', '张二'), ('6', '男', '1', '张四'), ('7', '女', '2', '铁锤'), ('8', '男', '2', '李三'), ('9', '男', '2', '李一'), ('10', '女', '2', '李二'), ('11', '男', '2', '李四'), ('12', '女', '3', '如花'), ('13', '男', '3', '刘三'), ('14', '男', '3', '刘一'), ('15', '女', '3', '刘二'), ('16', '男', '3', '刘四');
      COMMIT;
      
      -- ----------------------------
      --  Table structure for `teacher`
      -- ----------------------------
      DROP TABLE IF EXISTS `teacher`;
      CREATE TABLE `teacher` (
        `tid` int(11) NOT NULL AUTO_INCREMENT,
        `tname` varchar(32) NOT NULL,
        PRIMARY KEY (`tid`)
      ) ENGINE=InnoDB AUTO_INCREMENT=6 DEFAULT CHARSET=utf8;
      
      -- ----------------------------
      --  Records of `teacher`
      -- ----------------------------
      BEGIN;
      INSERT INTO `teacher` VALUES ('1', '张磊老师'), ('2', '李平老师'), ('3', '刘海燕老师'), ('4', '朱云海老师'), ('5', '李杰老师');
      COMMIT;
      
      SET FOREIGN_KEY_CHECKS = 1;
      ```

      从init.sql文件导入数据

      ```Python
      mysql> create database homework;
      mysql> use homework;
      mysql> source D:\programme\projectc\laonanhai\day40\init.sql  #写文件路径
      ```

      练习题：

      1. 查询男生、女生的人数；
      
      2. 查询姓“张”的学生名单；
      
      3. 课程平均分从高到低显示
      
      4. 查询有课程成绩小于60分的同学的学号、姓名；
      
      5. 查询至少有一门课与学号为1的同学所学课程相同的同学的学号和姓名；
      
         ```python
         select course_id from score where student_id =1;
         select * from score where course_id in (1,2,4);
         select distinct student_id from score where course_id in (select course_id from score where student_id =1) and student_id !=1;
         
         select sid,sname from student right join (select distinct student_id from score where course_id in (select course_id from score where student_id =1) and student_id !=1) as t on student.sid = t.student_id;
         ```
      
         
      
      6. 查询出只选修了一门课程的全部学生的学号和姓名；
      
      7. 查询各科成绩最高和最低的分：以如下形式显示：课程ID，最高分，最低分；
      
      8. 查询课程编号“2”的成绩比课程编号“1”课程低的所有同学的学号、姓名；
      
         ```python
         # 把课程编号2的所有人的学生id,成绩找出来
         select student_id as sid2,num as n2 from score where course_id = 2;
         # 把课程编号1的所有人的学生id,成绩找出来
         select student_id as sid1,num as n1 from score where course_id = 1;
         # 课程编号“2”的成绩比课程编号“1”课程低的所有同学的学号
         select sid1 from (select student_id as sid2,num as n2 from score where course_id = 2) as t2 inner join   (select student_id as sid1,num as n1 from score where course_id = 1) as t1
         on t1.sid1 = t2.sid2 where n2<n1;
         # 连表student,取名字
         # select sid,sname from student right join (
         #               select sid1 from (select student_id as sid2,num as n2 from score where course_id = 2) as t2
         #               inner join
         #               (select student_id as sid1,num as n1 from score where course_id = 1) as t1
         #               on t1.sid1 = t2.sid2 where n2<n1
      #         ) as tmp
         #         on student.sid = tmp.sid1;
      ```
      
      
      
   9. 查询“生物”课程比“物理”课程成绩高的所有学生的学号；
      
         ```python
         # 先获取生物课的id
         # select cid from course where cname = '生物';
         # 先获取生物理的id
         # select cid from course where cname = '物理';
         # 找到所有学物理的人
         # select student_id as sid2,num as n2 from score where course_id = (select cid from course where cname = '物理');
         # select student_id as sid2,num as n2 from score where course_id = (select cid from course where cname = '生物');
         
         # select sid1 from (select student_id as sid1,num as n1 from score where course_id = (select cid from course where cname = '物理')) t1
         #                 inner join
         #                 (select student_id as sid2,num as n2 from score where course_id = (select cid from course where cname = '生物')) t2
         #                 on t1.sid1 = t2.sid2 where n2>n1;
         ```
      
         
      
      10. 查询平均成绩大于60分的同学的学号和平均成绩;
      
      11. 查询所有同学的学号、姓名、选课数、总成绩；
      
      12. 查询姓“李”的老师的个数；
      
      13. 查询没学过“张磊老师”课的同学的学号、姓名；
      
          ```python
          # 找一下张磊老师教什么课,取课程id
          # select cid from course inner join teacher on course.teacher_id = teacher.tid where tname = '张磊老师';
          # 再从score表中找到学习张磊老师课程的学生
          # select distinct student_id from score where course_id in (select cid from course inner join teacher on course.teacher_id = teacher.tid where tname = '张磊老师');
          # 再找不是这些学生的其他人
          # select sid,sname from student where sid not in (
          #     select distinct student_id from score where course_id in (select cid from course inner join teacher on course.teacher_id = teacher.tid where tname = '张磊老师'));
          
          ```
      
          
      
      14. 查询学过“1”并且也学过编号“2”课程的同学的学号、姓名；
      
      15. 查询学过“李平老师”所教的所有课的同学的学号、姓名；
      
          ```python
          # 找到李平老师教的课程
          # select cid from course inner join teacher on course.teacher_id = teacher.tid where tname = '李平老师';
          # select count(cid) from course inner join teacher on course.teacher_id = teacher.tid where tname = '李平老师';
          # # 找到所有学习李平老师课的学生
          # select * from score where course_id in (select cid from course inner join teacher on course.teacher_id = teacher.tid where tname = '李平老师');
          # # 查询学号
          # select student_id  from score where course_id in (
          #     select cid from course inner join teacher on course.teacher_id = teacher.tid where tname = '李平老师') group by student_id having count(*) = (
          #     select count(cid) from course inner join teacher on course.teacher_id = teacher.tid where tname = '李平老师'
          # );
          # 查询姓名
          # select sid,sname from student right join (
          #     select student_id  from score where course_id in (
          #         select cid from course inner join teacher on course.teacher_id = teacher.tid where tname = '李平老师') group by student_id having count(*) = (
          #         select count(cid) from course inner join teacher on course.teacher_id = teacher.tid where tname = '李平老师'
          #     )) as tmp on tmp.student_id =  student.sid;
          
          ```
      
          
      
      16. 查询没有学全所有课的同学的学号、姓名；
      
      17. 查询和“002”号的同学学习的课程完全相同的其他同学学号和姓名；
      
      18. 删除学习“叶平”老师课的SC表记录；
      
      19. 向SC表中插入一些记录，这些记录要求符合以下条件：①没有上过编号“002”课程的同学学号；②插入“002”号课程的平均成绩； 
      
      20. 按平均成绩从低到高显示所有学生的“语文”、“数学”、“英语”三门的课程成绩，按如下形式显示： 学生ID,语文,数学,英语,有效课程数,有效平均分；
      
      21. 查询各科成绩最高和最低的分：以如下形式显示：课程ID，最高分，最低分；
      
      22. 按各科平均成绩从低到高和及格率的百分数从高到低顺序；
      
      23. 查询各科成绩前三名的记录:(不考虑成绩并列情况) 
      
      24. 查询每门课程被选修的学生数；
      
      25. 查询同名同姓学生名单，并统计同名人数；
      
      26. 查询每门课程的平均成绩，结果按平均成绩升序排列，平均成绩相同时，按课程号降序排列；
      
      27. 查询平均成绩大于85的所有学生的学号、姓名和平均成绩；
      
      28. 查询课程名称为“数学”，且分数低于60的学生姓名和分数；
      
      29. 查询课程编号为003且课程成绩在80分以上的学生的学号和姓名；
      
      30. 求选了课程的学生人数
      
      31. 查询选修“杨艳”老师所授课程的学生中，成绩最高的学生姓名及其成绩；
      
      32. 查询各个课程及相应的选修人数；
      
      33. 查询不同课程但成绩相同的学生的学号、课程号、学生成绩；
      
      34. 查询每门课程成绩最好的前两名；
      
      35. 检索至少选修两门课程的学生学号；
      
      36. 查询全部学生都选修的课程的课程号和课程名；
      
      37. 查询没学过“叶平”老师讲授的任一门课程的学生姓名；
      
      38. 查询两门以上不及格课程的同学的学号及其平均成绩；
      
      39. 检索“004”课程分数小于60，按分数降序排列的同学学号；
      
      40. 删除“002”同学的“001”课程的成绩；
   
   
   
   
   
   
   
   

