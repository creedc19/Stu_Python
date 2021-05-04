## 一、内容回顾

1. b+树

   b是balance 平衡的意思 ，为了保证每一个数据查找经历的IO次数都相同

   只在叶子节点存储数据 ，为了降低树的高度

   叶子节点之前加入了双向连接 ，为了查找范围的时候比较快

2. 聚集索引 (聚簇索引)

   全表数据都存储在叶子节点上 -- Innodb存储引擎中的主键

3. 非聚集索引  (非聚簇索引)   /辅助索引

   叶子节点不存放具体的整行数据,而是存储的这一行的主键的值

4. 索引的创建和删除

   ```python
   # create index ind_name on 表名(字段名);
   # create index ind_name on 表名(字段名,字段2);
   # drop index 索引名 on 表名
   
   ```

5. 正确的使用mysql数据库

   ```python
   # 从库的角度
       # 搭建集群
       # 读写分离
       # 分库
   # 从表的角度
       # 合理安排表与表之间的关系 :该拆的拆,该合的合
       # 把固定长度的字段放在前面
       # 尽量使用char而不是varchar
   # 从操作数据的角度
       # 尽量在where字段就约束数值到一个比较小的范围 : 分页
           # where a between value1 and value2
       # 尽量使用连表查询代替子查询
       # 删除数据和修改数据的时候条件尽量使用主键
       # 合理的创建和使用索引
           # 创建索引
               # 1.选择区分度比较大的列
               # 2.尽量选择短的字段创建索引
               # 3.不要创建不必要的索引,及时删除不用的索引
           # 使用索引
               # 1.查询的字段不是索引字段
               # 2.在条件中使用范围,结果的范围越大速度越慢,范围小就快
               # 3.like 'a%'命中索引,like '%a'不命中索引
               # 4.条件列不能参与计算\不能使用函数
               # 5.and/or
                   # and条件相连 有一列有索引都会命中
                   # or条件相连 所有列都有索引才能命中
               # 6.联合索引
                   # create index mix_ind on 表 (id,name,email)
                   # 遵循最左前缀原则,且从出现范围开始索引失效
                   # select * from 表 where id = 123; 命中索引
                   # select * from 表 where id > 123; 不命中索引
                   # select * from 表 where id = 123 and name = 'alex'; 命中索引
                   # select * from 表 where id > 123 and name = 'alex'; 不命中索引
                   # select * from 表 where id = 123 and email = 'alex@oldboy'; 命中索引
                   # select * from 表 where email = 'alex@oldboy'; 不命中索引,因为条件中没有id
                   # select * from 表 where name='alex' and email = 'alex@oldboy'; 不命中索引,因为条件中没有id
               # 7.条件中的数据类型和实际字段的类型必须一致
               # 8.select字段中应该包含order by 中的字段
                   # select age from 表 order by age;   快
                   # select name from 表 order by age;  慢
   
   ```

6. 覆盖索引 : 查询过程中不需要回表

   ```python
    select id from 表  where id > 10000000;
    select max(id) from 表  where id > 10000000;
    select count(id) from 表  where id > 10000000;
   ```

7. ```python
   # 索引合并 : 分别创建的两个索引在某一次查询中临时合并成一条索引  a=1 or b=2
   # 执行计划 : explain select 语句 ;能够查看sql语句有没有按照预期执行,可以查看索引的使用情况,type等级
   # 慢查询优化 :
       # 首先从sql的角度优化
           # 把每一句话单独执行,找到效率低的表,优化这句sql
           # 了解业务场景,适当创建索引,帮助查询
           # 尽量用连表代替子查询
           # 确认命中索引的情况
       # 考虑修改表结构
           # 拆表
           # 把固定的字段往前调整
       # 使用执行计划,观察sql的type通过以上调整是否提高
   # mysql的慢日志
       # 在mysql的配置中开启并设置一下
       # 在超过设定时间之后,这条sql总是会被记录下来,
       # 这个时候我们可以对这些被记录的sql进行定期优化
   ```

   

## 二、今日内容

