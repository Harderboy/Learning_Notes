# Linux 文件中追加内容

通过命令行在文件中追加内容主要有3种方式：

1. cat + EOF
2. tee + EOF
3. 重定向 `>>/>`

## EOF 介绍

shell 中通常将 `EOF` 与 `<<` 结合使用，表示后续的输入作为子命令或子 shell 的输入，直到遇到 `EOF` 为止，再返回到主调 shell。可以把 `EOF` 替换成其他东西，意思是把内容当作标准输入传给程序。

当 shell 看到 `<<` 的时候，它就会知道下一个词是一个分界符。在该分界符以后的内容都被当作输入，直到 shell 又看到该分界符(位于单独的一行)。这个分界符可以是你所定义的任何字符串。

主要有 3 种使用方式：

方式 1：`<<EOF` 后续的输入作为子命令或子 shell 的输入
例子：自动登录 mysql（root:root,passwd:123456)，查询test库，test1表里的user=aa的记录。

```bash
#!/bin/sh
mysql -uroot -p123456 <<EOF
use test;
select * from testaa while a=10000; 
exit
EOF
```

方式 2：注释 shell 脚本中的代码

```bash
:<< COMMENTBLOCK
   shell脚本代码段
COMMENTBLOCK

:<<EOF
注释内容...
注释内容...
注释内容...
EOF
```

用来注释整段脚本代码。`:`是 shell 中的空语句。

方式3：结合 tee/cat 对文件内容进行编辑

- 追加：`cat <<EOF/<<-EOF >>file`、`tee -a file <<EOF/<<-EOF`
- 覆盖：`cat <<EOF/<<-EOF >file`、`tee file <<EOF/<<-EOF`

**注意事项：**

需要注意的是，不论是覆盖还是追加，涉及到变量操作，如果需要保留该变量到文件中的话，需要加转义符 `\`。否则，shell脚本将会解释这些变量。
例如：

```bash
#!/bin/bash
cat <<EOF >> /root/a.txt
PATH=\$PATH:\$HOME/bin
export ORACLE_BASE=/u01/app/oracle
export ORACLE_HOME=\$ORACLE_BASE/10.2.0/db_1
export ORACLE_SID=yqpt
export PATH=\$PATH:\$ORACLE_HOME/bin
export NLS_LANG="AMERICAN_AMERICA.AL32UTF8"
EOF
```

## `<<EOF` 和 `<<-EOF` 区别

两个都是获取stdin,并在EOF处结束stdin，输出stdout。
对于`<<-`，man中写到:
> If the redirection operator is <<-, then all leading tab characters are stripped from input lines and  the  line  containing  delimiter.

翻译过来的意思就是：
如果重定向的操作符是 `<<-`，那么分界符（EOF）所在行的开头部分的制表符（Tab）都将被去除。

比如，下面的语句就不会出错：

```bash
cat <<EOF  
Hello,world!  
EOF
```

在我们使用`cat <<EOF`时，我们输入完成后，需要在一个新的一行输入EOF结束stdin的输入。EOF必须顶行写，前面不能用制表符或者空格。

而`<<-`就是为了解决这一问题：

```bash
cat <<-EOF  
Hello,world!  
      EOF
```

上面的写法，虽然最后的EOF前面有多个制表符和空格，但仍然会被当做结束分界符，表示stdin的结束。

## cat + EOF

- 追加：`cat <<EOF/<<-EOF >>file`  
- 覆盖：`cat <<EOF/<<-EOF >file`

## tee + EOF

**tee命令**

- Linux tee命令用于读取标准输入的数据，并将其内容输出成文件。
- tee命令会从标准输入设备读取数据，将其内容输出到标准输出设备，同时保存成文件。

**基本用法实例：**

- `tee file`
读取标准输入的数据，将其内容输出到文件中，同时在终端进行输出。
- `ls | tee ls.txt`
将会在终端上显示ls命令的执行结果，并把执行结果输出到ls.txt文件中，将会覆盖原文件的内容，若无ls.txt文件，将会自动创建该文件
- `ls | tee -a ls.txt`
保留ls.txt文件中原来的内容，并把ls命令的执行结果追加到ls.txt文件的最后，不覆盖原来的内容
- `ls | tee file1.txt file2.txt`
将执行结果同时保存到file1和file2中。

**tee + EOF 使用**：

- 追加：`tee -a file <<EOF/<<-EOF`
- 覆盖：`tee file <<EOF/<<-EOF`

tee命令和`>`重定向很相似，只有一点点区别，`>` 重定向只会将内容重定向到文件，而不会在终端输出，而tee命令会在输出到终端的同时，将内容重定向到文件。

**实例：**

```bash
tomato@DESKTOP-DNRME39:~$ tee file <<EOF
> aaa
> bbb
> ccc
> EOF
aaa
bbb
ccc
tomato@DESKTOP-DNRME39:~$ cat file
aaa
bbb
ccc
tomato@DESKTOP-DNRME39:~$ tee -a file <<EOF
> ddd
> EOF
ddd
tomato@DESKTOP-DNRME39:~$ cat file
aaa
bbb
ccc
ddd
```

## 重定向 `>>/>`

- `>`：覆盖
- `>>`：追加

cat + EOF 建立在 `>>/>`的基础之上

**实例**

`echo "123" >/dev/null`

- 等同于 `echo "123" 1>/dev/null`，不写默认为 `1`，其中 `1` 代表标准输出
类似的：
  - `command 0>filename`，`0` 代表标准输入
  - `command 2>filename`，`2`代表错误输出
- `/dev/null`：可以把 `dev/null` 看作一个“黑洞”，它非常等价于一个只写文件，所有写入它的内容都会永远丢失，而尝试从它那儿读取内容则什么也读不到。

**注意：**

- 文件的话可以使用 `cat <1.txt >file`，`>` 重定向覆盖
- 使用 EOF 的话必须使用 `<<EOF/<<-EOF`，`<<EOF/<<-EOF` 位于中间或者最后都一样

**`&>` 或者 `>&`**

- `&>`或`>&`含义：是绑定的意思
- `command >file2 2>&1`，意思是错误输出绑定标准输出，都输出（重定向）都file2中
- `command 2>&1 >file2`，错误输出输出到终端，标准输出输出（重定向）到file2中，注意区别

- `command >file2 2>file2` 这种方式不被接受，因为会造成冲突

参考:

- [shell中EOF用法](https://www.cnblogs.com/cangqinglang/p/12444577.html)
- [cat <<EOF 与 cat <<-EOF 的区别](https://blog.51cto.com/u_13566681/2072434)
