## 一、昨日内容回顾

1. 命名空间
   1. 在类的命名空间里 : 静态变量  、 绑定方法
   2. 在对象的命名空间里 : 类指针, 对象的属性(实例变量)
   3. 调用的习惯
      1. 类名.静态变量
      2. 对象.静态变量 (对象调用静态变量的时候,不能对变量进行赋值操作 对象.静态变量 = 1UI27)
      3. 绑定方法
      4. 对象.绑定方法() # ==> 类名.绑定方法(对象)
      5. 对象.实例变量
2. 组合
   1. 一个类的对象是另一个类对象的属性
      1. 两个类之间 有 什么有什么的关系 : 班级有学生 学生有班级 班级有课程 图书有作者 学生有成绩

## 二、今日内容

1. 作业讲解

   ```python
   # 定义一个圆形类,半径是这个圆形的属性,实例化一个半径为5的圆形,一个半径为10的圆形
   # 完成方法
           # 计算圆形面积
           # 计算圆形周长
   #上面这个题升级  计算环形面积！！！
   from math import pi
   class Circle:
       def __init__(self,r):
           self.r = r
   
       def area(self):
           return pi*self.r**2
   
       def perimeter(self):
           return 2*pi*self.r
   
   class Ring:
       def __init__(self,outer_r,inner_r):
           outer_r,inner_r = (outer_r,inner_r) if outer_r > inner_r else (inner_r,outer_r)
           self.out_c  = Circle(outer_r)
           self.in_c = Circle(inner_r)
   
       def area(self):
           return self.out_c.area() - self.in_c.area()
   
       def perimeter(self):
           return self.out_c.perimeter() + self.in_c.perimeter()
   
   r = Ring(10,8)
   print(r.area())  #Ring类的方法调用
print(r.perimeter()) #Ring类的方法调用
   ```
   
2. 继承

   ```python
   # 猫
       # 吃
       # 喝
       # 睡
       # 爬树
   # 狗
       # 吃
       # 喝
       # 睡
       # 看家
       
   class Cat:
       def __init__(self,name):
           self.name = name
       def eat(self):
           print('%s is eating'%self.name)
       def drink(self):
           print('%s is drinking'%self.name)
       def sleep(self):
           print('%s is sleeping'%self.name)
       def climb_tree(self):
           print('%s is climbing'%self.name)
   
   class Dog:
       def __init__(self,name):
           self.name = name
       def eat(self):
           print('%s is eating'%self.name)
       def drink(self):
           print('%s is drinking'%self.name)
       def sleep(self):
           print('%s is sleeping'%self.name)
       def house_keep(self):
           print('%s house keeping'%self.name)
   
   小白 = Cat('小白')
   小白.eat()
   小白.drink()
   小白.climb_tree()
   
   小黑 = Dog('小黑')
   小黑.eat()
   小黑.drink()
   小黑.house_keep()  
   
   #上面的代码，有没发现有好多重复的代码
   
   # 继承 -- 需要解决代码的重复
   # 继承语法
   '''
   class A:
       pass
   class B(A):
       pass
   '''
   # B继承A,A是父类,B是子类
   # A是父类 基类 超类
   # B是子类 派生类
   
   # 子类可以使用父类中的 : 方法 静态变量
   class Animal:
       def __init__(self,name):
           self.name = name
       def eat(self):
           print('%s is eating'%self.name)
       def drink(self):
           print('%s is drinking'%self.name)
       def sleep(self):
           print('%s is sleeping'%self.name)
   class Cat(Animal):
       def climb_tree(self):
           print('%s is climbing'%self.name)
   
   class Dog(Animal):
       def house_keep(self):
           print('%s house keeping'%self.name)
   
   小白 = Cat('小白')
   	# 先开辟空间,空间里有一个类指针-->指向Cat
       # 调用init,对象在自己的空间中找init没找到,到Cat类中找init也没找到,
       # 找父类Animal中的init
   小白.eat()  #小白 is eating
   小白.climb_tree()  #小白 is climbing
   小黑 = Dog('小黑')
   小黑.eat()    #小黑 is eating
   
   # 当子类和父类的方法重名的时候,我们只使用子类的方法,而不会去调用父类的方法了
   class Animal:
       def __init__(self,name):
           self.name = name
       def eat(self):
           print('%s is eating'%self.name)
       def drink(self):
           print('%s is drinking'%self.name)
       def sleep(self):
           print('%s is sleeping'%self.name)
   
   class Cat(Animal):
       def eat(self):
           print('%s吃猫粮'%self.name)
   
       def climb_tree(self):
           print('%s is climbing'%self.name)
   
   小白 = Cat('小白')
   小白.eat()
   
   # 子类想要调用父类的方法的同时还想执行自己的同名方法
   # 猫和狗在调用eat的时候既调用自己的也调用父类的,
   # 在子类的方法中调用父类的方法 :父类名.方法名(self) 
   class Animal:
       def __init__(self,name,food):
           self.name = name
           self.food = food
           self.blood = 100
           self.waise = 100
       def eat(self):
           print('%s is eating %s'%(self.name,self.food))
       def drink(self):
           print('%s is drinking'%self.name)
       def sleep(self):
           print('%s is sleeping'%self.name)
   
   class Cat(Animal):
       def eat(self):
           self.blood += 100
           Animal.eat(self)
       def climb_tree(self):
           print('%s is climbing'%self.name)
           self.drink()
   
   class Dog(Animal):
       def eat(self):
           self.waise += 100
           Animal.eat(self)  #此处就填self
       def house_keep(self):
           print('%s is keeping the house'%self.name)
   小白 = Cat('小白','猫粮')
   小黑 = Dog('小黑','狗粮')
   小白.eat()
   小黑.eat()
   print(小白.__dict__)
   print(小黑.__dict__)
   
   # 继承语法 class 子类名(父类名):pass
   # 父类和子类方法的选择:
       # 子类的对象,如果去调用方法
       # 永远优先调用自己的
           # 如果自己有 用自己的
           # 自己没有 用父类的
           # 如果自己有 还想用父类的 : 直接在子类方法中调父类的方法 父类名.方法名(self)
   
   # 思考一:下面代码的输出?
   class Foo:
       def __init__(self):
           self.func()   # 在每一个self调用func的时候,我们不看这句话是在哪里执行,只看self是谁
   
       def func(self):
           print('in foo')
   
   class Son(Foo):
       def func(self):
           print('in son')
   
   Son()  #in son
   
   # 思考二: 如果想给狗和猫定制个性的属性
   #猫 : eye_color眼睛的颜色
   #狗 : size型号
   class Animal:
       def __init__(self,name,food):
           self.name = name
           self.food = food
           self.blood = 100
           self.waise = 100
       def eat(self):
           print('%s is eating %s'%(self.name,self.food))
       def drink(self):
           print('%s is drinking'%self.name)
       def sleep(self):
           print('%s is sleeping'%self.name)
           
   class Cat(Animal):
       def __init__(self,name,food,eye_color):
           Animal.__init__(self,name,food)    # 调用了父类的初始化,去完成一些通用属性的初始化
           self.eye_color = eye_color         # 派生属性
   
   小白 = Cat('小白','猫粮','蓝色')
   print(小白.__dict__)
   
   # 单继承
   class D:
       def func(self):
           print('in D')
   class C(D):pass
   class A(C):
       def func(self):
           print('in A')
   class B(A):pass
   B().func()
   
   # 多继承 有好几个爹
       # 有一些语言不支持多继承 java
       # python语言的特点 : 可以在面向对象中支持多继承
   class B:
       def func(self):print('in B')
   class A:
        def func(self):print('in A')
   
   class C(B,A):pass
   C().func()
   
   # 单继承
   # 调子类的 : 子类自己有的时候
   # 调父类的 : 子类自己没有的时候
   # 调子类和父类的 :子类父类都有,在子类中调用父类的
   
   # 多继承
   # 一个类有多个父类,在调用父类方法的时候,按照继承顺序,先继承的就先寻找
   ```

