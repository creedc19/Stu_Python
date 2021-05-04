## 一、昨日内容回顾

1. 自定义模块
2. 模块的两种执行方式
   + `__name__` `__file__` , `__all__`
3. 模块导入的多种方式
4. 相对导入
5. random:
   + random.random()
   + random.uniform(a,b)
   + random.randint(a,b)
   + random.shuffle(x)
   + random.sample(x,k)

## 二、今日内容

常用模块介绍：

+ time , datetime
+ os , sys 
+ hashlib , json , pickle , collections



1. #### time模块，和时间相关

   ```python
   """
   time模块 三大对象：
   时间戳
   结构化时间对象（9大字段）
   字符串！！！！
   """
   
   import time
   # 获取时间戳
   # 时间戳：从时间元年（1970 1 1 00:00:00）到现在经过的秒数。
   print(time.time())  # 1611310950.468338
   
   # 获取格式化时间对象:是九个字段组成的。
   # 默认参数是当前系统时间的时间戳。
   print(time.gmtime())  # GMT:格林尼治时间  time.struct_time(tm_year=2021, tm_mon=1, tm_mday=22, tm_hour=10, tm_min=23, tm_sec=28, tm_wday=4, tm_yday=22, tm_isdst=0)
   
   print(time.localtime())  #time.struct_time(tm_year=2021, tm_mon=1, tm_mday=22, tm_hour=18, tm_min=23, tm_sec=28, tm_wday=4, tm_yday=22, tm_isdst=0)
   
   print(time.gmtime(1))       # 时间元年过一秒后，对应的时间对象
   
   # 时间对象 -> 时间戳 （用处不大）
   t1 = time.localtime()   # 时间对象
   t2 = time.mktime(t1)    # 获取对应的时间戳
   print(t2)   #1611311398.0
   print(time.time()) #可通过这个直接获取时间戳  ,1611311398.4540224
   
   # 格式化时间对象和字符串之间的转换
   s = time.strftime("%Y %m %d %H:%M:%S")  #注意大小写区别
   print(s)  #2021 01 22 18:35:58
   
   # 把时间字符串转换成时间对象
   time_obj = time.strptime('2012 01 24','%Y %m %d') #注意一一对应
   time_obj1 = time.strptime('2012 01 24 12:20:30','%Y %m %d %H:%M:%S')
   print(time_obj) #time.struct_time(tm_year=2012, tm_mon=1, tm_mday=24, tm_hour=0, tm_min=0, tm_sec=0, tm_wday=1, tm_yday=24, tm_isdst=-1)
   print(time_obj1) #time.struct_time(tm_year=2012, tm_mon=1, tm_mday=24, tm_hour=12, tm_min=20, tm_sec=30, tm_wday=1, tm_yday=24, tm_isdst=-1)
   
   # 暂停当前程序,睡眠xxx秒 , time.sleep(xxx)
   for x in range(5):
       print(time.strftime('%Y-%m-%d %H:%M:%S'))
       time.sleep(1)  # 休眠一秒钟
       
   for i in range(5):
       print("Hello World",time.strftime("%H:%M:%S"))
       time.sleep(2)
   '''
   Hello World 18:44:27
   Hello World 18:44:29
   Hello World 18:44:31
   Hello World 18:44:33
   Hello World 18:44:35
   '''
   ```

#####       time模块三大对象之间的转换关系：

​      

| ![截图_20213022113024](C:\Users\PengC Wang\Desktop\截图_20213022113024.png) |
| ------------------------------------------------------------ |
|                                                              |

