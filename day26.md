## 一、内容回顾

1. 类的种类

   1. 新式类 : 继承object,存在在py2,py3(py3中都是新式类,py2里主动继承object的才是新式类)
   2. 经典类 : 只在py2中,不继承object默认是经典类

2. 继承顺序

   1. 深度优先 : 经典类
   2. 广度优先 : 新式类
      1. 查看广度优先的顺序 : 类名.mro()
      2. 遵循的算法 :C3

3. 抽象类

   1. 为什么要用抽象类 : 为了规范子类必须实现和父类的同名方法

   2. 抽象类用到的格式(至少会写一种,两种见到了都要认识)

             #不需要模块的
             # class 父类:
             #     def 子类必须实现的方法名(self,参数们):
             #         raise NotImplementedError('提示信息')
             # class 子类(父类):
             #     def 父类要求实现的方法(self,参数们):
             #         print('''code''')
         
         
         
              #需要模块的
              # from abc import ABCMeta,abstractmethod
              # class 父类(metaclass = ABCMeta):
              #     @abstractmethod
              #     def 子类必须实现的方法名(self,参数们):pass
              # class 子类(父类):
              #     def 父类要求实现的方法(self,参数们):
              #         print('''code''')

4. 归一化设计

   ```
   class A:
   	def 同名功能(self):pass
   class B:
       def 同名功能(self):pass
       
   def 函数名(obj):
   	obj.同名功能()
   ```

5. 多态

   1. 什么是多态 : 一个类表现出的多种形态,实际上是通过继承来完成的

      1. 如果狗类继承动物类,猫类也继承动物类，那么我们就说猫的对象也是动物类型的，狗的对象也是动物类型的，在这一个例子里,动物这个类型表现出了猫和狗的形态。

   2. java中的多态是什么样

      ```
      def eat(动物类型 猫的对象/狗的对象,str 食物):
      	print('动物类型保证了猫和狗的对象都可以被传递进来')
      ```

   3. python中的多态是什么样 : 处处是多态

   4. 鸭子类型

      1. 子类继承父类,我们说子类是父类类型的(猫类继承动物,我们说猫也是动物)，在python中,一个类是不是属于某一个类型，不仅仅可以通过继承来完成，还可以是不继承,但是如果这个类满足了某些类型的特征条件，我们就说它长得像这个类型,那么他就是这个类型的鸭子类型。

   5. 就记住两件事儿:

      1. 所有的类都必须继承object
      2. 如果见到了抽象类的写法,一定要知道要在子类中实现同名方法

## 二、今日内容

1. 内容大纲

   ```
   # super
       # 在py3中怎么用?在py2(新式类/经典类)中怎么用?
       # 在单继承中执行父类的同名方法的时候怎么用?
       # super方法和mro方法的关系是什么?
   # 封装
       # 广义上的封装
       # 狭义上的封装
           # 使用私有的三种情况
               # 不想让你看也不想让你改
               # 可以让你看 但不让你改
               # 可以看也可以改 但是要求你按照我的规则改
           # 封装的语法
               # 私有的静态变量
               # 私有的实例变量
               # 私有的绑定方法
           # 私有的特点
               # 能不能类的内部使用?
               # 能不能类的外部使用?
               # 能不能类的子类中使用?
           # 原理
               # 如何变形?
               # 在哪里定义的时候变形?
           # 类中变量的级别,哪些是python支持的,哪些是python不支持的
               # 公有的
               # 保护的
               # 私有的
   # 类中的三个装饰器(内置函数)
       # property
           # 做了什么?
           # setter
           # delter(自选是否要了解)
       # classmethod
       # staticmethod
   # 反射
       # hasattr
       # getattr
       # 几种情况
           # 反射模块中的内容
           # 反射本文件中的内容
           # 反射对象的属性或者绑定方法
           # 反射类的静态变量
       # callable
       # 实际的例子 - 归一化设计的
   ```

