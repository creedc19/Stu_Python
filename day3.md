## 一、今日内容大纲（day3）

1. 基础数据类型总览
2. int
3. bool
4. **str**
   + 索引，切片
   + 常用操作方法
5. **for循环**

二、昨日内容以及作业讲解

1. pycharm  简单使用

2. while循环

   1. 结构
   2. while  else、break、continue

3. **格式化输出：针对str：让字符串的某些位置变成动态可变的，可传入的。**

   1. % 占位符    s  d   i  

   2. **%% (表示百分号，而不是占位符)**

      ```python
      注意：在格式化输出中，% 只想表示一个百分号，而不是作为占位符使用
      msg1 = '我叫%s，今年%s，学习进度1%' %('太白金星','18') 
      print(msg1)   #会报错
      msg2 = '我叫%s，今年%s，学习进度1%%' %('太白金星','18') 
      print(msg2)   #正确
      ```

4. 编码初识：

   1. 编码：密码本：二进制与文字的对应关系
      + ASCII：最早的密码本：二进制与英文字母、数字、特殊字符的对应关系
      + GBK：英文....占一个字节，中文占两个字节
        + 英文字母、数字、特殊字符  ASCII
        + 中文自己编写的
        + Unicode:万国码。（兼容性高，与任何密码本都有映射关系）
        + UTF-8： 一个英文占一个字节，欧洲的占两个字节，亚洲的占3个字节

   

## 二、具体内容:

1. 基础数据类型总览

   + 12345  **int** +-*/等
   + "今天吃饭了吗？"    **str**    存储少量的数据      + 、  *int       切片     其它操作方法
   + True   False   **bool**        判断真假 
   + [1,2,"ati",[1,2,2]]      **list**     存储大量的数据
   + (1,2,"ati",[1,2,2])      **tuple**    存储大量的数据,不可改变里面的元素
   + {"name":"太白金星"}     **dict**   存储大量的**关联型**的数据，查询速度非常快。
   + **set** 集合        交集，差集，并集。

2. int

   ```python
   # int 主要用于计算 + - * /
   #不同的进制之间的转换，10进制，2进制
   '''
   二进制转换成十进制
   0001 1010     ------> ?  26
   '''
   b = 1 * 2**4 + 1 * 2**3 + 0 * 2**2 + 1 * 2**1 + 0 * 2**0
   print(b)  # 26
   
   '''
   十进制转换成二进制
   42  -----> 0010 1010
   '''
   
   # int:  bit_lenth 有效的二进制的长度
   i = 4
   print(i.bit_length())  # 3
   i = 42
   print(i.bit_length())  # 6
   
   # bool 、str、 int之间的转换
   # bool  <---> int
   '''
   True 1        False 0
   非零即True     0是False
   '''
   print(int(True))    #1
   print(int(False))   #0
   n = 10
   print(bool(n))      #True
   s = 0
   print(bool(s))      #False
   
   #str <---> int   重点记
   '''
   s1 = ‘10’     int(s1)  : 此时字符串必须是数字组成,才能转化成int，否则会报错
   i = 100     str(i)  
   '''
   s = "10"
   print(int(s),type(int(s)))  #10 <class 'int'>
   n = 12
   print(str(n),type(str(n)))  #12 <class 'str'>
   
   #str ----> bool   重点记
   #非空即True
   s1 = ' '   #有一个空格键
   print(bool(s1))  #True
   s1 = ''  # 空字符串,什么都没，只是单引号引起来
   print(bool(s1))   #False
   #str-->bool的应用
   s = input('输入内容')
   if s:
       print('有内容')
   else:
       print('没有输入任何内容')
   
   # bool  ---> str  无意义
   print(str(True),type(str(True)))   #True <class 'str'>
   ```