2. #### datetime模块，和日期时间相关

   ```python
   """
   datetime:日期时间模块
   封装了一些和日期,时间相关的类.
   
   date
   time
   datetime
   timedelta
   """
   import datetime
   # date类: 需要年月日三个参数
   d = datetime.date(2012,1,2)
   print(d)  #2012-01-02
   # 获取date对象的各个属性
   print(d.year)  #2012
   print(d.month)  #1
   print(d.day)   #2
   
   # time类:需要时分秒三个参数
   t = datetime.time(18,21,39)
   print(t)  #18:21:39
   #time类的属性
   print(t.hour)   #18
   print(t.minute)  #21
   print(t.second)  #39
   
   # datetime类：需要年月日时分秒六个参数
   dt = datetime.datetime(2020,5,20,18,45,36)
   print(dt)  #2020-05-20 18:45:36
   #datetime类的属性
   print(dt.year)
   print(dt.month)
   print(dt.day)
   print(dt.hour)
   print(dt.minute)
   print(dt.second)
   #datetime中的类,主要是用于数学计算的.
   
   # timedelta:时间的变化量，需要一个时间段，可以是天、秒、微妙等
   td = datetime.timedelta(1)
   print(td)   #1 day, 0:00:00
   
   #timedelta能参与数学运算  ，timedelta只能和以下三类进行数学运算: datetime.date,datetime.datetime,datetime.timedelta。创建时间对象:
   td = datetime.timedelta(1)
   d = datetime.date(2010,10,10)
   res = d - td
   print(res)  #2010-10-09
   
   # 时间变化量的计算是否会产生进位?  会产生进位
   t1 = datetime.datetime(2021,2,26,10,25,59)
   t2 = datetime.timedelta(seconds=5)
   res = t1 + t2
   print(res)  #2021-02-26 10:26:04
   
   # 练习:计算某一年的二月份有多少天.
   # 普通算法:根据年份计算是否是闰年.是:29天,否:28
   
   # 用datetime模块.  首先创建出指定年份的3月1号.然后让它往前走一天.
   year = int(input("输入年份:"))
   # 然后创建指定年份的date对象
   d = datetime.date(year,3,1)
   # 最后创建一天 的时间段
   td = datetime.timedelta(days=1)
   res = d - td
   print(res)
   print(res.day)
   
   # 和时间段进行运算的结果 类型:和另一个操作数保持一致
   t1 = datetime.date(2019,5,17)
   t2 = datetime.timedelta(days=3)
   res = t1 + t2
   print(res,type(res)) #2019-05-20 <class 'datetime.date'>
   
   t3 = datetime.datetime(2021,1,2,12,36,56)
   t4 = datetime.timedelta(seconds=9)
   res = t3 + t4
   print(res,type(res)) # 2021-01-02 12:37:05 <class 'datetime.datetime'>
   
   t5 = datetime.timedelta(days=5)
   t6 = datetime.timedelta(seconds=10)
   res = t5 + t6
   print(res,type(res))  #5 days, 0:00:10 <class 'datetime.timedelta'>
   ```

3. ##### os模块：和操作系统相关的模块，主要是文件删除，目录删除，重命名等操作

   ```python
   """
   os:和操作系统相关的操作被封装到这个模块中.
   """
   import os 
   # 和文件操作相关,重命名,删除
   os.remove('a.txet')  #删除a.txet文件。a.txet文件之前创建好的
   os.rename('a.txet','b.txet')  #重命名，a.txet重命名成b.txet
   
   # 删除目录,必须是空目录
   os.removedirs('xx')  #xx是一个空目录名
   
   # 使用shutil模块可以删除带内容的目录
   import shutil
   shutil.rmtree('aa')
   
   #和路径相关的操作,被封装到另一个子模块中: os.path
   #获取路径
   res = os.path.dirname(r'd:/aa/bb/a.txet')  # 不判断路径是否存在.
   print(res)  #d:/aa/bb
   
   # 获取文件名
   res = os.path.basename(r'd:/aa/bb/a.txet')
   print(res)  #a.txet
   
   # 把路径中的路径名和文件名切分开,结果是元组.
   res = os.path.split(r'd:/aa/bb/a.txet')
   print(res)  #('d:/aa/bb', 'a.txet')
   
   # 拼接路径
   path = os.path.join('d:\\','aa','cc','hh','b.txet')
   print(path) #d:\aa\cc\hh\b.txet
   
   #os.path.abspath
   # 如果是/开头的路径,默认是在当前盘符下.
   res = os.path.abspath(r'/aa/cc')
   print(res) #D:\aa\cc
   # 如果不是以/开头,默认当前路径
   res = os.path.abspath(r'aa/cc' )
   print(res) #D:\programme\projectc\laonanhai\day16\aa\cc
   
   # 判断
   print(os.path.isabs('a.txt'))
   print(os.path.isdir('d:/aaaa.txt')) # 文件不存在.False
   print(os.path.exists('d:/a.txt'))
   print(os.path.isfile('d:/asssss.txt'))      # 文件不存在.False
   ```

