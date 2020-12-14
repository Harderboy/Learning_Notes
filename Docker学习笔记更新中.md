# 使用docker过程常遇问题和总结

- 在docker-compose中控制容器启动和关闭顺序的几种方式
  - `depends_on`：简单指定启动顺序，仅仅是限制容器先后启动顺序，而非等`depends_on`的容器启动就位再启动，详看[官方说明](https://docs.docker.com/compose/compose-file/)
  - `wait-for-it`脚本，代码级别可靠，可根据容器中服务是否启动为依据，需要定制代码
  - 以及官方所提到的`links`, `volumes_from`, 和 `network_mode: "service:..."`等方式，如何结合？
  - --未完--

- `links`：使用场景
  - 该`--link`标志是Docker的遗留功能。它最终可能会被删除。除非您绝对需要继续使用它，否则建议您使用 [user-defined networks](https://docs.docker.com/compose/networking/)  来促进两个容器之间的通信，而不要使用`--link`。
  - 区别：用户定义的网络不支持的功能之一：在容器之间共享环境变量而`--link`可以。但是，可以使用其他机制（例如卷）以更可控的方式在容器之间共享环境变量。
  - 链接到另一个服务中的容器。指定服务名称和链接别名（`"SERVICE:ALIAS"`），或者仅指定服务名称。
  - 链接还以与depends_on相同的方式表示服务之间的依赖关系，因此也能确定服务启动的顺序
  - --未完--
  - [官方文档](https://docs.docker.com/compose/compose-file/#links)

- `links`与`depend_on`区别
  - [docker-compose 中 links 和 depends_on 区别](http://einverne.github.io/post/2018/03/docker-compose-links-vs-depends-on-differences.html)
  - [docker-compose.yml 中 links 和 depends_on 区别](https://qastack.cn/programming/35832095/difference-between-links-and-depends-on-in-docker-compose-yml)

- `network_mode`：设置网络模式
  - 与 `docker run`中的 `--network` 选项参数一样
  - 默认是 `bridge` 桥接模式：`network_mode: "bridge"`  
    - 如果没有指定网络驱动，默认会创建一个 `bridge` 类型的网络
  - 新增的两种模式：可以指定使用服务或者容器的网络
    - `network_mode: "service:[service name]"`
    - `network_mode: "container:[container name/id]"`

- `healthcheck`：用于检查测试服务使用的容器是否正常

    ```json
    healthcheck:
    test: ["CMD", "curl", "-f", "http://localhost"]
    interval: 1m30s
    timeout: 10s
    retries: 3
    start_period: 40s
    ```

  - 具体参数含义待总结
    - 参考链接
      - [官方文档](https://docs.docker.com/compose/compose-file/)
      - [CSDN](https://blog.csdn.net/xcbeyond/article/details/84587134)

- 推送镜像：`docker push UserID/Repository_name:tagname`
  - `UserID`：docker hub 用户名（htomato）
  - `Repository_name`：可以是不存在的仓库，不存在将自动创建  
    - `e.g.`：`docker push htomato/grandnode-bug:4.40`
- 给镜像打标签，归入某一仓库（镜像位于新的仓库，镜像ID不变）：`docker tag image_name/image_id UserID/Repository_name:tagname`
  - `image_name`：ubuntu:15.10
  - `image_id`：ed14925495ea
  - `e.g.`：`docker tag bde6d05adacb htomato/lab:grandnode-bug-4.40`
- 删除多个 `Image ID` 相同的`tag`
  - `docker rmi repository:tag`
  - `e.g.`：`docker rmi htomato/grandnode-bug:4.40`

- `e.g.`用法

- docker-compose.yml中的`服务名`等价于 `ip + 端口号`
  - `http://web` = `http://192.168.56.114:8009`

- 本地虚拟机中docker volume占空间过大解决方法

  `docker volume rm $(docker volume ls -q)`

  - [Docker - 解决/var/lib/docker/overlay2占用很大、容器无法启动问题（清理磁盘）](https://www.hangge.com/blog/cache/detail_2555.html)