2. super方法

   ```Python
   class A(object):
       def func(self):
           print('A')
   class B(A):
       def func(self):
           super().func()
           print('B')
   class C(B):
       def func(self):
           super().func()
           print('C')
   C().func()  # 打印结果：A B C
   
   
   class A(object):
       def func(self):
           print('A')
   class B(A):
       def func(self):
           super().func()
           print('B')
   class C(A):
       def func(self):
           super().func()
           print('C')
   class D(B,C):
       def func(self):
           super().func()  #或者super(D,self).func()
           print('D')
   D().func()   # 打印结果A  C  B  D
   
   # super是按照mro顺序来寻找当前类的下一个类
   # 在py3中不需要传参数,自动就帮我们寻找当前类的mro顺序的下一个类中的同名方法
   # 在py2中的新式类中,需要我们主动传递参数super(子类的名字,子类的对象).函数名()
   # 这样才能够帮我们调用到这个子类的mro顺序的下一个类中的方法
   # 在py2的经典类中,并不支持使用super来找下一个类
   
   # 在D类中找super的func,那么可以这样写 super().func()
   # 也可以这样写 super(D,self).func() (并且在py2的新式类中必须这样写)
   
   # 在单继承的程序中,super就是找父类，（重点，记住下面这个例子）
   class User:
       def __init__(self,name):
           self.name = name
   class VIPUser(User):
       def __init__(self,name,level,strat_date,end_date):
           super().__init__(name) #推荐当前这种。或者super(VIPUser,self).__init__(name) 或者User.__init__(self,name)
           self.level = level
           self.strat_date = strat_date
           self.end_date = end_date
   
   太白 = VIPUser('太白',6,'2019-01-01','2020-01-01')
   print(太白.__dict__)
   ```