3. str

   + 对字符串进行索引，**切片出来的数据都是字符串类型。**

   + 按照索引取值：从左至右有顺序，下标或者索引。

     ```python
     s1 = 'python全栈22期'
     s2 = s1[0]
     print(s2,type(s2))    # p <class 'str'>   s1，s2是没有关系的
     s3 = s1[2]
     print(s3)  # t
     s4 = s1[-1]
     print(s4)    #期
     ```

   + 按照切片取值：**切片顾头不顾尾**

     ```python
     s5 = s1[0:6]
     s5 = s1[:6]
     print(s5)  #python
     s6 = s1[6:]
     print(s6)   # 全栈22期
     ```

   + 切片的步长

     ```python
     s7 = s1[:5:2] #2是步长，隔一个取一个，默认步长是1
     print(s7)   #pto
     print(s1[:])  #全取出来了,python全栈22期
     
     # 倒序取，步长改为-1：
     s8 = s1[-1:-6:-1]
     print(s8)
     
     # 按索引：s1[index]
     # 按照切片： s1[start_index: end_index+1]
     # 按照切片步长： s1[start_index: end_index+1:2]
     # 反向按照切片步长： s1[start_index: end_index后延一位:-1]
     ```

   + 字符串常用操作方法：**不会对原字符串进行任何操作，都是产生一个新的字符串**

     ```python
     s = 'taiBAifdsa'
     # upper 、lower 的操作方法
     s1 = s.upper()  #小写变大写
     print(s1)    #TAIBAIFDSA
     s2 = s.lower() #大写变小写
     print(s2,type(s2))  #taibaifdsa <class 'str'>
     
     # 应用：输入验证码不区分大小写
     username = input('用户名')
     password = input('密码')
     code = 'QweA'
     print(code)
     your_code = input('请输入验证码，不区分大小写：')
     if your_code.upper() == code.upper():  #把用户输入的验证码和已有的验证码都变成大写
         if username == '太白' and password == '123':
             print('登录成功')
         else:
             print('用户名或者密码错误')
     else:
         print('验证码错误')
     ```

     ```python
     # startswith 、 endswith 的操作方法
     s = 'taiBAifdsa'
     print(s.startswith('t'))  #True     重点
     print(s.startswith('taiBAi'))  #True   重点
     print(s.startswith('B',3,6))  #True （3,6）指BAi这三个字符是不是以B开始的   了解
     ```

     ```python
     # replace 的操作方法
     msg = 'alex 很nb,alex是老男孩教育的创始人之一，alex长得很帅'
     msg1 = msg.replace('alex','太白')  # 默认全部替换
     msg2 = msg.replace('alex','太白',2) # 2 指从左至右替换两个
     print(msg)
     print(msg1)  #太白 很nb,太白是老男孩教育的创始人之一，太白长得很帅
     print(msg2)  #太白 很nb,太白是老男孩教育的创始人之一，alex长得很帅
     ```

     ```python
     # strip:去除字符串两边的空白，空白指：空格，\t（tab） ，\n（换行符）
     s4 = '  \n太白\t'
     print(s4)
     s5 = s4.strip()
     print(s5)
     
     #作为了解， 可以去除指定的字符
     s6 = 'rre太白qsd'   #如果太和白中间有个r是不能被去掉的
     s7 = s6.strip('qrsed')
     print(s7)  #太白
     ```

     ```python
     # split : str ---> list   非常重要    把字符串分割成列表
     # 默认按照空格分割，返回一个列表
     s8 = '太白 女神 吴超'
     s9 = '太白女神吴超'
     l1 = s8.split()
     l2 = s9.split()
     print(l1)     #['太白', '女神', '吴超']
     print(l2)    #['太白女神吴超']
     
     # 指定分隔符
     s10 = '太白:女神:吴超'
     l3= s10.split(':')
     print(l3)    #['太白', '女神', '吴超']
     
     # 了解：
     s11 = ':barry:nvshen:wu'
     print(s11.split(':'))    #['', 'barry', 'nvshen', 'wu']   第一个是空字符串
     print(s11.split(":",2))  #['', 'barry', 'nvshen:wu']  2表示按照前两个分隔符来分隔
     
     ```

     ```python
     # join  非常好用
     s1 = 'alex'
     s2 = '+'.join(s1)
     print(s2,type(s2))  #a+l+e+x <class 'str'>
     
     l1 = ['太白', '女神', '吴超']
     # 前提：列表里面的元素必须都是str类型
     s3 = ':'.join(l1)
     print(s3)    #太白:女神:吴超
     l = ["aticc","alex","howard"]
   n = "".join(l)
     print(n)  #aticcalexhoward
     ```
     
     ```python
     # count
   s8 = 'sdfsdagsfdagfdhgfhgfhfghfdagsaa'
     print(s8.count('a'))   #5
     ```
     
     ```python
     # format: 格式化输出  重点       (%也是格式化输出)
     # 第一种用法：
     msg1 = '我叫{}今年{}性别{}'.format('大壮',25,'男') 
     print(msg1)  #我叫大壮今年25性别男
     
     # 第二种用法：
     msg2 = '我叫{0}今年{1}性别{2}我依然叫{0}'.format('大壮', 25,'男') 
     print(msg2) #我叫大壮今年25性别男我依然叫大壮
     
     # 第三种用法：
     age1 = 25
   msg3 = '我叫{name}今年{age}性别{sex}'.format(age = age1,sex='男',name='大壮') 
     print(msg3) #我叫大壮今年25性别男
     ```
     
     ```python
     # is 系列：
     name = 'taibai123'
     name1 = '100'
     print(name.isalnum()) #True #判断字符串由字母或数字组成
     print(name.isalpha()) #字符串只由字母组成
     print(name.isdecimal()) #字符串只由十进制组成
     print(name1.isdecimal()) #字符串只由十进制组成
     
     #应用：只能输入十进制
     s1 = input('请输入您的金额：')
     if s1.isdecimal():  #判断用户输入的是否全部都由十进制组成
          print(int(s1))
   else:
          print('输入有误')
     ```
     
     ```python
     #成员运算:
     s1 = '老男孩edu'
     print('老' in s1)  #True
     print('老男' in s1)  #True
     print('老ed' in s1) #False
     print('老ed' not in s1) #True
     ```

4. for循环

   ```python
   练习：把s1中每个字符打印各占一行，如引号内
   s1 = '老男孩教育最好的讲师：太白'  
   '''
   老      #s1[0]
   男      #s1[1]
   孩      #s1[2]
   教      #s1[3]
   育      ....
   ...
   '''
   # len :获取可迭代对象的元素总个数
   print(len(s1))   #13
   #代码：
   count = 0
   while count < len(s1):
        print(s1[count])
        count += 1
   ```

   ```python
   # for 循环
   '''
   for循环是有限循环，格式如下：
   for 变量 in iterable:       #interable指可迭代对象
       pass
   '''
   #上面这题用for循环写更简单
   s1 = '老男孩教育最好的讲师：太白'
   for i in s1:
        print(i)
        
   # for else与 while else的用法一样
   
   ```
   
   ```python
   #break 、continue
   s1 = '老男孩教育最好的讲师：太白'
   for i in s1:
        print(i)
        if i == '好':
             break
              
   s1 = 'fsdafsda'
   for i in s1:
        continue
        print(i) 
   ```
   
   

