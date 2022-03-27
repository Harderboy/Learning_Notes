# Linux 中父 shell 与子 shell

## export 命令

1. 什么是export命令？

    - 用户登录到Linux系统后，系统将启动一个用户shell。在这个shell中，可以使用shell执行命令或声明变量，也可以创建并运行 shell 脚本程序。运行shell脚本程序时，系统将创建一个子shell。此时，系统中将有两个shell，一个是登录时系统启动的shell，另一个是系统为运行脚本程序创建的shell。当一个脚本程序运行完毕，它的脚本shell将终止，可以返回到执行该脚本之前的shell。从这种意义上来说，用户可以有许多 shell，每个shell都是由某个shell（称为父shell）派生的。
    - 在子 shell中定义的变量只在该子shell内有效。如果在一个shell脚本程序中定义了一个变量，当该脚本程序运行时，这个定义的变量只是该脚本程序内的一个局部变量，其他的shell不能引用它，要使某个变量的值可以在其他shell中被使用，可以使用export命令对已定义的变量进行输出。
    - export命令将使系统在创建每一个新的shell时，定义这个变量的一个拷贝。这个过程称之为变量输出。

2. 为什么要用export命令？　　

    - 为了使我们定义一个变量时可以在子shell中被调用，而不需要重复去定义。

3. 怎么使用export命令？

    - Linux export命令用于设置或显示环境变量。
    - 在shell中执行程序时，shell会提供一组环境变量。export可新增，修改或删除环境变量，供后续执行的程序使用。export的效力仅限于该次登录或者该shell关闭前。

    语法：

    `export [-fnp][变量名称]=[变量设置值]`

    参数说明：

        -f 　代表[变量名称]中为函数名称。
        -n 　删除指定的变量。变量实际上并未删除，只是不会输出到后续指令的执行环境中。
        -p 　列出所有的shell赋予程序的环境变量。

## Linux 子 shell

Linux执行Scripts有两种方式，主要区别在于是否建立subshell

