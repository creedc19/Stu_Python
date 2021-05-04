## 一、 今日内容大纲

1. is 、 == 、 id 的用法

2. 代码块

3. 同一代码块下的缓存机制

4. 不同代码块下的缓存机制（小数据池）

5. 总结

6. 集合（了解）

7. 深浅copy

   

## 二、 昨日回顾以及作业讲解

1. 字典初识：
   + 查询速度快，{'name': '太白'}, 存储大量的关联型数据。
   + 键：**必须是不可变的数据类型**（int，str，bool，tuple），键是唯一的。
   + 值：任意数据类型，对象。
   + 字典3.5x 之前无序的，3.6x 按照初始时的顺序排列，3.7之后有序的。
2. 增删改查：
   + 增：setdefualt() 、dic['age'] = 18
   + 删：pop 键(可以设置返回值)、clear 清空、del dic['name']
   + 改：dic['name'] = 'wusir'
   + 查：dic['name'] 、 dic.get('name')  、  dic.keys()    dic.values()     dic.items()
3. 字典的嵌套

## 三、 具体内容

1. id   is    ==

   ```python
   # id (理解成身份证号）
   i = 100
   s = 'alex'
   print(id(i))
   print(id(s))
   
   # ==   比较的是两边的值是否相等
   l1 = [1, 2, 3]
   l2 = [1, 2, 3]
   print(l1 == l2)   #True
   s1 = 'alex'
   s2 = 'alex '   #后面多了个空格
   print(s1 == s2)  #False
   
   # is 判断的是内存地址是否相同
   l1 = [1, 2, 3]
   l2 = [1, 2, 3]
   print(id(l1))
   print(id(l2))
   print(l1 is l2)   #False
   
   s1 = 'alex'
   s2 = 'alex'
   print(id(s1))
   print(id(s2))
   print(s1 is s2)   #True
   
   # id 相同，值一定相同
   # 值相同，id不一定相同
   ```

2. 代码块

   + 代码块：我们所有的代码都需要依赖代码块执行。
   + 一个文件就是一个代码块。
   + 交互式命令下,一行就是一个代码块。

3. 两个机制：1) 、同一个代码块下，有一个机制。2)、不同的代码块下，遵循另一个机制。

4. 同一个代码块下的缓存机制。

   + 前提条件：同一个代码块内。
   + 机制内容：pass（见太白博客）
   + 适用的对象： int、 bool、 str
   + 具体细则：所有的数字，bool，几乎所有的字符串。
   + 优点：提升性能，节省内存。

   

5. 不同代码块下的缓存机制： 小数据池。

   - 前提条件：不同代码块内。
   - 机制内容：pass（见太白博客）
   - 适用的对象： int、 bool 、str
   - 具体细则：**-5~256数字**，bool，满足规则的字符串。
   - 优点：提升性能，节省内存。

   ```python
   同一个代码块下测试：
   i1 = 1000
   i2 = 1000
   i3 = 1000
   l1 = [1,2,3]
   l2 = [1,2,3]
   print(id(l1))
   print(id(l2))
   print(id(i1))
   print(id(i2))
   print(id(i3))
   
   同一个代码块下和不同代码块下分别测试：
   i = 800
   i1 = 800
   s1 = 'hfdjka6757fdslslgaj@!#fkdjlsafjdskl;fjds中国'
   s2 = 'hfdjka6757fdslslgaj@!#fkdjlsafjdskl;fjds中国'
   print(i is i1)
print(s1 is s2)
   ```
   
   

6. 总结：+
   1. 面试题考。
   2. 回答的时候一定要分清楚：同一个代码块下适用一个缓存机制。不同的代码块下适用另一个缓存机制（小数据池）
   3. 小数据池：数字的范围是-5~256.
   4. 缓存机制的优点：提升性能，节省内存

