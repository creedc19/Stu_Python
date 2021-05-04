1. Pycharm的基本使用

   1. 在Pycharm下为你的Python项目配要Python解程器

      1. Project当前项目名>Project Interpreter>add Local

   2. 在Pycharm 下创建Python文件、Python模块

      1. File>New>Python File
      2. File>New>Python Package

   3. 使用Pycharm安装Python第三方模块

      1. Project:当前项目名>Project Interpreter>点击右侧绿色小加号

   4. Pycharm基本设置，例如不使用tab、tab=4空格,、字体、字体颜色、主题、脚本头设置、显示行号等。如何导出和导入自定义设置。

      1. 不使用tab、tab= 4空格: Editor>Code Style> Python
      2. 字体、字体颜色: Edit>Colors & Fonts>Python
      3. 关闭自动更新: Appearance & Behavior> System Settings>Updates
      4. 脚本头设置: Edit>File and Code Templates>Python Script    注:其他类似
      5. 显示行号: Edit>General>Appearance> Show line numbers     注: 2016.2默认显示行号
      6. 右侧竖线是PEP8的代码规范，提示一行不要超过120个字符
      7. 导出、导入你自定义的配置: File>Export  Settings、Import Settings

   5. 常用快捷健，例如复制当前行、删除当前行，批量注释，缩进，查找和替换。

      1. 常用快捷性的询和配置: Keymap
         1. Ctrl+D：复制当前行
         2. Ctrl+E：删除当前行
         3. Shit + Enter：快速换行
         4. Ctrl+/：快速注释  (选中多行后可以批量注释)  ，注释完选中再按Ctrl+/就是取消注释
         5. Tab：缩进当前行(选中多行后可以批量缩进)
         6. Shift+ Tab：取消缩进(选中多行后可以批量取消缩进)
         7. Ctrl+F：查找
         8. Ctrl+H：替换
         9. Ctrl+Z： 撤回上一次记录，可以一直撤回
         10. Ctrl+Shift+Z： 恢复刚才撤回的记录

   6. Pycharm安装插件,例Markdown support、数据库支持插件等。

      1. Plugins>Browse repositories(下方三个按钮中间那个)>搜索 'markdown support'  > install
      2. 右上角View有三个选项可选,一般我们都用中间那个左侧编写,右侧实时预览

   7. Git配置

      1. 需要本地安装好Git
      2. Version Control>Git
      3. 配置了Git等版本控制系统之后，可以很方便的diff查看文件的不用

   8. 常用操作指南，例如复制文件路径、在文件管理器中打开、快速定位、查看模块结构视图、tab批量换space、TODO的使用、Debug的使用。

      1. 复制文件路径：左侧文件列表右健选中的文件>Copy Path
      2. 在文件T理器中打开：右键选中的文件>在下找到Show In Explorer
      3. 快速定位：Ctrl+某些内建模块之后，点击在源文件中展开
      4. 查看结构：IDE左衡边栏Structure查看当前项目的结构
      5. tab批量换space：Edit>Convert Indents
      6. TODO的使用：#TODO要记录的事情
      7. Tab页上右键>Move Right( Down)，把当前Tab页移到窗口右边(下边)，方便对比
      8. 文件中右键>Local History能够查文件修改前后的对比
      9. IDE右下角能看到一些有用的信息，光标当前在第几行的第几个字符、当前回车换行、当前编码类型、当前Git分支
      10. IDE右例边栏>Database

   9. 个性化设置之ctrl+鼠标滚轮控制字体大小：设置>搜索输入mouse>Editor>General> Mouse 中选中change font size(zoom) with ctrl +mouse wheel      就可以通过Ctrl+鼠标滚轮来调节代码大小

   10. 切换解释器的版本（例如python3和python2互相切换)：打开files-->settings-->Project:python24-->Project Interpreter

   11. 设置>搜索输入coding>Editor>file Encoding 里面project Encoding 和Default encoding for properties files  都改成UTF-8 格式，以后写代码不容易报错。

       

