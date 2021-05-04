## 一、今日内容大纲

1. 如何在工作中不让别人看出你是培训出来的？
   + 第一天环境安装等等，小白各种问。
   + 项目需求不清晰，也不敢问。
   + 我们6个月一定要学会自主学习，自己解决问题的能力。
2. 形参角度：
   + 万能参数。
   + *的魔性用法。
   + 仅限关键字参数（了解）。
   + 形参的最终顺序。
3. 名称空间：
   1. 全局名称空间，局部........
   2. 加载顺序，取值顺序。
   3. 作用域。
4. 函数的嵌套（高阶函数）
5. 内置函数 globals  locals

## 二、昨日内容回顾作业讲解

+ **函数是以功能为导向，减少重复代码，提高代码的可读性。**

  ```python
  def func():
  	函数体
  
  ```

+ 函数的调用：func()

  ```python
  func()
  func()
  func()
  #写几次函数执行几次
  ```

+ 函数的返回值 return

  + 终止函数。
  + return  单个值：
  + return  多个值：元组形式(1,2,3,'alex')

+ 函数的参数：

  + 实参角度：位置参数，关键字参数，混合参数。
  + 形参角度：位置参数，默认参数。

  

## 三、今日内容

1. 如何在工作中不让别人看出你是培训出来的？

   - 第一天环境安装等等，小白各种问。
   - 项目需求不清晰，也不敢问。
   - 我们6个月一定要学会自主学习，自己解决问题的能力。

2. **形参角度：**

   - 万能参数。

   - *的魔性用法。

     ```python
     # 万能参数：
     #引入举例：
     def eat(a,b,c,d):
          print('我请你吃：%s,%s,%s,%s' %(a,b,c,d))
     
     eat('蒸羊羔', '蒸熊掌', '蒸鹿邑','烧花鸭')
     
     #想要修改增加的话就得在定义函数里增加，很麻烦
     def eat(a,b,c,d,e,f):
          print('我请你吃：%s,%s,%s,%s,%s,%s' %(a,b,c,d,e,f))
     
     eat('蒸羊羔', '蒸熊掌', '蒸鹿邑','烧花鸭','烧雏鸡','烧子鹅')
     
     # 所以急需要一种形参，可以接受所有的实参。这就叫万能参数。
     # 万能参数： *args, 约定俗称：args
     # 函数定义时，*代表聚合。它将所有的位置参数聚合成一个元组，赋值给了 args。
     
     def eat(*args):
          print(args)  
          print('我请你吃：%s,%s,%s,%s,%s,%s' % args)
     
     eat('蒸羊羔', '蒸熊掌', '蒸鹿邑','烧花鸭','烧雏鸡','烧子鹅')   #('蒸羊羔', '蒸熊掌', '蒸鹿邑', '烧花鸭', '烧雏鸡', '烧子鹅')  ，我请你吃：蒸羊羔,蒸熊掌,蒸鹿邑,烧花鸭,烧雏鸡,烧子鹅
     
     # 写一个函数：计算你传入函数的所有的数字的和。
     def func(*args):
          count = 0
          for i in args:
              count += i
          return count
     print(func(1,2,3,4,5,6,7))
     
     
     tu1 = (1, 2, 3, 4, 5, 6, 7)
     count = 0
     for i in tu1:
          count += i
     print(count)
     
     # **kwargs
     # 函数的定义时： ** 将所有的 关键字参数 聚合到一个字典中，并将这个字典赋值给了kwargs.
     def func(**kwargs):
          print(kwargs)
     func(name='alex',age=73,sex='laddyboy')
     
     # 万能参数：*args, **kwargs，
     def func(*args,**kwargs):
          print(args)
          print(kwargs)
     func()
     print()
     
     # * 、**在函数的 调用 时，代表打散。
     def func(*args):
         print(args) 
     
     func([1,2,3],[22,33])   #注意和下面的区别   
     func(*[1,2,3],*[22,33])  #与这个 func(1,2,3,22,33) 效果一样
     func(*'fjskdfsa',*'fkjdsal') 
     
     def func(**kwargs):
         print(kwargs)
     func(**{'name': '太白'},**{'age': 18})  #和这个func(name='太白',age='18')结果一样
     
     ```

     

   - 仅限关键字参数（了解）。

   - 形参的最终顺序。

     ```python
     # 形参角度的参数的顺序
     # *args 的位置？
     def func(*args,a,b,sex= '男'):
          print(a,b)
     func(1,2,3,4)   #会报错
     
     # *args得到实参的前提：sex必须被覆盖了。
     def func(a,b,sex= '男',*args,):
          print(a,b)
          print(sex)
          print(args)
     func(1,2,3,4,5,6,7,)  #sex会被覆盖成3，想让args得3,4,5,6,7，这样不行
     
     #*args 位置如下：
     def func(a,b,*args,sex= '男'):
          print(a,b)
          print(sex)
          print(args)
     func(1,2,3,4,5,6,7,sex='女')
     
     # **kwargs 位置？--->如下
     def func(a,b,*args,sex= '男',**kwargs,):
         print(a,b)
         print(sex)
         print(args)
         print(kwargs)
     func(1,2,3,4,5,6,7,sex='女',name='Alex',age=80)
     
     #仅限关键字参数 (了解)：
     # 形参角度第四个参数：仅限关键字参数 (了解) ，只能用关键字形式给它传值
     def func(a,b,*args,sex= '男',c,**kwargs,):
         print(a,b)
         print(sex)
         print(args)
         print(c)
         print(kwargs)
     func(1,2,3,4,5,6,7,sex='女',name='Alex',age=80,c='666')
     
     # （重点）形参角度最终的顺序：位置参数,*args,默认参数，仅限关键字参数，**kwargs  （默认参数，仅限关键字参数两者顺序可以互换）
     ```

