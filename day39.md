## 一、内容回顾

1. 存储引擎

   1. Innodb mysql5.6之后的默认存储引擎,2个文件,4个支持(支持事务,行级锁,表级锁,外键)
   2. Myisam mysql5.5之前的默认存储引擎,3个文件 支持表级锁
   3. Memory,1个文件 数据断电消失

2. 数据类型

   1. 数字 : bool int float(7,2)
   2. 日期 : date time datetime year
   3. 字符串 :
      1. char    定长 效率高浪费空间 255
      2. varchar 变长 效率低节省空间 65535
   4. enum 和 set :单选和多选

3. 约束

   1. unsigned 无符号的
   2. not null 非空
   3. default  设置默认值
   4. unique   唯一,不能重复  ,unique(字段1,字段2,字段3) 联合唯一
   5. auto_increment 自增, int 必须至少unique字段,自带not null
   6. primary key 主键 ,not null + unique,一张表只能有一个主键
   7. foreign key 外键,a表中有一个字段关联b表中的一个unique,a表中的是外键

4. 建表

   ```
   create table 表名(
      字段名1 类型(长度) 约束,
      字段名1 类型(选项) 约束,
    );
   ```

5. 修改表结构

   ```
   # alter table 表名 rename 新名字;
   # alter table 表名 add 字段名 类型(长度) 约束 after 某字段;
   # alter table 表名 drop 字段名;
   # alter table 表名 modify 字段名 类型(长度) 约束 first;
   # alter table 表名 change 旧字名 新名字 类型(长度) 约束;
   ```

6. 表之间的关系:一对一，一对多，多对多

7. 删除表：drop table 表名;

## 二、今日内容

1. 数据的增删改

   ```python
   #建表
   create table t1(
     id int primary key auto_increment,
     username char(12) not null,
     sex enum('male','female') default 'male',
     hobby set('上课','写作业','考试') not null
   );
   
   # 增 insert into 表(字段,...) values (值,...);
   insert into t1 value (1,'大壮','male','上课,写作业');  #注意value增加一个数据可以不用加s
   insert into t1 values(2,'杜相玺','male','写作业,考试');
   insert into t1 values(3,'b哥','male','写作业'),(4,'庄博','male','考试');
   insert into t1(username,hobby) values ('杨得港','上课,写作业,考试'),('李帅','考试')
   
   insert into t2(id,name) select id,username from t1;
   
   # 删
   # 清空表
       # delete from 表;
           # 会清空表,但不会清空自增字段的offset(偏移量)值
       # truncate table 表;
           # 会清空表和自增字段的偏移量
   # 删除某一条数据
       # delete from 表 where 条件;
   
   # 改
       # update 表 set 字段=值 where 条件;
       # update 表 set 字段=值,字段=值 where 条件;
   
       
   1. 操作文件夹（库）
      增：create database db1 charset utf8;
      查：show databases;
      改：alter database db1 charset latin1;
      删除: drop database db1;
   
   
   2. 操作文件（表）
      先切换到文件夹下：use db1
      增：create table t1(id int,name char);
      查：show tables;
      改：alter table t1 modify name char(3);
         alter table t1 change name name1 char(2);
      删：drop table t1;
       
   
   3. 操作文件中的内容（记录）
      增：insert into t1 values(1,'egon1'),(2,'egon2'),(3,'egon3');
      查：select * from t1;
      改：update t1 set name='sb' where id=2;
      删：delete from t1 where id=1;
   
      清空表：
          delete from t1; #如果有自增id，新增的数据，仍然是以删除前的最后一样作为起始。
          truncate table t1;数据量大，删除速度比上一条快，且直接从零开始，
   ```

