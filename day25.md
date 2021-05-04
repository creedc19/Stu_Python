## 一、昨日内容回顾

1. 面向对象的回顾

   ```python
   # 类
   class 类名:
       静态变量 = '值'
       def 函数(self):
           '函数体的内容'
           pass
   # 所有的变量和函数的地址都存储在类的命名空间里        
   ```

2. 对象  ： 对象 = 类名()

3. 怎么用？

   1. 类能做什么用?
      1. 操作静态变量
      2. 实例化对象
   2. 什么时候是对类中的变量赋值,或者去使用类中的变量
      1. 类名.名字 = '值'
      2. print(类名.名字)
      3. print(对象名.名字) # 如果对象本身没有这个名字
   3. 什么时候是对对象中的变量赋值
      1. 对象.名字的时候
      2. self.名字的时候   

4. ```python
   # 所有的对象调用方法 就看这个对象是哪一个类的对象
   # 不要担心所有的类的方法都是一样的名字,并不影响的
   class A:
       role = []
       def __init__(self):
           self.l = []
       def append(self,obj):
           self.l.append(obj)
       def pop(self,index=-1):
           self.l.pop(index)
   print(A.role)  #[]
   a = A()
   print(A.role)  #[]
   
   class B:
       def append(self):
           print('bbbb')
   class C:
       def append(self):
           print('cccc')
   b = B()
   d = C()
   b.obj = []
   b.obj.append(1)
   print(b.obj) #[1]
   
   b = B()
   b.append()  #bbbb
   
   c = B()
   c.append()  #bbbb
   
   d = C()  
   d.append()  #cccc
   
   class Queue:
       def __init__(self):
           self.lst = []
       def append(self,value):
           print(666)
       def pop(self):
           print(888)
   
   q = Queue()
   q.lst.append(10)
   q.append(5)   #666
   print(q.lst)  #[10]
   q.pop()       #888
   print(q.lst)  #[10]
   
   # 怎么继承
   class A:
       def func(self):
           print('a')
   class B(A):
       def func(self):
           print('b')
   # A是父类,B是子类
   # 写代码的时候,是先有的父类还是先有的子类?
       # 在加载代码的过程中 需要先加载父类 所以父类写在前面
       # 从思考的角度出发 总是先把子类都写完 发现重复的代码 再把重复的代码放到父类中
   b = B()
   b.func()   # b  自己有不用父类的
   
   # 例2
   class A:
       def func(self):
           print('a')
   class B(A):
       pass
   b = B()
   b.func()   # a  自己没有用父类的
   
   # 例3
   class A:
       def func(self):
           print('a')
   class B(A):
       def func(self):
           A.func(self)
           print('b')
   b = B()
   b.func()     # a,b  先执行B.func,调用了A.func打印a,然后回到B.func打印b
   
   # 例4
   class A:
       def func(self):print('a')
   class B(A):
       def func(self):
           print('b')
           A.func(self)
   b = B()
   b.func()     # b,a
   
   # 例5
   class A:
       lst = []
       def func(self):
           self.lst.append(1)
   class B(A):
       lst = []
       def func(self):
           self.lst.append(2)
   b = B()
   b.func()
   print(A.lst)   # []
   print(B.lst)   #  [2]
   
   # 例6
   class A:
       lst = []
       def func(self):
           self.lst.append(1)
   class B(A):
       def func(self):
           self.lst.append(2)
   b = B()
   b.func()
   print(A.lst)   # [2]
   print(B.lst)   # [2]
   
   # 例7
   class A:
       lst = []
       def __init__(self):
           self.lst = []
       def func(self):
           self.lst.append(1)
   class B(A):
       def __init__(self):
           self.lst= []
       def func(self):
           self.lst.append(2)
   b = B()
   b.func()
   print(A.lst)  # []
   print(B.lst)  # []
   print(b.lst)  # [2]
   ```

