## 一、今日内容大纲（day7）

1. 基础数据类型的补充
2. 数据类型之间的转换
3. 编码的进阶

## 二、 昨日内容回顾以及作业讲解

1. id 、== 、is： 

   + == ：数值是否相同    is：判断内存地址是否相同，id： 获取对象的内存地址

2. 代码块：一个文件，交互式命令一行就是一个代码块。

3. 同一代码块下缓存机制（字符串驻留机制）：

   + 适用对象：**所有数字，bool ，几乎所有的字符串**
   + 优点：提升性能，节省内存空间。

4. 不同代码块下的缓存机制（小数据池）：在内存中开辟两个空间，一个空间存储-5~256之间的int，一个空间存储一定规则的字符串，如果你的代码中遇到了满足条件的数据，直接引用提前创建的。

   + 适用对象：**-5~256 int，bool，满足一定规则的字符串。**
   + 优点：提升性能，节省内存空间。

5. 集合：列表去重，关系测试： 交并差。

6. 深浅copy：

   + 浅copy：在内存中开辟一个新的空间存放copy的对象（列表，字典），**但是里面的所有元素与被copy对象的里面的元素共用一个**。
+ 深copy：在内存中开辟一个新的空间存放copy的对象，**不可变的数据类型沿用之前的，可变的数据类型创建新的。**
  

## 三、具体内容