3. 名称空间

   1. 全局名称空间，局部名称空间（临时名称空间）：

      ```python
   a = 1
      b = 2
      def func():
          f = 5
          print(f)
      c = 3
      func()
      #全局名称空间：py文件中，存放变量与值的关系、函数名与函数体内存地址的关系的一个空间叫做全局名称空间
      #局部名称空间：当执行一个函数时，内存中会临时开辟一个空间，临时存放函数中的变量与值的关系，这个叫做临时名称空间，随着函数的结束而消失。
      #内置名称空间：python源码给你提供的一些内置的函数，print input
      
      # python分为三个空间：相互独立
      # 内置名称空间（builtins.py）：存放python解释器为我们提供的名字, list, tuple, str, int这些都是内置命名空间
      # 全局名称空间（当前py文件）：我们直接在py文件中, 函数外声明的变量都属于全局命名空间
      # 局部名称空间（函数执行时才开辟）：在函数中声明的变量会放在局部命名空间
      ```
   
   2. 加载顺序，取值顺序：
   
      ```python
      # 加载顺序：内置名称空间 ---> 全局名称空间 ----> 局部名称空间（函数执行时）
      def func():
           pass
      func()
      a = 5
      print(666)
      
      # 取值顺序（就近原则或者LEGB原则，单向不可逆）:（从局部找时）局部名称空间 --->全局名称空间---> 内置名称名称空间
   name = 'ati'
      def func():
          name = 'alex'
          print(name)
      func()     #alex
      
      name = 'ati'
      def func():
          print(name)
      func()     #ati
      
      input = '太白金星'
      def func():
          input = 'alex'
          print(input)
      func()    #alex
      
      input = '太白金星'
      def func():
          print(input)
      func()    #太白金星
      
      
      def func():
          print(input)
      func()  #<built-in function input>    input的内存地址
      
      input = '太白金星'
   def func():
          input = 'alex'
   print(input)
      func()    #太白金星
      
      def func():
          input = 'alex'
      func()  
      print(input)  #<built-in function input>
      ```
   
   3. 作用域：
   
      ```python
      # 作用域就是作用范围
      # 两个作用域：
      # 全局作用域：内置名称空间+全局名称空间
      # 局部作用域：局部名称空间
      
      # 局部作用域可以引用全局作用域的变量：
      date = '周五'
      def func():
           print(date)
      func()   #周五
      
      # 局部作用域不能改变全局变量：
      count = 1
      def func():
           count = 100
           print(count)
      func()  #100    注意：这个叫在局部创建一个新的变量，这不叫修改全局变量
      
      count = 1
      def func():
           count += 2
           print(count)
      func()  #报错 local variable 'count' referenced before assignment
      
      # 局部作用域不能改变全局作用域的变量，因为当python解释器读取到局部作用域时，发现了你对一个变量进行修改的操作，解释器会认为你在局部已经定义过这个局部变量了，它就从局部找这个局部变量，就会报错了。
      
      # 使用可以，不能改变
      def func():
           count = 1
        def inner():
               print(count)
        inner()
      func()
      
      def func():
          count = 1
          def inner():
              count += 1
              print(count)
          inner()
      func()  #报错
      ```
   
   4. 函数的嵌套（高阶函数）：想要弄明白函数的嵌套，关键点：**只要遇见了   函数名+()   就是函数的调用，如果没有就不是函数的调用**，吃透这一点就算明白了。
   
      ```python
      #说出下面代码的执行顺序：
      # 例1：
      def func1():
          print('in func1')
          print(3)
      
      def func2():
          print('in func2')
          print(4)
      
      func1()
      print(1)
      func2()
      print(2)
      #结果顺序：in func1  3  1  in func2   4  2
      
      
      # 例2：
      def func1():
          print('in func1')
          print(3)
      
      def func2():
          print('in func2')
          func1()
          print(4)
      
      print(1)
      func2()
      print(2)
      #结果顺序：1  in func2  in func1  3  4  2
      
      # 例3：
      def fun2():
          print(2)
      
          def fun3():
              print(6)
   
          print(4)
       	fun3()
          print(8)
      
      print(3)
      fun2()
      print(5)
      #结果顺序：3  2  4  6  8  5
      
      ```
   
   5. 内置函数： globals()   ,  locals()
   
      ```python
      """
      本文件：研究内置函数：globals locals
      """
   a = 1
      b = 2
   def func():
          name = 'alex'
          age = 73
      print(globals())  # 返回的是字典：字典里面的键值对：全局作用域的所有内容。
      print(locals())  # 返回的是字典：字典里面的键值对：当前作用域的所有的内容。
      
      a = 1
      b = 2
      def func():
          name = 'alex'
          age = 73
          print(globals())  # 返回的是字典：字典里面的键值对：全局作用域的所有内容。
          print(locals())  # 返回的是字典：字典里面的键值对：当前作用域的所有的内容。
      func()   #两个代码运行一下，就能明显看出locals()的区别
      
      ```
   
   

## 四、今日重点总结

1. 参数：万能参数，仅限关键字参数（了解）,参数的顺序，*的魔性用法：聚合，打散。
2. 名称空间，作用域，取值顺序，加载顺序。
3. globals   locals
4. 高阶函数：执行顺序。
5. 以上全部都是重点。

## 五、明日预习内容

1. 函数名的运用，迭代器。



