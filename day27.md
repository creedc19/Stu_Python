## 一、昨日内容回顾

1. super

   1. super遵循的是mro算法
   2. 只在新式类中能使用
   3. py2新式类中需要自己添加参数(子类名,子类对象)

2. 封装

   1. 广义上的封装
   2. 狭义上的封装 __名字
      1. 私有化：方法名私有化、实例变量私有化、静态变量私有化
      2. 私有化的特点：只能在类的内部使用,不能在外部使用
      3. 私有的各种静态变量和方法能不能继承，不能被子类继承

3. 内置函数

   1. 判断一个变量是不是可调用的,判断这个变量后面能不能加括号，callable(名字)
   2. 装饰器，@property   把一个方法伪装成属性,使它调用的时候不用加括号，给伪装成属性的方法赋值 @函数名.setter装饰器
   3. 反射相关：hasattr ，getattr ，字符串数据类型的变量名,采用getattr(对象,'变量名')获取变量的值

   

## 二、今日内容

1. 内容大纲

   ```python
   # 两个装饰器
       # @classmethod  *****
           # 什么时候用?
           # 怎么定义?
               # 装饰器怎么加
               # 参数怎么改?
           # 怎么用?
               # 用谁来调用?
       # @staticmethod *
           # 什么时候用?
               # 帮助我们把一个普通的函数挪到类中来直接使用,制造静态方法用的
           # 怎么定义?
           # 怎么用?用谁来调用?
   # 一些内置的魔术方法
       # __new__  :
           # 什么时候执行
           # 为什么要有,用来做什么的?   创建一个对象需要的空间的
           # 单例模式(默写)
       # __call__ : 对象() 调用这个类中的__call__方法
       # __len__  : len(对象) 需要实现这个类中的__len__方法
       # __str__  : 帮助我们在打印\展示对象的时候更直观的显示对象内容 %s str() print()
       # __repr__ : repr是str的备胎,同时还和%r和repr有合作关系
       # __enter__:
       # __exit__ :
       # __eq__   :
       # __del__  :
   ```

   

2. 反射的例子

   ```python
   # 文件操作
   class File:
       lst = [('读文件','read'), ('写文件','write'),
              ('删除文件','remove'),( '文件重命名','rename'),
              ('复制','copy'),('移动文件','move')]
       def write(self):
           print('in write func')
       def read(self):
           print('in read func')
       def remove(self):
           print('in remove func')
       def rename(self):
           print('in rename func')
       def copy(self):
           print('in copy func')
       def move(self):
           print('in 移动文件 func')
   
   f = File()
   while True:
       for index,opt in enumerate(File.lst,1):
           print(index,opt[0])
       num = int(input('请输入您要做的操作序号>>'))
       if hasattr(f,File.lst[num-1][1]):
           getattr(f,File.lst[num-1][1])()
   ```

3. classmethod  ：被装饰的方法会成为一个静态方法

   ```python
   class Goods:
       __discount = 0.8
       def __init__(self):
           self.__price = 5
           self.price = self.__price * self.__discount
       def change_discount(self,new_discount): #注意这里  定义了一个方法,默认传self,但这个self没被使用
           Goods.__discount = new_discount
   
   apple = Goods()
   print(apple.price)  #4
   #修改折扣0.6
   apple.change_discount(0.6)  
   apple2 = Goods()
   print(apple2.price)  #3
   
   
   class Goods:
       __discount = 0.8
       def __init__(self):
           self.__price = 5
           self.price = self.__price * self.__discount
       @classmethod   # 把一个对象绑定的方法 修改成一个 类方法
       def change_discount(cls,new_discount):  #这个cls 指向的当前类的名字
           #print(cls,Goods)  #<class '__main__.Goods'> <class '__main__.Goods'>
           cls.__discount = new_discount  #注意这里用的是 cls了，即使类名改了，这里也不用改
   Goods.change_discount(0.6)   # 类方法可以通过类名调用
   apple = Goods()
   print(apple.price)   #3     
   
   apple.change_discount(0.5)  # 类方法可以通过对象名调用
   apple2 = Goods()
   print(apple2.price) #2.5
   
   # @classmethod   # 把一个对象绑定的方法 修改成一个 类方法
   # 第一,在方法中仍然可以引用类中的静态变量
   # 第二,可以不用实例化对象,就直接用类名在外部调用这个方法
   # 什么时候用@classmethod?
       # 1.定义了一个方法,默认传self,但这个self没被使用
       # 2.并且你在这个方法里用到了当前的类名,或者你准备使用这个类的内存空间中的名字的时候
   
   import time
   class Date:
       def __init__(self,year,month,day):
           self.year = year
           self.month = month
           self.day = day
       @classmethod
       def today(cls):
           struct_t = time.localtime()
           date = cls(struct_t.tm_year,struct_t.tm_mon,struct_t.tm_mday)
           return date
   
   date对象 = Date.today()
   print(date对象.year)
   print(date对象.month)
   print(date对象.day)
   
   ```