+ 数据类型的补充

  + str

    ```python
    # str补充操作方法：补充的方法练习一遍就行。
    s1 = 'taiBAi'
    # 1）capitalize： 首字母大写，其余变小写
    print(s1.capitalize())     #Taibai
    
    # 2）swapcase： 大小写翻转（大写变小写，小写变大写）
    print(s1.swapcase())      #TAIbaI
    
    # 3）title： 每个单词的首字母大写（非字母元素隔开的）
    msg= 'taibai say hi'
    print(msg.title())        #Taibai Say3 Hi
    msg= 'taibai say3hi'
    print(msg.title())        #Taibai Say3Hi
    
    #4）center ：居中
    s1 = 'barry'
    print(s1.center(20))
    print(s1.center(20,'*'))   #*******barry********
    
    # 5）find :通过元素找索引，找到第一个就返回，找不到 返回-1
    # 6）index:通过元素找索引，找到第一个就返回，找不到 报错
    print(s1.find('a'))
    print(s1.find('r'))
    print(s1.find('o'))
    print(s1.index('o'))
    ```

  + 元组

    ```python
    # tuple
    # 1）元组中如果只有一个元素，并且没有逗号，那么它不是元组，它与该元素的数据类型一致，如果有逗号，那么它是元组。重点
    tu1 = (2,3,4)
    print(tu1,type(tu1))   #(2, 3, 4) <class 'tuple'>
    tu2 = (2)
    print(tu2,type(tu2))  #2 <class 'int'>
    tu3 = ('太白')
    print(tu3,type(tu3))   #太白 <class 'str'>
    tu4 = ([1,2,3])       
    print(tu4,type(tu4))   #[1, 2, 3] <class 'list'>
    tu5 = (1,)        
    print(tu5,type(tu5))  #(1,) <class 'tuple'>
    
    # 2）count：获取某元素在元组中出现的次数
    tu = (1,2,3,3,3,2,2,3,)
    print(tu.count(3))
    # 3）index：通过元素找索引（可切片），找到第一个元素就返回，找不到该元素即报错。
    tu = ('太白', '日天', '太白')
    print(tu.index('太白'))
    ```

    

  + 列表

    ```python
    # 1）count：获取某元素在列表中出现的次数
    a = ["q","w","q","r","t","y"]
    print(a.count("q"))
    
    # 2）index：通过元素找索引
    l1 = ['太白', '123', '女神', '大壮']
    print(l1.index('大壮'))   #3
    
    # 3）sort：默认从小到大排序     重点
    l1 = [5, 4, 3, 7, 8, 6, 1, 9]
    l1.sort()    #它没有返回值，所以只能打印l1 
    print(l1)   #[1, 3, 4, 5, 6, 7, 8, 9]
    
    l1.sort(reverse=True)   # 从大到小排序  
    
    #4）reverse：反转  重点
    l1 = [5, 4, 3, 7, 8, 6, 1, 9]
    l1.reverse()  #它没有返回值，所以只能打印l1 
    print(l1)    #[9, 1, 6, 8, 7, 3, 4, 5]
    
    #5）列表可以相加:
    l1 = [1, 2, 3]
    l2 = ['太白', '123', '女神']
    print(l1 + l2)   #[1, 2, 3, '太白', '123', '女神']
    
    #6)列表与数字相乘:
    l1 = [1, 'daf', 3]
    l2 = l1*3
    print(l2)   #[1, 'daf', 3, 1, 'daf', 3, 1, 'daf', 3]
    
    #例子：（重要）
    # l1 = [11, 22, 33, 44, 55]，把索引为奇数对应的元素删除（不能一个一个删除，此l1只是举个例子，里面的元素不定）。
    # 正常思路：先将所有的索引整出来，然后加以判断，index % 2 == 1：最后删除： pop（index）。思路没错，但到最后有一个坑，代码如下（错误的）：
    l1 = [11, 22, 33, 44, 55]
    for index in range(len(l1)):
         if index % 2 == 1:
             l1.pop(index)
    print(l1)   #[11, 33, 44]     正确的结果应该是[11, 33, 55]  
    #为什么会出现上面这种错误呢？这是由于列表的特性，当第一个奇数位元素22删除后，后面的元素都会往前顺延一位，元素索引的奇偶性就变了，见Word图示补充文档。
    #正确代码：
    # 1）最简单的用del：
    l1 = [11, 22, 33, 44, 55]
    del l1[1::2]
    print(l1)
    
    #2）倒序法删除元素：（见Word图示补充更易理解）
    l1 = [11, 22, 33, 44, 55]
    for index in range(len(l1)-1,-1,-1): #中间那个-1本来是0，但是range顾头不顾尾，需要往后顺延一位，写0的话，删除奇数位元素没错，但删除偶数尾元素结果就会有问题。
         if index % 2 == 1:
             l1.pop(index)
    print(l1)
    
    #3）思维置换：
    l1 = [11, 22, 33, 44, 55]
    new_l1 = []
    for index in range(len(l1)):
         if index % 2 ==0:
                new_l1.append(l1[index])
    print(new_l1)
    l1 = new_l1
    print(l1)
    
    #重点总结： 循环一个列表的时，最好不要改变列表的大小（增加值，或者删除值），这样会影响你的最终的结果。
    
    ```

    

  + 字典

    ```python 
    # 字典dict的补充
    # 1）update：  重点
    dic = {'name': '太白', 'age': 18}
    dic.update(hobby='运动', hight='175')    #增加
    print(dic) #{'name': '太白', 'age': 18, 'hobby': '运动', 'hight': '175'}
    
    dic.update(name='太白金星')   #修改
    print(dic)    #{'name': '太白金星', 'age': 18}
    
    dic.update([(1, 'a'),(2, 'b'),(3, 'c'),(4, 'd')])    # 面试会考
    print(dic) #{'name': '太白', 'age': 18, 1: 'a', 2: 'b', 3: 'c', 4: 'd'}
    
    dic1 = {"name":"jin","age":18,"sex":"male"}
    dic2 = {"name":"alex","weight":75}
    dic1.update(dic2)    # 更新字典，有则覆盖，无则添加
    print(dic1)  #{'name': 'alex', 'age': 18, 'sex': 'male', 'weight': 75}
    print(dic2)  #{"name":"alex","weight":75}
    
    # 2）fromkeys：创建一个字典：字典的所有键来自一个可迭代对象，字典的值使用同一个值。
    dic = dict.fromkeys('abc', 100)
    print(dic)    #{'a': 100, 'b': 100, 'c': 100}
    
    dic = dict.fromkeys([1, 2, 3], 'alex')
    print(dic)   #{1: 'alex', 2: 'alex', 3: 'alex'}
    
    # 坑：值共用一个（面试题）
    dic = dict.fromkeys([1,2,3],[])
    print(dic)    #{1: [], 2: [], 3: []}
    dic[1].append(666)
    print(dic)    #{1: [666], 2: [666], 3: [666]} 
    
    #举例：
    dic = {'k1': '太白', 'k2': 'barry', 'k3': '白白', 'age': 18}
    # 将字典中键含有'k'元素的键值对删除。
    #错误代码：原因：循环一个字典时，如果改变这个字典的大小（增，删字典的元素），就会报错。（重点重点）
    for key in dic:
         if 'k' in key:
             dic.pop(key)
    print(dic)    #报错
    
    #正确代码1：
    l1 = []
    for key in dic:
       if 'k' in key:
             l1.append(key)
    print(l1)      #['k1', 'k2', 'k3']
    for i in l1:
         dic.pop(i)
    print(dic)     #{'age': 18}
    
    #正确代码2： 
    for key in list(dic.keys()):  # 转换成列表  ['k1', 'k2', 'k3','age']
         if 'k' in key:
             dic.pop(key)
    print(dic)    #{'age': 18}
    
    #重点总结：循环一个字典时，如果改变这个字典的大小（增，删字典的元素），就会报错。
    
    ```
    
    

