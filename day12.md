### 一、今日内容大纲

1. 毒鸡汤课

   坚持努力

2. 生成器

   + yield
   + yield return
   + yield from

3. 生成器表达式，列表推导式

4. 内置函数 I

   

### 二、昨日内容回顾作业讲解

1. 可迭代对象：
   + 可以更新迭代的实实在在的值。
   + 内部含有`'__iter__'`方法的。
   + str  list  tuple  dict  set  range 
   + 优点：操作方法多，操作灵活，直观，
   + 缺点：占用内存。
2. 迭代器：
   + 可以更新迭代的一个工具（数据结构）。
   + 内部含有`'__iter__'且含有'__next__'方法的`。
   + 文件句柄 是迭代器
   + 优点：节省内存。惰性机制。
   + 缺点：不直观，速度相对慢，操作方法单一，不走回头。

3. 格式化输出。
4. 函数名的应用。
5. 默认参数可变的数据类型坑。作用域的坑。



### 三、今日内容

1. 生成器

   生成器：python社区大部分人认为，生成器与迭代器看成是一种。**生成器的本质就是迭代器**。唯一的区别：生成器是我们自己用python代码构建的数据结构。迭代器都是提供的，或者转化得来的。

+ 获取生成器的三种方式：
  + 生成器函数。
  + 生成器表达式。
  + python内部提供的一些。

+ 生成器函数获得生成器：

```python
#函数
def func():
    print(111)
    print(222)
    return 3
ret = func()
print(ret)

#生成器函数：一个 next 对应一个yield
def func():
    print(111)
    print(222)
    yield 3
ret = func()
print(ret)   #不打印
print(next(ret))  #打印 ，next取到的值是3,111,222只是打印一下

def func():
    yield 3
    yield 4
ret = func()
print(next(ret))  #只打印3

def func():
    yield 3
    yield 4
ret = func()
print(next(ret))
print(next(ret))

def func():
    print(111)
    print(222)
    yield 3
    a = 1
    b = 2
    c = a + b
    print(c)
    yield 4
ret = func()
print(next(ret))
print(next(ret))
print(next(ret))   #多写会报错

```

+ yield 、return 区别

  return：函数中只存在一个return结束函数，并且给函数的执行者返回值。
  yield：**只要函数中有yield那么它就是生成器函数而不是函数了**。生成器函数中可以存在多个yield，yield不会结束生成器函数，一个yield对应一个next。

+ 吃包子练习题：

  ```python
  #全部加载，占用内存
  def func():
      l1 = []
      for i in range(1,5001):
          l1.append(f'{i}号包子')
      return l1
  ret = func()
  print(ret)
  
  #节省内存
  def gen_func():
      for i in range(1,5001):
          yield f'{i}号包子'
  ret = gen_func()
  for i in range(200):
      print(next(ret))
  
  for i in range(200):
      print(next(ret))
  
  ```

+ yield from 

  ```python
  def func():
      l1 = [1, 2, 3, 4, 5]
      yield l1
  ret = func()
  print(next(ret))   #[1, 2, 3, 4, 5]
  
  
  #yield from 将l1这个列表变成了迭代器返回。
  def func():
      l1 = [1, 2, 3, 4, 5]
      yield from l1
      '''
      yield from l1  和下面这样写等价
      yield 1
      yield 2
      yield 3
      yield 4
      yield 5
      '''    
  ret = func()
  print(next(ret))
  print(next(ret))
  print(next(ret))
  print(next(ret))
  print(next(ret))
  
  #面试题：
  def func():
      lst1 = ['卫龙', '老冰棍', '北冰洋', '牛羊配']
      lst2 = ['馒头', '花卷', '豆包', '大饼']
      yield from lst1
      yield from lst2
      
  g = func()
  for i in g:
      print(i)
  
  #上面三行代码不懂，可以看下面的，效果一样
  '''
  g = func()
  for i in range(8):
      print(next(g))
  '''
  ```

  

