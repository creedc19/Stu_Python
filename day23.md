## 一、昨日内容回顾

1. 面向对象
   1. 类 ，对象/实例 ，实例化
   2. 类是具有相同属性和相似功能的一类事物
      1. 一个模子\大的范围\抽象
      2. 你可以清楚的知道这一类事物有什么属性,有什么动作  （重点）
      3. 但是你不能知道这些属性具体的值  （重点）
   3. 对象==实例
      1. 给类中所有的属性填上具体的值就是一个对象或者实例
      2. 只有一个类,但是可以有多个对象都是这个类的对象
   4. 实例化
      1. 实例 = 类名()
      2. 首先开辟空间,调用init方法,把开辟的空间地址传递给self参数
      3. init方法中一般完成 : 把属性的值存储在self的空间里 ---对象的初始化
      4. self这个地址会作为返回值,返回给"实例"
   5. 方法 : 定义在类里的函数,并且还带有self参数
   6. 实例变量 : self.名字

## 二、今日内容

1. 作业讲解

   ```python
   # sys.argv练习
   # 写一个python脚本,在cmd里执行
   # python xxx.py 用户名 密码 cp 文件路径 目的地址
   # python xxx.py alex sb cp D:\python_22\day22\1.内容回顾.py D:\python_22\day21
   # python D:\python_22\day23\2.作业讲解_函数相关的.py alex sb rm D:\python_22\day23\6.作业.py
   # python D:\python_22\day23\2.作业讲解_函数相关的.py alex sb rename D:\python_22\day23  D:\python_22\day24
   # python D:\python_22\day23\2.作业讲解_函数相关的.py alex sb move D:\python_22\day23\文件  D:\python_22\day24
   # python D:\python_22\day23\2.作业讲解_函数相关的.py alex sb mkdir D:\python_22\新的文件夹名
   # 在这个基础上实现一个move移动 把一个文件或者文件夹移动到另一个位置
   # 创建文件夹
   
   import os
   import sys
   import shutil
   if len(sys.argv) >= 5:
       if sys.argv[1] =='alex' and sys.argv[2] == 'sb':
           if sys.argv[3] == 'cp' and len(sys.argv) == 6:
               if os.path.exists(sys.argv[4]) and os.path.exists(sys.argv[5]):
                   filename = os.path.basename(sys.argv[4])
                   path = os.path.join(sys.argv[5],filename)
                   shutil.copy2(sys.argv[4],path)
           elif sys.argv[3] == 'rm' and len(sys.argv) == 5:
               if os.path.exists(sys.argv[4]):
                   if os.path.isfile(sys.argv[4]):os.remove(sys.argv[4])
                   else:shutil.rmtree(sys.argv[4])
           elif sys.argv[3] == 'rename'and len(sys.argv) == 6:
               if os.path.exists(sys.argv[4]):
                       os.rename(sys.argv[4],sys.argv[5])
   else:
       print('您输入的命令无效')
       
       
   # 使用walk来计算文件夹的总大小    
   import os
   size = 0
   d = os.walk('D:\programme\projectc\laonanhai')
   for i in d:
       # print(i)  #得到的 i 是由三部分组成
       lu,w1,w2 = i
       for name in w2:
           abs_path = os.path.join(lu,name)  #拼接文件的绝对路径
           # print(abs_path)
           size += os.path.getsize(abs_path)  #获取每个文件的大小
   print(size)  
   
   # 定义一个圆形类,半径是这个圆形的属性,实例化一个半径为5的圆形,一个半径为10的圆形
   # 完成方法
           # 计算圆形面积
           # 计算圆形周长
   from math import pi
   class Circle:
       def __init__(self,r):
           self.r = r
       def area(self):
           return pi * self.r ** 2
       def perimeter(self):
           return pi * self.r * 2
   c = Circle(5)
   print(c.r)
   print(c.area())
   print(c.perimeter())
   
   c.r = 10
   print(c.area())
   print(c.perimeter())    
   
   # 定义一个用户类,用户名和密码是这个类的属性,实例化两个用户,分别有不同的用户名和密码
           # 登陆成功之后才创建用户对象
           # 设计一个方法 修改密码
   '''
   userinfo文件内容:
   alex|123
   wusir|456
   '''
           
   import os
   def login(name,passwd,filepath = 'userinfo'):
       with open(filepath,encoding='utf-8') as f:
           for line in f:
               user,pwd = line.strip().split('|')
               if user == name and passwd == pwd:
                   return True
           else:
               return False
   
   class User:
       def __init__(self,username,password):
           self.user = username
           self.pwd = password
   
       def change_pwd(self):
           oldpwd = input('输入原密码 :')
           newpwd = input('输入新密码 :')
           flag = False
           with open('userinfo',encoding='utf-8') as f1,open('userinfo.bak',mode='w',encoding='utf-8') as f2:
               for line in f1:
                   username,password = line.strip().split('|')
                   if username == self.user and password == oldpwd:
                       line = '%s|%s\n'%(username,newpwd)
                       flag = True
                   f2.write(line)
           os.remove('userinfo')
           os.rename('userinfo.bak','userinfo')
           return flag
   name = input('user : ')
   passwd = input('pwd : ')
   ret = login(name,passwd)
   if ret:
       print('登录成功')
       obj = User(name,passwd)
       res = obj.change_pwd()
       if res:print('修改成功')        
   ```