3. 封装：就是把属性或者方法装起来

   + 广义 :  把属性和方法装起来,外面不能直接调用了,要通过类的名字来调用

   + 狭义 :  把属性和方法藏起来,外面不能调用,只能在内部偷偷调用

     ```python
     #给一个名字前面加上了双下划綫的时候,这个名字就变成了一个私有的
     # 所有的私有的内容或者名字都不能在类的外部调用,只能在类的内部使用了
     
     # 私有的实例变量/私有的对象属性，不想让你看也不想让你改
     class User:
         def __init__(self,name,passwd):
             self.usr = name
             self.__pwd = passwd  # 加双下划线，私有的实例变量/私有的对象属性
     alex = User('alex','sbsbsb')
     print(alex.__pwd)  # 报错，不能调用
     print(alex.pwd)    # 报错，不能调用
     
     # 私有的实例变量/私有的对象属性，可以让你看 但不让你改
     class User:
         def __init__(self,name,passwd):
             self.usr = name
             self.__pwd = passwd  # 私有的实例变量/私有的对象属性
         def get_pwd(self):       # 表示的是用户不能改只能看 ， 私有 + 某个get方法实现的
             return self.__pwd
     alex = User('alex','sbsbsb')
     print(alex.get_pwd()) #sbsbsb   可以通过这样来查看密码  ，不能改
     
     # 私有的实例变量/私有的对象属性，可以看也可以改 但是要求你按照我的规则改
     class User:
         def __init__(self,name,passwd):
             self.usr = name
             self.__pwd = passwd  # 私有的实例变量/私有的对象属性
         def get_pwd(self):       # 表示的是用户不能改只能看 ， 私有 + 某个get方法实现的
             return self.__pwd
         def change_pwd(self):    # 表示用户必须调用我们自定义的修改方式来进行变量的修改 私用 + 某个change方法实现，还没写这个方法而已
             pass
         
     alex = User('alex','sbsbsb')
     print(alex.get_pwd()) #sbsbsb   可以通过这样来查看密码  ，不能改
     
     # 私有的静态变量
     class User:
         __Country = 'China'   # 私有的静态变量
     print(User.Country)  # 报错 在类的外部不能调用
     print(User.__Country)# 报错 在类的外部不能调用
     
     class User:
         __Country = 'China'   # 私有的静态变量
         def func(self):
             print(User.__Country)  # 在类的内部可以调用
     User().func()   #China
     
     # 私有的绑定方法
     import  hashlib
     class User:
         def __init__(self,name,passwd):
             self.usr = name
             self.__pwd = passwd  # 私有的实例变量
         def __get_md5(self):     # 私有的绑定方法
             md5 = hashlib.md5(self.usr.encode('utf-8'))
             md5.update(self.__pwd.encode('utf-8'))
             return md5.hexdigest()
         def getpwd(self):
             return self.__get_md5()
     alex = User('alex','sbsbsb')
     alex.__get_md5()  #报错
     print(alex.getpwd())  #d6170374823ac53f99e7647bab677b92
     
     # 所有的私有化都是为了让用户不在外部调用类中的某个名字
     # 如果完成私有化 那么这个类的封装度就更高了 封装度越高各种属性和方法的安全性也越高 但是代码越复杂
     
     # 加了双下划线的名字为啥不能从类的外部调用了?
     class User:
         __Country = 'China'   # 私有的静态变量
         __Role = '法师'   # 私有的静态变量
     print(User.__dict__)
     '''
     打印结果：
     {'__module__': '__main__', '_User__Country': 'China', '_User__Role': '法师', '__dict__': <attribute '__dict__' of 'User' objects>, '__weakref__': <attribute '__weakref__' of 'User' objects>, '__doc__': None}
     
     有没发现：
     __Country -->'_User__Country': 'China'
     __Role    -->'_User__Role': '法师'
     '''    
     #所以可以通过这样来调用
     print(User._User__Country) #China
     print(User._User__Role)  #法师
     User.__aaa = 'bbb'  # 在类的外部根本不能定义私有的概念
     
     class User:
         __Country = 'China'   # 私有的静态变量
         __Role = '法师'   # 私有的静态变量
         def func(self):
             print(self.__Country)  # 在类的内部使用的时候,自动的把当前这句话所在的类的名字拼在私有变量前完成变形
             
     # 私有的内容能不能被子类使用呢? 不能
      class Foo(object):
         def __init__(self):
             self.func()
         def func(self):
             print('in Foo')
     class Son(Foo):
         def func(self):
             print('in Son')
     Son()   #in Son
     
     class Foo(object):
         def __init__(self):
             self.__func()
         def __func(self):
             print('in Foo')
     class Son(Foo):
         def __func(self):
             print('in Son')
     Son()  #in Foo
     
     class Foo(object):
         def __func(self):
             print('in Foo')
     class Son(Foo):
         def __init__(self):
             self.__func()
     Son()  #报错
     
     # 在其他语言中的数据的级别都有哪些?在python中有哪些?
     # public  公有的 类内类外都能用,父类子类都能用         python支持
     # protect 保护的 类内能用,父类子类都能用,类外不能用    python不支持
     # private 私有的 本类的类内部能用,其他地方都不能用     python支持
     ```

