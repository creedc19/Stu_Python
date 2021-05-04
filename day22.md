## 一、昨日内容回顾

1. 递归练习

   + 遍历文件夹下的所有文件  -- 掌握

   + 斐波那契数列练习 -- 会写

   + 三级菜单  -- 看懂并知道实现方法

   + 查看文件夹的总大小 -- 看懂并知道实现方法

     + ```python
       #另外一种简便方法，使用walk来计算文件夹的总大小
       import os
       g = os.walk('D:\python_22')
       size = 0
       for i in g :
           path,dir_lst,name_lst = i
           for name in name_lst:
           abs_path = os.path.join(path,name)
               size += os.path.getsize(abs_path)
       print(size)
       ```
       
       

2. sys

   1. sys.path
   2. sys.argv 在执行python脚本的时候 写在python 之后的所有的内容,形成了一个列表
   3. sys.modules 查看已经加载到内存中的所有模块

3. os

   1. 和文件 文件夹相关的
   2. 和工作目录相关的
   3. 和执行操作系统命令相关的
   4. .path系列

4. logging

   1. 排错 数据分析 操作审计
   2. 普通的输出(文件/屏幕)
   3. 切分日志(时间/文件大小)

5. shutil

   1. 和文件相关的内容

## 二、今日内容



1. 面向对象

   ```python
   # 假如一个 游戏公司 开发一个游戏
       # 人狗大战
   # 人 :
       # 名字 性别 职业 等级 血条 武器 攻击力
       # 技能 : 搓澡
   # 狗 :
       # 名字 品种 血条 攻击力
       # 技能 : 舔
   
   alex = {
        'name': 'alex',
        'sex': '不详',
        'job': '搓澡工',
        'level': 0,
        'hp' : 250,
        'weapon':'搓澡巾',
        'ad' : 1
    }
   
   小白 = {
       'name':'小白',
       'kind':'泰迪',
       'hp':5000,
       'ad':249
   }
   
   # 1.你怎么保证所有的玩家初始化的时候都拥有相同的属性：如name、sex、job等属性
   # 2.每来一个新的玩家,我们都自己手动的创建一个字典,然后向字典中填值
   # 3.人物和狗的技能如何去写？
   
   
   def Person(name,sex,job,hp,weapon,ad,level=0):   # 人模子
       def 搓(dog):
           dog['hp'] -= dic['ad']
           print('%s 攻击了 %s,%s掉了%s点血' % (dic['name'], dog['name'], dog['name'], dic['ad']))
       dic = {
       'name': name,
       'sex': sex,
       'job': job,
       'level': level,
       'hp' :hp,
       'weapon':weapon,
       'ad' : ad,
       'action':搓
       }
       return dic
   
   
   def Dog(name,kind,hp,ad):
       def 舔(person):  # 函数不是一个公用的函数 是一个有归属感的函数
           person['hp'] -= dic['ad']
           print('%s 舔了 %s,%s掉了%s点血' % (dic['name'], person['name'], person['name'], dic['ad']))
       dic = {
       'name': name,
       'kind': kind,
       'hp': hp,
       'ad': ad,
       'action':舔
       }
       return dic
   
   alex = Person('alex','不详','搓澡工',250,'搓澡巾',1)
   wusir = Person('wusir','male','法师',500,'打狗棍',1000)
   小白 = Dog('小白','泰迪',5000,249)
   小金 = Dog('小金','柯基',10000,499)
   
   小白['action'](alex)
   alex['action'](小白)
   
   # 面向过程 : 想要一个结果，写代码 ，实现计算结果
   # 面向对象开发 : 有哪些角色 ，角色的属性和技能 ，两个角色之间是如何交互的
   
   # 复杂的， 拥有开放式结局的程序 ，比较适合使用面向对象开发
       # 游戏
       # 购物
   ```

   ```python
   # 先来定义模子,用来描述一类事物，具有相同的属性和动作
   class Person:       # Person是类名
       def __init__(self,name,sex,job,hp,weapon,ad):
           # 必须叫__init__这个名字,不能改变的,所有的在一个具体的人物出现之后拥有的属性
           self.name = name
           self.sex = sex
           self.job = job
           self.level = 0
           self.hp = hp
           self.weapon = weapon
           self.ad = ad
   '''
   注意：__init__传参的变量名字随意  ，下面也可以
   class Person:       # 类名
       def __init__(self,n,s,j,h,w,a):
           # 必须叫__init__这个名字,不能改变的,所有的在一个具体的人物出现之后拥有的属性
           self.name = n
           self.sex = s
           self.job = j
           self.level = 0
           self.hp = h
           self.weapon = w
           self.ad = a
   '''
           
   # 类名() ： 会自动调用类中的__init__方法        
   alex = Person('alex','不详','搓澡工',260,'搓澡巾',1)     # alex 就是对象  alex = Person()的过程 是通过类获取一个对象的过程 - 实例化。  #对象/实例，两者指的是同一个意思
   print(alex,alex.__dict__)
   wusir = Person('wusir','male','法师',500,'打狗棍',1000)
   print(wusir,wusir.__dict__)
   
   #属性的查看
   print(alex.name)   #或者 print(alex.__dict__['name'])  属性的查看
   # 属性的修改
   alex.name = 'alexsb'   
   print(alex.name)
   # 属性的增加
   alex.money = 1000000     
   print(alex.money)
   print(alex.__dict__)
   # 属性的删除
   del alex.money           
   print(alex.__dict__)
   
   # 类和对象之间的关系?
       # 类 是一个大范围 是一个模子 它约束了事物有哪些属性 但是不能约束具体的值
       # 对象 是一个具体的内容 是模子的产物 它遵循了类的约束 同时给属性赋上具体的值
   
   # Person是一个类 :alex wusir都是这个类的对象
   # 类有一个空间,存储的是定义在class中的所有名字
   # 每一个对象又拥有自己的空间,通过对象名.__dict__就可以查看这个对象的属性和值
   
   # 修改列表\字典中的某个值,或者是对象的某一个属性 都不会影响这个对象\字典\列表所在的内存空间
   d = {'k':'v'}
   print(d,id(d))
   d['k'] = 'vvvv'
   print(d,id(d)) 
   
   # 实例化所经历的步骤
       # 1.类名() 之后的第一个事儿 :开辟一块儿内存空间    #记住这个
       # 2.调用 __init__ 方法 把空间的内存地址作为self参数传递到函数内部
       # 3.所有的这一个对象需要使用的属性都需要和self关联起来
       # 4.执行完init中的逻辑之后,self变量会自动的被返回到调用处(发生实例化的地方)
   
       
   # dog类 实现狗的属性 名字 品种 血量 攻击力 都是可以被通过实例化被定制的
   class Dog():
       def __init__(self,name,blood,aggr,kind):
           self.dog_name = name
           self.hp = blood
           self.ad = aggr
           self.kind = kind
   
   小白 = Dog('小白',5000,249,'柴犬')
   print(小白.dog_name)
   print(小白.__dict__)
   
   
   ###  方法  的用法
   class Person:       # 类名
       def __init__(self,name,sex,job,hp,weapon,ad):
           # 必须叫__init__这个名字,不能改变的,所有的在一个具体的人物出现之后拥有的属性
           self.name = name          # 对象的属性/实例变量
           self.sex = sex
           self.hp = hp
           self.ad = ad
   
       def 搓(self,dog):    # 方法,又有一个必须传的参数-->self对象   
           dog.hp -= self.ad
           print('%s给%s搓了澡,%s掉了%s点血,%s当前血量%s'%(self.name,dog.dog_name,
                                               dog.dog_name,self.ad,dog.dog_name,dog.hp))
   
   class Dog():
       def __init__(self,name,blood,aggr,kind):
           self.dog_name = name
           self.hp = blood
           self.ad = aggr
           self.kind = kind
   
       def 舔(self,person):
   
           # 狗舔了人,人调血,人掉的血量,应该是狗的攻击力
           if person.hp >= self.ad:
               person.hp -= self.ad
           else:
               person.hp = 0
           print(self.__dict__)
           print(person.__dict__)
           print('%s舔了%s,%s掉了%s点血,%s当前血量%s'%(self.dog_name,person.name,
                                             person.name,self.ad,person.name,person.hp))
           
   # 对象\实例 = 类名() -->  实例化的过程
   alex = Person('alex','不详','搓澡工',260,'搓澡巾',1)  
   print('alex : ',alex)
   小白 = Dog('小白',5000,249,'柴犬')
   alex.搓(小白)
   小白.舔(alex)
   小白.舔(alex)        
   ```

   