2. 内存和命名空间

   ```python
   # 类 : 这个类有什么属性 用什么方法 大致的样子
   #      不能知道具体的属性对应的值
   # 对象 :之前所有的属性值就都明确了
   # 类型 :int float str dict list tuple set -- 类（内置的数据类型，内置的类）
   # 变量名 = xx数据类型对象
   # a = 10
   # b = 12.5
   # l = [1,2,3]
   # d = {'k':'v'}
   # o = 函数
   # q = 迭代器
   # u = 生成器
   # i = 类名
   
   # python中一切皆对象,对象的类型就是类
   
   # 所有的对象都有一个类型,class A实例化出来的对象的类型就是A类
   # 123的类型是int/float
   # 'ajksfk'的类型是str
   # {}的类型是dict
   # alex = Person()的类型是Person
   # 小白 = Dog()的类型是Dog
   
   # def abc():pass
   # print(type(abc))
   
   
   # 类的成员和命名空间
   class A:
       Country = '中国'     # 静态变量/静态属性 存储在类的命名空间里的
       def __init__(self,name,age):  # 绑定方法 存储在类的命名空间里的
           self.name = name
           self.age = age
       def func1(self):
           print(self)
       def func2(self):pass
       def func3(self):pass
       def func4(self):pass
       def func5(self):pass
   
   a = A('alex',83)
   print(a.name)
   print(a.Country)
   
   print(A.Country)
   a.func1() # == A.func1(a)
   
   class A:
       Country = '中国'     # 静态变量/静态属性 存储在类的命名空间里的
       def __init__(self,name,age,country):  # 绑定方法 存储在类的命名空间里的
           self.name = name
           self.age = age
           self.Country = country
       def func1(self):
           print(self)
       def func2(self):pass
       def func3(self):pass
       def func4(self):pass
       def func5(self):pass
   
   a = A('alex',83,'印度')
   print(a.name)
   print(a.Country)
   print(A.Country)
   
   class A:
       Country = '中国'     # 静态变量/静态属性 存储在类的命名空间里的
       def __init__(self,name,age,country):  # 绑定方法 存储在类的命名空间里的
           self.name = name
           self.age = age
           self.country = country
       def func1(self):
           print(self)
       def func2(self):pass
       def func3(self):pass
       def func4(self):pass
       def func5(self):pass
   
   a = A('alex',83,'印度')
   b = A('wusir',74,'泰国人')
   a.Country = '日本人'
   print(a.Country)
   print(b.Country)
   print(A.Country)
   
   class A:
       Country = '中国'     # 静态变量/静态属性 存储在类的命名空间里的
       def __init__(self,name,age,country):  # 绑定方法 存储在类的命名空间里的
           self.name = name
           self.age = age
       def func1(self):
           print(self)
       def func2(self):pass
       def func3(self):pass
       def func4(self):pass
       def func5(self):pass
   
   a = A('alex',83,'印度')
   b = A('wusir',74,'泰国人')
   A.Country = '日本人'
   print(a.Country)
   print(b.Country)
   print(A.Country)
   
   # 类中的变量是静态变量
   # 对象中的变量只属于对象本身,每个对象有属于自己的空间来存储对象的变量
   # 当使用对象名去调用某一个属性的时候会优先在自己的空间中寻找,找不到再去对应的类中寻找
   # 如果自己没有就引用类的,如果类也没有就报错
   # 对于类来说,类中的变量所有的对象都是可以读取的,并且读取的是同一份变量
   
   # 实现一个类,能够自动统计这个类实例化了多少个对象
   class A:
       count = 0
       def __init__(self):
           A.count += 1
   
   a1 = A()
   print(a1.count)
   a2 = A()
   print(A.count)
   
   # 类中的静态变量的用处
   # 如果一个变量 是所有的对象共享的值,那么这个变量应该被定义成静态变量
   # 所有和静态变量相关的增删改查都应该使用类名来处理
   # 而不应该使用对象名直接修改静态变量
   ```