4. ##### sys模块：和python解释器相关的操作模块

   ```python
   """
   和python解释器相关的操作模块
   主要有两个方面：
       解释器执行时获取参数：sys.argv[x]
       解释器执行时寻找模块的路径：sys.path
   """
   # 获取命令行方式运行的脚本后面的参数：
   #1、例如有test.py文件，内容如下
   import sys
   print("脚本名:",sys.argv[0])      # 脚本名
   print("第一个参数:",sys.argv[1])      # 第一个参数
   print("第二个参数:",sys.argv[2])      # 第二个参数
   print(type(sys.argv[1])) 
   
   #使用脚本方式运行：python test.py my name，结果如下
   D:\programme\projectc\laonanhai\day16>python test.py my name
   脚本名: test.py
   第一个参数: my
   第二个参数: name
   <class 'str'>
   
   #2、例如有test.py文件，内容如下
   import sys
   arg1 = int(sys.argv[1])
   arg2 = int(sys.argv[2])
   print(arg1 + arg2)
   
   #使用脚本方式运行：python test.py 1 2，结果如下
   D:\programme\projectc\laonanhai\day16>python test.py 1 2
   3
   
   # 解释器寻找模块的路径
   import sys
   print(sys.path)
   
   # 已经加载的模块
   import sys
   print(sys.modules)
   ```

5. json模块

   + JavaScript Object Notation:  java脚本兑现标记语言.
     已经成为一种简单的数据交换格式.
   + 序列化：将内存中的数据转换成字节串，用以保存在文件或通过网络传输，称为序列化过程。（将其他数据格式转换成json字符串的过程）
   + 反序列化：从文件中，网络中获取的数据，转换成内存中原来的数据类型，称为反序列化过程（将其他数据格式转换成json字符串的过程）

   

   +  序列化模块总共只有两种用法，要不就是用于网络传输的中间环节，要不就是文件存储的中间环节，所以json模块总共就有两对四个方法：

     + **用于网络传输：dumps、loads**

     + **用于文件写读：dump、load**

       + ##### json.dumps(obj):将obj转换成json字符串返回到内存中.

       + ##### json.dump(obj,fp):将obj转换成json字符串并保存在fp指向的文件中.

       + ##### json.loads(s):将内存中的json字符串转换成对应的数据类型对象

       + ##### json.load(f):从文件中读取json字符串,并转换回原来的数据类型.

   + 注意：

     + json并不能序列化所有的数据类型:例如:set.
     + 元组数据类型经过json序列化后,变成列表数据类型.
     + json文件通常是一次性写入,一次性读取.但是可以利用文件本身的方式实现:一行存储一个序列化json字符串,在反序列化时,按行反序列化即可.

   内存中的数据：结构化的数据

   + | ![image-20210122220059462](C:\Users\PengC Wang\AppData\Roaming\Typora\typora-user-images\image-20210122220059462.png) |
     | ------------------------------------------------------------ |
     |                                                              |

   磁盘上的数据：线性数据

   + | ![image-20210122220225758](C:\Users\PengC Wang\AppData\Roaming\Typora\typora-user-images\image-20210122220225758.png) |
     | ------------------------------------------------------------ |
     |                                                              |

   序列化比喻：

   + | ![image-20210122220311885](C:\Users\PengC Wang\AppData\Roaming\Typora\typora-user-images\image-20210122220311885.png) |
     | ------------------------------------------------------------ |
     |                                                              |

     ```python
     """
     json模块序列化:
     把内存中的数据,转换成字节或字符串的形式,以便于进行存储或者网络传输.
     
     内存中数据 -> 字节串/字符串 : 序列化
     字节串/字符串 -> 内存中的数据 : 反序列化
     """
     import json
     # json :将数据转换成字符串,用于存储或网络传输.
     s = json.dumps([1,2,3,4])
     print(type(s),s)   #<class 'str'> [1, 2, 3, 4]
     
     t = json.dumps((1,2,3,4))  #元组序列化后,变成列表
     print(t,type(t))  #[1, 2, 3, 4] <class 'str'>
     
     res = json.dumps(10)
     print(type(res),res)  #<class 'str'> 10
     
     dic = {'k1':'v1','k2':'v2','k3':'v3'}
     str_dic = json.dumps(dic)  #序列化：将一个字典转换成一个字符串
     print(type(str_dic),str_dic)  #<class 'str'> {"k3": "v3", "k1": "v1", "k2": "v2"}
     #注意，json转换完的字符串类型的字典中的字符串是由""表示的
     
     res = json.dumps(set('abc'))  # Object of type 'set' is not JSON serializable
     
     # 将json结果写到文件中
     with open('测试文件',encoding='utf-8',mode='at') as f:
         json.dump([1,2,3],f)
         
     # 反序列化    
     res = json.dumps([1,2,5])
     l = json.loads(res)  # 反序列化
     print(type(l),l)  #<class 'list'> [1, 2, 5]
     
     res = json.dumps((1,2,6))  # 元组会变成列表
     l = json.loads(res)  # 反序列化
     print(type(l),l)  #<class 'list'> [1, 2, 6]
     
     # 从文件中反序列化
     with open('测试文件',encoding='utf-8') as f:    #测试文件是上面代码写进去的序列化字符串
         res = json.load(f)
         print(type(res),res)  #<class 'list'> [1, 2, 3]
     
     # json文件通常是一次性写,一次性读.
     # 使用另一种方式,可以实现多次写,多次读：把需要序列化的对象.通过多次序列化的方式, 用文件的write方法,把多次序列化后的json字符串写到文件中.
     with open('b.txt',encoding='utf-8',mode='at') as f :
         f.write(json.dumps([1,5,8]) + '\n')
         f.write(json.dumps([2,5,8]) + '\n')
     #  把分次序列化的json字符串,反序列化回来
     with open('b.txt',encoding='utf-8',mode='rt') as f:
         res1 = json.loads(f.readline().strip())
         print(res1,type(res1))  #[1, 5, 8] <class 'list'>
         res2 = json.loads(f.readline().strip())
         print(res2,type(res2))  #[2, 5, 8] <class 'list'>
     ```

