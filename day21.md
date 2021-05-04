## 一、昨日内容回顾

1. re模块的常用方法

   + findall(正则,待匹配字符串,flag) :  返回所有匹配项的列表
   + search : 返回一个变量,通过group取到的是第一个匹配的项
   + finditer :返回一个迭代器,通过迭代取到的是一个变量,通过group取值
   + match : 从头开始找第一个,其他和search一样
   + compile(正则):同一个正则表达式需要多次使用的时候提前编译来节省时间
   + split 通过正则表达式匹配的内容进行分割
   + sub 替换,通过正则表达式匹配的内容来进行替换
   + subn 替换,在sub的基础上,返回一个元组,第一个内容是替换结果,第二个是替换次数

2. 分组命名 ,取消分组优先

   + 分组命名：(?P<组名>正则表达式)       引用分组命名：(?P=组名)

   + 取消分组优先：(?:正则表达式)

   + ```python
     # 用户输入身份证号匹配
     import re
     re.match('^[1-9]\d{14}(\d{2}[\dx])?$','15645465121464613')
     ```

## 二、今日内容

1. 补充：os , sys 模块，自学

   ```python
   #os模块
   # 文件和文件夹相关的
   os.listdir('dirname')    列出指定目录下的所有文件和子目录，包括隐藏文件，并以列表方式打印
   os.remove()  删除一个文件
   os.rename("oldname","newname")  重命名文件/目录
   os.mkdir('dirname')    生成单级目录；相当于shell中mkdir dirname
   os.rmdir('dirname')    删除单级空目录，若目录不为空则无法删除，报错；相当于shell中rmdir dirname
   os.makedirs('dirname1/dirname2')    可生成多层递归目录
   os.removedirs('dirname1')    若目录为空，则删除，并递归到上一级目录，如若也为空，则删除，依此类推
   
   # 和执行操作系统命令相关
   os.system("bash command")  运行shell命令，直接显示
   os.popen("bash command).read()  运行shell命令，获取执行结果
   
   import os
   os.system('dir')  #运行就直接显示
   
   import os
   ret = os.popen('dir')
   print(ret.read())
            
   # 工作目录:在哪个文件下执行的这个py文件,哪一个目录就是你的工作目录
   os.getcwd() 获取当前工作目录，即当前python脚本工作的目录路径
   os.chdir("dirname")  改变当前脚本工作目录；相当于shell下cd
   
   import os
   print('当前路径：',os.getcwd())
            
   #os.path
   os.path.dirname(path) 返回path的目录。其实就是os.path.split(path)的第一个元素 
   os.path.basename(path) 返回path最后的文件名。如何path以／或\结尾，那么就会返回空值。即os.path.split(path)的第二个元素
   os.path.exists(path)  如果path存在，返回True；如果path不存在，返回False
   os.path.isabs(path)  如果path是绝对路径，返回True
   os.path.isfile(path)  如果path是一个存在的文件，返回True。否则返回False
   os.path.isdir(path)  如果path是一个存在的目录，则返回True。否则返回False   
   os.path.split(path) 将path分割成目录和文件名二元组返回 
   os.path.join(path1[, path2[, ...]])  将多个路径组合后返回，第一个绝对路径之前的参数将被忽略
   os.path.getatime(path)  返回path所指向的文件或者目录的最后访问时间
   os.path.getmtime(path)  返回path所指向的文件或者目录的最后修改时间
   os.path.getsize(path) 返回path的大小
   os.path.abspath(path) 返回path规范化的绝对路径
            
   #sys模块是与python解释器交互的一个接口    
   sys.argv           命令行参数List，第一个元素是程序本身路径
   sys.exit(n)        退出程序，正常退出时exit(0),错误退出sys.exit(1)
   sys.version        获取Python解释程序的版本信息
   sys.path           返回模块的搜索路径，初始化时使用PYTHONPATH环境变量的值
   sys.platform       返回操作系统平台名称
   sys.modules 查看导入了哪些模块         
   ```

