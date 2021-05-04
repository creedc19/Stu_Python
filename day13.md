### 一、今日内容大纲

1. 如何学习？
2. 一定要把预习加上！
3. 分配比例：
   + 文字总结，占1/3   ， 晚上8~9之前 完成总结以及代码练习。
   + 8~9之后，写作业。
   + **多多动手动脑敲代码**

### 二、昨日内容回顾作业讲解

1. 生成器：**生成器就是迭代器**，生成器是自己用python代码构建的。

   创建生成器的三种方式：

   + 生成器函数
   + 生成器表达式
   + python内部提供的。

2. 如何判断是函数，还是生成器函数？
   + 有yield就是生成器函数
   + yield return 的区别

3. 吃包子。敲三遍。

4. **yield from 将一个可迭代对象，变成一个生成器。**

5. 列表推导式，生成器表达式。
   + 循环模式： [变量（或加工后的变量） for 变量 in iterable]
   + 筛选模式： [变量（或加工后的变量） for 变量 in iterable if 条件]

6. 内置函数Ⅰ，68个内置函数



### 三、今日内容

1. 匿名函数：一句话函数，比较简单的函数。

   1. 此函数不是没有名字，他是有名字的，他的名字就是你给其设置的变量，比如下面代码的func1.
   2. lambda 是定义匿名函数的关键字，相当于函数的def.
   
   3. lambda 后面直接加形参，形参加多少都可以，只要用逗号隔开就行。
   4. 返回值在冒号之后设置，返回值和正常的函数一样,可以是任意数据类型。
   5. 匿名函数不管多复杂.只能写一行.且逻辑结束后直接返回数据
   
   ```python
   #函数：
   def func(a,b):
    return a + b
   
   #构建匿名函数
   func1 = lambda a,b: a + b
   print(func1(1,2))
   ```
   
   + 例题：接收一个可切片的数据，返回索引为0与2的对应的元素（元组形式）。
   
+ ```python
     func2 = lambda a: (a[0],a[2])
  print(func2([22,33,44,55]))
  ```
  
   + 例题：写匿名函数：接收两个int参数，将较大的数据返回。
  
+ ```python
     func = lambda a,b: a if a > b else b
  ```
  