7. （了解）python基础数据类型：集合 set。容器型的数据类型，它要求它里面的元素是不可变的数据，但是它本身是可变的数据类型。集合是无序的。{}。

   + 集合的作用：
     + 列表的去重。
     + 关系测试： 交集，并集，差集，.....
     + pass

   ```python
   # 集合的创建：
   方式一：
   set1 = set({1, 3, 'Barry', False})
   方式二：
   set1 = {1, 3, '太白金星', 4, 'alex', False, '武大'}
   
   # 空集合：
   print({}, type({}))  #{} <class 'dict'>    空字典
   set1 = set()       #空集合
   print(set1)        #set()
   
   # 集合的有效性测试
   set1 = {[1,2,3], 3, {'name': 'alex'}}
   print(set1)  #会报错
   
   #增加：
   set1 = {'太白金星', '景女神',  '武大', '三粗', 'alexsb', '吴老师'}
   # 1）add
   set1.add('xx')
   print(set1)
   
   #2）update：迭代着增加
   set1.update('fdsafgsd')
   print(set1)
   
   #删除：
   #1）remove   按照元素删除
   set1.remove('alexsb')
   print(set1)
   #2）pop 随机删除
   set1.pop()
   print(set1)
   
   #变相改值：
   set1.remove('太白金星')
   set1.add('男神')
   print(set1)
   
   #关系测试：     重点
   # 交集：（&  或者 intersection）
   set1 = {1, 2, 3, 4, 5}
   set2 = {4, 5, 6, 7, 8}
   print(set1 & set2)      # {4, 5}
   print(set1.intersection(set2))  # {4, 5}
   
   # 并集：（| 或者 union）
   print(set1 | set2)   # {1, 2, 3, 4, 5, 6, 7,8}
   print(set2.union(set1))  # {1, 2, 3, 4, 5, 6, 7,8}
   
   # 差集：（- 或者 difference）
   print(set1 - set2)       # {1, 2, 3}
   print(set1.difference(set2))  # {1, 2, 3}
   
   # 反交集：（^ 或者 symmetric_difference）
   print(set1 ^ set2)        # {1, 2, 3, 6, 7, 8}
   print(set1.symmetric_difference(set2))  # {1, 2, 3, 6, 7, 8}
   
   # 子集
   set1 = {1,2,3}
   set2 = {1,2,3,4,5,6}
   print(set1 < set2)
   print(set1.issubset(set2))  # 这两个相同，都是说明set1是set2子集。
   
   # 超集
   print(set2 > set1)
   print(set2.issuperset(set1))  # 这两个相同，都是说明set2是set1超集。
   
   # 列表的去重    重点
   l1 = [1,'太白', 1, 2, 2, '太白',2, 6, 6, 6, 3, '太白', 4, 5, ]
   set1 = set(l1)
   l1 = list(set1)
   print(l1)
   
   #集合用处：数据之间的关系，列表去重。
   ```

8. 深浅copy（面试会考）

   ```python
   # 赋值运算
   l1 = [1, 2, 3, [22, 33]]
   l2 = l1
   l1.append(666)
   print(l1)      #[1, 2, 3, [22, 33], 666]
   print(l2)      #[1, 2, 3, [22, 33], 666]
   
   # 浅copy，就是copy一个外壳，里面的所有内容都是和原来共用一个
   l1 = [1, 2, 3, [22, 33]]
   l2 = l1.copy()
   l1.append(666)
   print(l1,id(l1))      #[1, 2, 3, [22, 33], 666]    2477542439616
   print(l2,id(l2))      #[1, 2, 3, [22, 33]]         2477542400960
   
   
   l1 = [1, 2, 3, [22, 33]]
   l2 = l1.copy()
   l1[-1].append(666)
   print(l1)       #[1, 2, 3, [22, 33, 666]]
   print(l2)       #[1, 2, 3, [22, 33, 666]]
   print(id(l1[-1]))
   print(id(l2[-1]))   #两者内存地址一样
   
   print(id(l1[0]))
   print(id(l2[0]))    #两者内存地址一样
   
   l1 = [1, 2, 3, [22, 33]]
   l2 = l1.copy()
   l1[0] = 90    #改变的不是1本身，只是改变了l1[0]这个槽位的指向关系
   print(l1)     #[90, 2, 3, [22, 33]]
   print(l2)     #[1, 2, 3, [22, 33]]
   
   # 深copy    python对深copy做了一个优化，将不可变的数据类型沿用同一个内存地址，可变的数据类型重新创建一个新的内存地址。
   import copy
   l1 = [1, 2, 3, [22, 33]]
   l2 = copy.deepcopy(l1)
   print(id(l1))
   print(id(l2))     #两者id不一样
   l1[-1].append(666)
   print(l1)       #[1, 2, 3, [22, 33, 666]]
   print(l2)       #[1, 2, 3, [22, 33]]
   
   
   # 相关面试题；
   l1 = [1, 2, 3, [22, 33]]
   l2 = l1[:]     #切片是浅copy
   l1[-1].append(666)
   print(l1)     #[1, 2, 3, [22, 33, 666]]
   print(l2)     #[1, 2, 3, [22, 33, 666]]
   
   #浅copy： list 、dict: 嵌套的可变的数据类型是同一个。
   #深copy： list 、dict: 嵌套的可变的数据类型不是同一个 。
   ```
   
   

## 四、今日总结

+ id 、is 、== 三个方法要会用，知道是做什么的。
+ 回答的时候一定要分清楚：同一个代码块下适用一个缓存机制。不同的代码块下适用另一个缓存机制（小数据池）
+ 小数据池：数字的范围是-5~256.
+ 缓存机制的优点：提升性能，节省内存。
+ 集合：列表去重，关系测试。
+ 深浅copy：理解浅copy，深浅copy,课上练习题整明白。