+ 列表推导式

  + 列表推导式：用一行代码构建一个比较复杂**有规律**的列表。

    ```python
    #正常用for循环构建一个1-10的列表
    l1 = []
    for i in range(1,11):
        l1.append(i)
    print(l1)
    
    #用列表推导式构建一个1-10的列表
    l1 = [i for i in range(1,11)]
    print(l1)
    ```

    

  + 列表推导式分为两类：

    + **循环模式：[变量(或加工后的变量)  for  变量  in  iterable]**

    + **筛选模式：[变量(或加工后的变量) for  变量  in  iterable if 条件]**

      ```python
      #练习题：
      #1）将10以内所有整数的平方写入列表
      l = [i**2 for i in range(1,11)]
      print(l)
      
      #2）100以内所有的偶数写入列表
      l = [i for i in range(2,101,2)]
      print(l)
      
      #3）从python1期到python100期写入列表lst
      lst = [f'python{i}期' for i in range(1,101)]
      print(lst)
      
      #4)30以内能被3整除的数写入列表
      l = [i for i in range(1,31) if i % 3 == 0]
      print(l)
      
      #5）过滤掉长度小于3的字符串列表，并将剩下的转换成大写字母，如列表l1 = ['barry', 'ab', 'alex', 'wusir', 'xo']
      l1 = ['barry', 'ab', 'alex', 'wusir', 'xo']
      l = [i.upper() for i in l1 if len(i) >= 3]
      print(l)
      
      #6）含有两个'e'的所有的人名全部大写并留下来，names = [['Tom', 'Billy', 'Jefferson', 'Andrew', 'Wesley', 'Steven', 'Joe'],['Alice', 'Jill', 'Ana', 'Wendy', 'Jennifer', 'Sherry', 'Eva']]
      #先正常代码写：
      names = [['Tom', 'Billy', 'Jefferson', 'Andrew', 'Wesley', 'Steven', 'Joe'],
               ['Alice', 'Jill', 'Ana', 'Wendy', 'Jennifer', 'Sherry', 'Eva']]
      l = []
      for i in names:
          for name in i:
              if name.count('e') == 2:
                  l.append(name.upper())
      print(l)
      
      #筛选模式写：
      names = [['Tom', 'Billy', 'Jefferson', 'Andrew', 'Wesley', 'Steven', 'Joe'],
               ['Alice', 'Jill', 'Ana', 'Wendy', 'Jennifer', 'Sherry', 'Eva']]
      l = [name.upper() for i in names for name in i if name.count('e') == 2]
      print(l)
      ```

      

+ 生成器表达式

  + 生成器表达式：与列表推导式的写法几乎一模一样,也有筛选模式，循环模式，多层循环构建。写法上只有一个不同：[ ] 换成 ( )

    ```python
    obj = (i for i in range(1,11))
    print(next(obj))
    print(next(obj))
    print(next(obj))
    print(next(obj))
    print(next(obj))
    
    #或者
    for i in obj:
        print(i)
    ```

+ 总结：

  + 列表推导式缺点：

    1）有毒，不要太着迷。列表推导式只能构建比较复杂并且有规律的列表。 

    2）超过三层循环才能构建成功的，就不建议用列表推导式。

    3）查找错误（debug模式）不行

  + 列表推导式优点：

    1）一行构建，简单

    2）装逼

    ```python
    #构建一个列表： [2,3,4,5,6,7,8,9,10,'j','Q','K','A']
    l1 = [i for i in range(2,11)] + list('JQKA')
    print(l1)
    ```

+ 列表推导式与生成器表达式区别：

  写法上：列表推导式用[]，生成器表达式用()

  本质上：列表推导式是可迭代对象（iterable），生成器表达式是迭代器（iterator）

