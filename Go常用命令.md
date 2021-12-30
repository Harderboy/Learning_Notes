# Go 命令总结

## TODO

- [ ] 在实际项目中使用以下命令
- [ ] 了解各命令对应的一些常用的参数的含义，个别参数还是很好用的

## GO 包相关知识点

### GOPATH 和 GOROOT

不同于其他语言，go中没有项目的说法，只有包, 其中有两个重要的路径，`GOROOT` 和 `GOPATH`

Go 开发相关的环境变量如下：

- `GOROOT`：`GOROOT`就是 Go 的安装目录，（类似于java的JDK）
- `GOPATH`：`GOPATH`是我们的工作空间，保存 go 项目代码和第三方依赖包

`GOPATH` 可以设置多个，其中，第一个将会是默认的包目录，使用 `go get` 下载的包都会在第一个 path 中的 src 目录下，使用 `go install` 时，在哪个`GOPATH` 中找到了这个包，就会在哪个`GOPATH`下的bin目录生成可执行文件

### GO111MODULE

最初，Go的包管理工具依赖于GOPATH或者vender目录，例如：

- govender
- dep
- glide
- godep

`GO111MODULE`：Go 1.11时，官方推出了版本管理工具 `go module`，并且从1.13开始，将其作为Go语言默认的依赖管理工具。

国内使用Go语言，必须配置Proxy，否则很多库依赖是访问不到的：

- Windows 下设置 GOPROXY 的命令为：
`set GOPROXY=https://goproxy.cn`
`go env -w GOPROXY=https://goproxy.cn,direct`

- MacOS 或 Linux 下设置 GOPROXY 的命令为：
`export GOPROXY=https://goproxy.cn`

MacOS或Linux下开启GO111MODULE的命令为：

`export GO111MODULE=on` 或者 `export GO111MODULE=auto`

其次是 `GO111MODULE`，通过它可以选择开启或者关闭模块支持，它主要有三个可选值：`off`、`on`、`auto`，默认值是`auto`:

- `GO111MODULE=off` 禁用模块支持，编译项目时，会从`GOPATH`和`vender`文件夹中查找包
- `GO111MODULE=on` 启用模块支持，编译时会忽略`GOPATH`和`vender`目录，只根据`go.mod`文件中的依赖信息下载、寻找依赖，依赖文件默认会存储在`GOPATH/pkg/mod`目录下（在使用模块的时候，GOPATH 是无意义的，不过它还是会把下载的依赖储存在 `$GOPATH/pkg/mod` 中，也会把 `go install` 的结果放在 `$GOPATH/bin` 中。）。
- `GOPATH111MODULE=auto` 将会根据项目目录下是否有`go.mod`文件、`vender`目录自动选择当前模式 （如果当前目录不在 `$GOPATH` 并且 当前目录（或者父目录）下有`go.mod`文件，则使用 `GO111MODULE`， 否则仍旧使用 `GOPATH` mode。）

## 其他命令

### go build

在非 `GOPATH` 目录下执行 `go build`，报错：

```bash
gobuild> go build
go: go.mod file not found in current directory or any parent directory; see 'go help modules'
```

原因是开启了 `GO111MODULE=on`

```bash
PS E:\Go-Learning\demo\module-usage\gobuild> go mod init gobuild
go: creating new go.mod: module gobuild
go: to add module requirements and sums:
        go mod tidy
PS E:\Go-Learning\demo\module-usage\gobuild> go build
```

==如果源码中没有依赖 GOPATH 的包引用，那么这些源码可以使用无参数 go build==
`go build` 生成的可执行程序默认名称和父文件夹一致

若要指定文件名，可使用参数 `-o`，也可指定文件输出的目录。
`go build -o main.exe`

```bash
目录: E:\Go-Learning\demo\module-usage\gobuild

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----        2021-12-15     17:59             24 go.mod
-a----        2021-12-15     17:59        2094592 gobuild.exe
-a----        2021-12-15     17:57             83 lib.go
-a----        2021-12-15     18:04            162 main.go
```

`no required module provides package github.com/astaxie/beego; to add it:`

```bash
PS E:\Go-Learning\demo\module-usage\gobuild> go build
main.go:5:2: no required module provides package github.com/astaxie/beego; to add it:
        go get github.com/astaxie/beego
PS E:\Go-Learning\demo\module-usage\gobuild> go install
main.go:5:2: no required module provides package github.com/astaxie/beego; to add it:
        go get github.com/astaxie/beego
```

### go install

`go install` 跟 `go build` 类似，只是多做了一件事就是安装编译后的文件到指定目录。

与 go build 命令类似，附加参数绝大多数都可以与 go build 通用。go install 只是将编译的中间文件放在 GOPATH 的 pkg 目录下，以及固定地将编译结果放在 GOPATH 的 bin 目录下。

这个命令在内部实际上分成了两步操作：第一步是生成结果文件（可执行文件或者 .a 包），第二步会把编译好的结果移到 `$GOPATH/pkg` 或者 `$GOPATH/bin`。

go install 的编译过程有如下规律：

- go install 是建立在 GOPATH 上的，无法在独立的目录里使用 go install。
- GOPATH 下的 bin 目录放置的是使用 go install 生成的可执行文件，可执行文件的名称来自于编译时的包名。
- go install 输出目录始终为 GOPATH 下的 bin 目录，无法使用-o附加参数进行自定义。
- GOPATH 下的 pkg 目录放置的是编译期间的中间文件。

新版变化，可参考：
[Go 1.16 中关于 go get 和 go install 你需要注意的地方](https://segmentfault.com/a/1190000038541867)

### go mod

`go mod download` 可以下载所需要的依赖，但是依赖并不是下载到`$GOPATH`中，而是`$GOPATH/pkg/mod`中，多个项目可以共享缓存的module。

参考：[跳出Go module的泥潭](https://colobu.com/2018/08/27/learn-go-module/)

## 参考资料

- [GO语言学习之GO MODULE](https://jeffdingzone.com/2020/10/go%E8%AF%AD%E8%A8%80%E5%AD%A6%E4%B9%A0%E4%B9%8Bgo-module/)
- [深入Go Module之go.mod文件解析](https://colobu.com/2021/06/28/dive-into-go-module-1/)
- [Go mod 使用](https://segmentfault.com/a/1190000018536993)
- [Go命令详解](https://zhuanlan.zhihu.com/p/161494871)
- [go build命令（go语言编译命令）完全攻略](http://c.biancheng.net/view/120.html)
- [go get命令——一键获取代码、编译并安装](http://c.biancheng.net/view/123.html)
- [go - 使用 go mod 管理项目依赖](https://segmentfault.com/a/1190000019724582)
- [Go 1.16 中关于 go get 和 go install 你需要注意的地方](https://segmentfault.com/a/1190000038541867)