2. 内置函数

   ```python
   #接昨天的内置函数Ⅰ   （两颗星要有印象，三颗星要记住）
   #10）divmod：计算除数与被除数的结果，返回一个包含商和余数的元组(a // b, a % b)  **
   print(divmod(10,3))  #(3,1)
   
   #11）round：保留浮点数的小数位数，默认保留整数  **
   print(round(3.141592653))     #3
   print(round(3.141592653, 2))  # 3.14
   
   #12）pow：求x**y次幂。（三个参数为x**y的结果对z取余） **
   print(pow(2,3))  
   print(pow(2,3,3))  # 三个参数为2**3次幂，对3取余。 2**3 % 3
   
   #13）bytes：用于不同编码之间的转化。 ***
   #之前这样转换
   s1 = '太白'
   b = s1.encode('utf-8')
   print(b)
   
   #也可以这样转换，和上面没有区别，只是用法不一样
   s1 = '太白'
   b = bytes(s1,encoding='utf-8')
   print(b)
   
   #14）ord:输入字符找该字符编码的位置 **
   print(ord('a'))  #97
   print(ord('中'))  #20013  Unicode中的位置
   
   #15）chr:输入位置数字找出其对应的字符  **
   print(chr(97))    #a
   print(chr(20013))  
   
   #16）repr:返回一个对象的string形式（原形毕露）。  ***
   s1 = '存龙'
   print(s1)
   print(repr(s1))
   
   s = '太白'
   msg1 = '我叫%s' %(s)
   msg2 = '我叫%r' %(s)
   print(msg1)
   print(msg2)
   
   #17）all：可迭代对象中，全都是True才是True
   l1 = [1, 2, '太白', True, [1,2,3], '']
   print(all(l1))
   
   #18）any：可迭代对象中，有一个True 就是True
   l1 = [ 0, '太白', False, [], '',()]
   print(any(l1))
   
   #内置函数重点讲解部分：
   #1）print()：屏幕输出
   '''
   源码分析：print(self, *args, sep=' ', end='\n', file=None)
   file:  默认是输出到屏幕，如果设置为文件句柄，输出到文件
   sep:   打印多个值之间的分隔符，默认为空格
   end:   每一次打印的结尾，默认为换行符
   '''
   print(1,2,3,4)
   print(1,2,3,4,sep='&')
   
   print(11)
   print(22)  #1 2不在同一行
   
   print(11, end=' ')
   print(22)    #1 2在同一行
   
   #2）list()：将一个可迭代对象转换成列表
   l1 = list()   #l1空列表
   l2 = list('fjfdsklagjsflag')
   print(l2)
   
   #3）dict(): 通过相应的方式创建字典。
   #a.直接创建字典
   #b.利用元组的解构创建字典
   dic = dict([(1,'one'),(2,'two'),(3,'three')])
   #c. 
   dic = dict(one=1,two=2)
   #d.  fromkeys创建字典
   #e.  update创建字典
   #f.  字典的推导式创建字典
   
   #4）abs()：返回绝对值  ***
   print(abs(-6))
   
   #5）sum():求和  ***
   l1 = [i for i in range(10)]
   print(sum(l1))
   print(sum(l1,100)) #可以设置初始值，100是初始值
   
   s1 = '12345'
   print(sum(s1))  #错误
   
   #6）reversed()： 将一个序列翻转, 返回的是一个翻转序列的迭代器   ***
   l1 = [i for i in range(10)]
   l1.reverse()  # 列表的方法
   print(l1)
   
   l1 = [i for i in range(10)]
   obj = reversed(l1)
   print(obj)
   print(list(obj))
   
   #7）zip()：拉链方法。函数用于将可迭代的对象作为参数,将对象中对应的元素打包成一个个元组,然后返回由这些元组组成的内容,如果各个迭代器的元素个数不一致,则按照长度最短的返回， ***
   l1 = [1, 2, 3, 4, 5]
   tu1 = ('太白', 'b哥', '德刚')
   s1 = 'abcd'
   obj = zip(l1,tu1,s1)
   print(obj)  #内部提供的迭代器
   
   #获取值
   for i in obj:
        print(i)
           
   #以下方法最最最重要！！！
   #8）min()：求最小值   max()：求最大值    两者用法相同
   l1 = [33, 2, 3, 54, 7, -1, -9]
   print(min(l1))
   
   # 以绝对值的方式取最小值
   l1 = [33, 2, 3, 54, 7, -1, -9]
   l2 = []
   func = lambda a: abs(a)
   for i in l1:
        l2.append(func(i))
   print(min(l2)) #1
   
   #以绝对值的方式取最小值 （简单）
   l1 = [33, 2, 3, 54, 7, -1, -9]
   print(min(l1,key=abs))  #-1，  key可以等于一个函数名，函数名一定不能加括号，如abs，以绝对值形式获取最小值
   
   #内置函数后面凡是可以加key的：它会自动的将可迭代对象中的每个元素按照顺序传入key对应的函数中，以返回值比较大小。
   
   #写个函数深入分析，便于理解上面的两行代码
   def abss(a):
   '''
       第一次：a = 33  返回值是绝对值33，返回值比较大小取最小值  33
       第二次：a = 2   返回值是绝对值2，返回值比较大小取最小值  2
       第三次：a = 3   返回值是绝对值3，返回值比较大小取最小值  2
       ......
       第六次：a = -1  返回值是绝对值1，返回值比较大小取最小值  1
   '''
        return abs(a)
   print(min(l1,key=abss))
   
   #例题：dic = {'a': 3, 'b': 2, 'c': 1}，求出值最小的键
   print(min(dic))  #a， min默认会按照字典的键去比较大小。
   
   dic = {'a': 3, 'b': 2, 'c': 1}
   def func(x):   #因为min(),函数默认接收字典的键 （ min默认把字典的键传给函数的形参）
       return dic[x]
   '''
   内部过程：
   第一次：x = 'a' ,返回值是3， 返回值比较大小取最小值  3
   第二次：x = 'b' ,返回值是2， 返回值比较大小取最小值  2
   第三次：x = 'c' ,返回值是1， 返回值比较大小取最小值  1
   '''    
   print(min(dic,key=func))
   
   #匿名函数写：
   func = lambda x:dic[x]
   print(min(dic,key=func))
   
   #最后这样：
   print(min(dic,key=lambda x: dic[x]))
   
   #练习：l2 = [('太白',18), ('alex', 73), ('wusir', 35), ('口天吴', 41)]，列表中的元组，一个对应人名，一个对应年龄，返回年龄最小的那个元组
   l = [('太白',18), ('alex', 73), ('wusir', 35), ('口天吴', 41)]
   def func(x):
       return x[1]
   print(min(l,key=func))
   
   #匿名函数写：
   print(min(l,key=lambda x:x[1]))   #('太白',18)
   
   print(min(l,key=lambda x:x[1])[1])  #18
   print(min(l,key=lambda x:x[1])[0])  #太白
   
   #9）sorted()：排序函数
   l1 = [22, 33, 1, 2, 8, 7,6,5]
   l2 = sorted(l1)
   print(l1)
   print(l2)
   
   l2 = [('大壮', 76), ('雪飞', 70), ('纳钦', 94), ('张珵', 98), ('b哥',96)]
   print(sorted(l2))
   print(sorted(l2,key= lambda x:x[1]))  # 返回的是一个列表，默认从低到高
   print(sorted(l2,key= lambda x:x[1],reverse=True)) 
   
   dic = {1:'a',3:'c',2:'b'}
   print(sorted(dic))   # 字典排序返回的就是排序后的key
   
   #10）filter()：筛选过滤，类似于列表推导式的筛选模式
   #把列表l1中大于3的留下，l1 = [2, 3, 4, 1, 6, 7, 8]
   l1 = [2, 3, 4, 1, 6, 7, 8]
   print([i for i in l1 if i > 3])  # 返回的是列表
   
   ret = filter(lambda x: x > 3,l1)  # 返回的是迭代器
   print(ret)
   print(list(ret))  #转换成列表打印出来
   
   #11）map()：类似于列表推导式的循环模式
   #创建列表l1，l1 = [1, 4, 9, 16, 25]
   l1 = [1, 4, 9, 16, 25]
   print([i**2 for i in range(1,6)])  # 返回的是列表
   
   ret = map(lambda x: x**2,range(1,6))  # 返回的是迭代器
   print(ret)
   print(list(ret))  ##转换成列表打印出来
   
   #12）reduce()  在python2是内置函数，python3就不是了，在Python2.x版本中recude是直接 import就可以的, Python3.x版本中需要从functools这个包中导入
   # reduce 的使用方式:reduce(函数名,可迭代对象)  ，这两个参数必须都要有,缺一个不行
   from functools import reduce
   def func(x,y):
       return x + y
   '''
       第一次：x = 11    y = 2     x + y =     记录： 13
       第二次：x = 13    y  = 3    x +  y =   记录： 16
       第三次  x = 16    y = 4     x + y =    记录：20
   '''  
   l = reduce(func,[11,2,3,4])
   print(l)   #20
   
   #reduce的作用是先把列表中的前俩个元素取出计算出一个值然后临时保存着,接下来用这个临时保存的值和列表中第三个元素进行计算,求出一个新的值将最开始临时保存的值覆盖掉,然后在用这个新的临时值和列表中第四个元素计算.依次类推
   #注意:我们放进去的可迭代对象没有更改
   
   #以上这个例子我们使用sum就可以完全的实现了.我现在有[1,2,3,4]想让列表中的数变成1234,就要用到reduce了
   #普通函数版
   from functools import reduce
   def func(x,y):
       return x * 10 + y
       # 第一次的时候 x是1 y是2  x乘以10就是10,然后加上y也就是2最终结果是12然后临时存储起来了
       # 第二次的时候x是临时存储的值12 x乘以10就是 120 然后加上y也就是3最终结果是123临时存储起来了
       # 第三次的时候x是临时存储的值123 x乘以10就是 1230 然后加上y也就是4最终结果是1234然后返回了
   l = reduce(func,[1,2,3,4])
   print(l)
   
   #匿名函数版
   l = reduce(lambda x,y:x*10+y,[1,2,3,4])
   print(l)
   ```