+ 数据类型的转换补充：

  ```python
  #所有数据都可以转化成bool值
  # 0,''，(),[],{},set(),None这些转换成bool值为False，其它的都是Ture。
  
  # list ---> set
  s1 = [1, 2, 3]
  print(set(s1))
  
  # set ---> list
  set1 = {1, 2, 3, 3,}
  print(list(set1))  # [1, 2, 3]
  
  # str ---> list
  s1 = 'alex 太白 武大'
  print(s1.split())  # ['alex', '太白', '武大']
  
  # list ---> str  # 前提 list 里面所有的元素必须是字符串类型才可以
  l1 = ['alex', '太白', '武大']
  print(' '.join(l1))  # 'alex 太白 武大'
  
  ```

  

+ 数据类型的分类，见博客（了解）

  

+ 编码的进阶：

  + **ASCII码：包含英文字母，数字，特殊字符与01010101对应关系。**

  　　a   01000001  一个字符用一个字节表示。

  + **GBK：只包含本国文字（以及英文字母，数字，特殊字符）与0101010对应关系。**

  　　a   01000001  ascii码中的字符：一个字符用一个字节表示。

  　　中  01001001 01000010  中文：一个字符用两个字节表示。

  + **Unicode**：**包含全世界所有的文字与二进制0101001的对应关系（一个字符用四个字节表示）。**

  　　a  01000001 01000010 01000011 00000001        

  　　b  01000001 01000010 01100011 00000001        

  　　中 01001001 01000010 01100011 00000001

  + **UTF-8:包含全世界所有的文字与二进制0101001的对应关系（最少用8位一个字节表示一个字符）。**

  　    a   01000001  ascii码中的字符：一个字符用一个字节表示。

  　　To 01000001 01000010   (欧洲文字：葡萄牙，西班牙等)一个字符用两个字节表示。

  　　中  01001001 01000010 01100011  亚洲文字；一个字符用三个字节表示。

  

  1. 不同的密码本之间能否互相识别？     不能。
  
     
2. **数据在内存中全部是以Unicode编码的**，但是当你的数据用于网络传输或者存储到硬盘中，必须转换成非Unicode编码（utf-8,gbk等）。



英文：

​    str： 'hello '

​	内存中的编码方式： Unicode

​	表现形式： 'hello'

​    bytes（特殊的字符串类型） ：

​	内存中的编码方式： 非Unicode

  ​	表现形式：b'hello'

 中文：

​	    str： '中国'

​		内存中的编码方式： Unicode

​		表现形式：'中国'

​	    bytes ： 

​		内存中的编码方式： 非Unicode    

​		表现形式：b'\xe4\xb8\xad\xe5\x9b\xbd'  

```python
# str ---> bytes
s1 = '中国'
b1 = s1.encode('utf-8')  # encode叫编码，将字符串转换成bytes类型，转换成什么样的bytes类型，括号里就写什么
print(b1,type(b1))  # b'\xe4\xb8\xad\xe5\x9b\xbd' <class 'bytes'>  字符串s1通过编码变成了utf-8编码本编码的bytes类型。     用utf-8编码一个字符用三个字节表示。

b1 = s1.encode('gbk')  
print(b1,type(b1))  # b'\xd6\xd0\xb9\xfa' <class 'bytes'>，用gbk编码一个字符用两个字节表示。

# bytes----> str
b1 = b'\xe4\xb8\xad\xe5\x9b\xbd'
s2 = b1.decode('utf-8')  # decode解码，将bytes类型转换成字符串类型，用什么编码本编的就用什么编码本把它编回来（解码）
print(s2)   #中国

```

```python
# gbk ---> utf-8    先把gbk解码成unicode编码的字符串，然后再把unicode编码成utf-8的，就能实现gbk转换成utf-8（utf-8 --->gbk则相反）
b1 = b'\xd6\xd0\xb9\xfa'    #gbk编码的
s = b1.decode('gbk')
print(s)   #中国  （unicode编码的中国）
b2 = s.encode('utf-8')
print(b2)  # b'\xe4\xb8\xad\xe5\x9b\xbd'

```



## 四、今日总结

+ 数据类型的补充：list（sort,revrse,列表的相加，与数字乘，循环问题），dict （update， 循环问题）重点
+ 编码的进阶：
  + bytes为什么存在？ 
  + str  --->bytes (就是Unicode ---> 非Unicode)  
  + **gbk  <-----> utf-8**    （重中之重）

## 五、预习内容

预习内容：文件操作