3. 知识点补充

   ```python
   # object类 类祖宗
   # 所有在python3当中的类都是继承object类的
   # 所有的类都默认的继承object
   class A(object):  #也可以默认不写 , 这样 class A:   ，一般建议写
       pass
   a = A()
   
   #__bases__ 打印父类
   class A:pass
   print(A.__bases__)  #(<class 'object'>,)
   class C:pass
   class B(A,C):pass
   print(B.__bases__) #(<class '__main__.A'>, <class '__main__.C'>)
   
   # isinstance 和 type 区别
   a = 1
   b = 'abc'
   print(isinstance(a,int))
   print(isinstance(a,float))
   print(isinstance(b,str))
   
   a = 1
   b = 'abc'
   print(type(a) is int)
   print(type(b) is str)
   
   class Cat:
       pass
   小白 = Cat()
   print(type(小白) is Cat)
   print(isinstance(小白,Cat))
   
   class Animal:pass
   class Cat(Animal):pass
   小白 = Cat()
   print(type(小白) is Cat)
   print(type(小白) is Animal)
   print(isinstance(小白,Cat))
   print(isinstance(小白,Animal))
   
   # 绑定方法和普通的函数
   # FunctionType : 函数
   # MethodType : 方法
   from types import FunctionType,MethodType
   class A:
       def func(self):
           print('in func')
   
   print(A.func)  # 函数
   a = A()
   print(a.func)  # 方法
   print(isinstance(a.func,FunctionType))
   print(isinstance(a.func,MethodType))
   print(isinstance(A.func,FunctionType))
   print(isinstance(A.func,MethodType))
   
   
   class A:
       role = '法师'
       def func1(self):pass
       def func2(self):pass
   class B:pass
   class C(B,A):pass
   print(A.__base__)
   print(C.__base__)
   print(C.__bases__)
   print(A.__dict__)
   print(A.__name__)
   print(A.__class__)
   print(B.__class__)
   print(C.__class__)
   print(C.__module__)
   
   
   # __doc__ #打印注释
   def func():
       '''
       这个函数主要是用来卖萌
       :return:
       '''
       pass
   print(func.__doc__)
   
   class Cat:
       '''
       这个类是用来描述游戏中的猫咪角色
       '''
       pass
   print(Cat.__doc__)
   
   ```

4. pickle补充

   ```python
   #pickle 可以存储和读取对象
   class Course:
       def __init__(self,name,period,price):
           self.name = name
           self.period = period
           self.price = price
   python = Course('python','6 moneth',21800)
   
   import  pickle
   with open('pickle_file','ab') as f:
       pickle.dump(python,f)
       
   with open('pickle_file','rb') as f:
       ret = pickle.load(f)
       print(ret)  #输出我们之前文件写进去的内容
   
    class Course:
       def __init__(self,name,period,price):
           self.name = name
           self.period = period
           self.price = price
   
           
   python = Course('python','6 moneth',21800)
   linux = Course('linux','5 moneth',19800)
   go = Course('go','4 moneth',12800)
   
   import  pickle
   with open('pickle_file','ab') as f:
       pickle.dump(python,f)
       pickle.dump(linux,f)
       pickle.dump(go,f)
       
   with open('pickle_file','rb') as f:
       while True:
           try:
               obj = pickle.load(f)
               print(obj.name,obj.period)
           except EOFError:
               break   
   ```

   

