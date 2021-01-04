# Linux 常用的命令

- 查看端口占用情况
  - lsof：lsof(list open files)是一个列出当前系统打开文件的工具。
  - lsof 查看端口占用语法格式：`lsof -i:端口号`，需要 root 用户权限来执行

    ```bash
    root@liuheng-VirtualBox:~# lsof -i:22
    COMMAND  PID    USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
    sshd    1434    root    3u  IPv4 170257      0t0  TCP *:ssh (LISTEN)
    sshd    1434    root    4u  IPv6 170259      0t0  TCP *:ssh (LISTEN)
    sshd    5464    root    3u  IPv4  53439      0t0  TCP bogon:ssh->bogon:1203 (ESTABLISHED)
    sshd    5477    root    3u  IPv4  53511      0t0  TCP bogon:ssh->bogon:1205 (ESTABLISHED)
    sshd    5661 liuheng    3u  IPv4  53439      0t0  TCP bogon:ssh->bogon:1203 (ESTABLISHED)
    sshd    5662 liuheng    3u  IPv4  53511      0t0  TCP bogon:ssh->bogon:1205 (ESTABLISHED)
    sshd    8595    root    3u  IPv4 103476      0t0  TCP bogon:ssh->bogon:1139 (ESTABLISHED)
    sshd    8597    root    3u  IPv4 103521      0t0  TCP bogon:ssh->bogon:1144 (ESTABLISHED)
    sshd    8711 liuheng    3u  IPv4 103476      0t0  TCP bogon:ssh->bogon:1139 (ESTABLISHED)
    sshd    8726 liuheng    3u  IPv4 103521      0t0  TCP bogon:ssh->bogon:1144 (ESTABLISHED)
    ```

  - `netstat -tunlp` 用于显示 tcp，udp 的端口和进程等相关情况。
    - netstat 查看端口占用语法格式：`netstat -tunlp | grep 端口号`

       ```bash
       root@liuheng-VirtualBox:/home/liuheng# netstat -tunlp | grep 22
      tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      1434/sshd
      tcp6       0      0 :::22                   :::*                    LISTEN      1434/sshd
      ```

    - 可以看到，netstat 显示表示 sshd 既监听在 ipv4 的地址，又监听在 ipv6 的地址。

  - [Linux 查看端口占用情况](https://www.runoob.com/w3cnote/linux-check-port-usage.html)

- Linux ps （英文全拼：process status）命令用于显示当前进程的状态，类似于 windows 的任务管理器。  
ps 是显示瞬间进程的状态，并不动态连续；如果想对进程进行实时监控应该用 top 命令  
使用该命令可以确定有哪些进程正在运行和运行的状态、进程是否结束、进程有没有僵尸、哪些进程占用了过多的资源等等.总之大部分信息都是可以通过执行该命令得到。

  - 这个命令的结果或许会很长。为了便于查看，可以结合 less 命令、grep 命令和管道来使用。
    `ps -aux | less`
    `ps -aux | grep ssh`

    ```bash
    root@liuheng-VirtualBox:~# ps -aux | grep apache2
    USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND  # 手动添加的
    root      1561  0.0  0.5 481732 23008 ?        Ss   12月29   0:02 /usr/sbin/apache2 -k start
    root     29732  0.0  0.0  16180  1044 pts/1    S+   14:04   0:00 grep --color=auto apache2
    www-data 31953  0.0  0.3 484064 13140 ?        S    00:08   0:00 /usr/sbin/apache2 -k start
    www-data 31954  0.0  0.3 484064 13140 ?        S    00:08   0:00 /usr/sbin/apache2 -k start
    www-data 31955  0.0  0.3 484064 13140 ?        S    00:08   0:00 /usr/sbin/apache2 -k start
    www-data 31956  0.0  0.3 484064 13140 ?        S    00:08   0:00 /usr/sbin/apache2 -k start
    www-data 31957  0.0  0.3 484064 13140 ?        S    00:08   0:00 /usr/sbin/apache2 -k start
    ```

**参考链接：**  
[Linux进程之如何查看进程详情？（ps命令](https://juejin.cn/post/6844903721369862152)  
[linux 的ps STAT 说明](http://ask.apelearn.com/question/382)  
[Linux基础命令---显示进程ps](http://blog.itpub.net/29270124/viewspace-2564950/)  
[Linux下ps -ef和ps aux的区别及格式详解](http://www.linuxidc.com/Linux/2016-07/133515.htm)

- Linux top 命令用于实时显示 process 的动态。  
使用权限：所有使用者。

- 切换 root 用户
  - 初始化 root 用户：  

    `sudo root passwd`  自定义 root 密码

  - 切换 root 用户：
    - `su -` (默认切换到root用户)
    - `su root`

- 查看日志，并搜索关键词
  - less access.log
  - tail -f acess.log ：滚动日志输出