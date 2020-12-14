# 官方文档——永远的神

- 字符串常量是通过将文本括在一对双引号（"）中来创建的，字符串常量不能跨越Zeek脚本中的多行，反斜杠字符（\）引入转义序列
- `?`也属于特殊字符，过滤或者搜索需将其转义
- 转义的含义理解清楚
- `NOTICE（）`函数的作用域
- `@load`的使用

**参考链接**  
[转义字符-wiki](https://zh.wikipedia.org/wiki/%E8%BD%AC%E4%B9%89%E5%AD%97%E7%AC%A6)  
[转义符号-zeek](https://s0docs0zeek0org.icopy.site/en/v3.0.6/script-reference/types.html)  
[Zeek 技术介绍与应用](https://blog.csdn.net/ChiWu98/article/details/107517574)  
[zeek脚本编写介绍](https://www.cnblogs.com/DF-Kyun/p/12354060.html)  
[快速入门指南](https://docs.zeek.org/en/current/quickstart/)  
[csdn-个人笔记三篇](https://blog.csdn.net/roshy?t=1)

## 遇到的问题

```text
warning in /usr/local/zeek/share/zeek/base/bif/plugins/./Zeek_TCP.events.bif.zeek, line 255: Wrong number of arguments for function. Expected 2, got 7. (event(c:connection; is_orig:bool; flags:string; seq:count; ack:count; len:count; payload:string;))
error in /usr/local/zeek/share/zeek/base/bif/plugins/./Zeek_TCP.events.bif.zeek, line 255 and ./notice.zeek, line 13: incompatible types (event(c:connection; is_orig:bool; flags:string; seq:count; ack:count; len:count; payload:string;) and event(c:connection; payload:string;)
```

- `expression error in ./detect_attack.zeek, line 12: field value missing (c$http)`
- 待添加图片（notes_images）和代码
