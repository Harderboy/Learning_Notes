# Linux的五个查找命令：find,locate,whereis,which,type 及其区别

## Linux type命令的用法

 一般情况下，type命令被用于判断另外一个命令是否是内置命令，但是它实际上有更多的用法。  

1. 判断一个名字当前是否是alias、keyword、function、builtin、file或者什么都不是： 

    - type ls 的输出是 ls 是 `ls --color=auto' 的别名
    - type if 的输出是 if 是 shell 关键字  
    - type type 的输出是 type 是 shell 内嵌  
    - type frydsh 的输出是 bash: type: frydsh: 未找到
          
2. 判断一个名字当前是否是alias、keyword、function、builtin（内置命令）、file或者什么都不是的另一种方法（适用于脚本编程）：  

    - type -t ls 的输出是 alias  
    - type -t if 的输出是 keyword  
    - type -t type 的输出是 builtin  
    - type -t gedit 的输出是 file  
    - type -t frydsh 没有输出           
                                      
3. 显示一个名字的所有可能： 

    - type -a kill 的输出是 kill 是 shell 内嵌 和 kill 是 /bin/kill  
    - type -at kill 的输出是 builtin 和 file
                       
4. 查看一个命令的执行路径（如果它是外部命令的话，查看外部命令的路径）（加上-p参数后，就相当于which命令）：

    - type -p gedit 的输出是 /usr/bin/gedit
    - type -p kill 没有输出（因为kill是内置命令）       
          
5. 强制搜索外部命令的执行路径：

    - type -P kill 的输出是 /bin/kill  

**附：**
- type:不加任何参数时，type会显示 name是外部命令还是内置命令
- -t :当加入 -t 时，type会将name一下面的这些字眼显示出它的意义
- file :表示为外部命令
- alias :表示为命令别名
- builtin :表示为内置命令
- -a :会有PATH变量定义的路径中，将所含有的name的命令列出来，包括alias

# **总结**

## type
**显示指定命令的类型**

## 补充说明

type命令 用来显示指定命令的类型，判断给出的指令是内部指令还是外部指令。

## 命令类型：

- alias：别名。
- keyword：关键字，Shell保留字。
- function：函数，Shell函数。
- builtin：内建命令，Shell内建命令。
- file：文件，磁盘文件，外部命令。
- unfound：没有找到。

## 语法

type(选项)(参数)

## 选项

- -t：输出“file”、“alias”或者“builtin”，分别表示给定的指令为“外部指令”、“命令别名”或者“内部指令”；
- -p：如果给出的指令为外部指令，则显示其绝对路径；
- -a：在环境变量“PATH”指定的路径中，显示给定指令的信息，包括命令别名。

## 参数

指令：要显示类型的指令。

## 具体实例见上 

## 其他命令

## 1. find

find是最常见和最强大的查找命令，你可以用它找到任何你想找的文件。  

find的使用格式如下：

　　`$ find <指定目录> <指定条件> <指定动作>`  

- <指定目录>： 所要搜索的目录及其所有子目录。默认为当前目录。
- <指定条件>： 所要搜索的文件的特征。
- <指定动作>： 对搜索结果进行特定的处理。
 
如果什么参数也不加，find默认搜索当前目录及其子目录，并且不过滤任何结果（也就是返回所有文件），将它们全都显示在屏幕上。

**find的使用实例：**

    $ find . -name "my*"

搜索当前目录（含子目录，以下同）中，所有文件名以my开头的文件。

    $ find . -name "my*" -ls

搜索当前目录中，所有文件名以my开头的文件，并显示它们的详细信息。

    $ find . -type f -mmin -10

搜索当前目录中，所有过去10分钟中更新过的普通文件。如果不加-type f参数，则搜索普通文件+特殊文件+目录。

 

## 2. locate

locate命令其实是“find -name”的另一种写法，但是要比后者快得多，原因在于它不搜索具体目录，而是搜索一个数据库（/var/lib/locatedb），这个数据库中含有本地所有文件信息。Linux系统自动创建这个数据库，并且每天自动更新一次，所以使用locate命令查不到最新变动过的文件。为了避免这种情况，可以在使用locate之前，先使用updatedb命令，手动更新数据库。

locate命令的使用实例：

    $ locate /etc/sh

搜索etc目录下所有以sh开头的文件。

    $ locate ~/m

搜索用户主目录下，所有以m开头的文件。

    $ locate -i ~/m

搜索用户主目录下，所有以m开头的文件，并且忽略大小写。

## 3. whereis

whereis命令只能用于程序名的搜索，而且只搜索二进制文件（参数-b）、man说明文件（参数-m）和源代码文件（参数-s）。如果省略参数，则返回所有信息。

whereis命令的使用实例：

    $ whereis grep

## 4. which

which命令的作用是，在PATH变量指定的路径中，搜索某个系统命令的位置，并且返回第一个搜索结果。也就是说，使用which命令，就可以看到某个系统命令是否存在，以及执行的到底是哪一个位置的命令。

which命令的使用实例：

    $ which grep

## 总结

- which      查看可执行文件的位置 
- whereis    查看文件的位置 
- locate     配合数据库查看文件位置 
- find       实际搜寻硬盘查询文件名称