4. @staticmethod  ：被装饰的方法会成为一个静态方法

   ```python
   class User:
       pass
       @staticmethod
       def login(a,b):      # 本身是一个普通的函数,被挪到类的内部执行,那么直接给这个函数添加@staticmethod装饰器就可以了，不用加self参数
           print('登录的逻辑',a,b)
           # 在函数的内部既不会用到self变量,也不会用到cls类
   
   obj = User()
   User.login(1,2)  # 可以用 类 来调用，也可以用对象来调用，只是不会传self参数
   obj.login(3,4)
   ```

5. 小结一下

   ```python
   # 能定义到类中的内容
   '''
   静态变量 是个所有的对象共享的变量  有对象\类调用 但是不能重新赋值
   绑定方法 是个自带self参数的函数    由对象调用
   类方法   是个自带cls参数的函数     由对象\类调用
   静态方法 是个啥都不带的普通函数    由对象\类调用
   property属性 是个伪装成属性的方法  由对象调用 但不加括号
   '''
   class A:
       country = '中国'  #静态变量
       def func(self):   #绑定方法
           print(self.__dict__)
       @classmethod  #类方法
       def clas_func(cls):
           print(cls)
       @staticmethod  #静态方法
       def stat_func():
           print('普通函数')
       @property  #property属性
       def name(self):
           return 'wahaha'
   ```

6. `__call__`方法 : 对象() 调用这个类中的`__call__`方法

   ```python
   # callable(对象)
   # 对象() 能不能运行就是callable判断的事儿
   
   class A:
       pass
   obj = A()
   print(callable(obj))  #False
   
   
   class A:
       def __call__(self, *args, **kwargs):
           print('666')
   
   obj = A()
   print(callable(obj))  #True
   obj()  #666, 此时 对象() 会调用 __call__ 下面的逻辑
   # Flask框架的源码 ，就用到了这个 ，它是 A()()
   ```

7. `__len__`方法 :len(对象) 需要实现这个类中的`__len__`方法

   ```python
   class Cls:
       def __init__(self,name):
           self.name = name
           self.students = []
   py22 = Cls('py22')
   py22.students.append('杜相玺')
   py22.students.append('庄博')
   py22.students.append('大壮')
   #查看有多少学生
   print(len(py22.students))
   
   class Cls:
       def __init__(self,name):
           self.name = name
           self.students = []
       def __len__(self):
           return len(self.students)
   py22 = Cls('py22')
   py22.students.append('杜相玺')
   py22.students.append('庄博')
   py22.students.append('大壮')
   #查看有多少学生
   print(len(py22))
   ```

