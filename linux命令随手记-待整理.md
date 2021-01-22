# 一些命令

1. **Linux下如何查看版本信息，包括位数、版本信息以及CPU内核信息、CPU具体型号等等，整个CPU信息一目了然**
    - `uname －a`（Linux查看版本当前操作系统内核信息）
    `Linux localhost.localdomain 2.4.20-8 #1 Thu Mar 13 17:54:28 EST 2003 i686 athlon i386 GNU/Linux`

    - `cat /proc/version`（Linux查看当前操作系统版本信息）

        ```bash
        Linux version 2.4.20-8 (bhcompile@porky.devel.redhat.com)
        (gcc version 3.2.2 20030222 (Red Hat Linux 3.2.2-5)) #1 Thu Mar 13 17:54:28 EST 2003
        ```

    - `cat /etc/issue` 或 `cat /etc/redhat-release`（Linux查看版本当前操作系统发行版信息）

        `Red Hat Linux release 9 (Shrike)`

    - `cat /proc/cpuinfo`（Linux查看cpu相关信息，包括型号、主频、内核信息等）

        ```bash
        processor         : 0
        vendor_id         : AuthenticAMD  
        cpu family        : 15  
        model             : 1  
        model name        : AMD A4-3300M APU with Radeon(tm) HD Graphics
        stepping          : 0
        cpu MHz           : 1896.236
        cache size        : 1024 KB
        fdiv_bug          : no
        hlt_bug           : no
        f00f_bug          : no
        coma_bug          : no
        fpu               : yes
        fpu_exception     : yes
        cpuid level       : 6
        wp                : yes  
        flags             : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 syscall mmxext lm 3dnowext 3dnow
        bogomips          : 3774.87  
        ```  

    - `getconf LONG_BIT`（Linux查看版本说明当前CPU运行在32bit模式下，但不代表CPU不支持64bit）

      `32`

    - `lsb_release -a` (可列出所有版本信息)

       ```bash
        No LSB modules are available.
        Distributor ID: Ubuntu
        Description:    Ubuntu 18.04 LTS
        Release:        18.04
        Codename:       bionic
        ```  

2. which、file、find、type 的使用方法已完成，文档文件

3. **vim把光标停到一个关键字上面+K直接调用man**

4. **`ctrl+u` 清除已经输入的密码**

5. 目录属性解析：重点为 **x**
    - r(Read，读取)：对文件而言，具有读取文件内容的权限；对目录来说，具有浏览目录的权限。
    - w(Write,写入)：对文件而言，具有新增、修改文件内容的权限；对目录来说，具有删除、移动目录内文件的权限。
    - x(eXecute，执行)：对文件而言，具有执行文件的权限；对目录了来说该用户具有进入目录的权限

6. 文件夹权限（以添加执行权限 **x** 为例）
    - 授予执行权限：  
    `chmod +x filename`
    - 剥夺权限：  
    `chmod -x filename`

7. **hostnamectl使用，改主机系统名称 `hostnamectl + set-hostname NAME`**

8. docker中：运行起来的叫容器，没有运行的叫镜像

9. 配置了 `host-only` 网卡的虚拟机（Linux） ping 不通主机（windows），但是主机可以 ping 通虚拟机，解决方法：关闭 windows 防火墙即可

10. linux 域名解析失败
    - 可以 ping 通 ip，不能 ping 通域名
    - 说明是 DNS 解析 有问题！
      - 修改 /etc/resolv.conf 文件(DNS resolver配置文件) ：
        `nameserver 114.114.114.114`（注意先备份原有文件）
    - 每次重启 linux，都需修改域名解析配置文件，待解决
      - 应该是跟域名解析服务器动静态分配相关？