3. 闭包

   ```python
#封闭的东西，保证数据的安全。
   
#由于闭包这个概念比较难以理解，尤其是初学者来说，相对难以掌握，所以我们通过示例去理解学习闭包。
   #给大家提个需求，然后用函数去实现：完成一个计算不断增加的系列值的平均值的需求。
#例如：整个历史中的某个商品的平均收盘价。什么叫平局收盘价呢？就是从这个商品一出现开始，每天记录当天价格，然后计算他的平均值：平均值要考虑直至目前为止所有的价格。
   #比如大众推出了一款新车：小白轿车。
#第一天价格为：100000元，平均收盘价：100000元
   #第二天价格为：110000元，平均收盘价：（100000 + 110000）/2 元
   #第三天价格为：120000元，平均收盘价：（100000 + 110000 + 120000）/3 元
   
   
   # 方案一：
   l1 = []  # l1是全局变量，数据不安全，中途要是不小心操作了，数据就会出错
   def make_averager(new_value):
        l1.append(new_value)
        total = sum(l1)
        averager = total/len(l1)
        return averager
   print(make_averager(100000))
   print(make_averager(110000))
   # # 很多代码.....
   # l1.append(666)
   print(make_averager(120000))
   print(make_averager(90000))
   
   # 方案二：要保证数据安全，l1不能是全局变量。
   def make_averager(new_value):
        l1 = []
        l1.append(new_value)
        total = sum(l1)
        averager = total/len(l1)
        return averager
   print(make_averager(100000))
   print(make_averager(110000))
   print(make_averager(120000))
   print(make_averager(90000))
   
   #这样结果是错误的，因为每次执行的时候，l1列表都会重新赋值成[]。此方案二不行
   
   # 方案三： 闭包
   def make_averager():
       l1 = []
       def averager(new_value):
           l1.append(new_value)
           #print(l1)
           total = sum(l1)
           return total/len(l1)
       return averager
   
   avg = make_averager()  # avg = averager
   #make_averager()函数执行完后，列表l1应当消失，但没有消失是因为第2至7行代码产生一个闭包，此时l1叫自由变量，会与内层函数形成一种绑定关系
   print(avg(100000))
   print(avg(110000))
   print(avg(120000))
   print(avg(190000))
   
   
   # 闭包： 多用于面试题： 什么是闭包？ 闭包有什么作用。
   # 1，闭包只能存在嵌套函数中。
   # 2，内层函数对外层函数非全局变量的引用（使用），就会形成闭包。（1,2闭包的定义）
   # 被引用的非全局变量也称作自由变量，这个自由变量会与内层函数产生一个绑定关系，
   # 自由变量不会再内存中消失。
   # 闭包的作用：保存局部信息不被销毁，保证数据的安全。
   
   # 如何判断一个嵌套函数是不是闭包
   
   # 1，闭包只能存在嵌套函数中。
   # 2，内层函数对外层函数非全局变量的引用（使用），就会形成闭包。
   
   # 例一：
   def wrapper():
        a = 1
        def inner():
            print(a)
        return inner
   ret = wrapper()   #是闭包
   
   
   # 例二：
   a = 2
   def wrapper():
        def inner():
            print(a)
        return inner
   ret = wrapper()   #不是闭包
   
   
   # 例三：
   def wrapper(a,b):
        def inner():
            print(a)
            print(b)
        return inner
   a = 2
   b = 3
   ret = wrapper(a,b)    #是闭包
   
   # 如何代码判断闭包？
   # 函数名.__code__.co_freevars: 查看函数的自由变量
   print(ret.__code__.co_freevars)  #('a', 'b')  ，判断有没有自由变量的，有则是闭包
   
   #刚才那个函数判断一下有没自由变量
   def make_averager():
       l1 = []
       def averager(new_value):
           l1.append(new_value)
           print(l1)
           total = sum(l1)
           return total/len(l1)
       return averager
   
   avg = make_averager() 
   print(avg.__code__.co_freevars)  #('l1',)
   ```
   
   

### 四、今日重点总结

+ 匿名函数。
+ 内置函数。`***` 一定要记住，敲3遍以上。  `**` 尽量记住，2遍。
+ 闭包


### 五、预习内容

+ 装饰器。