1. 内容大纲

   ```python
   # pymysql
       # python操作mysql数据库
           # 连接数据库
           # 获取游标
           # 执行sql(增删改查)
           # 如果涉及到修改 : 提交
           # 关闭游标
           # 关闭库
       # sql注入
           # 传参数,注意sql注入的问题,传参数通过execute方法来传
           # execute('select * from 表 where name = %s',('alex',))
   # mysql的库/表备份
   # 事务
       # 锁
   ```

2. Python连接数据库

   ```python
   import pymysql
   conn = pymysql.connect(host='127.0.0.1',
                          user='root',
                          password='123',
                          database='homework')   #打开数据库连接
   cur = conn.cursor()  #  cursor游标，cur = conn.cursor(cursor=pymysql.cursors.DictCursor)  # 查询返回字典
   cur.execute('select * from student;')   # 使用 execute() 方法执行 SQL 查询
   
   print(cur.rowcount)  # 获取查出多少行,便于使用fetchone取所有结果
   for i in range(cur.rowcount):
       ret = cur.fetchone()
       print(ret)
   
   try:
       ret1 = cur.fetchone()  # 使用 fetchone() 方法获取单条数据
       print(ret1)
       ret2 = cur.fetchmany(5) # 获取多条结果
       print(ret2)
       ret3 = cur.fetchall()  # 获取全部结果
       print(ret3)
   except pymysql.err.ProgrammingError as e:
       print(e)
   
   cur.close()
   conn.close()
   
   # 实际操作mysql的时候会遇到的一个问题
   # 结合数据库 和python 写一个登录
   user = input('username :')
   pwd = input('password :')
   conn = pymysql.connect(host='127.0.0.1',
                          user='root',
                          password="123",
                          database='day42')
   sql = 'select * from userinfo where user = %s and password = %s'  #这里不要用字符串拼接，会引起sql注入
   cur = conn.cursor()
   cur.execute(sql,(user,pwd))
   print(cur.fetchone())
   
   # sql注入
   # select * from userinfo where user = "1869" or 1=1;-- " and password = "3714";
   ```

3. 数据的备份和恢复

   ```python
   # 表和数据的备份
       # 备份数据 在cmd命令行直接执行
       mysqldump -uroot -p123 -h127.0.0.1 homework > D:\python_22\day42\tmp.sql 
   
       # 恢复数据 在mysql中执行命令
       # 切换到一个要备份的数据库中
       # source D:\python_22\day42\tmp.sql
   
   # 备份库
       # 备份
       mysqldump -uroot -p123 --databases homework > D:\python_22\day42\tmp2.sql
       # 恢复
       source D:\python_22\day42\tmp2.sql
   ```

