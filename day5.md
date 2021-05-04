## 一、今日内容大纲

+ 字典的初识
+ 字典的使用（增删改查）
+ 字典的嵌套

## 二、内容回顾以及作业

+ 列表：容器型数据类型，可以承载大量的数据，有序的数据
  + 增加：
    + append 追加
    + insert 插入
    + extend 迭代着追加
  + 删除：
    + pop 按照索引删除，有返回值，默认删除最后一个
    + remove 按照元素删除
    + clear 清空
    + del 索引，切片（步长）
  + 修改：
    + l1[1] = '大壮'
    + l1[1:3] = 'yhjgfade'
    + l1[1:4:2] = '太白'
  + 查找 ：索引，切片，for循环
+ 元组：只读列表，（），元组的拆包
+ range：看做：可以自己控制范围的数字列表，但是它不是列表。

## 三、具体内容

+ 字典的初识：

  + why：

    + 列表可以存储大量的数据，数据之间的关联性不强
      + ['太白', 18, '男', '大壮', 3, '男']
    + 列表的查询速度比较慢。

  + what：容器型数据类型：dict。

  + how：

    + 数据类型的分类（可变与不可变）：

      + 可变（不可哈希）的数据类型：list 、dict、 set
      + 不可变（可哈希）的数据类型： str 、bool、 int、 tuple

    + **字典：{}括起来，以键值对形式存储的容器型数据类型：**

      + ```python
        dic = {'太白':
             {'name': '太白金星','age': 18, 'sex': '男'},
         'python22期': 
             ['朱光亚', '大壮', '雪飞', '岑哥'],
         }
        ```

    + **键必须是不可变的数据类型：int , str**    (bool tuple几乎不用)    键要唯一。

    + **值可以是任意数据类型，对象**。

    + 字典3.5x版本之前（包括3.5）是无序的。

    + 字典3.6x会按照初次建立字典的顺序排列，学术上不认为是有序的。

    + 字典3.7x以后都是有序的。

    + 字典的优点：查询速度非常快，存储关联性的数据。

    + 字典的缺点：以空间换时间。

  + 字典的创建方式（面试会考）：

    + ```python
      # 方式一：
      dic = dict((('one', 1), ('two', 2), ('three', 3)))
      print(dic)  # {'one': 1, 'two': 2, 'three': 3}
      
      # 方式二：
      dic = dict(one=1, two=2, three=3)
      print(dic)   #{'one': 1, 'two': 2, 'three': 3}
      
      # 方式三：
      dic = dict({'one': 1, 'two': 2, 'three': 3})
      print(dic)
      ```
    
+ 验证字典的合法性：
  
  ```python
    dic = {[1,2,3]: 'alex', 1: 666}  # 错误，键要不可变的数据类型
    
    dic = {1: 'alex', 1: '太白', 2: 'wusir'}  # 键要唯一
    print(dic)   #{1: '太白', 2: 'wusir'}
    ```
  