2. 单表数据查询

   ```python
   #来自课堂：
   # 最简单的select
       # select * from 表;
       # select 字段,... from 表;
   # 重命名字段
       # select 字段 as 新名字 from 表;  #取别名
       # select 字段 新名字 from 表;  #重命名
   # 去重
       # select distinct 字段 from 表;
       # select distinct age,sex from employee;
   # 使用函数
       # concat
       # concat_ws
   # 四则运算的
       #  select emp_name,salary*12 from employee; 乘法
       #  select emp_name,salary*12 as annual_salary from employee;
   # 使用判断逻辑
       # case when语句 相当于 if条件判断句
   
   # where 筛选所有符合条件的行
       # 比较运算符
           # > < >= <=  <> != 最后两个都是不等于
       # 范围
           # between 10000 and 20000 要1w-2w之间的
           # in (10000,20000)   只要10000或者20000的
       # 模糊匹配
           # like
               # % 通配符 表示任意长度的任意内容
               # _ 通配符 一个字符长度的任意内容
           # regexp
               # '^这是文件夹'
               # 'g$'
       # 逻辑运算
           # not\and\or
   
   # 查看岗位描述不为NULL的员工信息
       # 这个要用 is
       # select * from employee where post_comment is not null;
   # 查看岗位是teacher且薪资不是10000或9000或30000的员工姓名、年龄、薪资
       # select emp_name, age, salary
       # from employee wherepost = 'teacher' and salary not in(10000,9000,30000)
   # 查看岗位是teacher且名字是jin开头的员工姓名、年薪
       #  select emp_name,salary*12 from employee where post = 'teacher' and emp_name like 'jin%';
   
   # 分组 group by 根据谁分组,可以求这个组的总人数,最大值,最小值,平均值,求和 但是这个求出来的值只是和分组字段对应
       # 并不和其他任何字段对应,这个时候查出来的所有其他字段都不生效.
   # 聚合函数
       # count 求个数
       # max  求最大值
       # min  求最小值
       # sum  求和
       # avg  求平均
   
       # SELECT post,emp_name FROM employee GROUP BY post;
       # SELECT post,GROUP_CONCAT(emp_name) FROM employee GROUP BY post;
   
   # having 过滤语句
       # 在having条件中可以使用聚合函数,在where中不行
       # 适合去筛选符合条件的某一组数据,而不是某一行数据
       # 先分组再过滤 : 求平均薪资大于xx的部门,求人数大于xx的性别,求大于xx人的年龄段
   # 查询各岗位内包含的员工个数小于2的岗位名、岗位内包含员工名字、个数
   # group by post having count(id) < 2;
   
   # 排序 order by
       # 默认是升序  asc
       # 降序  desc
       # order by age ,salary desc
           # 优先根据age从小到大排,在age相同的情况下,再根据薪资从大到小排
   
   # limit m,n  /  limit n offset m  两者一样
       # 从m+1项开始,取n项
       # 如果不写m,m默认为0
   ```

   ```python
   #来自博客：
   #单表查询语法
   '''
   SELECT DISTINCT 字段1,字段2... FROM 表名
                                 WHERE 条件
                                 GROUP BY field
                                 HAVING 筛选
                                 ORDER BY field
                                 LIMIT 限制条数
   '''
   
   #关键字执行的优先级
   '''
   from
   where
   group by
   select
   distinct
   having
   order by
   limit
   
   1.找到表:from
   2.拿着where指定的约束条件，去文件/表中取出一条条记录
   3.将取出的一条条记录进行分组group by，如果没有group by，则整体作为一组
   4.执行select（去重）
   5.将分组的结果进行having过滤
   6.将结果按条件排序：order by
   7.限制结果的显示条数
   '''
   
   ##1、建表和数据准备:
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
   emp_name varchar(20) not null,
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
   | emp_name     | varchar(20)           | NO   |     | NULL    |                |
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
   insert into employee(emp_name,sex,age,hire_date,post,salary,office,depart_id) values
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
   
   
   ##2、简单查询
       SELECT id,emp_name,sex,age,hire_date,post,post_comment,salary,office,depart_id 
       FROM employee;
   
       SELECT * FROM employee;
   
       SELECT emp_name,salary FROM employee;
   
   #避免重复DISTINCT
       SELECT DISTINCT post FROM employee;    
   
   #通过四则运算查询
       SELECT emp_name, salary*12 FROM employee;
       SELECT emp_name, salary*12 AS Annual_salary FROM employee;
       SELECT emp_name, salary*12 Annual_salary FROM employee;
   
   #定义显示格式
      CONCAT() 函数用于连接字符串
      SELECT CONCAT('姓名: ',emp_name,'  年薪: ', salary*12)  AS Annual_salary 
      FROM employee;
      
      CONCAT_WS() 第一个参数为分隔符
      SELECT CONCAT_WS(':',emp_name,salary*12)  AS Annual_salary 
      FROM employee;
   
      结合CASE语句：
      SELECT
          (
              CASE
              WHEN emp_name = 'jingliyang' THEN
                  emp_name
              WHEN emp_name = 'alex' THEN
                  CONCAT(emp_name,'_BIGSB')
              ELSE
                  concat(emp_name, 'SB')
              END
          ) as new_name
      FROM
          employee;
       
   #小练习：
   '''
   1 查出所有员工的名字，薪资,格式为
       <名字:egon>    <薪资:3000>
   2 查出所有的岗位（去掉重复）
   3 查出所有员工名字，以及他们的年薪,年薪的字段名为annual_year
   '''
   '''
   答案；
   select concat('<名字:',emp_name,'>    ','<薪资:',salary,'>') from employee;
   select distinct depart_id from employee;
   select emp_name,salary*12 annual_salary from employee;
   '''
   
   #where约束，
   '''
   where字句中可以使用：
   1. 比较运算符：> < >= <= <> !=
   2. between 80 and 100 值在80到100之间
   3. in(80,90,100) 值是80或90或100
   4. like 'e%'
       通配符可以是%或_，
       %表示任意多字符
       _表示一个字符 
   5. 逻辑运算符：在多个条件直接可以使用逻辑运算符 and or not
   '''
   #1:单条件查询
       SELECT emp_name FROM employee
           WHERE post='sale';
           
   #2:多条件查询
       SELECT emp_name,salary FROM employee
           WHERE post='teacher' AND salary>10000;
   
   #3:关键字BETWEEN AND
       SELECT emp_name,salary FROM employee 
           WHERE salary BETWEEN 10000 AND 20000;
   
       SELECT emp_name,salary FROM employee 
           WHERE salary NOT BETWEEN 10000 AND 20000;
       
   #4:关键字IS NULL(判断某个字段是否为NULL不能用等号，需要用IS)
       SELECT emp_name,post_comment FROM employee 
           WHERE post_comment IS NULL;
   
       SELECT emp_name,post_comment FROM employee 
           WHERE post_comment IS NOT NULL;
           
       SELECT emp_name,post_comment FROM employee 
           WHERE post_comment=''; 注意''是空字符串，不是null
       ps：
           执行
           update employee set post_comment='' where id=2;
           再用上条查看，就会有结果了
   
   #5:关键字IN集合查询
       SELECT emp_name,salary FROM employee 
           WHERE salary=3000 OR salary=3500 OR salary=4000 OR salary=9000 ;
       
       SELECT emp_name,salary FROM employee 
           WHERE salary IN (3000,3500,4000,9000) ;
   
       SELECT emp_name,salary FROM employee 
           WHERE salary NOT IN (3000,3500,4000,9000) ;
   
   #6:关键字LIKE模糊查询
       通配符’%’
       SELECT * FROM employee 
               WHERE emp_name LIKE 'eg%';
   
       通配符’_’
       SELECT * FROM employee 
               WHERE emp_name LIKE 'al__';
           
   #小练习：
   '''
   1. 查看岗位是teacher的员工姓名、年龄
   2. 查看岗位是teacher且年龄大于30岁的员工姓名、年龄
   3. 查看岗位是teacher且薪资在9000-10000范围内的员工姓名、年龄、薪资
   4. 查看岗位描述不为NULL的员工信息
   5. 查看岗位是teacher且薪资是10000或9000或30000的员工姓名、年龄、薪资
   6. 查看岗位是teacher且薪资不是10000或9000或30000的员工姓名、年龄、薪资
   7. 查看岗位是teacher且名字是jin开头的员工姓名、年薪
   '''
   '''
   答案：
   select emp_name,age from employee where post = 'teacher';
   select emp_name,age from employee where post='teacher' and age > 30; 
   select emp_name,age,salary from employee where post='teacher' and salary between 9000 and 10000;
   select * from employee where post_comment is not null;
   select emp_name,age,salary from employee where post='teacher' and salary in (10000,9000,30000);
   select emp_name,age,salary from employee where post='teacher' and salary not in (10000,9000,30000);
   select emp_name,salary*12 from employee where post='teacher' and emp_name like 'jin%';
   '''
   
   #group by 
   '''
   单独使用GROUP BY关键字分组
       SELECT post FROM employee GROUP BY post;
       注意：我们按照post字段分组，那么select查询的字段只能是post，想要获取组内的其他相关信息，需要借助函数
   
   GROUP BY关键字和GROUP_CONCAT()函数一起使用
       SELECT post,GROUP_CONCAT(emp_name) FROM employee GROUP BY post;#按照岗位分组，并查看组内成员名
       SELECT post,GROUP_CONCAT(emp_name) as emp_members FROM employee GROUP BY post;
   
   GROUP BY与聚合函数一起使用
       select post,count(id) as count from employee group by post;#按照岗位分组，并查看每个组有多少人
       
   #强调：
   如果我们用unique的字段作为分组的依据，则每一条记录自成一组，这种分组没有意义
   多条记录之间的某个字段值相同，该字段通常用来作为分组的依据
   '''
   
   #聚合函数
   #强调：聚合函数聚合的是组的内容，若是没有分组，则默认一组
   '''
   示例：
       SELECT COUNT(*) FROM employee;
       SELECT COUNT(*) FROM employee WHERE depart_id=1;
       SELECT MAX(salary) FROM employee;
       SELECT MIN(salary) FROM employee;
       SELECT AVG(salary) FROM employee;
       SELECT SUM(salary) FROM employee;
       SELECT SUM(salary) FROM employee WHERE depart_id=3;
   '''
   #小练习
   '''
   1. 查询岗位名以及岗位包含的所有员工名字
   2. 查询岗位名以及各岗位内包含的员工个数
   3. 查询公司内男员工和女员工的个数
   4. 查询岗位名以及各岗位的平均薪资
   5. 查询岗位名以及各岗位的最高薪资
   6. 查询岗位名以及各岗位的最低薪资
   7. 查询男员工与男员工的平均薪资，女员工与女员工的平均薪资
   '''
   #答案：
   #题1：分组
   mysql> select post,group_concat(emp_name) from employee group by post;
   +-----------------------------------------+---------------------------------------------------------+
   | post                                    | group_concat(emp_name)                                      |
   +-----------------------------------------+---------------------------------------------------------+
   | operation                               | 张野,程咬金,程咬银,程咬铜,程咬铁                        |
   | sale                                    | 歪歪,丫丫,丁丁,星星,格格                                |
   | teacher                                 | alex,wupeiqi,yuanhao,liwenzhou,jingliyang,jinxin,成龙   |
   | 老男孩驻沙河办事处外交大使              | egon                                                    |
   +-----------------------------------------+---------------------------------------------------------+
   
   
   #题目2：
   mysql> select post,count(id) from employee group by post;
   +-----------------------------------------+-----------+
   | post                                    | count(id) |
   +-----------------------------------------+-----------+
   | operation                               |         5 |
   | sale                                    |         5 |
   | teacher                                 |         7 |
   | 老男孩驻沙河办事处外交大使              |         1 |
   +-----------------------------------------+-----------+
   
   
   #题目3：
   mysql> select sex,count(id) from employee group by sex;
   +--------+-----------+
   | sex    | count(id) |
   +--------+-----------+
   | male   |        10 |
   | female |         8 |
   +--------+-----------+
   
   #题目4：
   mysql> select post,avg(salary) from employee group by post;
   +-----------------------------------------+---------------+
   | post                                    | avg(salary)   |
   +-----------------------------------------+---------------+
   | operation                               |  16800.026000 |
   | sale                                    |   2600.294000 |
   | teacher                                 | 151842.901429 |
   | 老男孩驻沙河办事处外交大使              |   7300.330000 |
   +-----------------------------------------+---------------+
   
   #题目5
   mysql> select post,max(salary) from employee group by post;
   +-----------------------------------------+-------------+
   | post                                    | max(salary) |
   +-----------------------------------------+-------------+
   | operation                               |    20000.00 |
   | sale                                    |     4000.33 |
   | teacher                                 |  1000000.31 |
   | 老男孩驻沙河办事处外交大使              |     7300.33 |
   +-----------------------------------------+-------------+
   
   #题目6
   mysql> select post,min(salary) from employee group by post;
   +-----------------------------------------+-------------+
   | post                                    | min(salary) |
   +-----------------------------------------+-------------+
   | operation                               |    10000.13 |
   | sale                                    |     1000.37 |
   | teacher                                 |     2100.00 |
   | 老男孩驻沙河办事处外交大使              |     7300.33 |
   +-----------------------------------------+-------------+
   
   #题目七
   mysql> select sex,avg(salary) from employee group by sex;
   +--------+---------------+
   | sex    | avg(salary)   |
   +--------+---------------+
   | male   | 110920.077000 |
   | female |   7250.183750 |
   +--------+---------------+
   
   #HAVING过滤
   #HAVING与WHERE不一样的地方在于!!!!!!
   #！！！执行优先级从高到低：where > group by > having 
   #1. Where 发生在分组group by之前，因而Where中可以有任意字段，但是绝对不能使用聚合函数。
   #2. Having发生在分组group by之后，因而Having中可以使用分组的字段，无法直接取到其他字段,可以使用聚合函数
   '''
   验证：
   mysql> select @@sql_mode;
   +--------------------+
   | @@sql_mode         |
   +--------------------+
   | ONLY_FULL_GROUP_BY |
   +--------------------+
   row in set (0.00 sec)
   
   mysql> select * from emp where salary > 100000;
   +----+------+------+-----+------------+---------+--------------+------------+--------+-----------+
   | id | emp_name | sex  | age | hire_date  | post    | post_comment | salary     | office | depart_id |
   +----+------+------+-----+------------+---------+--------------+------------+--------+-----------+
   |  2 | alex | male |  78 | 2015-03-02 | teacher | NULL         | 1000000.31 |    401 |         1 |
   +----+------+------+-----+------------+---------+--------------+------------+--------+-----------+
   row in set (0.00 sec)
   
   mysql> select post,group_concat(emp_name) from emp group by post having salary > 10000;#错误，分组后无法直接取到salary字段
   ERROR 1054 (42S22): Unknown column 'salary' in 'having clause'
   mysql> select post,group_concat(emp_name) from emp group by post having avg(salary) > 10000;
   +-----------+-------------------------------------------------------+
   | post | group_concat(emp_name) |
   +-----------+-------------------------------------------------------+
   | operation | 程咬铁,程咬铜,程咬银,程咬金,张野 |
   | teacher | 成龙,jinxin,jingliyang,liwenzhou,yuanhao,wupeiqi,alex |
   +-----------+-------------------------------------------------------+
   rows in set (0.00 sec)
   '''
   #小练习：
   '''
   1. 查询各岗位内包含的员工个数小于2的岗位名、岗位内包含员工名字、个数
   3. 查询各岗位平均薪资大于10000的岗位名、平均工资
   4. 查询各岗位平均薪资大于10000且小于20000的岗位名、平均工资
   '''
   #答案
   #题1：
   mysql> select post,group_concat(emp_name),count(id) from employee group by post having count(id) < 2;
   +-----------------------------------------+--------------------+-----------+
   | post                                    | group_concat(emp_name) | count(id) |
   +-----------------------------------------+--------------------+-----------+
   | 老男孩驻沙河办事处外交大使              | egon               |         1 |
   +-----------------------------------------+--------------------+-----------+
   
   #题目2：
   mysql> select post,avg(salary) from employee group by post having avg(salary) > 10000;
   +-----------+---------------+
   | post      | avg(salary)   |
   +-----------+---------------+
   | operation |  16800.026000 |
   | teacher   | 151842.901429 |
   +-----------+---------------+
   
   #题目3：
   mysql> select post,avg(salary) from employee group by post having avg(salary) > 10000 and avg(salary) <20000;
   +-----------+--------------+
   | post      | avg(salary)  |
   +-----------+--------------+
   | operation | 16800.026000 |
   +-----------+--------------+
   
   #ORDER BY 查询排序
   '''
   按单列排序
       SELECT * FROM employee ORDER BY salary;
       SELECT * FROM employee ORDER BY salary ASC;
       SELECT * FROM employee ORDER BY salary DESC;
   
   按多列排序:先按照age排序，如果年纪相同，则按照薪资排序
       SELECT * from employee
           ORDER BY age,
           salary DESC;
   '''
   #小练习
   '''
   1. 查询所有员工信息，先按照age升序排序，如果age相同则按照hire_date降序排序
   2. 查询各岗位平均薪资大于10000的岗位名、平均工资,结果按平均薪资升序排列
   3. 查询各岗位平均薪资大于10000的岗位名、平均工资,结果按平均薪资降序排列
   '''
   #答案：
   #题目1
   mysql> select * from employee ORDER BY age asc,hire_date desc;
   
   #题目2
   mysql> select post,avg(salary) from employee group by post having avg(salary) > 10000 order by avg(salary) asc;
   +-----------+---------------+
   | post      | avg(salary)   |
   +-----------+---------------+
   | operation |  16800.026000 |
   | teacher   | 151842.901429 |
   +-----------+---------------+
   
   #题目3
   mysql> select post,avg(salary) from employee group by post having avg(salary) > 10000 order by avg(salary) desc;
   +-----------+---------------+
   | post      | avg(salary)   |
   +-----------+---------------+
   | teacher   | 151842.901429 |
   | operation |  16800.026000 |
   +-----------+---------------+
   
   #LIMIT 限制查询的记录数
   '''
   示例：
       SELECT * FROM employee ORDER BY salary DESC 
           LIMIT 3;                    #默认初始位置为0 
       
       SELECT * FROM employee ORDER BY salary DESC
           LIMIT 0,5; #从第0开始，即先查询出第一条，然后包含这一条在内往后查5条
   
       SELECT * FROM employee ORDER BY salary DESC
           LIMIT 5,5; #从第5开始，即先查询出第6条，然后包含这一条在内往后查5条
   '''
   #小练习：分页显示，每页5条
   #答案：
   mysql> select * from  employee limit 0,5;
   +----+-----------+------+-----+------------+-----------------------------------------+--------------+------------+--------+-----------+
   | id | emp_name      | sex  | age | hire_date  | post                                    | post_comment | salary     | office | depart_id |
   +----+-----------+------+-----+------------+-----------------------------------------+--------------+------------+--------+-----------+
   |  1 | egon      | male |  18 | 2017-03-01 | 老男孩驻沙河办事处外交大使              | NULL         |    7300.33 |    401 |         1 |
   |  2 | alex      | male |  78 | 2015-03-02 | teacher                                 |              | 1000000.31 |    401 |         1 |
   |  3 | wupeiqi   | male |  81 | 2013-03-05 | teacher                                 | NULL         |    8300.00 |    401 |         1 |
   |  4 | yuanhao   | male |  73 | 2014-07-01 | teacher                                 | NULL         |    3500.00 |    401 |         1 |
   |  5 | liwenzhou | male |  28 | 2012-11-01 | teacher                                 | NULL         |    2100.00 |    401 |         1 |
   +----+-----------+------+-----+------------+-----------------------------------------+--------------+------------+--------+-----------+
   rows in set (0.00 sec)
   
   mysql> select * from  employee limit 5,5;
   +----+------------+--------+-----+------------+---------+--------------+----------+--------+-----------+
   | id | emp_name       | sex    | age | hire_date  | post    | post_comment | salary   | office | depart_id |
   +----+------------+--------+-----+------------+---------+--------------+----------+--------+-----------+
   |  6 | jingliyang | female |  18 | 2011-02-11 | teacher | NULL         |  9000.00 |    401 |         1 |
   |  7 | jinxin     | male   |  18 | 1900-03-01 | teacher | NULL         | 30000.00 |    401 |         1 |
   |  8 | 成龙       | male   |  48 | 2010-11-11 | teacher | NULL         | 10000.00 |    401 |         1 |
   |  9 | 歪歪       | female |  48 | 2015-03-11 | sale    | NULL         |  3000.13 |    402 |         2 |
   | 10 | 丫丫       | female |  38 | 2010-11-01 | sale    | NULL         |  2000.35 |    402 |         2 |
   +----+------------+--------+-----+------------+---------+--------------+----------+--------+-----------+
   rows in set (0.00 sec)
   
   mysql> select * from  employee limit 10,5;
   +----+-----------+--------+-----+------------+-----------+--------------+----------+--------+-----------+
   | id | emp_name      | sex    | age | hire_date  | post      | post_comment | salary   | office | depart_id |
   +----+-----------+--------+-----+------------+-----------+--------------+----------+--------+-----------+
   | 11 | 丁丁      | female |  18 | 2011-03-12 | sale      | NULL         |  1000.37 |    402 |         2 |
   | 12 | 星星      | female |  18 | 2016-05-13 | sale      | NULL         |  3000.29 |    402 |         2 |
   | 13 | 格格      | female |  28 | 2017-01-27 | sale      | NULL         |  4000.33 |    402 |         2 |
   | 14 | 张野      | male   |  28 | 2016-03-11 | operation | NULL         | 10000.13 |    403 |         3 |
   | 15 | 程咬金    | male   |  18 | 1997-03-12 | operation | NULL         | 20000.00 |    403 |         3 |
   +----+-----------+--------+-----+------------+-----------+--------------+----------+--------+-----------+
   rows in set (0.00 sec)
   
   #使用正则表达式查询
   '''
   SELECT * FROM employee WHERE emp_name REGEXP '^ale';
   
   SELECT * FROM employee WHERE emp_name REGEXP 'on$';
   
   SELECT * FROM employee WHERE emp_name REGEXP 'm{2}';
   
   小结：对字符串匹配的方式
   WHERE emp_name = 'egon';
   WHERE emp_name LIKE 'yua%';
   WHERE emp_name REGEXP 'on$';
   '''
   #小练习：查看所有员工中名字是jin开头，n或者g结果的员工信息
   #答案：
   select * from employee where emp_name regexp '^jin.*[gn]$';
   ```

   