4. property

   ```python
   from math import pi
   class Circle:
       def __init__(self,r):
           self.r = r
       def area(self):
           return pi * self.r**2
   c1 = Circle(5)
   print(c1.r)
   print(c1.area()) 
   # 变量的属性和方法?
       # 属性 :圆形的半径\圆形的面积,            但是上面的面积定义成方法了
       # 方法 :登录  注册
   
   from math import pi    
   class Circle:
       def __init__(self,r):
           self.r = r
   
       @property   # 把一个方法伪装成一个属性,在调用这个方法的时候不需要加()就可以直接得到返回值
       def area(self):
           return pi * self.r**2
   c1 = Circle(5)
   print(c1.r)
   print(c1.area)  #注意这个是没有括号，给人的感觉就是通过属性来调用
   
   # property应用场景
   import time
   class Person:
       def __init__(self,name,birth):
           self.name = name
           self.birth = birth
       @property
       def age(self):   # 装饰的这个方法 不能有参数  ****
           return time.localtime().tm_year - self.birth
   
   太白 = Person('太白',1998)
   print(太白.age)
   
   # property的第二个应用场景 : 和私有的属性合作的
   class User:
       def __init__(self,usr,pwd):
           self.usr = usr
           self.__pwd = pwd
       @property
       def pwd(self):
           return self.__pwd
   
   alex = User('alex','sbsbsb')
   print(alex.pwd)
   
   class Goods:
       discount = 0.8
       def __init__(self,name,origin_price):
           self.name = name
           self.__price = origin_price
       @property
       def price(self):
           return self.__price * self.discount
   
   apple = Goods('apple',5)
   print(apple.price)
   
   # property进阶（了解）
   class Goods:
       discount = 0.8
       def __init__(self,name,origin_price):
           self.name = name
           self.__price = origin_price
       @property
       def price(self):
           return self.__price * self.discount
       @price.setter   #必须是@price. 要和上面那个同名
       def price(self,new_value):  #这个是用来修改价格的，必须定义一个和上面同名的。也就是说它们三者必须相同
           self.__price = new_value
   
   apple = Goods('apple',5)
   print(apple.price)   # 调用的是被@property装饰的price
   apple.price = 10     # 调用的是被setter装饰的price
   print(apple.price)
   
   class Goods:
       discount = 0.8
       def __init__(self,name,origin_price):
           self.name = name
           self.__price = origin_price
       @property
       def price(self):
           return self.__price * self.discount
       @price.setter  
       def price(self,new_value): 
           if isinstance(new_value,int):  #这里这样设置意思就是 你必须按照我的要求修改。price没变成私有变量的话，修改的时候就可以是任意数据类型！！！
               self.__price = new_value
   
   apple = Goods('apple',5)
   print(apple.price)   
   apple.price = '10'    #这样就不能修改   
   print(apple.price)
   
   class Goods:
       discount = 0.8
       def __init__(self,name,origin_price):
           self.name = name
           self.__price = origin_price
       @property
       def price(self):
           return self.__price * self.discount
   
       @price.setter
       def price(self,new_value):
           if isinstance(new_value,int):
               self.__price = new_value
   
       @price.deleter   #删除，注意 也必须和上面保持一致。
       def price(self):
           del self.__price
   apple = Goods('apple',5)
   print(apple.price)
   del apple.price   # 并不能真的删除什么,只是调用对应的被@price.deleter装饰的方法而已，方法里面写了  就可以删除价格
   print(apple.price)
   ```

