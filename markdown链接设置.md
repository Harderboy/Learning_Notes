# Markdown 中添加链接的五种方式总结

**提升效率的几种方式总结：**

- 实例 1
  - 变量方式引用

  ```markdown
  示例语法：
  [知乎-Google搜索技巧][1]

  [再次引用同一网址][1]
  以上实现了两处甚至多处访问同一链接，提高效率

  [1]:https://zhuanlan.zhihu.com/p/41246198
  注意：链接（引用）序号文档内全局有效，不可重复，且与引用链接之间保留一空行
  ```

  - [知乎-Google搜索技巧][1]
  - [再次引用同一网址][1]

  以上实现了两处甚至多处访问同一链接，提高效率

  [1]:https://zhuanlan.zhihu.com/p/41246198

- 实例 2
  - 图片的一般格式:  
    `![图片名称](图片地址)`
  - 采用变量形式：  

    ```markdown
    // 中间空行不可缺少
    [微信头像][2]

    [2]:https://b-ssl.duitang.com/uploads/item/201704/10/20170410095843_SEvMy.thumb.700_0.jpeg
    ```

    [微信头像][2]

    [2]:https://b-ssl.duitang.com/uploads/item/201704/10/20170410095843_SEvMy.thumb.700_0.jpeg

- 实例 3
  - 嵌套使用（图片链接网址）

    ```markdown
    示例语法：
    [![Docker Hub](https://img.shields.io/badge/Docker-Hub-blue.svg)](https://hub.docker.com/r/bludit/docker/)
    ```

    [![Docker Hub](https://img.shields.io/badge/Docker-Hub-blue.svg)](https://hub.docker.com/r/bludit/docker/)

    [![test](https://b-ssl.duitang.com/uploads/item/201704/10/20170410095843_SEvMy.thumb.700_0.jpeg)](https://b-ssl.duitang.com/uploads/item/201704/10/20170410095843_SEvMy.thumb.700_0.jpeg)

- 实例 4
  - 类似标识符的方式（给引用取个名称，一般为英文和数字），暂称为引用标识符

    ```markdown

    [知乎][zhihu]

    <!-- 图片必须加上'!'，否则会被认为是单独的链接 -->
    [![2]](https://www.zhihu.com/)

    <!-- 这样写很简洁 -->
    [![2]][zhihu]

    <!-- 这里的'zhihu'和'2'暂且称为引用标识符（要做到见名知意），一般将其放在文章尾末 -->

    [zhihu]:https://www.zhihu.com/
    [2]:https://b-ssl.duitang.com/uploads/item/201704/10/20170410095843_SEvMy.thumb.700_0.jpeg

    ```

    [知乎][zhihu]

    [![2]](https://www.zhihu.com/)

    [![2]][zhihu]

    [zhihu]:https://www.zhihu.com/

- 实例5

  - 锚点的使用
    - 任意 1-6 个 # 标注的标题都会被添加上同名的锚点链接

        ```markdown
        [标题1](#标题1) 
        [标题2](#标题2) 
        [标题3](#标题3) 

        # 标题1
        ## 标题2
        ### 标题3
        ```

    - 锚点跳转的标识名称，可使用任意字符，大写字母要转换成小写

        ```markdown
        [Github标题1](#github标题1)

        ### Github标题1
        ```

        [Github标题2](#github标题2)

    - 多单词锚点的空格用 `-` 代替

        ```markdown
        [Github 标题2 Test](#github-标题2-test)

        ### Github 标题2 Test
        ```

        [Github 标题3 Test](#github-标题3-test)  
        [第二章节、标题4](#第二章节标题4)

    - 多级序号需要去除 `.`

        ```markdown
        [2.3. Github 标题](#23-github-标题)

        ### 2.3. Github 标题
        ```

        [2.3. Github 标题4](#23-github-标题4)

    注意：非英文的锚点字符，在单击跳转时，在浏览器的 url 中会按照规则进行 encode 和 decode

  **参考链接**  
  [Github 中 Markdown 锚点链接如何写](https://my.oschina.net/antsky/blog/1475173)

## Github标题2

### Github 标题3 Test

#### 2.3. Github 标题4

#### 第二章节、标题4