6. pickle模块

   python专用的序列化模块，和json的方法一致.

   ```python
   """
   pickle:
   将Python中所有的数据类型,转换成字节串.称为序列化过程
   将字节串转换成python中数据类型,称为反序列化过程.
   """
   
   import pickle
   b = pickle.dumps([1,2,3])
   print(type(b))  #<class 'bytes'>
   print(b)  #b'\x80\x04\x95\x0b\x00\x00\x00\x00\x00\x00\x00]\x94(K\x01K\x02K\x03e.'
   
   # 保存了元组的数据类型
   b = pickle.dumps((1,2,3))
   print(type(b))   #<class 'bytes'>
   print(b)  #b'\x80\x04\x95\t\x00\x00\x00\x00\x00\x00\x00K\x01K\x02K\x03\x87\x94.'
   b1 = pickle.loads(b)
   print(type(b1),b1)  #<class 'tuple'> (1, 2, 3)
   
   # 所有的数据类型都可以进行序列化
   b = pickle.dumps((12,'fhgj'))
   s = pickle.loads(b)
   print(s)  #(12, 'fhgj')
   
   # 把pickle序列化内容写入文件中
   with open('c.txt',mode='wb') as f:  #注意是以字节方式wb写入，不用指定编码模式
       pickle.dump(('hfjdgsk'),f)
       
   # 从文件中反序列化pickle数据
   with open('c.txt',mode='rb') as f:
       res = pickle.load(f)
       print(res,type(res))  #hfjdgsk <class 'str'>
       
   # 多次pickle数据到同一个文件中
   with open('w.txt',mode='wb') as f :
       pickle.dump([1,2,3],f)
       pickle.dump(('hgf'),f)
       pickle.dump(1256,f)
       pickle.dump({'name':'alex','age':25},f)
   # 从文件中反序列化pickle数据
   with open('w.txt',mode='rb') as f:
       for i in range(4):  #上面写入了4次，这里最多循环4次
           res = pickle.load(f)
           print(res)
   '''
   [1, 2, 3]
   hgf
   1256
   {'name': 'alex', 'age': 25}
   '''
   # pickle常用场景:和json一样,一次性写入,一次性读取. 很少多次写入读取
   
   # json,pickle的比较:
   """
   json:
   1.不是所有的数据类型都可以序列化.结果是字符串.
   2.不能多次对同一个文件序列化.
   3.json数据可以跨语言
   
   pickle:
   1.所有python类型都能序列化,结果是字节串.
   2.可以多次对同一个文件序列化
   3.不能跨语言.
   """
   ```