+ **字典的增删改查**
  
  ```python
    dic = {'name': '太白', 'age': 18}
    # 增加：
    # 1）直接增加    有则改之，无则增加
    dic['sex'] = '男'
    dic['age'] = 23    # 改
    print(dic)   #{'name': '太白', 'age': 23, 'sex': '男'}
    
    # 2）setdefault    有则不变，无则增加
    dic.setdefault('hobby')
    print(dic)    #{'name': '太白', 'age': 18, 'hobby': None}
    dic.setdefault('hobby', '球类运动')
    print(dic)    #{'name': '太白', 'age': 18, 'hobby': '球类运动'}
    dic.setdefault('age', 45)
    print(dic)   #{'name': '太白', 'age': 18}
    
    #删除：
    # 1）pop    按照键删除键值对, 有返回值    重点
    dic.pop('age')
    print(dic)    #{'name': '太白'}
    ret = dic.pop('age')
    print(ret)   #18，返回值是键所对应的值
    
    # 设置第二个参数则无论字典中有无此键都不会报错
    ret = dic.pop('hobby','你好')  #当要删除某个键值对时，又不知道是否在字典里时，可以设置第二个值，有则删除键值对，没有也不会报错，返回值就是设置的第二个值。
    print(ret)   #你好
    
    #2）clear  清空  
    dic.clear()
    print(dic)   #{}
    
    #3）del  按照键删除  
    del dic['age']
    print(dic)     #{'name': '太白'}
    del dic['age1']  #没有此键时会报错
    
    #修改：
    dic['name'] = 'alex'
    print(dic)   #{'name': 'alex', 'age': 18}
    
    #查找：
    dic1 = {'name': '太白', 'age': 18, 'hobby_list': ['直男', '钢管', '开车']}
    #1）按照键查询值 （了解）
    print(dic1['hobby_list'])  #['直男', '钢管', '开车']
    print(dic1['hobby_list1'])  #没有此键时会报错，不好用这种
    
    #2）get  重点
    l1 = dic1.get('hobby_list')
    print(l1)    #['直男', '钢管', '开车']
    l1 = dic1.get('hobby_list1','没有此键sb')  # 可以设置返回值
    print(l1)     #没有此键也不会报错，没设置返回值，则返回None,设置返回值，则返回设置的返回值
    
    # 3）三个特殊的   keys()、 values()、 items()
    #keys():
    print(dic1.keys(),type(dic1.keys()))   #dict_keys(['name', 'age', 'hobby_list']) <class 'dict_keys'>
    
    # 可以转化成列表
    print(list(dic1.keys()))   #['name', 'age', 'hobby_list']
    
    for key in dic1.keys():
         print(key)
     for key in dic1:        
         print(key)            #这两个一样，都是循环打印键
    
    
    # values():
    print(dic1.values())     #dict_values(['太白', 18, ['直男', '钢管', '开车']])
    
    # 可以转化成列表
    print(list(dic1.values()))   #['太白', 18, ['直男', '钢管', '开车']]
    for value in dic1.values():
         print(value)
    
    # items():
    print(dic.items()) #dict_items([('name', '太白'), ('age', 18), ('hobby_list', ['直男', '钢管', '开车'])])
    
    for i in dic1.items():
        print(i)   #('name', '太白')    ('age', 18)  ('hobby_list', ['直男', '钢管', '开车'])
      
    for key,value in dic.items():
       print(key,value)     #敲一下看下效果
    
    #元组的拆包：      
    a,b = ('name', '太白')
    print(a,b)
    # 面试题：利用一行将a,b的值互换，a = 18，b = 12
    a = 18
    b = 12
    a,b = b,a
    print(a,b)   #12 18
    
    ```
  
  相关练习题：
  
  ```python
    dic = {'k1': "v1", "k2": "v2", "k3": [11,22,33]}
    # 1.请在字典中添加一个键值对，"k4": "v4"，输出添加后的字典
    # 2.请在修改字典中 "k1" 对应的值为 "alex"，输出修改后的字典
    # 3.请在k3对应的值中追加一个元素 44，输出修改后的字典
    l1 = dic.get('k3')
    print(l1)  #[11, 22, 33]
    dic.get('k3').append(44)
    print(dic)
    # 4.请在k3对应的值的第 1 个位置插入个元素 18，输出修改后的字典
    ```
  
  + 字典的嵌套
  
    ```python
    dic = {
        'name': '汪峰',
      'age': 48,
        'wife': [{'name': '国际章', 'age': 38},],
        'children': {'girl_first': '小苹果','girl_second': '小怡','girl_three': '顶顶'}
    }
    
    # 1. 获取汪峰的名字。
    print(dic['name'])
    print(dic.get('name'))
    # 2.获取这个字典：{'name':'国际章','age':38}。
    print(dic['wife'])   #[{'name': '国际章', 'age': 38}]
    print(dic['wife'][0])
    # 3. 获取汪峰妻子的名字。
    print(dic['wife'][0])   #{'name': '国际章', 'age': 38}
  print(dic['wife'][0]['name'])
    # 4. 获取汪峰的第三个孩子名字。
    print(dic['children']['girl_three'])
    
    #练习题：
    dic1 = {
     'name':['alex',2,3,5],
     'job':'teacher',
     'oldboy':{'alex':['python1','python2',100]}
     }
    #1，将name对应的列表追加⼀个元素’wusir’。
    #2，将name对应的列表中的alex字⺟变成⼤写。
    #3，oldboy对应的字典加⼀个键值对’⽼男孩’,’linux’。
    #4，将oldboy对应的字典中的alex对应的列表中的python2删除
    ```
    
    

## 四、 今日总结

+ 字典：查询速度快，数据的关联性强。
  + 键不可变的数据类型，（str、 int），键唯一。
  + 值：任意数据类型，对象。
  + 增删改查（全部都要会默写）  **重点**
  + 字典的嵌套。**重点**

## 五、明日预习内容

+ 明天讲理论性偏多：id 、is 、== 、小数据池、集合。