## 一、昨日内容回顾以及作业讲解（day8）

1. 数据类型的补充

   + str：pass
   + tuple：
     + (1)  ----> int     、 ('alex') ----> str
     + count 、 index
   + list:
     + sort 排序、 sort(reverse= True) 、 reverse()反转
     + 列表相加  、 列表与数字相乘
     + **循环列表的问题**
   + dict:
     + update 更新：增加值，修改值，创建字典，将一个字典的所有键值对覆盖添加到另一个字典。
     + dict.fromkeys(iterable,value)  创建字典    # 面试经常考
     + **循环字典的问题**
   + 数据类型的转换：0 , {} , [] , set() , '' , None 等 转化为bool是False。

2. 编码的进阶：

   ​	ASCII , gbk , Unicode , utf-8 , big5........

   1. 所有的编码本（除去Unicode之外）不能直接互相识别。

   2. **在内存中所有的数据必须是unicode编码存在**，除去bytes。(bytes也是python的基础数据类型)

      int                                                                                                   

      bool

      tuple   

      list

      dict

      set

       str（只有字符串可以直接转换成bytes类型，其它数据类型不能直接转换成bytes类型）

      bytes（只有bytes是非unicode编码，**bytes能用于网络传输、磁盘存储**，只可以和字符串互相转换）

      

      

      **字符串转换成bytes：**

      str-------> encode('utf-8'或者其它) ---------> bytes
      
      ```python
      # encode称作编码:将 str 转化成 bytes类型
      s1 = '中国'
      b1 = s1.encode('utf-8')  # 转化成utf-8的bytes类型
      print(s1)  # 中国
      print(b1)  # b'\xe4\xb8\xad\xe5\x9b\xbd'
      
      s1 = '中国'
      b1 = s1.encode('gbk')  # 转化成gbk的bytes类型
      print(s1)  # 中国
print(b1)  # b'\xd6\xd0\xb9\xfa'
      ```

      

      **bytes转换成字符串：**
      
      bytes------>decode(与字符串转成bytes的一致)--------->str
      
      ```python
      # decode称作解码, 将 bytes 转化成 str类型
b1 = b'\xe4\xb8\xad\xe5\x9b\xbd'
      s1 = b1.decode('utf-8')
      print(s1)  # 中国
      ```
      
      

​	        区别		str                                                                    bytes

​			称呼：  文字文本                                                            字节文本

​						  '' ""  """  """  ''' '''                                                b''/ b"" ........

​						  Unicode                                                             非Unicode



## 二、今日内容

1. 文件操作的初识

   + 护士空姐少妇的联系方式.txt 

   + 利用python代码写一个很low的软件，去操作文件。

     + 文件路径：path
     + 打开方式：读，写，追加，读写，写读......
     + 编码方式：utf-8 , gbk , gb2312......

   + ```python
     f1 = open('d:\少妇空姐模特联系方式.txt',encoding='utf-8',mode='r')
     content = f1.read()
     print(content)
     f1.close()
     ```

   + 上面代码解释：

     ```python
     open 内置函数，open底层调用的是操作系统的接口。
     f1是个变量，其它设置方式：f1,fh,file_handler,f_h,见到f开头的变量叫文件句柄。 对文件进行的任何操作，都得通过文件句柄. 的方式，（如f1.read()）。
     encoding:可以不写，不写参数，默认编码本是：操作系统的默认的编码
     windows系统默认: gbk
     linux系统默认： utf-8
     mac系统默认： utf-8
     f1.close() 关闭文件句柄。
     ```

   + 文件操作的三部曲：

     + 1、打开文件。
     + 2、对文件句柄进行相应操作。
     + 3、关闭文件。

   + 报错原因：

     + UnicodeDecodeError：文件存储时与文件打开时编码本运用不一致。

     + 第二个错误： 路径分隔符产生的问题：（前面加个r解决或者两个\）

       + ```
         r'C:\Users\oldboy\Desktop\主妇空姐模特联系方式.txt'
         'C:\\Users\oldboy\Desktop\主妇空姐模特联系方式.txt'  #计算机会把\U连在一起就像\n换行的作用，所以会报错，搞两个\\就可以避免，最好还是前面加r。
         ```

