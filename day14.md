### 一、今日内容大纲

1. 90。自我。
2. 调整自己，适当听取一些其他人的意见。
3. 承受压力的能力一定要有所提高。

### 二、昨天内容回顾以及作业讲解

1. 匿名函数：一句话函数。 多与内置函数，列表推导式结合。

2. 内置函数： 加key的：min，max，sorted，map，reduce，filter

3. 闭包：

   1. 内层函数对外层函数非全局变量的使用。
   2. 一定要存在嵌套函数中。
   3. 作用：保证数据安全。因为自由变量不会再内存中消失，而且全局还引用不到。

   

### 三、今日内容

1. 开放封闭原则：**开放：对代码的拓展是开放的。封闭：对源码的修改是封闭的。**

   + 装饰器字面意思：装饰，装修，比如新房子本来就可以住，如果装修，不影响你住，而且体验更加，让你生活中增加了很多功能：洗澡，看电视，沙发。器：工具，也是功能，那装饰器就好理解了：就是添加新的功能。
   + 装饰器：完全遵循开放封闭原则。
   + **装饰器： 在不改变原函数的代码以及调用方式的前提下，为其增加新的功能。**
   + 装饰器就是一个函数。

   

2. 装饰器的初识：

   + 版本一：写一些代码测试一下index函数的执行效率。

   ```python
   # time 模块  计时作用
   #time.sleep()模拟网络延迟，下面三行代码先打印111，三秒后在打印222
   print(111)
   time.sleep(3)
   print(222)
   
   #写一些代码测试一下index函数的执行效率。
   import time
   def index():
        '''有很多代码.....'''
        time.sleep(2) # 模拟的网络延迟或者代码效率
        print('欢迎登录博客园首页')
   start_time = time.time()
   index()
   end_time = time.time()
   print(end_time-start_time)
   
   #再写一个测试dariy()函数的执行效率
   def dariy():
        '''有很多代码.....'''
        time.sleep(3) # 模拟的网络延迟或者代码效率
        print('欢迎登录日记页面')
   start_time = time.time()
   dariy()
   end_time = time.time()
   print(end_time-start_time)
   
   # 版本一有问题： 如果测试别人的代码，必须要重新赋值粘贴。
   
   ```

   + 版本二：利用函数，解决代码重复使用的问题

     ```python
     import time
     def index():
         '''有很多代码.....'''
         time.sleep(2) # 模拟的网络延迟或者代码效率
         print('欢迎登录博客园首页')
     
     def dariy():
         '''有很多代码.....'''
         time.sleep(3) # 模拟的网络延迟或者代码效率
         print('欢迎登录日记页面')
     
     def timmer(f):  # f= index
         start_time = time.time()
         f()  # index()
         end_time = time.time()
         print(f'测试本函数的执行效率{end_time-start_time}')
     timmer(index)
     
     #版本二还是有问题：原来index函数源码没有变化，给原函数添加了一个新的功能测试原函数的执行效率的功能。但是不满足开放封闭原则，原函数的调用方式改变了。
     
     ```
     
   + 版本三：不能改变原函数的调用方式。

     ```python
     import time
     def index():
          '''有很多代码.....'''
          time.sleep(2) # 模拟的网络延迟或者代码效率
          print('欢迎登录博客园首页')
     
     def timmer(f):  # f = index  
          def inner(): 
              start_time = time.time()
              f()  # index() 
              end_time = time.time()
              print(f'测试本函数的执行效率{end_time-start_time}')
          return inner 
     
     '''
     ret = timmer(index)  # inner
     ret()  # inner()
     '''
     
     '''
     def func():
          print('in func')
     
     def func1():
          print('in func1')
     
     func()
     func = 666   #func原来指向一个内存地址，现在重新赋值为666，然后再func + (),是不会执行的
     func(0)
     '''
     
     index = timmer(index)  # inner 
     index()  # inner()
     
     #满足：在不改变原函数的代码以及调用方式的前提下，为其增加新的功能。上面代码就是最原始的装饰器。
     ```

   + 版本四：具体研究 （day14的03视频有流程讲解）

     ```python
     import time
     def index():
         '''有很多代码.....'''
         time.sleep(2) # 模拟的网络延迟或者代码效率
         print('欢迎登录博客园首页')
     
     def timmer(f):
         f = index
         # f = <function index at 0x0000023BA3E8A268> ，f指向的就是index函数的内存地址
         def inner():
             start_time = time.time()
             f()    # index() ，这个index是上面f指向的内存地址
             end_time = time.time()
             print(f'测试本函数的执行效率{end_time-start_time}')
         return inner
     
     index = timmer(index)
     index()
     
     #装饰器的流程：本质就是闭包
     ```

   + 版本五：python做了一个优化；提出了一个语法糖的概念。此版本是 标准版的装饰器

     ```python
     import time
     # timmer装饰器，写在上面
     def timmer(f):
         def inner():
             start_time = time.time()
             f()
             end_time = time.time()
             print(f'测试本函数的执行效率{end_time-start_time}')
         return inner
     
     @timmer  # @timmer 就等同于 index = timmer(index)，@timmer 就叫语法糖。运行到这一行代码的时候就相当于执行这个index = timmer(index)，同时还会读取下面一行代码index函数的内存地址
     def index():
         '''有很多代码.....'''
         time.sleep(0.6) # 模拟的网络延迟或者代码效率
         print('欢迎登录博客园首页')
     index()   #有语法糖@timmer，这样就可以执行装饰器功能
     
     
     def dariy():
         '''有很多代码.....'''
         time.sleep(3) # 模拟的网络延迟或者代码效率
         print('欢迎登录日记页面')
     dariy()    #没有语法糖，不执行装饰器功能
     
     ```
     
   + 版本六：被装饰函数带返回值，返回值应该返回给函数的执行者index，但因为装饰器，被装饰函数的返回值返回给了装饰器函数里面的f()。要解决：不管有没有装饰器，返回值都得返回给函数的执行者。

     ```python
     import time
     # timmer装饰器
     def timmer(f):   # f = index
         def inner():
             start_time = time.time()
             r = f() #index函数的返回值给了f()啊！！！可以print(f'这个是f():{f()}!!!')看一下
             end_time = time.time()
             print(f'测试本函数的执行效率{end_time-start_time}')
             return r
         return inner
     
     @timmer # index = timmer(index)
     def index():
         '''有很多代码.....'''
         time.sleep(0.6) # 模拟的网络延迟或者代码效率
         print('欢迎登录博客园首页')
         return 666
     # 加上装饰器不应该改变原函数的返回值，所以666 应该返回给我下面的ret，
     # 但是下面的这个ret实际接收的是inner函数的返回值，而666返回给的是装饰器里面的f() 也就是 r,所以我们现在要解决的问题就是将r给inner的返回值。
     ret = index()   # inner()
     print(ret)
     
     ```
     
   + 版本七：被装饰函数带参数，要解决：加上装饰器后函数传参报错问题。

     ```python
     import time
     # timmer装饰器
     def timmer(f):  # f = index
         def inner(*args,**kwargs):
             #  函数的定义：* 代表聚合  args = ('李舒淇',18)
             start_time = time.time()
             r = f(*args,**kwargs)
             # 函数的执行：* 代表打散：f(*args) --> f(*('李舒淇',18))  --> f('李舒淇',18)
             end_time = time.time()
             print(f'测试本函数的执行效率{end_time-start_time}')
             return r
         return inner
     
     @timmer # index = timmer(index)
     def index(name):
         '''有很多代码.....'''
         time.sleep(0.6) # 模拟的网络延迟或者代码效率
         print(f'欢迎{name}登录博客园首页')
         return 666
     index('纳钦')  # inner('纳钦')
     
     @timmer
     def dariy(name,age):
         '''有很多代码.....'''
         time.sleep(0.5) # 模拟的网络延迟或者代码效率
         print(f'欢迎{age}岁{name}登录日记页面')
     dariy('李舒淇',18)  # inner('李舒淇',18)
     
     ```
     
     ```python
   #标准版的装饰器；
     
     def wrapper(f):
         def inner(*args,**kwargs):
             '''添加额外的功能：执行被装饰函数之前的操作'''
             ret = f(*args,**kwargs)
             ''''添加额外的功能：执行被装饰函数之后的操作'''
             return ret
         return inner
     
     ```