8. `__new__`

   ```python
   # 实例化的时候
   # 先创建一块对象的空间,有一个指针能指向类 --> __new__
   # 调用init --> __init__
   
   class A:
       def __new__(cls, *args, **kwargs):   # 构造方法
           o = object.__new__(cls)  # o = super().__new__(cls)
           print('执行new',o)
           return o
       def __init__(self):
           print('执行init',self)
   A() 
   '''
   注意:先执行的是__new__
   执行new <__main__.A object at 0x0000028BEE0FAE80>
   执行init <__main__.A object at 0x0000028BEE0FAE80>
   ''' 
   
   # 设计模式 -- 单例模式
   # 一个类 从头到尾 只会创建一次self的空间
   class Baby:
       __instance = None
       def __new__(cls, *args, **kwargs):
           if cls.__instance is None:
               cls.__instance = super().__new__(cls)
           return cls.__instance
       def __init__(self,cloth,pants):
           self.cloth = cloth
           self.pants = pants
   b1 = Baby('红毛衣','绿皮裤')
   print(b1.cloth) #红毛衣
   b2 = Baby('白衬衫','黑豹纹')
   print(b1,b2) #<__main__.Baby object at 0x000001AAF37EAD30> <__main__.Baby object at 0x000001AAF37EAD30>
   print(b1.cloth) #白衬衫
   print(b2.cloth) #白衬衫
   
   ```

9. `__str__`

   ```python
   # 在打印一个对象的时候 调用__str__方法
   # 在%s拼接一个对象的时候 调用__str__方法
   # 在str一个对象的时候 调用__str__方法
   
   class Course:
       def __init__(self,name,price,period):
           self.name = name
           self.price = price
           self.period = period
   python = Course('python',21800,'6 months')
   print(python) #<__main__.Course object at 0x000001BC40BFAFA0>，这样打印的是内存地址
   linux = Course('linux',19800,'5 months')
   mysql = Course('mysql',12800,'3 months')
   go = Course('go',15800,'4 months')
   lst =[python,linux,mysql,go]
   for index ,c in enumerate(lst,start=1):
       print(index,c.name,c.price,c.period)  #直接打印c 是内存地址
   '''
   1 python 21800 6 months
   2 linux 19800 5 months
   3 mysql 12800 3 months
   4 go 15800 4 months
   '''
   
   class Course:
       def __init__(self,name,price,period):
           self.name = name
           self.price = price
           self.period = period
       def __str__(self):
           return ','.join([self.name,str(self.price),self.period])
   python = Course('python',21800,'6 months')
   print(python) #python,21800,6 months  #直接打印的是课程信息
   linux = Course('linux',19800,'5 months')
   mysql = Course('mysql',12800,'3 months')
   go = Course('go',15800,'4 months')
   lst =[python,linux,mysql,go]
   for index,c in enumerate(lst,start=1):
       print(index,c)
   '''
   1 python,21800,6 months
   2 linux,19800,5 months
   3 mysql,12800,3 months
   4 go,15800,4 months
   '''
   
   class clas:
       def __init__(self):
           self.student = []
       def append(self,name):
           self.student.append(name)
       def __str__(self):
           return str(self.student)
   
   py22 = clas()
   print(py22)  #[]  在打印一个对象的时候 调用__str__方法
   py22.append('大壮')
   print(py22) #['大壮']  
   print('我们py22班 %s'%py22) #我们py22班 ['大壮']   在%s拼接一个对象的时候 调用__str__方法
   print(str(py22)) #['大壮']  在str一个对象的时候 调用__str__方法
   ```

10. `__repr__`

    ```python
    class clas:
        def __init__(self):
            self.student = []
        def append(self,name):
            self.student.append(name)
        def __repr__(self):
            return str(self.student)
        def __str__(self):
            return 'aaa'
    
    py22 = clas()
    py22.append('大壮')
    print(py22) #aaa
    print(str(py22))  #aaa
    print('我们py22班 %s'%py22)  #我们py22班 aaa
    
    print('我们py22班 %r'%py22)  #我们py22班 ['大壮']
    print(repr(py22)) #['大壮']
    # 当我们打印一个对象 用%s进行字符串拼接 或者str(对象)总是调用这个对象的__str__方法
    # 如果找不到__str__,就调用__repr__方法
    # __repr__不仅是__str__的替代品,还有自己的功能
    # 用%r进行字符串拼接 或者用repr(对象)的时候总是调用这个对象的__repr__方法
    
    ```

    