1. `source filename` or `. filename`

    不创建subshell，只有一个PID，在当前shell环境下读取并执行filename中的命令，相当于顺序执行filename里面的命令，filename可以为**包含任意命令**的文件，如脚本文件、配置文件、txt文件等

    - source命令通常用命令`.`来替代，`source filename`等效于`. filename`
    - source命令（从 C Shell 而来）是bash shell的内置命令。点命令，就是个点符号，（从Bourne Shell而来）是source的另一名称。

    - source（或点）命令通常用于重新执行刚修改的初始化文档，如 `.bash_profile` 和 `.profile` 这些配置文件。
      - 假如在登录后对 `.bash_profile` 中的 EDITER 和 TERM 变量做了修改，这时就可以用 source 命令重新执行 `.bash_profile`文件，使修改立即生效而不用注销并重新登录。
      - 再比如在一个可执行的脚本 `a.sh` 里写这样一行代码：`export KKK=111`
        - 假如用 `./a.sh` 执行该脚本，执行完毕后，在当前shell环境中运行 `echo $KKK`，发现没有值，但是用 `source a.sh` 来执行 ，然后再 `echo $KKK`，就会发现 打印 111 。
        - 原因：因为调用`./a.sh`来执行shell命令是在一个子shell里运行的，所以执行后，该变量并没有返回到父shell里，但是source不同，它就是在本shell中执行的，所以能够看到结果。
    - source命令可用于：
      - 刷新当前的shell环境
      - 在当前环境使用source执行Shell脚本
      - 从脚本中导入环境中一个Shell函数
      - 从另一个Shell脚本中读取变量
      - 读取并执行命令 `source cmd.txt`
      参考 [Linux中source命令的使用方式](https://zhuanlan.zhihu.com/p/357335122)

2. `bash filename` or `./filename`
    创建subshell，在当前bash环境下再新建一个子shell执行filename中的命令

    - 子shell继承父shell的变量，但子shell不能使用父shell的变量，除非使用export（这和命名空间是相似的道理，甚至和c中的函数也有些类似）

    - 子Shell从父Shell继承得来的属性如下：

        - 当前工作目录
        - 环境变量
        - 标准输入、标准输出和标准错误输出
        - 所有已打开的文件标识符
        - 忽略的信号

    - 子Shell不能从父Shell继承的属性，归纳如下：

        - 除环境变量和.bashrc文件中定义变量之外的Shell变量
        - 未被忽略的信号处理

以下几种命令也会创建subshell

- `$(command)`
它的作用是让命令在子shell中执行

- `command`
和`$(commond)`差不多。

- `exec commond`
替换当前的shell却没有创建一个新的进程。进程的pid保持不变
作用:
  - shell的内建命令exec并不启动新的shell，而是用要被执行命令替换当前的shell进程，并且将老进程的环境清理掉，而且exec命令后的其它命令将不再执行。当在一个shell里面执行exec ls后，会列出了当前目录，然后这个shell就自己退出了。后续命令不再执行。因为这个shell已被替换为仅执行ls命令的进程，执行结束自然也就退出了。
  - 需要使用的时候可以用subshell 避免这个影响，一般将exec命令放到一个shell脚本里面，用主脚本调用这个脚本，调用点处可以用`bash a.sh`（a.sh就是存放该命令的脚本），这样会为a.sh建立一个subshell去执行，当执行到exec后，该子脚本进程就被替换成了相应的exec的命令。

**一些结论：**

- `bash filename` or `./filename`执行脚本时是在一个子shell环境运行的，脚本执行完后该子shell自动退出；

- 一个shell中的系统环境变量和用export定义的变量会被复制到子shell中，对该shell或者它的子shell有效，该shell结束时变量消失；

- 不用export定义的变量只对该shell有效，对子shell也是无效的。

## 环境变量

**Linux 环境**

在 Linux Shell 登录成功以后，Linux 会从文件中获取一系列的数据为该次登录所用，这些数据会在某些指令或某些程序中被使用到。这些数据就称为 Linux Shell 运行时的环境。环境中的数据可以大致分为四种：环境变量，Shell 变量，别名(alias)，Shell 函数。

环境变量里有什么？

- 可以直接用无参数的 printenv 命令来输出当前 session 的环境变量以及环境变量的值。若加上参数，则是输出某个变量的值。若想更方便地查看，可以将 printenv 的输出传给 less来查看环境变量：

    `printenv | less`

- 也可通过`env`、`export` 这两个命令查看环境变量

在 Linux 系统中，除了 export 之外，env、set 和 declare 这三个命令也可以显示 Shell 中的变量。

export/env/set/declare 的区别：

- env：显示当前用户的环境变量，但不会显示其自定义变量。
- export：功能同 env 一样，也是显示当前用户的环境变量，只不过该命令的输出是按变量名进行排序的。
- declare：显示当前 Shell 中定义的所有变量，包括用户的环境变量和自定义变量，该命令的输出按变量名进行排序。
- set：功能同 declare 一样，显示当前 Shell 中定义的所有变量，包括用户的环境变量和自定义变量。

环境变量作用域可以简单的分成用户自定义的环境变量以及系统级别的环境变量。

- 用户级别环境变量定义文件：`~/.bashrc`、`~/.profile`（部分系统为：`~/.bash_profile`）
- 系统级别环境变量定义文件：`/etc/bashrc`、`/etc/profile`(部分系统为：`/etc/bash_profile`）、`/etc/environment`

另外在用户环境变量中，系统会首先读取`~/.bash_profile`（或者`~/.profile`）文件，如果没有该文件则读取`~/.bash_login`，根据这些文件中内容再去读取`~/.bashrc`。

按照生命周期来分，Linux环境变量可以分为两类：

- 永久的：需要用户修改相关的配置文件，变量永久生效。
- 临时的：用户利用export命令，在当前终端下声明环境变量，关闭Shell终端失效。

Linux环境变量文件加载顺序可参考

- [Linux 系统中 profile 文件介绍](https://github.com/Harderboy/Internship-Notes/blob/main/%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/linux%E7%B3%BB%E7%BB%9F%E4%B8%ADprofile%E6%96%87%E4%BB%B6%E4%BB%8B%E7%BB%8D.md)
- [Linux环境变量配置全攻略](https://www.cnblogs.com/youyoui/p/10680329.html)

### $PATH

`$PATH`：决定了shell将到哪些目录中寻找命令或程序，PATH的值是一系列目录组成的，各目录之间用冒号`:`隔开。。当执行某个命令时，Linux 会依照 PATH 中包含的目录依次搜寻该命令的可执行文件，一旦找到，即正常执行；反之，则提示无法找到该命令。

参考

- [Linux下source命令详解](https://www.cnblogs.com/shuiche/p/9436126.html)
- [shell编程之export](https://www.cnblogs.com/lfxiao/p/9438474.html)
- [shell与subshell与执行脚本的几种方式](https://blog.csdn.net/lineuman/article/details/52443422)
- [export/set/env/declare 的区别](http://c.biancheng.net/linux/export.html)
- [Linux 环境变量(Environment Variable)](https://zhuanlan.zhihu.com/p/71939491)
- [Linux环境变量配置全攻略](https://www.cnblogs.com/youyoui/p/10680329.html)
- [Linux环境变量总结](https://www.jianshu.com/p/ac2bc0ad3d74)