2. shutil模块

   ```python
   import shutil
   
   # 拷贝文件
   # shutil.copy2('原文件', '现文件')
   # shutil.copy2('file', 'temp')
   
   # 拷贝目录
   # shutil.copytree("原目录", "新目录", ignore=shutil.ignore_patterns("*.pyc"))
   # shutil.copytree("/Users/jingliyang/PycharmProjects/面试题/常用模块/logging模块", "logging模块2", ignore=shutil.ignore_patterns("__init__.py"))
   
   # 删除目录
   # shutil.rmtree("temp", ignore_errors=True)
   # shutil.rmtree("logging模块2", ignore_errors=True)
   
   # 移动文件/目录
   # shutil.move("logging模块", "logging2", copy_function=shutil.copy2)
   
   # 获取磁盘使用空间
   # total, used, free = shutil.disk_usage(".")
   # print("当前磁盘共: %iGB, 已使用: %iGB, 剩余: %iGB"%(total / 1073741824, used / 1073741824, free / 1073741824))
   import shutil
   total, used, free = shutil.disk_usage(".") #也可以指定磁盘名
   print("当前磁盘共: %iGB, 已使用: %iGB, 剩余: %iGB"%(total / 1073741824, used / 1073741824, free / 1073741824))  #当前磁盘共: 219GB, 已使用: 64GB, 剩余: 154GB
   
   import shutil
   total, used, free = shutil.disk_usage("c://")
   print("当前磁盘共: %iGB, 已使用: %iGB, 剩余: %iGB"%(total / 1073741824, used / 1073741824, free / 1073741824))   #当前磁盘共: 245GB, 已使用: 90GB, 剩余: 154GB
   
   
   # 压缩文件
   # shutil.make_archive('压缩文件夹的名字', 'zip','待压缩的文件夹路径')
   # shutil.make_archive('logging2', 'zip','/Users/jingliyang/PycharmProjects/面试题/常用模块/随机数')
   
   # 解压文件
   # shutil.unpack_archive('zip文件的路径.zip'，'解压到目的文件夹路径')
   # shutil.unpack_archive('/Users/jingliyang/PycharmProjects/面试题/常用模块/shutil模块/logging2.zip','/Users/jingliyang/PycharmProjects/面试题/常用模块/shutil模块/tmp')
   ```

3. logging模块

   ```python
   # 为什么要写log?
       # log是为了排错
       # log用来做数据分析的
   
   # 比如购物商城 - 数据库里，记录
       # 什么时间购买了什么商品
       # 把哪些商品加入购物车了
   
   # 做数据分析的内容 - 记录到日志
   # 1.一个用户什么时间在什么地点 登录了购物程序
   # 2.搜索了哪些信息,所长时间被展示出来了
   # 3.什么时候关闭了软件
   # 4.对哪些商品点进去看了
   
   
   # 输出内容是有等级的 : 默认处理warning级别以上的所有信息,（日志级别等级CRITICAL > ERROR > WARNING > INFO > DEBUG），即处理warning/error/critical
   import logging
   logging.debug('debug message')          # 调试
   logging.info('info message')            # 信息
   logging.warning('warning message')      # 警告
   logging.error('error message')          # 错误
   logging.critical('critical message')    # 批判性的
   
   #无论你希望日志里打印哪些内容,都得你自己写,没有自动生成日志这种事儿
   
   #输出到屏幕
   import logging
   logging.basicConfig(
       format='%(asctime)s - %(name)s - %(levelname)s[line :%(lineno)d]-%(module)s:  %(message)s',
       datefmt='%Y-%m-%d %H:%M:%S %p',
   )
   logging.warning('warning message test2')
   logging.error('error message test2')
   logging.critical('critical message test2')
   
   # 输出到文件,并且设置信息的等级
   import logging
   logging.basicConfig(
       format='%(asctime)s - %(name)s - %(levelname)s[line :%(lineno)d]-%(module)s:  %(message)s',
       datefmt='%Y-%m-%d %H:%M:%S %p',
       filename='tmp.log',#输出到文件
       level= logging.DEBUG  #设置信息的等级
   )
   
   logging.debug('debug 信息错误 test2')
   logging.info('warning 信息错误 test2')
   logging.warning('warning message test2')
   logging.error('error message test2')
   logging.critical('critical message test2')
       
   #以下两个是要记住的：很重要
   # 解决上面的两个 不能同时向文件和屏幕上输出 和 输出到文件乱码问题  
   import logging
   fh = logging.FileHandler('tmp.log',encoding='utf-8')
   sh = logging.StreamHandler()
   logging.basicConfig(
       format='%(asctime)s - %(name)s - %(levelname)s[line :%(lineno)d]-%(module)s:  %(message)s',
       datefmt='%Y-%m-%d %H:%M:%S %p',
       level= logging.DEBUG,
       handlers=[fh,sh]
   )
   logging.debug('debug 信息错误 test2')
   logging.info('warning 信息错误 test2')
   logging.warning('warning message test2')
   logging.error('error message test2')
   logging.critical('critical message test2')
   
   # 做日志的切分
   import logging
   import time
   from logging import handlers
   sh = logging.StreamHandler()
   rh = handlers.RotatingFileHandler('myapp.log', maxBytes=1024,backupCount=5)   # 按照大小做切割
   fh = handlers.TimedRotatingFileHandler(filename='x2.log', when='s', interval=5, encoding='utf-8')
   logging.basicConfig(
       format='%(asctime)s - %(name)s - %(levelname)s[line :%(lineno)d]-%(module)s:  %(message)s',
       datefmt='%Y-%m-%d %H:%M:%S %p',
       level= logging.DEBUG,
       handlers=[fh,rh,sh]
   )
   for i in range(1,100000):
       time.sleep(1)
       logging.error('KeyboardInterrupt error %s'%str(i))
   ```

   

