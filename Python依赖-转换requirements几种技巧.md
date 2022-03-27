# 转换成requirements的技巧以及生成Pipfile和Pipfile.lock

- 进入虚拟环境：`pipenv`
- 安装需要的包(库)：`pipenv install some-packages`
(此项目目录下如果没有`Pipfile`、`Pipfile.lock`的话，会自动生成)
- 将`Pipfile/Pipfile.lock`转换为`requirements.txt`
  - 工具：pipfile2req
    - 安装命令：`pip install pipfile-requirements`
  - 转换：`pipfile2req Pipfile/Pipfile.lock > requirements.txt`（Pipfile.lock 优先）

- 其他转换方式见参考文献

**参考文献**  
[Pipfile 文件转换利器——pipfile-freeze-简书-2020.02](https://www.jianshu.com/p/b7644a1fb503)  
[pipenv使用指南](https://crazygit.wiseturtles.com/2018/01/08/pipenv-tour/)  
[转换工具——pipfile2req](https://pypi.org/project/pipfile-requirements/)
