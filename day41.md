## 一 、内容回顾

1. 多表查询

   1.  连表查

      1. 内连接  必须左表和右表中条件互相匹配的项才会被显示出来 ；表1 inner join 表2 on 条件

      2. 外链接 会显示条件不匹配的项

         left join 左表显示全部,右表中的数据必须和左表条件互相匹配的项才会被显示出来

         right join 右表显示全部,左表中的数据必须和右表条件互相匹配的项才会被显示出来

         全外连接：left join...union...right join

   2. 子查询

      1. select * from 表 where 字段 = (select 字段 from 表 where 条件)
      2. select * from 表 where 字段 > (select 字段 from 表 where 条件)
      3. select * from 表 where 字段 in (select 字段 from 表 where 条件)

## 二、今日内容

1. 内容大纲         参考博客：https://www.cnblogs.com/Eva-J/articles/10126413.html

   ```python
   # 数据准备
   # 读取一次硬盘的时间开销
       # 磁盘的预读性原理
   # 新的数据结构 -- 树
   # mysql中存储数据的两种方式
       # 聚集索引
       # 非聚集索引
   # 索引的创建与删除
       # 创建主键 primary key 聚集索引 + 非空 + 唯一
       # 创建唯一约束 unique  辅助索引 + 唯一
       # 添加一个普通索引
           # create index 索引名 on 表(字段);
           # drop index 索引名 on 表;
   # 正确使用索引
       # 
   
   # 索引
       # 创建
       # 删除
       # 知道用了它会加快查询速度
       
   # 每一次读取硬盘的单位不是你要多少就读多少
   # 每一次读取的数据块的大小都是固定的
   ```

2. 树

   ```python
   # root 根节点
   # branch 分支节点
   # leaf 叶子节点
   
   # 父子节点
   
   # b+树
       # 平衡树(btree-balance tree) 能够让查找某一个值经历的查找速度尽量平衡
       # 分支节点不存储数据 -- 让树的高度尽量矮,让查找一个数据的效率尽量的稳定
       # 在所有叶子结点之间加入了双向的地址链接 -- 查找范围非常快
   
   # 两种索引的差别
   # 聚集索引 聚簇索引
       # Innodb 必有且仅有一个 :主键
   # 非聚集(簇)索引 辅助索引
       # innodb
       # myisam
   
   # innodb存储引擎中的 主键默认就会创建一个聚集索引
   ```

3. 正确使用索引

   ```python
   # 数据库使用的时候有什么注意事项
       # 从搭建数据库的角度上来描述问题
       # 建表的角度上
           # 1.合理安排表关系
           # 2.尽量把固定长度的字段放在前面
           # 3.尽量使用char代替varchar
           # 4.分表: 水平分,垂直分
       # 使用sql语句的时候
           # 1.尽量用where来约束数据范围到一个比较小的程度,比如说分页的时候
           # 2.尽量使用连表查询而不是子查询
           # 3.删除数据或者修改数据的时候尽量要用主键作为条件
           # 4.合理的创建和使用索引
               # 1.查询的条件字段不是索引字段
                   # 对哪一个字段创建了索引,就用这个字段做条件查询
               # 2.在创建索引的时候应该对区分度比较大的列进行创建
                   # 1/10以下的重复率比较适合创建索引
               # 3.范围
                   # 范围越大越慢
                   # 范围越小越快
                   # like 'a%'  快
                   # like '%a'  慢
               # 4.条件列参与计算/使用函数
               # 5.and和or
                   # id name
                   # select * from s1 where id = 1800000 and name = 'eva';
                   # select count(*) from s1 where id = 1800000 or name = 'eva';
                   # 多个条件的组合,如果使用and连接
                       # 其中一列含有索引,都可以加快查找速度
                   # 如果使用or连接
                       # 必须所有的列都含有索引,才能加快查找速度
               # 6.联合索引 : 最左前缀原则(必须带着最左边的列做条件,从出现范围开始整条索引失效)
                   # (id,name,email)
                   # select * from s1 where id = 1800000 and name = 'eva' and email = 'eva1800000@oldboy';
                   # select * from s1 where id = 1800000 and name = 'eva';
                   # select * from s1 where id = 1800000 and email = 'eva1800000@oldboy';
                   # select * from s1 where id = 1800000;
                   # select * from s1 where name = 'eva' and email = 'eva1800000@oldboy';
                   # (email,id,name)
                   # select * from s1 where id >10000 and email = 'eva1800000@oldboy';
               # 7.条件中写出来的数据类型必须和定义的数据类型一致
                   # select * from biao where name = 666   # 不一致
               # 8.select的字段应该包含order by的字段
                   # select name,age from 表 order by age;  # 比较好
                   # select name from 表 order by age;  # 比较差
   ```

4. 索引合并和覆盖索引

   ```python
   # 联合索引
   # (id,email)
   # id = 100 and email like 'eva%';
   
   # 索引合并 :分开创建在查询过程中临时合并成一条 Using union(ind_id,ind_email)
       # 创建索引的时候
       # create index ind_id on s1(id)
       # create index ind_email on s1(email)
       # select * from s1 where id=100 or email = 'eva100@oldboy'
       # 临时把两个索引ind_id和ind_email合并成一个索引
   
   
   # 覆盖索引:在查询过程中不需要回表   Using index
       # 对id字段创建了索引
       # select id from s1 where id =100     覆盖索引:在查找一条数据的时候,命中索引,不需要再回表
       # select count(id) from s1 where id =100     覆盖索引:在查找一条数据的时候,命中索引,不需要再回表
       # select max(id) from s1 where id =100     覆盖索引:在查找一条数据的时候,命中索引,不需要再回表
       # select name from s1 where id =100   相对慢
   
   # 什么是mysql的执行计划?用过explain么?
       # 在执行sql语句之前,mysql进行的一个优化sql语句执行效率的分析(计划),可以看到有哪些索引,实际用到了那个索引,执行的type等级
       # id name email
       # select * from s1 where id = 1000000 and name=eva and email = 'eva1000000@oldboy';
           # 有没有索引
           # 有几个
           # 用哪一个索引比较效率高
       # explain select * from s1 where id = 1000000 and name=eva and email = 'eva1000000@oldboy';
   
   ```

   

