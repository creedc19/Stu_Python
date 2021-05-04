### 一、今日内容

1. ##### 正则表达式    （非常重要）

   + 模块和实际工作的关系？
         time模块和时间是什么关系？
         re模块和正则表达式的关系？：有了re模块就可以在python语言中操作正则表达式了

   + 什么是正则表达式？

     + 正则就是用一些具有特殊含义的符号组合到一起（称为正则表达式）来描述字符或者字符串的方法。或者说：正则就是用来描述一类事物的规则。
     + 一套规则 ------匹配字符串的规则

   + 能做什么？
         1.检测一个输入的字符串是否合法  -- web开发项目 表单验证
             用户输入一个内容的时候,我们要提前做检测
             能够提高程序的效率并且减轻服务器的压力
         2.从一个大文件中找到所有符合规则的内容 -- 日志分析\爬虫
             能够高效的从一大段文字中快速找到符合规则的内容

   + ##### 正则规则：所有的规则中的字符就可以刚好匹配到字符串中的内容

     ##### 字符组 []：描述的是一个位置上能出现的所有可能性，接受范围,可以描述多个范围,连着写就可以了

     ```python
     [abc]   一个中括号只表示一个字符位置，匹配a或者b或者c
     #根据ascii进行范围的比对
     [0-9]   匹配0-9之间的数字，
     [a-z]   匹配a-z之间的小写字母
     [A-Z]   匹配A-Z之间的小写字母
     
     [a-zA-Z] 匹配大小写
     [0-9a-z] 匹配数字字母
     [0-9a-zA-Z] 匹配数字大小写字母
     
     ```

   + 元字符：在正则表达式中能够帮助我们表示匹配的内容的符号都是正则中的 元字符

     ```python
     [0-9]        -->  \d   表示匹配一位任意数字 （digit）
     [0-9a-zA-Z_] -->  \w   表示匹配数字字母下划线 （word）
     空格 -->     （前面有一个空格，用空格去匹配空格）
     tab  -->  \t
     enter回车  -->  \n
     空格,tab和回车 --> \s  表示所有空白 包括空格 tab和回车
     ```

   + ##### 元字符 -- 匹配内容的规则

     | 元字符                 | 匹配内容                                                     |
     | ---------------------- | ------------------------------------------------------------ |
     | \d                     | 匹配数字                                                     |
     | \w                     | 匹配字母（包含中文）或数字或下划线                           |
     | \s                     | 匹配任意的空白符                                             |
     | \t                     | 匹配一个制表符(TAB键)                                        |
     | \n                     | 匹配一个换行符（enter键）                                    |
     | \W                     | 匹配非字母（包含中文）或数字或下划线                         |
     | \S                     | 匹配任意非空白符                                             |
     | \D                     | 匹配非数字                                                   |
     | [\d\D]、[\w\W]、[\s\S] | 表示匹配所有                                                 |
     | []                     | 匹配字符组中的字符                                           |
     | [^]                    | 匹配除了字符组中的字符的所有字符                             |
     | ^                      | 匹配字符串的开始                                             |
     | $                      | 匹配字符串的结尾                                             |
     | .                      | 匹配任意字符，除了换行符，当re.DOTALL标记被指定时，则可以匹配包括换行符的任意字符。 |
     | a表达式\|b表达式       | 匹配a或者b表达式中的内容,如果匹配a成功了,不会继续和b匹配,所以,如果两个规则有重叠部分,总是把长的放在前面 |
     | ()                     | 匹配括号内的表达式，也表示一个组,约束 \| 描述的内容的范围问题 |

     ```python
     [\d]
     [^\d]  匹配所有的非数字
     [^1]  匹配所有的非数字1
     
     #http://tool.chinaz.com/regex/， 进这个网站测试正则规则
     www\.oldboy\.com|www\.baidu\.com|www\.jd\.com|www\.taobao\.com
     www\.(oldboy|baidu|jd|taobao)\.com
     
     # 记忆元字符 : 都是表示能匹配哪些内容,一个元字符总是表示一个字符位置上的内容，建议如下记忆顺序
         # \d \w \s \t \n \D \W \S
         # [] [^] .
         # ^ $
         # | ()
     ```

   + ##### 量词

     | 量词  |                                |
     | ----- | ------------------------------ |
     | {n}   | 表示匹配n次                    |
     | {n,}  | 表示匹配至少n次                |
     | {n,m} | 表示至少匹配n次,至多m次        |
     | ？    | 表示匹配0次或1次 ，相当于{0,1} |
     | +     | 表示1次或多次  ，相当于{1,}    |
     | *     | 表示0次或多次 ，相当于{0,}     |

     ```python
     1、匹配手机号，11位数字，第一位为1，第二位为3-9？
     	1[3-9]\d{9} #符合规则的11位都能匹配
     	判断用户输入的内容是否合法,如果用户输入的对就能查到结果,如果输入的不对就不能查到结果？
     	^1[3-9]\d{9}$ #这个就只能输入11位手机号，前后都不能多
     	从一个大文件中找到所有符合规则的内容？
     	1[3-9]\d{9}
     ```

   + 贪婪匹配和非贪婪(惰性)匹配

     + 贪婪匹配：在量词范围允许的情况下,尽量多的匹配内容

     + `.*x`      表示匹配任意字符 ，任意多次数 ，遇到最后一个x才停下来

     + ##### 非贪婪(惰性)匹配：规定：在量词后面加个问号 ?  就表示这个量词是非贪婪的，非贪婪就是在量词允许的范围内尽量少匹配 。     `元字符  量词  ?`  表示在这个量词范围内的尽量少匹配     

     + `.*?x`  表示匹配任意字符，任意多次数，但是一旦遇到x，就停下来 (用的最多)

     + `元字符?`这个问号只是量词，表示匹配元字符0次或者1次。`元字符??`表示非贪婪的匹配0次或者1次， 第一个问号表示量词，第二个问号表示非贪婪

   + 转义符

     + 原本有特殊意义的字符,到了表达它本身的意义的时候,需要转义
     + 有一些有特殊意义的内容,放在字符组中,会取消它的特殊意义
       + [().*+?]   内所有的内容在字符组中会取消它的特殊意义
       + `[a\-c]  -`在字符组中表示范围,如果不希望它表示范围,需要转义,或者放在字符组的最前面或者最后面

   + 小总结

     ```python
     # 元字符
         # \d \s \w \t \n \D \S \W
         # [] [^] .
         # ^ $
         # () |
     # 量词
         # {}  表示任意的次数,任意的次数范围,至少多少次
         # ? + *
     # 贪婪和非贪婪匹配
         # 总是在量词范围内尽量多匹配 - 贪婪
         # 总是在量词范围内尽量少匹配 - 惰性
         #在量词后面加个问号 ?  就表示这个量词是非贪婪的，这是规定
         	# .*?x  匹配任意内容任意次数 遇到x就停止
             # .+?x  匹配任意内容至少1次 遇到x就停止
     # 转义符问题
         # . 有特殊的意义,取消特殊的意义\.
         # 取消一个元字符的特殊意义有两种方法
             # 在这个元字符前面加\
             # 对一部分字符生效,把这个元字符放在字符组里
                 # [.()+?*]
                 
     #习题：匹配18/15位的身份证号
     	#15位身份证，要求1-9开头
         [1-9]\d{14}
         #18位身份证，要求1-9开头 ，中间16位任意，最后一位是0-9或者x
         [1-9]\d{16}[0-9x]
         #同时匹配15位或者18位身份证
         [1-9]\d{16}[0-9x]|[1-9]\d{14}
     	^([1-9]\d{16}[0-9x]|[1-9]\d{14})$
     	[1-9]\d{14}(\d{2}[\dx])?
     	^[1-9]\d{14}(\d{2}[\dx])?$  注意这四个的区别
     ```

   + ##### re 模块

     ```python
     import re
     ret1 = re.findall('\d','dsfds654zxc3s532xz5ds')
     ret2 = re.findall('\d+','dsfds654zxc3s532xz5ds')
     print(ret1) #['6', '5', '4', '3', '5', '3', '2', '5']
     print(ret2) #['654', '3', '532', '5']
     
     ret1 = re.search('\d+','dsfds654zxc3s532xz5ds')
     print(ret1)  #<re.Match object; span=(5, 8), match='654'>
     print(ret1.group()) #654
     
     #分组()
     # findall 还是按照完整的正则进行匹配,只是显示括号里匹配到的内容
     ret1 = re.findall('9\d\d','asd96451addcsd46a')
     ret2 = re.findall('9(\d)\d','asd96451addcsd46a')
     print(ret1)  #['964']
     print(ret2)  #['6']  只显示括号里匹配到的内容
     
     # search 还是按照完整的正则进行匹配,显示也显示匹配到的第一个内容,但是我们可以通过给group方法传参数来获取具体分组中的内容
     ret1 = re.search('9\d\d','asd96451addcsd46a')
     ret2 = re.search('9(\d)(\d)','asd96451addcsd46a')
     print(ret1)  #<re.Match object; span=(3, 6), match='964'>
     print(ret2)  #<re.Match object; span=(3, 6), match='964'>
     print(ret2.group())  #964
     print(ret2.group(1))  #6
     print(ret2.group(2))  #4
     print(ret1.group(1))  #报错
     
     # findall
         # 取所有符合条件的,优先显示分组中的
     # search 只取第一个符合条件的,没有优先显示这件事儿
         # 得到的结果是一个变量
             # 变量.group() 的结果 完全和 变量.group(0)的结果一致
             # 变量.group(n) 的形式来指定获取第n个分组中匹配到的内容
      
     
     # 为什么在search中不需要分组优先 而在findall中需要?
     # findall 加上括号 是为了对真正需要的内容进行提取 
     #获取字符串'<h1>askh930s02391j192agsj</h1>'中askh930s02391j192agsj的这些字符，findall这时候利用分组优先显示来获取就比较方便了
     ret = re.findall('<\w+>(\w+)</\w+>','<h1>askh930s02391j192agsj</h1>')
     print(ret) #['askh930s02391j192agsj']
     
     #search  通过变量.group(n) 的形式来指定获取第n个分组中匹配到的内容
     ret = re.search('<(\w+)>(\w+)</\w+>','<h1>askh930s02391j192agsj</h1>')
     print(ret.group())
     print(ret.group(1))
     print(ret.group(2))
     
     # 为什么要用分组,以及findall的分组优先到底有什么好处？
     	#如果我们要查找的内容在一个复杂的环境中，我们要查的内容并没有一个突出的 与众不同的特点 甚至会和不需要的杂乱的数据混合在一起，这个时候我们就需要把所有的数据都统计出来,然后对这个数据进行筛选,把我们真正需要的数据对应的正则表达式用()圈起来，这样我们就可以筛选出真正需要的数据了
     
     exp = '2-3*(5+6)'
     # 从exp中匹配加法 a+b 或者是减法 a-b 并且计算他们的结果
     ret = re.search('\d+[+]\d+',exp)
     print(ret.group())
     a,b = ret.group().split('+')
     print(int(a)+int(b))  #这方法有点麻烦，看下面，利用分组来
     
     ret = re.search('(\d+)[+](\d+)',exp)
     print(ret)
     print(ret.group(1))
     print(ret.group(2))
     print(int(ret.group(1)) + int(ret.group(2)))
     
     #简单爬虫例子
     '''
     douban.html 文件内容
     
     iv class="item">
                     <div class="pic">
                         <em class="">1</em>
                         <a href="https://movie.douban.com/subject/1292052/">
                             <img width="100" alt="肖申克的救赎" src="https://img3.doubanio.com/view/photo/s_ratio_poster/public/p480747492.webp" class="">
                         </a>
                     </div>
                     <div class="info">
                         <div class="hd">
                             <a href="https://movie.douban.com/subject/1292052/" class="">
                                 <span class="title">肖申克的救赎</span>
                                         <span class="title">&nbsp;/&nbsp;The Shawshank Redemption</span>
                                     <span class="other">&nbsp;/&nbsp;月黑高飞(港)  /  刺激1995(台)</span>
                             </a>
     
     
                                 <span class="playable">[可播放]</span>
                         </div>
                         <div class="bd">
                             <p class="">
                                 导演: 弗兰克·德拉邦特 Frank Darabont&nbsp;&nbsp;&nbsp;主演: 蒂姆·罗宾斯 Tim Robbins /...<br>
                                 1994&nbsp;/&nbsp;美国&nbsp;/&nbsp;犯罪 剧情
                             </p>
     
     
                             <div class="star">
                                     <span class="rating5-t"></span>
                                     <span class="rating_num" property="v:average">9.6</span>
                                     <span property="v:best" content="10.0"></span>
                                     <span>1433861人评价</span>
                             </div>
     
                                 <p class="quote">
                                     <span class="inq">希望让人自由。</span>
                                 </p>
                         </div>
                     </div>
                 </div>
             </li>
             <li>
                 <div class="item">
                     <div class="pic">
                         <em class="">2</em>
                         <a href="https://movie.douban.com/subject/1291546/">
                             <img width="100" alt="霸王别姬" src="https://img3.doubanio.com/view/photo/s_ratio_poster/public/p1910813120.webp" class="">
                         </a>
                     </div>
                     <div class="info">
                         <div class="hd">
                             <a href="https://movie.douban.com/subject/1291546/" class="">
                                 <span class="title">霸王别姬</span>
                                     <span class="other">&nbsp;/&nbsp;再见，我的妾  /  Farewell My Concubine</span>
                             </a>
     
     
                                 <span class="playable">[可播放]</span>
                         </div>
                         <div class="bd">
                             <p class="">
                                 导演: 陈凯歌 Kaige Chen&nbsp;&nbsp;&nbsp;主演: 张国荣 Leslie Cheung / 张丰毅 Fengyi Zha...<br>
                                 1993&nbsp;/&nbsp;中国大陆 香港&nbsp;/&nbsp;剧情 爱情 同性
                             </p>
     
     
                             <div class="star">
                                     <span class="rating5-t"></span>
                                     <span class="rating_num" property="v:average">9.6</span>
                                     <span property="v:best" content="10.0"></span>
                                     <span>1062101人评价</span>
                             </div>
     
                                 <p class="quote">
                                     <span class="inq">风华绝代。</span>
                                 </p>
                         </div>
                     </div>
     '''
     #爬取上述网页源码的电影名字？
     #版本一：
     import re
     with open('douban.html',encoding='utf-8') as f:
         content = f.read()
     ret = re.findall('<span class="title">(.*?)</span>\s*<span class="title">.*?</span>',content)
     print(ret)   #少了一步电影 霸王别姬，那部电影格式有点不一样
     
     #版本二：
     with open('douban.html',encoding='utf-8') as f:
         content = f.read()
     ret = re.findall('<span class="title">(.*?)</span>(?:\s*<span class="title">.*?</span>)?',content)   #?:  取消分组优先显示
     print(ret)
     
     ```

   + 什么是爬虫

     + 通过代码获取到一个网页的源码，要的是源码中嵌着的网页上的内容 。可以通过requests模块获取一个网页的源码，然后通过正则表达式获取所需的内容。

       ```python
       import requests
       ret = requests.get('https://movie.douban.com/top250?start=0&filter=')
       print(ret.content.decode('utf-8'))
       ```

   + ```python
     #小结：
     # 分组和findall的现象
         # 为什么要用分组?
             # 把想要的内容放分组里
     # 如何取消分组优先
         # 如果在写正则的时候由于不得已的原因 导致不要的内容也得写在分组里
         # (?:)  取消这个分组的优先显示
     ```

     