4. 来自博客

   ```python
   #1、创建表操作
   import pymysql
    
   # 打开数据库连接
   db = pymysql.connect("localhost","testuser","test123","TESTDB" )
    
   # 使用 cursor() 方法创建一个游标对象 cursor
   cursor = db.cursor()
    
   # 使用 execute() 方法执行 SQL，如果表存在则删除
   cursor.execute("DROP TABLE IF EXISTS EMPLOYEE")
    
   # 使用预处理语句创建表
   sql = """CREATE TABLE EMPLOYEE (
            FIRST_NAME  CHAR(20) NOT NULL,
            LAST_NAME  CHAR(20),
            AGE INT,  
            SEX CHAR(1),
            INCOME FLOAT )"""
    
   cursor.execute(sql)
    
   # 关闭数据库连接
   db.close()
   
   #2、操作数据，插入操作
   import pymysql
    
   # 打开数据库连接
   db = pymysql.connect("localhost","testuser","test123","TESTDB" )
    
   # 使用cursor()方法获取操作游标 
   cursor = db.cursor()
    
   # SQL 插入语句
   sql = """INSERT INTO EMPLOYEE(FIRST_NAME,
            LAST_NAME, AGE, SEX, INCOME)
            VALUES ('Mac', 'Mohan', 20, 'M', 2000)"""
   try:
      cursor.execute(sql) # 执行sql语句
      db.commit()         # 提交到数据库执行
   except:
      db.rollback()       # 如果发生错误则回滚
    
   # 关闭数据库连接
   db.close()
   
   #另一种形式
   import pymysql
    
   # 打开数据库连接
   db = pymysql.connect("localhost","testuser","test123","TESTDB" )
    
   # 使用cursor()方法获取操作游标 
   cursor = db.cursor()
    
   # SQL 插入语句
   sql = "INSERT INTO EMPLOYEE(FIRST_NAME, \
          LAST_NAME, AGE, SEX, INCOME) \
          VALUES (%s, %s,  %s,  %s,  %s )" % \
          ('Mac', 'Mohan', 20, 'M', 2000)
   try:
      
      cursor.execute(sql)  # 执行sql语句
      db.commit()          # 执行sql语句
   except:
      db.rollback()        # 发生错误时回滚
    
   # 关闭数据库连接
   db.close()
   
   #3、操作数据，查询操作
   '''
   Python查询Mysql使用 fetchone() 方法获取单条数据, 使用fetchall() 方法获取多条数据。
   
   fetchone(): 该方法获取下一个查询结果集。结果集是一个对象
   fetchall(): 接收全部的返回结果行.
   rowcount: 这是一个只读属性，并返回执行execute()方法后影响的行数。
   '''
   import pymysql
    
   # 打开数据库连接
   db = pymysql.connect("localhost","testuser","test123","TESTDB" )
    
   # 使用cursor()方法获取操作游标 
   cursor = db.cursor()
    
   # SQL 查询语句
   sql = "SELECT * FROM EMPLOYEE \
          WHERE INCOME > %s" % (1000)
   try:
      
      cursor.execute(sql)# 执行SQL语句
      results = cursor.fetchall()# 获取所有记录列表
      for row in results:
         fname = row[0]
         lname = row[1]
         age = row[2]
         sex = row[3]
         income = row[4]
          # 打印结果
         print ("fname=%s,lname=%s,age=%s,sex=%s,income=%s" % \
                (fname, lname, age, sex, income ))
   except:
      print ("Error: unable to fetch data")
    
   # 关闭数据库连接
   db.close()
   
   #4、操作数据，更新操作
   import pymysql
    
   # 打开数据库连接
   db = pymysql.connect("localhost","testuser","test123","TESTDB" )
    
   # 使用cursor()方法获取操作游标 
   cursor = db.cursor()
    
   # SQL 更新语句
   sql = "UPDATE EMPLOYEE SET AGE = AGE + 1 WHERE SEX = '%c'" % ('M')
   try:
      cursor.execute(sql)  # 执行SQL语句
      db.commit()          # 提交到数据库执行
   except
      db.rollback()        # 发生错误时回滚
    
   # 关闭数据库连接
   db.close()
   
   #4、操作数据，删除操作
   import pymysql
    
   # 打开数据库连接
   db = pymysql.connect("localhost","testuser","test123","TESTDB" )
    
   # 使用cursor()方法获取操作游标 
   cursor = db.cursor()
    
   # SQL 删除语句
   sql = "DELETE FROM EMPLOYEE WHERE AGE > %s" % (20)
   try
      cursor.execute(sql)  # 执行SQL语句
      db.commit()          # 提交修改
   except
      db.rollback()        # 发生错误时回滚# 关闭连接
   db.close()
   
   #5、数据库的逻辑备份
   #语法：
   # mysqldump -h 服务器 -u用户名 -p密码 数据库名 > 备份文件.sql
   
   #示例：
   #单库备份
   mysqldump -uroot -p123 db1 > db1.sql
   mysqldump -uroot -p123 db1 table1 table2 > db1-table1-table2.sql
   
   #多库备份
   mysqldump -uroot -p123 --databases db1 db2 mysql db3 > db1_db2_mysql_db3.sql
   
   #备份所有库
   mysqldump -uroot -p123 --all-databases > all.sql 
   
   #6、数据恢复
   #方法一：
   [root@egon backup]# mysql -uroot -p123 < /backup/all.sql
   
   #方法二：
   mysql> use db1;
   mysql> SET SQL_LOG_BIN=0;   #关闭二进制日志，只对当前session生效
   mysql> source /root/db1.sql
   
   #7、事务和锁
   begin;  # 开启事务
   select * from emp where id = 1 for update;  # 查询id值，for update添加行锁；
   update emp set salary=10000 where id = 1; # 完成更新
   commit; # 提交事务
   
   
   ```

   

