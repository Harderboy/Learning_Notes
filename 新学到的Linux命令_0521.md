# Linux的一些命令

- 查看端口占用情况
  - lsof -i:端口号
  - netstat
  - 注意区别 ps作用：？

- 切换root用户
  - 初始化root用户：`sudo root passwd` 自定义root密码
  - 切换root用户：su - (默认切换到root用户)

- 查看日志，并搜索关键词
  - less access.log
  - tail -f acess.log ：滚动日志输出

- 