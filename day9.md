## 一、今日内容大纲（day9）

1. 内容回顾及作业讲解
2. 函数的初识
3. 函数的结构与调用
4. 函数的返回值
5. 函数的参数

## 二、昨日内容回顾及作业讲解

1. 文件操作初识

   三部曲：

   +  打开文件  open()。
   +  操作文件（对文件句柄进行操作）。
   +  关闭文件。
   +  文件路径path，编码方式encoding, 模式 mode

2. 读，写，追加。

   + 读： r , rb , r+ , r+b
     + read()  
     + read(n)  : 
       + r: read(n)   n是字符。
       + rb:read(n)  n是字节。
     + readline(): 加strip() 去除\n,\t,空格
     + readlines()：返回一个列表 ['第一行\n','第二行\n',]
     + for 循环 ： 对文件句柄循环。
   + 写：w,wb,w+,w+b
     + w: 没有文件创建新文件，写入内容。
     + w: 有文件，先清空，在写入新内容。
   + 追加：a,ab,a+,a+b
     - a: 没有文件创建新文件，追加内容。
     - a: 有文件，追加新内容。

3. r+:    先读，后写。

4. 其他功能：tell() 、 seek() 、flush()

   + f.seek(0):调整光标到文件的开头  ；f.seek(0,2):调整光标到文件的最后

5. with open() as f1:

6. 文件的改的操作。（重点）

## 三、今日内容

1. 函数的初识

   + 写一个获取字符串总个数的代码，不能用len:

     ```python
     s1 = 'fjkdsfjdssudafurpojurojregreuptotuproq[t'
     count = 0
     for i in s1:
         count += 1
     print(count)
     
     ```
     
+ 写一个获取列表总个数的代码，不用len：
   
  ```python
     l1 = [1, 2, 3, 4, 5, 6]
     count = 0
     for i in l1:
         count += 1
     print(count)
     
     ```
     
   这样写的代码low，**重复代码太多，代码的可读性差。**
  
     
     
   + 利用函数写出上面的功能：

     ```python
  s1 = 'fsjkdafshdjfsdhafjksda'
     l1 = [1,2,3,4,5,6]
     
     def my_len(s):
         count = 0
         for i in s:
             count += 1
         print(count)
     
     my_len(s1)
     my_len(l1)
     
     ```
   
+ **函数**：以功能（完成一件事）为导向，比如登录，注册，len等，一个函数就是一个功能。 随调随用。
   
+ 函数的优点：
   
  + 减少代码的重复性。
     + 增强了代码的可读性。
   

   
2. 函数的结构与调用

   ```python
   #教学举例：
   def meet():
       print('打开tantan')
       print('左滑一下')
       print('右滑一下')
       print('找美女')
       print('悄悄话....')
       print('约....走起...')
   meet()
   meet()
   ```

   

   + 分析上面代码结构： def 关键字，定义函数。
         meet 函数名：与**变量**设置相同，具有可描述性。
         函数体 ：缩进。函数中尽量不要出现 print

   + **函数的结构：**
     
     ```python
   def 函数名():
     
         函数体
     ```
     
     def 关键词开头，空格之后接函数名称和圆括号()，最后还有一个":"。
     
     def 是固定的，不能变，他就是定义函数的**关键字**。
     
     空格 为了将def关键字和函数名分开，必须空(四声)，空1格。
     
     函数名：函数名只能包含字母、下划线和数字且不能以数字开头。虽然函数名可以随便起，但我们给函数起名字还是要尽量简短，并且**要具有可描述性**
     
     括号：是必须加的
     
     下面的函数体一定全部都要缩进，这代表是这个函数的代码。
     
   + **函数什么时候执行？**

     + 当函数遇到      **函数名()**       函数才会执行！函数名后面一定要有括号， 比如上面这个代码的meet()。函数名()  写几次，函数里面的代码就运行几次.

3. 函数的返回值

   ```python
   def meet():
       print('打开tantan')
       print('左滑一下')
       return          #注意这
       print('右滑一下')
       print('找美女')
       print('悄悄话....')
       print('约....走起...')
   meet()
   #return: 在函数中遇到return直接结束函数。
   ```

   ```python
   def meet():
       print('打开tantan')
       print('左滑一下')
       print('右滑一下')
       print('找美女')
       print('悄悄话....')
       print('约....走起...')
       return '妹子一枚' #注意这，如果没有这个，下面print则返回None，有则返回return后面的字符串
   
   ret = meet()
   print(ret)    #这两步合起来写print(meet())
   #return：将数据返回给函数的执行者（调用者）： meet()。
   
   s1 = 'jfdkslfjsda'
   print(len(s1))#len(s1)这个整体是len函数的执行者，len(s1)会得到s1具体元素的个数，并返回给len(s1)这个整体。
   
   ```

   ```python
   def meet():
       print('打开tantan')
       print('左滑一下')
       print('右滑一下')
       print('找美女')
       print('悄悄话....')
       print('约....走起...')
       # return '妹子一枚'
       return '妹子', 123, [22, 33]
   ret= meet()
   print(ret,type(ret))
   #return: 返回多个元素是以元组的形式返回给函数的执行者。
   ```

   + return 总结：
     + **在函数中，遇到return,函数终止,return下面的（函数内）的代码不会执行。**
     + **return 可以给函数的执行者返回值：**
       + return 单个值         单个值
       + return 多个值         (多个值,)             以元组的形式返回

