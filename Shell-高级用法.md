# shell 编程的一些高级用法

## 简单总结

### 大括号的用法

```shell
# ls {ex1,ex2}.sh    
ex1.sh  ex2.sh    
# ls {ex{1..3},ex4}.sh    
ex1.sh  ex2.sh  ex3.sh  ex4.sh    
# ls {ex[1-3],ex4}.sh    
ex1.sh  ex2.sh  ex3.sh  ex4.sh

LOCAL_OUTPUT_ROOT="${IAM_ROOT}/${OUT_DIR:-_output}"
# ${var:-string}和${var:=string}:若变量var为空，则用在命令行中用string来替换${var:-string}，否则变量var不为空时，则用变量var的值来替换${var:-string}；对于${var:=string}的替换规则和${var:-string}是一样的，所不同之处是${var:=string}若var为空时，用string替换${var:=string}的同时，把string赋给变量var： ${var:=string}很常用的一种用法是，判断某个变量是否赋值，没有的话则给它赋上一个默认值。
```

### 双冒号（一种函数命名方式）

> The :: is just a Naming Convention for function names.

```shell
function guess_built_binary_path {
  local hyperkube_path=$(kube::util::find-binary "hyperkube")
  if [[ -z "${hyperkube_path}" ]]; then
    return
  fi
  echo -n "$(dirname "${hyperkube_path}")"
}


function iam::mariadb::install()
{
  # 1. 配置 MariaDB 10.5 Yum 源
  echo ${LINUX_PASSWORD} | sudo -S bash -c "cat << 'EOF' > /etc/yum.repos.d/mariadb-10.5.repo
# MariaDB 10.5 CentOS repository list - created 2020-10-23 01:54 UTC
# http://downloads.mariadb.org/mariadb/repositories/
[mariadb]
name = MariaDB
baseurl = https://mirrors.aliyun.com/mariadb/yum/10.5/centos8-amd64/
module_hotfixes=1
gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
gpgcheck=0
EOF"

  # 2. 安装MariaDB和MariaDB客户端
  iam::common::sudo "yum -y install MariaDB-server MariaDB-client"

  # 3. 启动 MariaDB，并设置开机启动
  iam::common::sudo "systemctl enable mariadb"
  iam::common::sudo "systemctl start mariadb"

  # 4. 设置root初始密码
  iam::common::sudo "mysqladmin -u${MARIADB_ADMIN_USERNAME} password ${MARIADB_ADMIN_PASSWORD}"

  iam::mariadb::status || return 1
  iam::mariadb::info
  iam::log::info "install MariaDB successfully"
}

# 安装后打印必要的信息
function iam::mariadb::info() {
cat << EOF
MariaDB Login: mysql -h127.0.0.1 -u${MARIADB_ADMIN_USERNAME} -p'${MARIADB_ADMIN_PASSWORD}'
EOF
}


# 状态检查
function iam::mariadb::status()
{
  # 查看mariadb运行状态，如果输出中包含active (running)字样说明mariadb成功启动。
  systemctl status mariadb |grep -q 'active' || {
    iam::log::error "mariadb failed to start, maybe not installed properly"
    return 1
  }

  mysql -u${MARIADB_ADMIN_USERNAME} -p${MARIADB_ADMIN_PASSWORD} -e quit &>/dev/null || {
    iam::log::error "can not login with root, mariadb maybe not initialized properly"
    return 1
  }
}


function iam::common::sudo {
  echo ${LINUX_PASSWORD} | sudo -S $1
}
```

### `source` 命令

`source filename` 与 `sh filename` 及 `./filename` 执行脚本的区别

- 当shell脚本具有可执行权限时，用`sh filename`与`./filename`执行脚本是没有区别的。`./filename`是因为当前目录没有在PATH中，所以 `.` 是用来表示当前目录的。
- `sh filename` 重新建立一个子shell，在子shell中执行脚本里面的语句，该子shell继承父shell的环境变量，但子shell新建的、改变的变量不会被带回父shell。
- `source filename`：这个命令其实只是简单地读取脚本里面的语句依次在当前shell里面执行，没有建立新的子shell。那么脚本里面所有新建、改变变量的语句都会保存在当前shell里面。

### 转义字符

shell 中转义字符为 `\`，vim 中搜索命令为 `/`

### 函数封装，函数调用，函数传参等

```bash
# Log an error and exit.
# Args:
#   $1 Message to log with the error
#   $2 The error code to return
local message="${1:-}"
local code="${2:-1}"
```

```bash
# test.sh 内容如下：
iam::log::info() {
for message; do
    echo "${message}"
done
}

iam::log::info "ddd"

# bash 执行
# [liuheng@localhost ~]$ ./test.sh
# ddd
```

==出现以上执行结果的原因：==

调用函数的时候，给函数传参：`"ddd"`，函数将 `"ddd"` 传递给函数 `iam::log::info()` 中未声明的变量 `message`，导致输出 `ddd`
==我学废了==

**和其它编程语言不同的是，Shell 函数在定义时不能指明参数，但是在调用时却可以传递参数，并且给它传递什么参数它就接收什么参数。**

### 等等，继续发现，继续学习基本语法

### 参考

- [shell 中各种括号的作用()、(())、[]、[[]]、{}](https://www.runoob.com/w3cnote/linux-shell-brackets-features.html)
- [Shell Style Guide](https://google.github.io/styleguide/shellguide.html)
- [Camel case](https://es.wikipedia.org/wiki/Camel_case)
- [Snake case](https://en.wikipedia.org/wiki/Snake_case)
- [Shell Command Language](https://pubs.opengroup.org/onlinepubs/9699919799/utilities/V3_chap02.html#tag_18_09_05)
- [Shell脚本中参数传递方法常用有8种](https://codeantenna.com/a/yB50JTcznc)