7. ##### hashlib模块：封装一些用于加密的类

   主要用md5()加密算法，......

   加密的目的:用于判断和验证,而并非解密.

   特点：

   + 把一个大的数据,切分成不同块,分别对不同的块进行加密,再汇总的结果,和直接对整体数据加密的结果是一致的.
   + 单向加密,不可逆.
   + 原始数据的一点小的变化,将导致结果的非常大的差异,'雪崩'效应.

   ```python
   """
   md5加密算法:
   给一个数据加密的三大步骤:
   1.获取一个加密对象
   2.使用加密对象的update,进行加密,update方法可以调用多次
   3.通常通过hexdigest获取加密结果,或digest()方法.
   """
   # 步骤1、获取一个加密对象
   m = hashlib.md5()
   # 步骤2、使用加密对象的update,进行加密
   m.update('abc'.encode('utf-8'))   #m.update(b'abc') 没有中文可以用这钟，有中文必须前面那种，编码模式
   #步骤3、通过hexdigest获取加密结果
   res = m.hexdigest()   # res = m.digest()
   print(res)  #900150983cd24fb0d6963f7d28e17f72
   
   # 给一个数据加密, 怎么验证？ 用另一个数据加密的结果和第一次加密的结果对比.如果结果相同,说明原文相同.如果不相同,说明原文不同.
   
   # 不同加密算法:实际上就是加密结果的长度不同;
   m = hashlib.md5()
   m.update(b'def')
   re = m.hexdigest()
   print(len(re))  #32
   
   s = hashlib.sha224()
   s.update('def'.encode('utf-8'))
   res = s.hexdigest()
   print(len(res))  #56
   
   ss = hashlib.sha256()
   ss.update(b'def')
   ress = ss.hexdigest()
   print(len(ress))  #64
   
   # 在创建加密对象时,可以指定参数,称为加salt.
   m = hashlib.md5(b'asd')
   print(m.hexdigest())  #7815696ecbf1c96e6894b779456d330e
   
   m = hashlib.md5()
   m.update(b'asd')
   res = m.hexdigest()
   print(res)  #7815696ecbf1c96e6894b779456d330e  两者结果一致
   
   #把一个大的数据,切分成不同块,分别对不同的块进行加密,再汇总的结果,和直接对整体数据加密的结果是一致的.
   m = hashlib.md5()
   m.update(b'abc')
   m.update(b'def')
   print(m.hexdigest())  #e80b5017098950fc58aad83c8c14978e
   
   m = hashlib.md5()
   m.update(b'abcdef')
   print(m.hexdigest())  #e80b5017098950fc58aad83c8c14978e  两者结果一致
   
   #不同的加密对象,结果长度不同,长度越长,越耗时.常用的是md5.
   
   # 注册,登录程序:  **********
   import hashlib
   def get_md5(username,passwd):
       m = hashlib.md5()
       m.update(username.encode('utf-8'))
       m.update(passwd.encode('utf-8'))
       return m.hexdigest()
   
   
   def register(username,passwd):
       # 加密
       res = get_md5(username,passwd)
       # 写入文件
       with open('login',mode='at',encoding='utf-8') as f:
           f.write(res)
           f.write('\n')
   
       # username:xxxxxx
   
   def login(username,passwd):
       # 获取当前登录信息的加密结果
       res = get_md5(username, passwd)
       # 读文件,和其中的数据进行对比
       with open('login',mode='rt',encoding='utf-8') as f:
           for line in f:
               if res == line.strip():
                   return True
           else:  #注意这个地方的缩进在哪？？？？
               	return False
   
   while True:
       op = int(input("1.注册 2.登录 3.退出"))
       if op == 3 :
           break
       elif op == 1:
           username = input("输入用户名:")
           passwd = input("输入密码:")
           register(username,passwd)
       elif op == 2:
           username = input("输入用户名:")
           passwd = input("输入密码:")
           res = login(username,passwd)
           if res:
               print('登录成功')
           else:
               print('登录失败')
   ```

8. ##### collections模块

   ```python
   """
   collections模块
   
   namedtuple():命名元组
   defaultdict():默认值字典.
   Counter():计数器
   """
   import collections
   #namedtuple
   Rectangle = collections.namedtuple('rectangle_class',['length','width'])
   r = Rectangle(10,5)
   # 通过属性访问元组的元素
   print(r.length) #10
   print(r.width) #5
   # 通过索引的方式访问元素
   print(r[0]) #10
   print(r[1])  #5
   
   # defautldict:
   d = collections.defaultdict(int,name='ati',age=18)
   print(d['name'])   #ati
   print(d['age'])    #18
   print(d['addr'])  #   0  没有会被添加，{'addr':0} 
   print(d)          #defaultdict(<class 'int'>, {'name': 'ati', 'age': 18, 'addr': 0})
   
   # 自定义函数充当第一个参数:
   # 要求,不能有参数
   def f():
       return 'hello'
   
   d = defaultdict(f,name='Andy',age=10)
   print(d['addr'])  #hello
   print(d)  #defaultdict(<function f at 0x0000020F288F9E50>, {'name': 'Andy', 'age': 10, 'addr': 'hello'})
   
   # Counter:计数器,注意第一个字母大写
   c = collections.Counter('abcdefabccccdd')
   print(c)  #Counter({'c': 5, 'd': 3, 'a': 2, 'b': 2, 'e': 1, 'f': 1})
   print(c.most_common(3))  #[('c', 5), ('d', 3), ('a', 2)] 返回最多的前三个
   
   ```

   