+ 字典推导式（了解）

  ```python
  #构建一个字典，字典的键来自lst2，字典的值来自lst1，lst1 = ['jay', 'jj', 'meet']，lst2 = ['周杰伦','林俊杰','元宝']
  lst1 = ['jay', 'jj', 'meet']
  lst2 = ['周杰伦','林俊杰','元宝']
  dic = { lst2[i]: lst1[i] for i in range(len(lst1))}
  print(dic)
  ```

+ 集合推导式（了解）

  ```python
  s = {i for i in range(1,11)}
  print(s)
  ```

  

+ 内置函数 I

  + 首先来说，函数就是以功能为导向，一个函数封装一个功能，那么Python将一些常用的功能（比如len）给我们封装成了一个一个的函数，供我们使用，他们不仅效率高（底层都是用C语言写的），而且是拿来即用，避免重复早轮子，那么这些函数就称为内置函数，到目前为止python给我们提供的内置函数一共是68个。

  + 一带而过，不是很重要的内置函数（了解）：all()  any()  bytes() callable() chr() complex() divmod() eval() exec() format() frozenset() globals() hash() help() id() input() int()  iter() locals() next()  oct()  ord()  pow()    repr()  round()

  + **重点讲解，重要的内置函数**：abs() enumerate() filter()  map() max()  min() open()  range() print()  len()  list()  dict() str()  float() reversed()  set()  sorted()  sum()    tuple()  type()  zip()  dir() 

  + 未来会讲，**重要的的内置函数**：classmethod()  delattr() getattr() hasattr()  issubclass()  isinstance()  object() property()  setattr()  staticmethod()  super()      （讲完面向对象会补充）

    ```python
    #1）eval： 剥去字符串的外衣运算里面的代码，有返回值。
    s1 = '1 + 3'
    print(s1)   #1 + 3
    print(eval(s1))  #4
    
    s = '{"name": "alex"}'
    print(s,type(s))   #{"name": "alex"} <class 'str'>
    print(dict(s)) # 这样不能转换成字典，会报错
    print(eval(s),type(eval(s)))  #{'name': 'alex'} <class 'dict'>
    #网络传输的str ，input 输入的时候,sql注入等等绝对不能使用eval。
    
    #2）exec：与eval几乎一样， 但是exec是处理代码流。
    msg = """
    for i in range(10):
        print(i)
    """
    exec(msg)
    
    #3）hash：获取一个对象（可哈希对象：int，str，Bool，tuple）的哈希值。
    print(hash(12322))
    print(hash('123'))
    print(hash('alex'))
    print(hash(True))
    print(hash((1,2,3)))
    
    #4）help：用于查看函数或模块用途的详细说明。
    print(help(str))
    print(help(str.upper))
    print(help(list))
    
    #5）callable：用于检查一个对象是否是可调用的。如果返回True，object仍然可能调用失败；但如果返回False，调用对象ojbect绝对不会成功。
    name = 'alex'
    def func():
        pass
    print(callable(name))  # False
    print(callable(func))  # True
    
    #6）int：用于将一个字符串或数字转换为整型。
    
    #7）float：函数用于将整数和字符串转换成浮点数。
    
    #8）complex：函数用于创建一个值为 real + imag * j 的复数或者转化一个字符串或数为复数。如果第一个参数为字符串，则不需要指定第二个参数
    print(complex(1,2))  # (1+2j)
    
    #9）bin：将十进制转换成二进制并返回。oct：将十进制转化成八进制字符串并返回。hex：将十进制转化成十六进制字符串并返回。
    print(bin(10),type(bin(10)))  # 0b1010 <class 'str'>
    print(oct(10),type(oct(10)))  # 0o12 <class 'str'>
    print(hex(10),type(hex(10)))  # 0xa <class 'str'>
    ```

    

### 四、今日重点总结

1. 生成器     ***
2. 生成器函数 yield 
3. yield与return 区别。yield from
4. 列表推导式，生成器表达式。 ***
5. 内置函数：今天讲的内置函数，了解。



### 五、预习内容

1. lambda表达式。
2. 内置函数 II.
3. 闭包。