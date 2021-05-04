1. mysql 的安装、启动和基础配置— windows版本

   1. 下载（来自博客：https://www.cnblogs.com/Eva-J/articles/9669675.html）

      + 第一步：打开网址，https://www.mysql.com，点击downloads之后跳转到https://www.mysql.com/downloads
      + 第二步 ：跳转至网址https://dev.mysql.com/downloads/，选择Community选项
      + 第三步 ：点击MySQL Community Server进入https://dev.mysql.com/downloads/mysql/页面，再点击5.6版本的数据库。推荐使用5.6版本的，这个版本好用。
      + 第四步：windows操作系统 点击5.6版本之后会跳转到https://dev.mysql.com/downloads/mysql/5.6.html#downloads 网址，页面如下，确认好要下载的版本和操作系统，点击Download
      + 第五步：可以不用登陆或者注册，直接点击No thanks,just start my download就可以下载了。

   2. 解压：下载的zip文件解压，将解压之后的文件夹放到任意目录下，这个目录就是mysql的安装目录。

   3. 配置：打开目录，会看到my-default.ini配置文件，复制这个配置文件到pycharm里面，然后重命名为my.ini,pycharm里打开这个文件，把这个文件里面的内容删除，然后把下面这段代码复制进去，把basedir= 后面的地址改成mysql的安装目录，datadir=后面的地址改成mysql数据库的数据data的存放目录，保存，然后复制回去刚刚的位置。

      ```python
      [mysql]
      # 设置mysql客户端默认字符集
      default-character-set=utf8 
      [mysqld]
      #设置3306端口
      port = 3306 
      # 设置mysql的安装目录
      basedir=C:\Program Files\mysql-5.6.39-winx64 
      # 设置mysql数据库的数据的存放目录
      datadir=C:\Program Files\mysql-5.6.39-winx64\data 
      # 允许最大连接数
      max_connections=200
      # 服务端使用的字符集默认为8比特编码的latin1字符集
      character-set-server=utf8
      # 创建新表时将使用的默认存储引擎
      default-storage-engine=INNODB
      ```

   4. 环境变量：在系统变量PATH后面添加: 你的mysql bin文件夹的路径（如C:\Program Files\mysql-5.6.41-winx64\bin）

   5. 安装MySQL服务:以管理员身份打开cmd窗口后，将目录切换到你解压文件的bin目录，输入mysqld install回车运行

   6. 启动mysql服务:以管理员身份在cmd中输入:net start mysql

      服务启动成功之后，就可以登录了，输入mysql -u root -p（第一次登录没有密码，直接按回车过）

2. mysql基本命令语句

   ```python
   #安装mysql服务  mysql服务就被注册到操作系统中
   mysqld install
   
   #启动mysql服务
   net start mysql
   
   #进入mysql客户端
   mysql
   mysql> select user();  #查看当前用户，#注意后面有冒号的就是带着，输入的时候要输入
   mysql> exit     # 也可以用\q quit退出
   
   # 默认用户登陆之后并没有实际操作的权限
   # 需要使用管理员root用户登陆
   mysql -uroot -p   # mysql5.6默认是没有密码的
   #遇到password直接按回车键
   
   # 给当前数据库设置密码
   mysql> set password = password('root'); #引号里面填要设置的密码，不是填root啊。我设置的密码是123  #注意后面有冒号的就是带着，输入的时候要输入
   
   # 创建账号
   mysql> create user 'eva'@'192.168.10.%'   IDENTIFIED BY '123';# 指示这个网段192.168.10. 机器的都可以连接
   mysql> create user 'eva'@'192.168.10.5'   # 指示某机器可以连接
   mysql> create user 'eva'@'%'                    #指示所有机器都可以连接  
   mysql> show grants for 'eva'@'192.168.10.5'; #查看某个用户的权限 
   
   # 远程登陆
   mysql -uroot -p123 -h 192.168.10.3
   
   # 给一个用户授权
   # grant 权限类型 on ftp.* to 'guest'@'192.168.14.%';  #ftp是指某个文件
   # grant all  授权所有
   # grant select  授权查看
   # grant insert  授权修改
   # 创建账号并授权
   mysql> grant all on *.* to 'eva'@'%' identified by '123' 
   
   
   
   
   # 操作数据库
   # 查看所有数据库  
   show databases;
   # 创建一个数据库  
   create database 数据库名;
   # 切换到这个库下  
   use 数据库的名字
   # 查看这个库下有多少表 
   show tables;
   
   # 操作表
   # 创建一张表
   #create table 表名(name char(12),age int); 
   create table student(name char(12),age int);
   # 查看表结构
   # desc student;
   
   # 操作数据
   # 插入数据 : insert into student values ('wusir',73);
   # 查询数据 : select * from student;
   # 修改数据 : update student set age=85 where name='alex';
   # 删除数据 : delete from student where name = 'alex';
   
   
   #来自博客
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
   
   *auto_increment 表示：自增
   *primary key 表示：约束（不能重复且不能为空）；加速查找
   
   ```

   