5. 反射：用字符串数据类型的名字 来操作这个名字对应的函数\实例变量\绑定方法\各种方法

   ```python
   #用户输入变量名，打印对应的值
   name = 'alex'
   age = 123
   n = input('>>>')
   if n == 'name':print(name)
   elif n == 'age':print(age)
   # 有些时候 你明明知道一个变量的字符串数据类型的名字,你想直接调用它,但是调不到>>>使用反射
   
   # 1.反射对象的 实例变量 /绑定方法
   # 2.反射类的 静态变量/其他方法
   # 3.反射模块中的 所有变量
       # 被导入的模块
       # 当前执行的py文件 - 脚本
    
   
   class Person:
   	def __init__(self,name,age):
           self.name = name
           self.age = age
   al = Person('alex',83)
   wu = Person('wusir',74)
   ret = getattr(al,'name')
   print(ret)  #alex
   ret = getattr(wu,'name')
   print(ret)  #wusir
   
   class Person:
   	def __init__(self,name,age):
           self.name = name
           self.age = age
       def qqxing(self):
            print('qqxing')
   wusir = Person('wusir',74)
   ret = getattr(wusir,'qqxing')
   print(ret)  #<bound method Person.qqxing of <__main__.Person object at 0x00000231DB357F10>>
   print(wusir.qqxing) #<bound method Person.qqxing of <__main__.Person object at 0x00000231DB357F10>>   这两个的地址，结果一样啊
   
   ret() #qqxing
   
   #反射对象的 实例变量/绑定方法   反射类的 静态变量
   class A:
       Role = '治疗'
       def __init__(self):
           self.name = 'alex'
           self.age = 84
       def func(self):
           print('wahaha')
           return 666
   
   a = A()
   print(getattr(a,'name')) # 反射对象的实例变量
   print(getattr(a,'func')()) # 反射对象的绑定方法，注意最后的括号
   print(getattr(A,'Role')) #反射类的 静态变量
   
   #反射模块中的 所有变量
   '''
      a.py 文件内容
      class Wechat:pass
      def sww():
      	print('爽歪歪')
   
      lst = [1,2,3,4,5]
      dic = {'k','v'}
      we = Wechat()
   '''
   import a   # 引用模块中的任意的变量
   print(getattr(a,'sww'),a.sww)
   getattr(a,'sww')()  #爽歪歪
   print(getattr(a,'lst'),a.lst) #[1,2,3,4,5] [1,2,3,4,5]
   print(getattr(a,'dic'),a.dic)
   print(getattr(a,'we'),a.we)
   
   import sys # 反射本模块中的名字
   cat = '小a'
   dog = '小b'
   def pig():
       print('小p')
   print(getattr(sys.modules['__main__'],'cat')) #小a
   print(getattr(sys.modules['__main__'],'dog')) #小b
   getattr(sys.modules['__main__'],'pig')()  #小p
   
   
   #反射的例子：
   class Payment:pass
   class Alipay(Payment):
       def __init__(self,name):
           self.name = name
       def pay(self,money):
           dic = {'uname':self.name,'price':money}
           print('%s通过支付宝支付%s钱成功'%(self.name,money))
   
   class WeChat(Payment):
       def __init__(self,name):
           self.name = name
       def pay(self,money):
           dic = {'username':self.name,'money':money}
           print('%s通过微信支付%s钱成功'%(self.name,money))
   
   class Apple(Payment):
       def __init__(self,name):
           self.name = name
       def pay(self,money):
           dic = {'name': self.name, 'number': money}
           print('%s通过苹果支付%s钱成功' % (self.name, money))
   
   class QQpay:
       def __init__(self,name):
           self.name = name
       def pay(self,money):
           print('%s通过qq支付%s钱成功' % (self.name, money))
   import sys
   def pay(name,price,kind):
       class_name = getattr(sys.modules['__main__'],kind)
       obj = class_name(name)
       obj.pay(price)
       '''
       之前是下面这样写的，现在用反射写只需要上面三行代码就可以实现 不同的支付方式
       if kind == 'Wechat':
           obj = WeChat(name)
       elif kind == 'Alipay':
           obj = Alipay(name)
       elif kind == 'Apple':
           obj = Apple(name)
       obj.pay(price)
       '''
   pay('alex',400,'WeChat')
   pay('alex',400,'Alipay')
   pay('alex',400,'Apple')
   pay('alex',400,'QQpay') 
   
   
   #反射的另一个函数
   class A:
       Role = '治疗'
       def __init__(self):
           self.name = 'alex'
           self.age = 84
       def func(self):
           print('wahaha')
           return 666
   
   a = A()
   print(hasattr(a,'sex'))  #False
   print(hasattr(a,'age'))  #True
   print(hasattr(a,'func')) #True
   
   if hasattr(a,'name'): #先写这个能防止报错
       print(getattr(a,'name'))
   
   if hasattr(a,'func'):
       print(getattr(a,'func')) #<bound method A.func of <__main__.A object at 0x0000029FD0287FD0>>
       getattr(a,'func')() #wahaha   
       
       
   if hasattr(a,'age'):
       print(getattr(a,'age'))
       getattr(a,'age')()  #age不是方法，这样写会报错，怎样防止呢，看下面
       
   #判断一个东西可不可以调用
   print(callable(a.age)) #False
   print(callable(a.func)) #True
   
   if hasattr(a,'age'):
       if callable(getattr(a,'age')): #先判断一下
           getattr(a,'age')()
   ```