5. 早默写

   ```python
   # 内置的数据结构
       # {}  key-value 通过key找v非常快
       # []  序列 通过index取值非常快
       # ()  元组
       # {1,}集合
       # 'sx'字符串
   # 不是python内置的
       # Queue队列 : 先进先出 FIFO(FIRST IN FIRST OUT)
           # put
           # get
       # Stack栈   : 后进先出 LIFO(Last In First Out)
           # put
           # get
       # 继承关系
           # 完成代码的简化
   
   '''
   用pickle自定义模块，第二次写入文件的时候 又得打开，这样比较麻烦啊
   import  pickle
   with open('pickle_file','ab') as f:
       pickle.dump(python,f)
   
   with open('pickle_file','rb') as f:
       ret = pickle.load(f)
       print(ret)
   
   
   class Course:
       def __init__(self,name,period,price):
           self.name = name
           self.period = period
           self.price = price
   
   python = Course('python','6 moneth',21800)
   linux = Course('linux','5 moneth',19800)
   go = Course('go','4 moneth',12800)
   
    #第二次写入文件
   import  pickle
   with open('pickle_file','ab') as f:
       pickle.dump(linux,f)
       pickle.dump(go,f)
   
   with open('pickle_file','rb') as f:
       while True:
           try:
               obj = pickle.load(f)
               print(obj.name,obj.period)
           except EOFError:
               break
   '''        
   # 所以： 作业要求：自定义Pickle,借助pickle模块来完成简化的dump和load
       # pickle dump
           # 打开文件
           # 把数据dump到文件里
       # pickle load
           # 打开文件
           # 读数据
   # 对象 = Mypickle('文件路径')
   # 对象.load()  能拿到这个文件中所有的对象
   # 对象.dump(要写入文件的对象)
   import pickle
   class Mypickle:
       def __init__(self,path):
           self.file = path
       def dump(self,obj):
           with open(self.file, 'ab') as f:
               pickle.dump(obj, f)
       def load(self):
           with open(self.file,'rb') as f:
               while True:
                   try:
                       yield pickle.load(f)
                   except EOFError:
                       break
   pic = Mypickle('pickle_file')
   #直接调用方法写入文件就行：
   pic.dump('asdfg')
   pic.dump('你好啊')
   pic.dump(6666)
   #读取文件：
   for i in pic.load():
       print(i)
   
   #方法二：只是定义load方法不一样，这个容易理解些
   import pickle
   class Mypickle:
       def __init__(self,path):
           self.file = path
       def dump(self,obj):
           with open(self.file, 'ab') as f:
               pickle.dump(obj, f)
   	def load(self):
           l = []
           with open(self.file,'rb') as f:
               while True:
                   try:
                       l.append(pickle.load(f))
                   except EOFError:
                       break
           return l            
   pic = Mypickle('pickle_file')
   #直接调用方法写入文件就行：
   pic.dump('asdfg')
   pic.dump('你好啊')
   pic.dump(6666)
   #读取文件：
   print(pic.load())
   
   
   ###讲解
   # 用代码实现Queue队列 : 先进先出 FIFO(FIRST IN FIRST OUT)
   class Queue:
       def __init__(self):
           self.l = []
       def put(self,item):
           self.l.append(item)
       def get(self):
           return self.l.pop(0)
   q = Queue()
   q.put(1)
   q.put(2)
   q.put(3)
   print(q.get()) #1  先进先出
   print(q.get()) #2
   print(q.get()) #3  
   
   # 假设你希望一个类的多个对象之间 的某个属性 是各自的属性,而不是共同的属性
   # 这个时候我们要把变量存储在对象的命名空间中,不能建立静态变量,
   # 建立静态变量是所有的对象共同使用一个变量
   
   # 用代码实现 Stack栈   : 后进先出 LIFO(Last In First Out)
   class Stack:
       def __init__(self):
           self.l = []
       def put(self,item):
           self.l.append(item)
       def get(self):
           return self.l.pop()
   
   s1 = Stack()
   s2 = Stack()
   s1.put(1)
   s2.put('a')
   s2.put('b')
   s2.put('c')
   s1.put(2)
   s1.put(3)
   print(s2.get()) #c
   print(s1.get()) #3
   print(s2.get()) #b
   print(s2.get()) #a
   print(s1.get()) #2
   s1.put(4)
   print(s1.get()) #4
   print(s1.get()) #1
   
   #用代码实现Queue队列 和 Stack栈 
   # 方法一
   class Foo:
       def __init__(self):
           self.l = []
   
       def put(self, item):
           self.l.append(item)
   
   class Queue(Foo):
       def get(self):
           return self.l.pop(0)
   
   class Stack(Foo):
       def get(self):
           return self.l.pop()
   
   # 方法二:
   class Foo:
       def __init__(self):
           self.l = []
   
       def put(self,item):
           self.l.append(item)
   
       def get(self):
           if self.index == 0:
               return self.l.pop(0)
           else:
               return self.l.pop()   #或者这4句简写成return self.l.pop() if self.index else self.l.pop(0)
   
   class Queue(Foo):
       def __init__(self):
           self.index = 0
           Foo.__init__(self)
   
   class Stack(Foo):
       def __init__(self):
           self.index = 1
           Foo.__init__(self) 
   
   # 方法三
   class Foo:
       def __init__(self):
           self.l = []
   
       def put(self, item):
           self.l.append(item)
   
       def get(self):
           return self.l.pop()
   
   
   class Queue(Foo):
       def get(self):
           return self.l.pop(0)
   
   class Stack(Foo):pass  #这里可以不用写get方法，直接就调用父类的        
   ```