3. 组合

   ```python
   # 组合
       # 一个类的对象是另外一个类对象的属性
   
   # 学生类
       # 姓名 性别 年龄 学号 班级 手机号
   # 班级信息
       # 班级名字
       # 开班时间
       # 当前讲师
       
   class Student:
       def __init__(self,name,sex,age,number,clas,phone):
           self.name = name
           self.sex = sex
           self.age = age
           self.number = number
           self.clas = clas
           self.phone = phone
   class Clas:
       def __init__(self,cname,begint,teacher):
           self.cname = cname
           self.begint = begint
           self.teacher = teacher
   # 查看的是大壮的班级的开班日期是多少？
   # 查看的是雪飞的班级的开班日期是多少 ？
   py22 = Clas('python全栈22期','2019-4-26','小白')
   py23 = Clas('python全栈23期','2019-5-28','宝元')
   大壮 = Student('大壮','male',18,27,py23,13812012012)
   雪飞 = Student('雪飞','male',18,17,py22,13812012013)
   print(大壮.clas,py23)
   print(py23.begint)
   print(大壮.clas.begint)
   
   # 练习 :
   # 对象变成了一个属性
   # 班级类
       # 包含一个属性 - 课程
   # 课程
       # 课程名称
       # 周期
       # 价格
   
   # 创建两个班级 linux57
   # 创建两个班级 python22
   # 查看linux57期的班级所学课程的价格
   # 查看python22期的班级所学课程的周期
   
   class Clas:
       def __init__(self,cname,begint,teacher):
           self.cname = cname
           self.begint = begint
           self.teacher = teacher
   class Course:
       def __init__(self,name,period,price):
           self.name = name
           self.period = period
           self.price = price
   py22 = Clas('python全栈22期','2019-4-26','小白')
   linux57 = Clas('linux运维57期','2019-3-27','李导')
   linux58 = Clas('linux运维58期','2019-6-27','李导')
   python = Course('python','6 months',21800)
   linux = Course('linux','5 months',19800)
   
   py22.course = python  #py22 的 course  和 python这个对象就关联起来了
   linux57.course = linux
   linux58.course = linux
   
   print(py22.course.period)
   print(linux57.course.price)
   
   #组合的作用：方便统一修改价格，如果在每个班级对象里面设置价格 ，然后就需要一个一个修改
   linux.price = 21800   #组合方便能同时修改这两期的价格，形成了一种绑定关系
print(linux57.course.price)
   print(linux58.course.price)
   ```
   
   

