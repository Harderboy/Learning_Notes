# Selenium学习笔记

- 先看完下方链接个人博客，结合项目需求操作一遍，再做总结

- 遇到的问题
  - 下拉框（类下拉框）怎么选择
- 标签分类待熟悉，e.g. `input`、`h1`
- 命名空间：`xmlns="http://www.w3.org/1999/xhtml"`

- 需要注意的点
  - 使用此策略，将返回具有匹配`id`（`name`等）属性的第一个元素。如果没有元素具有匹配的`id`（`name`等）属性，则将引发`NoSuchElementException`

    ```python
    find_element_by_id
    find_element_by_name
    find_element_by_xpath
    find_element_by_link_text
    find_element_by_partial_link_text
    find_element_by_tag_name
    find_element_by_class_name
    find_element_by_css_selector
    ```

- 推荐先看个人博客，通俗易懂（基本使用皆涵盖且节约时间）

- 上传文件方式：send_keys('文件路径（绝对路径或者相对路径）') # 相对路径未测试
定位上传按钮，添加本地文件
`driver.find_element_by_name("file").send_keys('D:\\upload_file.txt')`

- 截图
  - 最好是（`png`格式），jpg也可
  - 指定路径方式三种方式（不支持相对路径）（r \\ /）（特别注意windows路径指定方式）
截取当前窗口，并指定截图图片的保存位置
`driver.get_screenshot_as_file("D:\\baidu_img.jpg")`

- 在Web应用中经常会遇到frame/iframe表单嵌套页面的应用，WebDriver只能在一个页面上对元素识别与定位，对于frame/iframe表单内嵌页面上的元素无法直接定位。这时就需要通过 `switch_to.frame()` 方法将当前定位的主体切换为`frame/iframe`表单的内嵌页面中。

- 涉及到多层`frame`，先切换到`iframe`表单,使用`switch_to.frame()`
  - `switch_to.frame()` 默认可以直接取表单的 `id` 或 `name` 属性。  
  `id = "x-URS-iframe"`  
  `driver.switch_to.frame('x-URS-iframe')`
  - 如果`iframe`没有可用的`id`和`name`属性，则可以通过下面的方式进行定位。
    - 通过 `xpth` 定位到 `iframe`
    `driver.find_element_by_xpath()`

- 关闭浏览器
  - 关闭当前标签：`driver.close()`  # 退出当前页面。所有页面都关闭后,整个浏览器就会退出。当前只有一个标签（页面），相当于退出浏览器
  - 退出浏览器：`driver.quit()`  # 退出浏览器
  - 区别：使用`close()`方法的时候，是通过查看任务管理器发现，ChromeDriver进程仍存在内存中。如果使用`quit()`方法，整个浏览器都直接关闭，ChromeDriver进程也会被结束。`quit()`会退出驱动并且关闭所关联的所有窗口
  - 参考链接：[selenium框架中driver.close()和driver.quit()关闭浏览器-2018.11](https://blog.csdn.net/yangfengjueqi/article/details/84338167)

- 截屏的两种方法
  - `driver.save_screenshot('screenshot.png')`
  - `driver.get_screenshot_as_file(r"C:\Users\liu heng\Desktop\Selenium\shot_img_plugin.png")`

- 创建一个参数对象，用来控制chrome以无界面模式打开

    ```python
    from selenium.webdriver.chrome.options import Options

    chrome_options = Options()
    chrome_options.add_argument('--headless')
    chrome_options.add_argument('--disable-gpu')

    # 创建浏览器对象
    driver = webdriver.Chrome(chrome_options=chrome_options)
    ```

- 填写表格，`Select`类的使用，详见官方文档、WebDriver API

    ```python
    from selenium.webdriver.support.ui import Select
    select = Select(driver.find_element_by_name('name'))
    select.select_by_index(index)
    select.select_by_visible_text("text")
    select.select_by_value(value)
    # 要获取所有可用选项
    options = select.options
    ```

- 弹出对话框
  - 返回当前打开的警报对象
`alert = driver.switch_to.alert`
  - 接受或者关闭

    ```python
    Alert(driver).accept() # driver.switch_to.alert.accept()
    Alert(driver).dismiss() # driver.switch_to.alert.dismiss()
    ```

- 有关更多信息，请参考 WebDriver API 文档

- 经常用到的函数：获取网页源码、获取cookie等方法总结
  - 获取网页源代码：`driver.page_source`

    ```python
    driver = webdriver.Chrome()
    driver.get("https://www.baidu.com")
    html_source = driver.page_source
    if "whatever" in html_source:
        # do something
    else:
        # do something else
    ```

  - 获取`cookie`：`driver.get_cookies()`

    ```python
    driver = webdriver.Chrome()
    driver.get("https://www.baidu.com")
    driver_cookies = driver.get_cookies()
    cookies_dict = {}
    for cookie in driver_cookies:
        cookies_dict[cookie['name']] = cookie['value']
    print(cookies_dict)
    ```

- 无头模式

  ```python
  from selenium.webdriver.chrome.options import Options

  chrome_options = Options()
  chrome_options.add_argument('--headless')
  driver = webdriver.Chrome(chrome_options=chrome_options)
  ```

- 支持跑两个 `driver`

  ```python
  chrome_options = Options()
  chrome_options.add_argument('--headless')
  driver = webdriver.Chrome(chrome_options=chrome_options)
  driver.get("http://" + host)

  # xxxx
  driver.quit()

  driver = webdriver.Chrome()
  driver.get("http://" + host)

  # xxxx
  driver.quit()
  ```

- 熟悉与之相关的第三方库的功能

  ```python
  from selenium import webdriver
  from selenium.webdriver.common.keys import Keys
  from selenium.webdriver.chrome.options import Options
  ```

**参考链接**  
[Selenium with Python-官方文档](https://selenium-python.readthedocs.io/)  
[Selenium-Python-中文文档](https://selenium-python-zh.readthedocs.io/en/latest/)  
[Selenium浏览器自动化项目-官方](https://www.selenium.dev/documentation/en/getting_started/)  
[WebDriver-MDN](https://developer.mozilla.org/en-US/docs/Web/WebDriver)  
[Selenium Python-个人博客-2018](http://www.testclass.net/selenium_python)  
[Selenium处理alert/confirm/prompt提示框，无头浏览器，规避网站监测-2019.11](https://www.cnblogs.com/sundawei7/p/11959459.html)  
[07-selenium、PhantomJS（无头浏览器）-2019.10](https://my.oschina.net/u/4258221/blog/3381382)  
[Selenium+PhantomJS使用详解-个人博客（含具体例子）-2018.01](https://www.jianshu.com/p/4b89c92ff9b4)  
[Python3+Selenium获取session和token供Requests使用教程-2019.03](https://www.cnblogs.com/lsdb/p/10515759.html)  
[Selenium笔记——可当做简略教程使用-2019.09](https://blog.csdn.net/qq_21774161/article/details/101550958)