2. 文件操作的读

   r , rb, r+,r+b 四种模式

   r:  read()  重点                               read(n) 、readline() 、readlines() 

   ​		 for   重点

   **rb: 操作的是非文本的文件,如果你要是带有b的模式操作文件，那么不用声明编码方式**。图片，视频，音频。

   ```python
   #1）read：将文件中的内容全部读取出来;弊端 如果文件很大就会非常的占用内存,容易导致内存奔溃  重点
   f = open('文件的读', encoding='utf-8')    #同一文件夹下是相对路径，直接写文件名就能操作文件，不用写路径（'文件的读'是文件名）。
   content = f.read()
   print(content,type(content))
   f.close()
   
   #2）read(n)：文件打开方式为文本模式时，代表读取n个字符;文件打开方式为b模式时，代表读取n个字节.
   f = open('文件的读', encoding='utf-8')
   content = f.read(5)  #读5个字符
   print(content)
   f.close()
   
   #3）readline()：每次只读取一行,注意点:readline()读取出来的数据在后面都有一个\n，解决这个问题只需要在我们读取出来的文件后边加一个strip()就OK了
   f = open('文件的读', encoding='utf-8')
   print(f.readline())
   print(f.readline())
   msg = f.readline().strip()
   print(msg)
   f.close()
   
   #4）readlines()： 返回一个列表，列表中的每个元素是源文件的每一行,如果文件很大，占内存，容易崩盘。
   f = open('文件的读', encoding='utf-8')
   l1 = f.readlines()
   print(l1)   #['太白金星最帅\n', '老男孩是最好的老师\n', '老男孩教育是最好的学校\n', 'jhdgjkjkjcn\n', 'fjhgvgfhv']
   f.close()
   
   #5）for：循环读取,特点就是每次循环只在内存中占一行的数据，非常节省内存。  重点
   f = open('文件的读', encoding='utf-8')   # f类似于这个列表['太白金星最帅\n', '老男孩是最好的老师\n', '老男孩教育是最好的学校\n', 'jhdgjkjkjcn\n', 'fjhgvgfhv']
   for line in f:
        print(line)
   f.close()
   
   
   #rb：操作非文本文件
   f = open('美女.jpg',mode='rb')
   content = f.read()
   print(content)
   f.close()
   
   #注意点:读完的文件句柄一定要关闭
   
   ```

   

3. 文件操作的写

   w,wb, w+,w+b 四种模式

   ```python
   #1）w: 没有文件，创建文件，写入内容
   f = open('文件的写',encoding='utf-8',mode='w')#只写了文件名，没有路径，文件会自动写入当前文件夹
   f.write('随便写一点')
   f.close()
   
   #2)w: 如果文件存在，先清空原文件内容，在写入新内容
   f = open('文件的写',encoding='utf-8',mode='w')
   f.write('太白最帅....')
   f.close()
   
   # wb：以二进制格式打开一个文件只用于写入。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。如果该文件不存在，创建新文件。一般用于非文本文件如：图片，音频，视频等。
   #以rb的模式将一个图片的内容以bytes类型全部读取出来，然后在以wb将全部读取出来的数据写入一个新文件，这样我就完成了类似于一个图片复制的流程。具体代码如下：
   f = open('美女.jpg',mode='rb')
   content = f.read()
   f.close()
   f1 = open('美女2.jpg',mode='wb')
   f1.write(content)
   f1.close()    
   
   ```

   

4. 文件操作的追加

   a, ab, a+,a+b 四种模式

   ```python
   #1）a:没有文件创建文件，追加内容
   f = open('文件的追加',encoding='utf-8',mode='a')
   f.write('太白最帅....')
   f.close()
   
   #2) a:有文件，在原文件的最后面追加内容。
   f = open('文件的追加',encoding='utf-8',mode='a')
   f.write('大壮，舒淇，b哥，雪飞')
   f.close()
   
   ```

   

5. 文件操作的其他模式 : r+

   ```python
   # r+:读并追加, 顺序不能错误。
   f = open('文件的读写', encoding='utf-8', mode='r+')  #文件需要先存在，这个模式不能创建文件
   content = f.read()
   print(content)
   f.write('人的一切痛苦，本质都是对自己无能的愤怒。')
   f.close()
   
   # 错误示例：不要先写后读
   f = open('文件的读写', encoding='utf-8', mode='r+')
   f.write('人的一切痛苦,,,本质都是对自己无能的愤怒,,,')
   content = f.read()
   print(content)
   f.close()
   ```

   

