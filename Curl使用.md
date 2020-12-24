# Curl使用

## 常用的功能

- 获取网页返回状态码
  `curl -o /dev/null -sL -w %{http_code} http://192.168.56.114:8080`