3. 装饰器的应用

   ```python
   # 装饰器的应用：登录认证
   # 这周的周末作业：模拟博客园登录的作业。会用到装饰器的认证功能。
   #周末作业思路
   def login():
       pass
   
   
   def register():
       pass
   
   
   status_dict = {
       'username': None,
       'status': False,
   }
   
   def auth(f):
       '''
       你的装饰器完成：访问被装饰函数之前，写一个三次登录认证的功能。
       登录成功：让其访问被装饰得函数，登录没有成功，不让访问。
       '''
       def inner(*args,**kwargs):
           '''访问函数之前的操作，功能'''
           if status_dict['status']:
               ret = f(*args,**kwargs)
               '''访问函数之后的操作，功能'''
               return ret
           else:
               username = input('请输入用户名').strip()
               password = input('请输入密码').strip()
               if username == 'taibai' and password == '123':
                   print('登录成功')
                   status_dict['username'] = username
                   status_dict['status'] = True
                   ret = f(*args, **kwargs)
                   return ret
               else:
                   print('登录失败')
       return inner
   @auth  # article = auth(article)
   def article():
       print('欢迎访问文章页面')
   @auth
   def comment():
       print('欢迎访问评论页面')
   @auth
   def dariy():
       print('欢迎访问日记页面')
   
   article()  # inner()
   
   comment()  #inner()
   dariy()
   ```

   