6. 类的继承顺序

   ```python
   # 只要继承object类就是新式类
   # 不继承object类的都是经典类
   
   # python3 所有的类都继承object类,都是新式类
   # 在py2中 不继承object的类都是经典类
   #          继承object类的就是新式类
   
   # 经典类 :在py3中不存在,在py2中不主动继承object的类
   
   # 在py2中
   class A:pass         # 经典类
   class B(object):pass # 新式类
   # 在py3中
   class A:pass         # 新式类
   class B(object):pass # 新式类
   
   # 在单继承方面继承顺序(无论是新式类还是经典类都是一样的)
   class A:
       def func(self):pass
   class B(A):
       def func(self):pass
   class C(B):
       def func(self):pass
   class D(C):
       def func(self):pass
   d = D()
   # 寻找某一个方法的顺序:D->C->B->A
   # 越往父类走,是深度
   
   # 多继承
   class A:
       def func(self):
           print('A')
   
   class B(A):
       pass
       # def func(self):
       #     print('B')
   class C(A):
       pass
       # def func(self):
       #     print('C')
   class D(B,C):
       pass
       # def func(self):
       #     print('D')
   print(D.mro())   # 用mro,查看继承顺序,只在新式类中有,经典类没有的
   # d = D()
   # d.func()
   # 在新式类中，在走到一个点,下一个点既可以从深度走,也可以从广度走的时候,总是先走广度,再走深度,广度优先
   # 在经典类中，都是深度优先，总是在一条路走不通之后再换一条路，走过的点不会再走了
   
   
   # ###C3算法
   # A(O) = [AO]
   # B(A) = [BAO]
   # C(A) = [CAO]
   # D(B) = [DBAO]
   # E(C) = [ECAO]
   
   # F(D,E) = merge(D(B) + E(C))
   #          = [F] + [DBAO] + [ECAO]
   #        F = [DBAO] + [ECAO]
   #     FD   = [BAO] + [ECAO]
   #     FDB  = [AO] + [ECAO]
   #     FDBE = [AO] + [CAO]
   #     FDBEC= [AO] + [AO]
   #     FDBECA= [O] + [O]
   #     FDBECAO
   
   # 算法的内容
       # 如果是单继承 那么总是按照从子类->父类的顺序来计算查找顺序
       # 如果是多继承 需要按照自己本类,父类1的继承顺序,父类2的继承顺序,...
       # merge的规则 :如果一个类出现在从左到右所有顺序的最左侧,并且没有在其他位置出现,那么先提出来作为继承顺序中的一个
                   # 或 一个类出现在从左到右顺序的最左侧,并没有在其他顺序中出现,那么先提出来作为继承顺序中的一个
                   # 如果从左到右第一个顺序中的第一个类出现在后面且不是第一个,那么不能提取,顺序向后继续找其他顺序中符合上述条件的类
   
               
   # 经典类 - 深度优先 ， 新式类 - 广度优先
   # 深度优先要会看,自己能搞出顺序来
   # 广度优先遵循C3算法,要会用mro,会查看顺序
   # 经典类没有mro,但新式类有
   ```

   | ![day25多继承C3算法图示](C:\Users\PengC Wang\Desktop\老男孩python全栈\图片\day25多继承C3算法图示.png) |
   | ------------------------------------------------------------ |
   | 了解即可                                                     |