4. 函数的参数

   ```python
   def meet():
       print('打开tantan')
       print('进行筛选：性别：女')
       print('左滑一下')
       print('右滑一下')
       print('找美女')
       print('悄悄话....')
       print('约....走起...')
   
   s1 = 'jfdsklafjsda'
   l1 = [1,2,3]
   len(s1)
   ```

+ **函数的传参**：让函数封装的这个功能，盘活。
  分两个角度：实参，形参。

```python
def meet(sex):  #函数的定义 接受的参数是 形式参数
    print('打开tantan')
    print('进行筛选：性别：%s' %(sex))
    print('左滑一下')
    print('右滑一下')
    print('找美女')
    print('悄悄话....')
    print('约....走起...')

meet('男')  # 函数的执行 传的参数是 实际参数
```

+ **实参角度**

  1.位置参数: **从左至右，一一对应。**

  ```python 
  def meet(sex,age,skill):
      print('打开tantan')
      print('进行筛选：性别：%s,年龄：%s,%s' %(sex,age,skill))
      print('左滑一下')
      print('右滑一下')
      print('找美女')
      print('悄悄话....')
      print('约....走起...')
  
  meet('女',25,'python技术好的')
  
  #举例：写一个函数，只接受两个int的参数，函数的功能是将较大的数返回。
  def compile(a,b):
      if a > b:
          return a
      else:
          return b
  print(compile(10,20))
  print(compile(1000,1))
  
  #三元与运算符：用于简单的if  else
  a = 1000
  b = 2000
  c = a if a > b else b  #如果a>b则c=a，否则c=b,相当于下面代码 if else部分
  
  a = 1000
  b = 2000
  if a > b:
      c = a
  else:
      c = b   #遇到这四行if else代码，可以简写成上面三元运算的一行代码。
  print(c)
  
  #上面举例可以简写成：
  def complie(a,b):
      c = a if a > b else b
      return c
  
  #还可以简写成：
  def complie(a,b):
      return a if a > b else b
  
```
  
  2. 关键字参数：**一一对应**

  ```python
  def meet(sex,age,skill,hight,weight,):
        print('打开tantan')
        print('进行筛选：性别：%s,年龄：%s,技术：%s,身高：%s,体重%s' %(sex,age,skill,hight,weight))
        print('左滑一下')
        print('右滑一下')
        print('找美女')
        print('悄悄话....')
        print('约....走起...')
  meet(age=25,weight=100,hight=174,skill='python技术好的',sex='女') #一一对应，顺序无所谓
  
  #写个函数：传入两个字符串参数，将两个参数拼接完成后形成的结果返回。
  def func(a,b):
      return a + b
  print(func('太白','无敌'))
  #print(func(b='太白',a='无敌'))
  ```
  
  3. 混合参数：**位置参数一定要在关键字参数的前面。**
  
     ```python
     def meet(sex,age,skill,hight,weight,):
         print('打开tantan')
         print('进行筛选：性别：%s,年龄：%s,技术：%s,身高：%s,体重%s' %(sex,age,skill,hight,weight))
         print('左滑一下')
         print('右滑一下')
         print('找美女')
         print('悄悄话....')
         print('约....走起...')
         return '筛选结果：性别：%s,体重%s' %(sex,weight)
   
     print(meet('女',25,weight=100,hight=174,skill='python技术好的'))
   ```
  
+ 形参角度：

  1. 位置参数：与实参角度的位置参数是一种。

     ```python
     def meet(sex,age,skill):
         print('打开tantan')
         print('进行筛选：性别：%s,年龄：%s,%s' %(sex,age,skill))
         print('左滑一下')
         print('右滑一下')
         print('找美女')
         print('悄悄话....')
         print('约....走起...')
     
     meet('女',25,'python技术好的',)
     
     #写函数，检查传入列表的长度，如果大于2，那么仅保留前两个长度的内容，并将新内容返回给调用者。
     def func(l):
         if len(l) > 2:
             return l[:2]
         else:
             return l
     print(func([1,2,3,4,5]))
     print(func([1,]))
     
     #用三元运算符简化：
     def func(l):
         c = l[:2] if len(l) > 2 else l
         return c
     print(func([1,2,3,4,5]))
     print(func([1,]))
     
     #还可以简化：因为如果列表是一个元素的话，如l1 = [1,]，打印print(l1[:2])不会报错，返回[1]。
     def func(l):
         return l[:2]
     
   ```
  
2. 默认参数    **默认参数设置的意义：普遍经常使用的。**
  
     ```python
     def meet(age,skill,sex='女',):  #默认参数sex='女',注意在位置参数之后
         print('打开tantan')
         print('进行筛选：性别：%s,年龄：%s,技能：%s' %(sex,age,skill))
         print('左滑一下')
         print('右滑一下')
         print('找美女')
         print('悄悄话....')
         print('约....走起...')
     
     meet(25,'python技术好的')
     
     #默认参数可以修改：
     meet(25,'python技术好的',sex='男')
     
     ```
     
   

## 四、今日总结

1. 函数：
2. 函数的作用：以功能为导向，减少代码重复，使代码可读性好。
3. 函数的结构，函数的执行。
4. 函数的返回值：return   终止函数或者给函数的调用者返回值。
5. 函数的参数：
   + 实参角度
     + 位置参数
     + 关键字参数
     + 混合参数
   + 形参角度
     + 位置参数
     + 默认参数 （经常使用的参数）