6. 文件操作的其他功能

   总结：

   三个大方向： 

   **读**，四种模式： r, rb, r+, r+b

   **写**，四种模式 :  w,wb, w+,w+b 

   **追加** ,四种模式:  a, ab, a+,a+b

   相应的功能：对文件句柄的操作：read ,read(n), readline() ,readlines() ,write()  

   ```python
   # 1) tell:获取光标的位置 ,单位字节。  重点
   f = open('文件的读写', encoding='utf-8')
   print(f.tell())
   content = f.read()
   print(f.tell())
   f.close()
   
   # 2) seek: 调整光标的位置，单位字节   重点
   f = open('文件的读写', encoding='utf-8')
   f.seek(7)
   content = f.read()
   print(content)
   f.close()
   
   # 3) flush: 强制刷新
   f = open('文件的其他功能', encoding='utf-8',mode='w')
   f.write('fdshdsjgsdlkjfdf')
   f.flush()
   f.close()
   ```

   

7. 打开文件的另一种方式

   ```python
   #with open():
   # 优点1： 不用手动关闭文件句柄
   with open('文件的读',encoding='utf-8') as f1:
        print(f1.read())   #有缩进
   
   # 优点2：同时操作多个文件
   with open('文件的读', encoding='utf-8') as f1,open('文件的写', encoding='utf-8', mode='w')as f2:
       print(f1.read())
       f2.write('hfdsjkghkajhsdjg')
   
   # 缺点:虽然使用with语句方式打开文件，不用你手动关闭文件句柄，比较省事儿，但是依靠其自动关闭文件句柄，是有一段时间的，这个时间不固定，所以这里就会产生问题，如果你在with语句中通过r模式打开t1文件，那么你在下面又以a模式打开t1文件，此时有可能你第二次打开t1文件时，第一次的文件句柄还没有关闭掉，可能就会出现错误,他的解决方式只能在你第二次打开此文件前，手动关闭上一个文件句柄。
   ```

   

8. 文件操作的改

   + **文件操作改的流程**：
     1, 以读的模式打开原文件。
     2，以写的模式创建一个新文件。
     3，将原文件的内容读出来修改成新内容，写入新文件。
     4，将原文件删除。
     5，将新文件重命名成原文件。
   + 具体代码：

   ```python
   # low版：
   import os
   #1，以读的模式打开原文件。
   #2，以写的模式创建一个新文件。
   with open('alex自述',encoding='utf-8') as f1,\
        open('alex自述.bak',encoding='utf-8',mode='w') as f2:
   #3，将原文件的内容读出来修改成新内容，写入新文件。
        old_content = f1.read()#read是全部读出来，如果文件很大就会非常的占用内存,容易导致内存奔溃。
        new_content = old_content.replace('alex', 'SB')
        f2.write(new_content)
   #4，将原文件删除。
   os.remove('alex自述')
   #5，将新文件重命名成原文件。
   os.rename('alex自述.bak','alex自述')
   
   
   # 进阶版：
   import os
   # 1, 以读的模式打开原文件。
   # 2，以写的模式创建一个新文件。
   with open('alex自述',encoding='utf-8') as f1,\
       open('alex自述.bak',encoding='utf-8',mode='w') as f2:
   # 3，将原文件的内容读出来修改成新内容，写入新文件。
       for line in f1:   #for循环：读一行写一行，能防止文件过大，内存无法读取这么多。
           # 第一次循环： SB是老男孩python发起人，创建人。
           new_line = line.replace('SB', 'alex')
           f2.write(new_line)
   os.remove('alex自述')
   os.rename('alex自述.bak','alex自述')
   
   
   # 有关清空的问题：
   # 清空：关闭文件句柄，再次以w模式打开此文件时，才会清空；如果没有关闭文件句柄（如在循环体内），第二次以后写入内容的时候，之前写入的内容不会被清空
   with open('文件的写', encoding='utf-8',mode='w') as f1:
        for i in range(9):
               f1.write('恢复贷款首付款')  #'文件的写'这个文件写入这句话'恢复贷款首付款'9次，而不是每写一次前一次就清空。
   ```

## 三、今日总结重点

+ 文件操作：
  + **r** ， w ，a ，rb， wb， r+ ，ab 重点记
  + read() ， write ，tell ，seek ， flush
  + 文件的改的代码必须会默写

## 四、预习内容

+ 函数的初识  （见博客）