7. 父类对子类的约束

   抽象类 是一个开发的规范 约束它的所有子类必须实现一些和它同名的方法

   ```python
   # 支付程序
       # 微信支付 url连接,告诉你参数什么格式
           # {'username':'用户名','money':200}
       # 支付宝支付 url连接,告诉你参数什么格式
           # {'uname':'用户名','price':200}
       # 苹果支付
   class Payment:     # 抽象类
       def pay(self,money):
           '''只要你见到了项目中有这种类,你要知道你的子类中必须实现和pay同名的方法'''
           raise NotImplementedError('请在子类中重写同名pay方法')  #主动抛异常
   
   class Alipay(Payment):
       def __init__(self,name):
           self.name = name
       def pay(self,money):
           dic = {'uname':self.name,'price':money}
           # 想办法调用支付宝支付 url连接 把dic传过去
           print('%s通过支付宝支付%s钱成功'%(self.name,money))
   
   class WeChat(Payment):
       def __init__(self,name):
           self.name = name
       def pay(self,money):
           dic = {'username':self.name,'money':money}
           # 想办法调用微信支付 url连接 把dic传过去
           print('%s通过微信支付%s钱成功'%(self.name,money))
   
   class Apple(Payment):
       def __init__(self,name):
           self.name = name
       def pay(self,money):
           dic = {'name': self.name, 'number': money}
           # 想办法调用苹果支付 url连接 把dic传过去
           print('%s通过苹果支付%s钱成功' % (self.name, money))
   
   aw = WeChat('alex')
   aw.pay(400)
   aa = Alipay('alex')
   aa.pay(400)
   
   #归一化设计 ****
   def pay(name,price,kind):
       if kind == 'Wechat':
           obj = WeChat(name)
       elif kind == 'Alipay':
           obj = Alipay(name)
       elif kind == 'Apple':
           obj = Apple(name)
       obj.pay(price)
   
   pay('alex',400,'Wechat')
   pay('alex',400,'Alipay')
   pay('alex',400,'Apple')  
   
   
   # 实现抽象类的另一种方式,约束力强,依赖abc模块
   from abc import ABCMeta,abstractmethod
   class Payment(metaclass=ABCMeta):
       @abstractmethod
       def pay(self,money):
           '''只要你见到了项目中有这种类,你要知道你的子类中必须实现和pay同名的方法'''
           raise NotImplementedError('请在子类中重写同名pay方法')
   
   class Alipay(Payment):
       def __init__(self,name):
           self.name = name
       def pay(self,money):
           dic = {'uname':self.name,'price':money}
           # 想办法调用支付宝支付 url连接 把dic传过去
           print('%s通过支付宝支付%s钱成功'%(self.name,money))
   
   class WeChat(Payment):
       def __init__(self,name):
           self.name = name
       def pay(self,money):
           dic = {'username':self.name,'money':money}
           # 想办法调用微信支付 url连接 把dic传过去
           print('%s通过微信支付%s钱成功'%(self.name,money))
   
   ```

8. 多态和鸭子类型（了解，不会就听下视频）

   ```python
   # 多态
   # def add(int a,int b):
   #     return a+b
   #
   # print(add(1,'asuhjdhDgl'))
   
   # class Payment:pass
   # class WeChat(Payment):
   #     def __init__(self,name):
   #         self.name = name
   #     def pay(self,money):
   #         dic = {'username':self.name,'money':money}
   #         # 想办法调用微信支付 url连接 把dic传过去
   #         print('%s通过微信支付%s钱成功'%(self.name,money))
   #
   # class Apple(Payment):
   #     def __init__(self,name):
   #         self.name = name
   #     def pay(self,money):
   #         dic = {'name': self.name, 'number': money}
   #         # 想办法调用苹果支付 url连接 把dic传过去
   #         print('%s通过苹果支付%s钱成功' % (self.name, money))
   
   #JAVA
   # def pay(Payment a, int b):
   #     a.pay(b)
   # obj = Apple('alex')
   # pay(obj,400)
   # obj = WeChat('alex')
   # pay(obj,400)
   
   # 一个类型表现出来的多种状态
   # 支付 表现出的 微信支付和苹果支付这两种状态
   # 在java情况下: 一个参数必须制定类型
   # 所以如果想让两个类型的对象都可以传,那么必须让这两个类继承自一个父类,在制定类型的时候使用父类来指定
   
   # 鸭子类型
   # class list:
   #     def __init__(self,*args):
   #         self.l = [1,2,3]
   #     def __len__(self):
   #         n = 0
   #         for i in self.l:
   #             n+= 1
   #         return n
   # l = [1,2,3]
   # l.append(4)
   # def len(obj):
   #     return obj.__len__()
   
   # 所有实现了__len__方法的类,在调用len函数的时候,obj都说是鸭子类型
   # 迭代器协议 __iter__ __next__ 是迭代器
   # class lentype:pass
   # class list(lentype):pass
   # class dict(lentype):pass
   # class set(lentype):pass
   # class tuple(lentype):pass
   # class str(lentype):pass
   #
   # def len(lentype obj):
   #     pass
   
   # len(list)
   # len(dict)
   # len(set)
   # len(tuple)
   # len(str)
   
   # class list:
   #     def __len__(self):pass
   # class dict:
   #     def __len__(self): pass
   # class set:
   #     def __len__(self): pass
   # class tuple:
   #     def __len__(self): pass
   # class str:
   #     def __len__(self): pass
   # def len(鸭子类型看起来有没有实现一个__len__ obj):
   #     return obj.__len__()
   ```

   

