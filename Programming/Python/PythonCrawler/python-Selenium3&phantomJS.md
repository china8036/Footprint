- [python Selenium3 & phantomJS NOTE](#python-selenium3-phantomjs-note)
  - [Selenium3](#selenium3)
    - [介绍](#%E4%BB%8B%E7%BB%8D)
    - [安装](#%E5%AE%89%E8%A3%85)
    - [工作原理](#%E5%B7%A5%E4%BD%9C%E5%8E%9F%E7%90%86)
    - [使用](#%E4%BD%BF%E7%94%A8)
      - [获取 webdriver 对象](#%E8%8E%B7%E5%8F%96-webdriver-%E5%AF%B9%E8%B1%A1)
      - [设置 request header](#%E8%AE%BE%E7%BD%AE-request-header)
        - [设置 phantomjs 请求头](#%E8%AE%BE%E7%BD%AE-phantomjs-%E8%AF%B7%E6%B1%82%E5%A4%B4)
        - [设置 chrome 请求头](#%E8%AE%BE%E7%BD%AE-chrome-%E8%AF%B7%E6%B1%82%E5%A4%B4)
        - [设置 chrome–cookie](#%E8%AE%BE%E7%BD%AE-chrome%E2%80%93cookie)
        - [设置 phantomjs- 图片不加载](#%E8%AE%BE%E7%BD%AE-phantomjs--%E5%9B%BE%E7%89%87%E4%B8%8D%E5%8A%A0%E8%BD%BD)
      - [使用代理](#%E4%BD%BF%E7%94%A8%E4%BB%A3%E7%90%86)
      - [访问链接](#%E8%AE%BF%E9%97%AE%E9%93%BE%E6%8E%A5)
      - [当前 URL](#%E5%BD%93%E5%89%8D-url)
      - [页面源码](#%E9%A1%B5%E9%9D%A2%E6%BA%90%E7%A0%81)
      - [页面操作](#%E9%A1%B5%E9%9D%A2%E6%93%8D%E4%BD%9C)
        - [获取元素对象（定位)](#%E8%8E%B7%E5%8F%96%E5%85%83%E7%B4%A0%E5%AF%B9%E8%B1%A1%EF%BC%88%E5%AE%9A%E4%BD%8D)
          - [常见 NoSuchElement 异常解决方法](#%E5%B8%B8%E8%A7%81-nosuchelement-%E5%BC%82%E5%B8%B8%E8%A7%A3%E5%86%B3%E6%96%B9%E6%B3%95)
          - [元素对象常用属性](#%E5%85%83%E7%B4%A0%E5%AF%B9%E8%B1%A1%E5%B8%B8%E7%94%A8%E5%B1%9E%E6%80%A7)
        - [模拟输入](#%E6%A8%A1%E6%8B%9F%E8%BE%93%E5%85%A5)
        - [滚动到页面底部](#%E6%BB%9A%E5%8A%A8%E5%88%B0%E9%A1%B5%E9%9D%A2%E5%BA%95%E9%83%A8)
        - [拖拽](#%E6%8B%96%E6%8B%BD)
        - [页面切换](#%E9%A1%B5%E9%9D%A2%E5%88%87%E6%8D%A2)
          - [frame 切换](#frame-%E5%88%87%E6%8D%A2)
          - [window 切换](#window-%E5%88%87%E6%8D%A2)
        - [弹窗处理](#%E5%BC%B9%E7%AA%97%E5%A4%84%E7%90%86)
        - [历史记录](#%E5%8E%86%E5%8F%B2%E8%AE%B0%E5%BD%95)
      - [Cookie 操作](#cookie-%E6%93%8D%E4%BD%9C)
        - [基本操作](#%E5%9F%BA%E6%9C%AC%E6%93%8D%E4%BD%9C)
        - [结合 js 操作](#%E7%BB%93%E5%90%88-js-%E6%93%8D%E4%BD%9C)
          - [操作 cookie 实现登陆操作](#%E6%93%8D%E4%BD%9C-cookie-%E5%AE%9E%E7%8E%B0%E7%99%BB%E9%99%86%E6%93%8D%E4%BD%9C)
      - [等待](#%E7%AD%89%E5%BE%85)
        - [强制等待](#%E5%BC%BA%E5%88%B6%E7%AD%89%E5%BE%85)
        - [隐性等待](#%E9%9A%90%E6%80%A7%E7%AD%89%E5%BE%85)
        - [显式等待](#%E6%98%BE%E5%BC%8F%E7%AD%89%E5%BE%85)
          - [使用 expected_conditions 模块中的预定义条件](#%E4%BD%BF%E7%94%A8-expectedconditions-%E6%A8%A1%E5%9D%97%E4%B8%AD%E7%9A%84%E9%A2%84%E5%AE%9A%E4%B9%89%E6%9D%A1%E4%BB%B6)
          - [使用 lambda 表达式](#%E4%BD%BF%E7%94%A8-lambda-%E8%A1%A8%E8%BE%BE%E5%BC%8F)
      - [调用 JavaScript](#%E8%B0%83%E7%94%A8-javascript)
      - [关闭浏览器](#%E5%85%B3%E9%97%AD%E6%B5%8F%E8%A7%88%E5%99%A8)
      - [程序结构](#%E7%A8%8B%E5%BA%8F%E7%BB%93%E6%9E%84)
  - [PhantomJS](#phantomjs)
    - [介绍](#%E4%BB%8B%E7%BB%8D)
    - [安装](#%E5%AE%89%E8%A3%85)
    - [使用](#%E4%BD%BF%E7%94%A8)
      - [REPL 环境](#repl-%E7%8E%AF%E5%A2%83)
      - [webpage 模块](#webpage-%E6%A8%A1%E5%9D%97)
      - [system 模块](#system-%E6%A8%A1%E5%9D%97)
      - [应用](#%E5%BA%94%E7%94%A8)
        - [过滤资源](#%E8%BF%87%E6%BB%A4%E8%B5%84%E6%BA%90)
        - [截图](#%E6%88%AA%E5%9B%BE)
        - [抓取图片](#%E6%8A%93%E5%8F%96%E5%9B%BE%E7%89%87)
        - [生成网页](#%E7%94%9F%E6%88%90%E7%BD%91%E9%A1%B5)
  - [Refer Links](#refer-links)

# python Selenium3 & phantomJS NOTE

> 为什么不直接用浏览器而用一个没界面的 PhantomJS 呢？答案是：效率更高。

搭配方法：PhantomJS 用来渲染解析 JS，Selenium 用来驱动以及与 Python 的对接，Python 进行具体逻辑的处理。

## Selenium3

### 介绍

浏览器的自动化测试包括很多方面，如性能测试、UI 测试、页面测试等。本文测试背景是页面测试部分，如：登录某个页面，检查是否登录成功等。对浏览器页面进行自动化测试，一般选取的工具都是 selenium。selenium 是一个免费的 web 自动化测试工具，支持多平台（windows、linux 等）、多浏览器（ie、ff、safari、opera、chrome、PhantomJS）、多语言（C、 java、ruby、python 等）。

Selenium 是 web 自动化测试工具集，包括 IDE、Grid、RC（selenium 1.0）、WebDriver（selenium 2.0）等。其中 WebDriver（也称为 selenium 2.0），它的主要新特性是去掉了 Selenium RC。	

Selenium 测试直接运行在浏览器中，就像真实用户操作一样。Selenium 框架底层使用 JavaScript 模拟用户对浏览器进行操作，测试脚本执行时，浏览器自动按脚本代码做出点击，输入，打开，验证等操作。

- 支持多平台：windows、linux、MAC
- 支持多浏览器：ie、ff、safari、opera、chrome、PhantomJS
- 支持多语言：C、C#、java、ruby、python 等

### 安装

Selenium3 将 Webdriver 从各个浏览器中分离出来了，所以对浏览器进行自动化测试时，需要单独安装对应浏览器的 Webdriver，安装过程：将浏览器程序和 webdriver 程序的路径加入系统环境变量即可。

例如：
- 使用 selenium 操作 webkit 核浏览器（如 chrome) 时，需要安装 chromeDriver.exe；
- 使用 selenium 操作 IE 浏览器时，需要安装 IEDriver.exe；
- 使用 selenium 操作 Edge 浏览器时，需要安装 MicrosoftWebDriver.exe；
- 使用 selenium 操作 firefox 浏览器时，需要安装 geckodriver.exe；

### 工作原理

以 selenium 和 chormedriver 为例：

chromedriver 是 google 为网站开发人员提供的自动化测试接口，是网站测试架构 selenium 的 chrome 基础部分，主要是通过 http 通信实现的。它是 selenium 和 浏览器进行通信的桥梁。他们的工作原理是：

  1. selenium 通过一套协议和 chromedriver 进行通信，selenium 实质上是对这套协议的底层封装，同时提供外部 WebDriver 的上层调用类库。Selenium 提供的操作包括：Session 相关、Element（页面）相关、Window 相关、Alert 相关

  2. selenium 通过指定的 port（如 9515）调用起 chromedriver 的实例，具体实现见 chromium 源码

  3. chromedriver 在本地 port（如 9515） 端口打开一个 http 服务，selenium 通过网络编程与这个 http 服务进行通信，通信协议即 1 中提到的 protocol

  4. chromedriver 和外部（如 selenium）的通信，通过 session 进行标识，selenium 中每创建一个 WebDriver 实例，则 chromedriver 新建一个进程，通过包含唯一的远程调试端口的命令（如：--remote-debugging-port=12996）启动一个 chromebrowser 实例，并将其保存在一个 session 中，这个 session 保持 selenium 和 对应的 chromebrowser 的通信；

  5. chromedriver 中的 session 和 chromebrowser 通过 socket 进行 TCP 通信，即 4 中通过命令启动 chromebrowser 的同时，session 中包含一个 socket 的 client，和 chromebrowser 中的 socket 服务进行通信，进而控制 chromebrowser；

  6. selenium 中的 WebDriver 实例和 chromedriver 的关系为多对一，chromedriver 和 chromebrowser 的关系为一对多；

### 使用

英文教程：https://selenium-python.readthedocs.io/index.html 
中文教程：https://selenium-python-zh.readthedocs.io/en/latest/index.html 
官方 API 文档：https://seleniumhq.github.io/selenium/docs/api/py/api.html 
python selenium 教程：https://huilansame.github.io/huilansame.github.io/category/ 

#### 获取 webdriver 对象

获取特定浏览器的 WebDriver 对象是进行自动化测试几乎所有操作的第一步，各种操作都封装在 webdriver 类中。
```python
from selenium import webdriver

# 指定 chromedriver.exe 路径，若已添加到环境变量中则参数为空即可
driver=webdriver.Chrome (executable_path = 'D:\\software\\web\\webDriver\\chromedriver_win32\\chromedriver.exe') 

# 指定 phantomjs.exe 路径，若已添加到环境变量中则参数为空即可
driver=webdriver.PhantomJS(executable_path = ‘D:\\software\\web\\phantomJS\\phantomjs-2.1.1-windows\\bin\\phantomjs.exe’)

# 其他浏览器 webdriver 对象的获取类似
```

#### 设置 request header

https://www.urlteam.org/2017/02/selenium%E8%AE%BE%E7%BD%AEchrome%E5%92%8Cphantomjs%E7%9A%84%E8%AF%B7%E6%B1%82%E5%A4%B4%E4%BF%A1%E6%81%AF/ 

##### 设置 phantomjs 请求头
```python
# 访问 https://httpbin.org/get?show_env=1  该网站能呈现你请求的头部信息
# !/usr/bin/python
# -*- coding: utf-8 -*-
 
from selenium import webdriver
from selenium.webdriver.common.desired_capabilities import DesiredCapabilities
 
dcap = dict(DesiredCapabilities.PHANTOMJS)
dcap["phantomjs.page.settings.userAgent"] = (
"Mozilla/5.0 (Linux; Android 5.1.1; Nexus 6 Build/LYZ28E) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/48.0.2564.23 Mobile Safari/537.36"
)
driver = webdriver.PhantomJS(desired_capabilities=dcap)
driver.get("https://httpbin.org/get?show_env=1")
driver.get_screenshot_as_file('01.png')
driver.quit()
```

##### 设置 chrome 请求头
```python
# !/usr/bin/python
# -*- coding: utf-8 -*-
 
from selenium import webdriver
# 进入浏览器设置
options = webdriver.ChromeOptions()
# 设置中文
options.add_argument('lang=zh_CN.UTF-8')
# 更换头部
options.add_argument('user-agent="Mozilla/5.0 (iPod; U; CPU iPhone OS 2_1 like Mac OS X; ja-jp) AppleWebKit/525.18.1 (KHTML, like Gecko) Version/3.1.1 Mobile/5F137 Safari/525.20"')
browser = webdriver.Chrome(chrome_options=options)
url = "https://httpbin.org/get?show_env=1"
browser.get(url)
browser.quit()
```

##### 设置 chrome–cookie
```python
# 设置 cookie 用于模拟登陆
# !/usr/bin/python
# -*- coding: utf-8 -*-
 
from selenium import webdriver
browser = webdriver.Chrome()
 
url = "https://www.baidu.com/"
browser.get(url)
# 通过 js 新打开一个窗口
newwindow='window.open("https://www.baidu.com");'
# 删除原来的 cookie
browser.delete_all_cookies()
# 携带 cookie 打开
browser.add_cookie({'name':'ABC','value':'DEF'})
# 通过 js 新打开一个窗口
browser.execute_script(newwindow)
input("查看效果")
browser.quit()
```
##### 设置 phantomjs- 图片不加载
```python
from selenium import webdriver
 
options = webdriver.ChromeOptions()
prefs = {
    'profile.default_content_setting_values': {
        'images': 2
    }
}
options.add_experimental_option('prefs', prefs)
browser = webdriver.Chrome(chrome_options=options)
 
# browser = webdriver.Chrome()
url = "http://image.baidu.com/"
browser.get(url)
input("是否有图")
browser.quit()
```

#### 使用代理
```python
# 搭配 Tor 代理服务器使用，使 tor 运行在本地 9150 端口（默认端口）
service_args = [‘—proxy=localhost:9150’, ‘—proxy-type=socks5’,]
driver = wevdriver.PhantomJS(executable_path=’xxxx’, service_args=service_args)

driver.get(‘http://icanhazip.com’)
print(driver.page_source)
driver.close()
```

#### 访问链接
```python
url = ‘https://www.google.com’
driver.get(url) # 使用 get 方法访问链接，该方法会将 url 资源完全加载完后才执行下一行代码（但若页面包含大量 ajax，则无法确定 ajax 完全加载完的时间，对于包含大量 ajax 的页面，应使用 wait）
```

#### 当前 URL

使用 driver.current_url 即可获取当前 URL；

#### 页面源码

使用 driver.page_source 即可获取页面源码；

#### 页面操作

##### 获取元素对象（定位)

API：https://selenium-python.readthedocs.io/locating-elements.html 

获取第一个匹配的元素（返回一个 element 对象）
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

获取所有匹配的元素（返回一个 element 对象的 list）
```python
find_elements_by_name
find_elements_by_xpath
find_elements_by_link_text
find_elements_by_partial_link_text
find_elements_by_tag_name
find_elements_by_class_name
find_elements_by_css_selector
```

- by_css_selector 是最快的查找方式；
  ```python
  find_element_by_css_selector('#txt_1_value1')：查找 id，使用“#”；
  find_element_by_css_selector('.txt_1_value1')：查找 class，使用“.”；
  find_element_by_css_selector('div.txt_1_value1')：查找标签名为 div，直接将标签名作为参数；
  find_element_by_css_selector('div.txt_1_value1#id')：查找 div 标签，并指定 class 和 id； 
  ```

- 还可以利用 By 类来确定哪种选择方式：
  ```python
  driver.find_element(By.XPATH, '//button[text()="Some text"]')
  driver.find_elements(By.XPATH, '//button')
  ```
  By 类的常用属性：
    - ID = "id"
    - XPATH = "xpath"
    - LINK_TEXT = "link text"
    - PARTIAL_LINK_TEXT = "partial link text"
    - NAME = "name"
    - TAG_NAME = "tag name"
    - CLASS_NAME = "class name"
    - CSS_SELECTOR = "css selector"

  例：
  ```html
  <input type="text" name="passwd" id="passwd-id" />
  ```

  ```python
  element = driver.find_element_by_id("passwd-id")
  element = driver.find_element_by_name("passwd")
  element = driver.find_element_by_xpath("//input[@id='passwd-id']")
  # You can also look for a link by its text, but be careful! The text must be an exact match
  #  If there’s more than one element that matches the query, then only the first will be returned. If nothing can be found, a NoSuchElementException will be raised.
  ```

- 层级定位查找标签：
  ```python
  driver.find_element_by_xpath(‘xxxxx’).find_element_by_name(‘ssss’)
  # 先用 xpath 定位找到一个元素节点，再以该节点为根节点用 name 找到另一个节点
  ```

###### 常见 NoSuchElement 异常解决方法

http://blog.csdn.net/mrlevo520/article/details/51954203 

http://blog.csdn.net/mrlevo520/article/details/51926145 

-	页面元素未加载完成

  - 使用显式等待：
    ```python
    WebDriverWait(driver_item, 10).until(lambda driver: driver.find_element_by_xpath("xpath 表达式"))
    ```

  - 使用 while+try…except：
    ```python
    driver_item=webdriver.Firefox()
    url="https://movie.douban.com/"
    driver_item.get(url)

    while 1:
        start = time.clock()
        try:
            driver_item.find_element_by_xpath("//div[@class='fliter-wp']/div/form/div/div/label[5]").click()
            print(‘已定位到元素’)
            end=time.clock()
            break
        except:
            print("还未定位到元素！")
    print('定位耗费时间：'+str(end-start))
    ```

- 定位方法错误

- 定位元素在 frame 中

  使用`switch_to_frame()`

- 发生页面重定向

  - 客户端重定向
    ```python
    driver.switch_to_window(driver.window_handles[-1])
    ```
  - 服务器端重定向
    <!-- TODO: -->

###### 元素对象常用属性

- 当前元素的 ID：id_

- 获取元素标签名的属性：tag_name

- 获取该元素的文本：text

##### 模拟输入

- 向文本框 text/textarea 标签输入文本：
  ```python
  element.send_keys("some text") # enter some text into a text field
  element.clear() # clear the contents of a text field or textarea
  ```

  https://selenium-python.readthedocs.io/navigating.html#filling-in-forms 

- 键盘按键输入：使用 Keys 类

  python selenium 常用 Keys 按键：
    - BACKSPACE（或者 BACK_SPACE） ——退格、删除键
    - TAB ——有时可用来切换 input 框的焦点
    - ENTER ——回车键，有时可用来代替点击提交按钮
    - SHIFT（或 LEFT_SHIFT） ——和其他按键同时发送，可发送大写字母或特殊符号
    - CONTROL（或 LEFT_CONTROL） ——和其他按键同时发送可实现一些功能如‘CONTROL+A’、‘CONTROL+C’、‘CONTROL+X’、‘CONTROL+V’等等
    - ALT（或 LEFT_ALT） ——和其他键一起使用
    - SPACE ——输入空格，或选中 checkbox、radio 框
    - PAGE_UP/PAGE_DOWN ——通过按键可上下翻页
    - F1~F12 ——功能键 
  
  例如：
  ```python
  # 按下键盘上的“向下”键
  element.send_keys(Keys.ARROW_DOWN) 

  # 同时按下多个键
  element.send_keys(Keys.CONTROL, ‘a’)
  element.send_keys(Keys.CONTROL, ‘c’)
  element.send_keys(Keys.CONTROL, ‘v’)
  ```

- 鼠标操作输入：使用 ActionChains 类

  ActionChains 的基本执行原理：当你调用 ActionChains 的方法时，不会立即执行，而是会将所有的操作按顺序存放在一个队列里，当你调用 perform() 方法时，队列中的时间会依次执行。
  ```python
  from selenium.webdriver.common.action_chains import ActionChains
  ```
  - [ActionChains 类方法列表](https://huilansame.github.io/huilansame.github.io/archivers/mouse-and-keyboard-actionchains )

  - 常用 ActionChains 类方法：
    
    - context_click()：右击
      ```python
      RightClick = driver.find_element_by_id("id")
      ActionChains(driver).context_click(RightClick).perform()
      ```
    - double_click()：双击
      ```python
      DoubleClick = driver.find_element_by_name("name")
      ActionChains(driver).double_click(DoubleClick).perform()
      ```
    - drag_and_drop(source, target)：鼠标拖放

      source：鼠标按下的源元素；target：鼠标释放的目标元素
      ```python
      element = driver.find_element_by_name("name")
      target = driver.find_element_by_name("name")
      ActionChains(driver).drag_and_drop(element, target).perform()
      ```
    - move_to_element()：鼠标悬停在一个元素上
      ```python
      above = driver.find_element_by_xpath("xpath 路径")
      ActionChains(driver).move_to_element(above).perform()
      ```
    - click_and_hold()：按下鼠标左键在一个元素上
      ```python
      left = driver.find_element_by_name("name")
      ActionChains(driver).click_and_hold(left).perform()
      ```

    例：两种写法本质是一样的，ActionChains 都会按照顺序执行所有的操作
    ```python
    # 链式写法
    menu = driver.find_element_by_css_selector(".nav")
    hidden_submenu = driver.find_element_by_css_selector(".nav #submenu1")
    ActionChains(driver).move_to_element(menu).click(hidden_submenu).perform()

    # 分步写法
    menu = driver.find_element_by_css_selector(".nav")
    hidden_submenu = driver.find_element_by_css_selector(".nav #submenu1")

    actions = ActionChains(driver)
    actions.move_to_element(menu)
    actions.click(hidden_submenu)
    actions.perform()
    ```

- 下拉选项卡：

  - 简单做法：
    ```python
    # 首先获取了第一个 select 元素，也就是下拉选项卡。然后轮流设置了 select 选项卡中的每一个 option 选项。
    element = driver.find_element_by_xpath("//select[@name='name']")
    all_options = element.find_elements_by_tag_name("option")
    for option in all_options:
        print("Value is: %s" % option.get_attribute("value"))
    option.click()
    ```
  
  - 使用 WebDriver 中提供的 Select 方法：
    ```python
    # Select 方法可以根据索引、值、文字来选择选项卡中的选项
    element = driver.find_element_by_xpath("//select[@name='name']")
    select = Select(element)
    select.select_by_index(index)
    select.select_by_visible_text("text")
    select.select_by_value(value)

    # 全部取消选择
    select.deselect_all()
    # 获取所有已选项
    all_selected_options = select.all_selected_options
    # 获取所有可选项
    options = select.options
    ```

- 提交表单：

  方式一：模拟点击 submit 按钮
  ```python
  # Assume the button has the ID "submit" 
  driver.find_element_by_id("submit").click()
  ```
  方式二：
  ```python
  element.submit()
  # element 对象有 submit() 方法。如果在表单中的元素上调用此方法，则 WebDriver 将向上移动 DOM，直到找到包围的表单，然后调用 submit 提交表单。如果元素不在表单中，则会引发 NoSuchElementException 异常
  ```

##### 滚动到页面底部

```python
for i in range(10):
    dirver.execute_script('window.scrollTo(0,document.body.scrollHeight)')
    time.sleep(1)
```

##### 拖拽

要完成元素的拖拽，首先你需要指定被拖动的元素和拖动目标元素，然后利用 ActionChains 类来实现。

```python
# 元素从 source 拖动到 target 的操作
element = driver.find_element_by_name("source")
target = driver.find_element_by_name("target")

action_chains = ActionChains(driver)
action_chains.drag_and_drop(element, target).perform()
```

##### 页面切换

https://www.cnblogs.com/kongzhongqijing/p/3534197.html 

###### frame 切换

```python
# 切换 frame，使焦点切换到一个 id 为 frameResult 的 frame 上，参数为 frame 的 id
# 当网页使用 ajax 加载 frame 时，直接 find_element 或者获取 page_source 都无法抓取 frame 中的元素，应当先切换到该 frame 中，才能对该 frame 中的元素进行操作
# 若有多层嵌套的 frame，要抓取最里层的 frame 中的元素时，需要先调用多次 switch_to_frame，将焦点一层层切换到最里层的 frame，才能对其中的元素进行操作
driver.switch_to_frame("innerFrame")
driver.switch_to_frame(“in_innerFrame”)
```

###### window 切换

```python
# 当浏览器中有多个窗口时，实现窗口的切换，参数为窗口名或窗口的句柄：
driver.switch_to_window("windowName")
driver.switch_to_window(driver.window_handles[-1]) 
```

```python
# 可以使用 driver.window_handles 属性来获取当前 driver 所有窗口的操作句柄（以列表形式返回）
# 获得当前最后的窗口，通过遍历，switch 到最后 / 最新一个窗口
for handle in driver.window_handles:
driver.switch_to_window(handle) 
# 等效于
driver.switch_to_window(driver.window_handles[-1])
```

```python
# 切换到当前的第二个窗口
switch_to_window(driver.window_handles[2])
```

```python
# 当请求发生客户端重定向时，页面已经跳转到新的 url，但是 driver 所绑定的仍是重定向前页面的 DOM，此时相当于是开了一个新的窗口，因此必须要 switch_to_window 才能获取新页面的 DOM 元素
# driver.window_handles[-1] 表示最新出现的窗口的句柄
driver.switch_to_window(driver.window_handles[-1])
```

##### 弹窗处理

```python
# 获取弹窗对象
alert = driver.switch_to_alert()
# This will return the currently open alert object. With this object you can now accept, dismiss, read its contents or even type into a prompt. This interface works equally well on alerts, confirms, prompts.
```

##### 历史记录

```python
driver.get("http://www.example.com")
# 前进
driver.forward()
# 后退
driver.back()
# 刷新
driver.refresh()
```

- 应用场景：

  一种情况就是，当你从一个父页面跳转到子页面进行操作，操作完之后没有“返回”之类的按钮或链接，重新进入父页面又很麻烦，back() 可以帮你。forward() 与此类似，相对没有 back() 那么常用。

  当你修改了页面信息但是没有即时刷新时，可以手动 refresh()。

  NOTE：

  当你前进并退回原来的页面或刷新页面之后，页面的 element 对象的 id 是改变了的。不要妄图用原来定位好的 WebElement 去操作现在的页面元素！否则会出现如下报错：
  ```
  selenium.common.exceptions.StaleElementReferenceException: Message: Element not found in the cache - perhaps the page has changed since it was looked up
  ```
  因此，要在刷新之后重新获取一下元素进行操作。

#### Cookie 操作

##### 基本操作

```python
# Go to the correct domain
driver.get("http://www.example.com")

# Now set the cookie. This one's valid for the entire domain
cookie = {‘name’ : ‘foo’, ‘value’ : ‘bar’}
driver.add_cookie(cookie)

# And now output all the available cookies for the current URL
driver.get_cookies()
```

##### 结合 js 操作

http://xxuan.me/2016-07-16-webscraper.html 

虽然 selenium 对取得 cookies 有极为方便的方法`get_cookies()`, 但遗憾的是 phantomJS 对 cookies 的格式支持不是那么完善，使用 phantomjs 直接取用时会报错。因此我们需要定义函数手动整理好 cookies 的格式并将其保存在一个 js 文件里，当下次 driver 启动的时候可以通过`execute_script()` 方法直接调用：
```python
def save_cookies(driver, file_path):
    # The format could vary
    LINE = "document.cookie = '{name}={value}; path={path}; domain={domain}';\n"
    with open(file_path, 'w') as fd:
        for cookie in driver.get_cookies():
            fd.write(LINE.format(**cookie))
def load_cookies(driver, file_path):
    with open(file_path, 'r') as fd:
        driver.execute_script(fd.read())
```
生成的 js 文件示例：
```javascript
document.cookie = 'iflyssesse=AAA; path=/; domain=mis.sse.ustc.edu.cn';
document.cookie = 'ASP.NET_SessionId=bbb; path=/; domain=mis.sse.ustc.edu.cn';
```

###### 操作 cookie 实现登陆操作

使用以上方法，辅以完善的报错机制，可以把登录操作封装成一个函数，完成一次登录操作之后就把 cookies 保存到文件之中，以便其后的爬虫直接通过 cookies 实现登录：

```python
def login(login_url, username, password):
    driver = webdriver.PhantomJS(executable_path=phantomjsPath,
                                 desired_capabilities=dcap)
    try:
        driver.get(login_url)
        wait = WebDriverWait(driver, 10)
        wait.until(EC.presence_of_element_located((By.XPATH, idXpath)))
        wait.until(EC.presence_of_element_located((By.XPATH, pwXpath)))
        wait.until(EC.element_to_be_clickable((By.XPATH, loginXpath)))
        driver.find_element_by_xpath(idXpath).send_keys(username)
        driver.find_element_by_xpath(pwXpath).send_keys(password)
        driver.find_element_by_xpath(loginXpath).click()
        print("Login Success!")
        wait.until(EC.presence_of_element_located((By.XPATH, redirectXpath)))
        print("Current url is: " + driver.current_url)
        print("Current cookies are: " + str(driver.get_cookies()))
        save_cookies(driver, r'cookies.js')
    except Exception as ex:
        print("Login failed!")
        print(ex)
    finally:
        driver.close()
```
直接使用 cookie 实现登陆：
```python
try:
        driver.get(login_url)
        driver.delete_all_cookies() # 先清空自己的 cookies
        load_cookies(driver, r'cookies.js') # 载入预先保存的 cookies 
        driver.get(scrape_url)
		# 直接访问要去的地址，比如 scrape_url 这个子域名。之所以先登录再访问而不是直接访问是因为 cookies 的域名必须和 driver 原先的域名一致，不然会报错，因此先登录主页初始化一下域名
        if driver.current_url == login_url:
            raise NameError('Redirect failed!')
        print("Scraper login through cookie success!")
        print("scrapping...")
        # Do anything here! Scrape!
    except Exception as ex:
        print("Scraper login through cookie failed!")
        print(ex)
    finally:
        logout(driver, login_url)
```
封装退出登录操作：
```python
def logout(driver, login_url):
    try:
        driver.get(login_url)
        driver.delete_all_cookies()
        load_cookies(driver, r'cookies.js')
        # set window size for phantomJS or it won'r find button
        driver.set_window_size(1024, 768)
        driver.get(login_url)

        wait = WebDriverWait(driver, 10)
        wait.until(EC.element_to_be_clickable((By.XPATH, logoutXpath)))
        driver.find_element_by_xpath(logoutXpath).click()
        print("Logout Success!")
    except Exception as ex:
        print("Logout Failed")
        print(ex)
    finally:
        driver.close()
```

#### 等待

https://huilansame.github.io/huilansame.github.io/archivers/sleep-implicitlywait-wait 

##### 强制等待

使用 time.sleep(xxx)，强制使程序暂停运行 xx 秒；

缺点：太死板，严重影响程序执行速度。

##### 隐性等待

使用 implicitly_wait(xx)；

隐形等待是设置了一个最长等待时间，如果在规定时间内网页加载完成，则执行下一步，否则一直等到时间截止，然后执行下一步。 默认等待时间是 0 秒。

缺点：implicitly_wait() 会等待页面的所有元素（包括所有 js 元素）加载完毕后才进行下一步，然而很多情况下我们需要的只是页面中的某些元素。

注意：一旦设置该值，就设置了该 WebDriver 的实例的生命周期，对整个 driver 实例的周期都起作用，所以只要设置一次即可。

例：
```python
driver = webdriver.Firefox()
driver.implicitly_wait(10) # seconds
driver.get("http://somedomain/url_that_delays_loading")
myDynamicElement = driver.find_element_by_id("myDynamicElement")
```

##### 显式等待

使用 wait 模块中的 WebDriverWait 类配合该类的 until() 和 until_not() 方法，以及 ExpectedCondition 模块；

显式等待是你在代码中定义等待一定条件发生后再进一步执行你的代码，能够根据判断条件而进行灵活地等待；主要的意思就是：程序每隔 xx 秒看一眼，如果条件成立了，则执行下一步，否则继续等待，直到超过设置的最长时间，然后抛出 TimeoutException；

WebDriverWait 类：
```
selenium.webdriver.support.wait.WebDriverWait（类）

__init__
    driver: 传入 WebDriver 实例，即我们上例中的 driver
    timeout: 超时时间，等待的最长时间（同时要考虑隐性等待时间）
    poll_frequency: 调用 until 或 until_not 中的方法的间隔时间，默认是 0.5 秒
    ignored_exceptions: 忽略的异常，如果在调用 until 或 until_not 的过程中抛出这个元组中的异常，则不中断代码，继续等待，如果抛出的是这个元组外的异常，则中断代码，抛出异常。默认只有 NoSuchElementException。

until
    method: 在等待期间，每隔一段时间调用这个传入的方法，直到返回值不是 False
    message: 如果超时，抛出 TimeoutException，将 message 传入异常

until_not ：与 until 相反，until 是当某元素出现或什么条件成立则继续执行，until_not 是当某元素消失或什么条件不成立则继续执行，参数也相同，不再赘述。
    method
    message
```

###### 使用 expected_conditions 模块中的预定义条件

[官方 API](https://seleniumhq.github.io/selenium/docs/api/py/webdriver_support/selenium.webdriver.support.expected_conditions.html )

```
selenium.webdriver.support.expected_conditions（模块）

# 这两个条件类验证 title，验证传入的参数 title 是否等于或包含于 driver.title
title_is
title_contains

# 这两个条件验证元素是否出现，传入的参数都是元组类型的 locator，如 (By.ID, 'kw')
# 顾名思义，一个只要一个符合条件的元素加载出来就通过；另一个必须所有符合条件的元素都加载出来才行
presence_of_element_located
presence_of_all_elements_located

# 这三个条件验证元素是否可见，判断某个元素是否可见。可见代表元素非隐藏，并且元素的宽和高都不等于 0
# 前两个传入参数是元组类型的 locator，第三个传入 WebElement
# 第一个和第三个其实质是一样的
visibility_of_element_located
invisibility_of_element_located
visibility_of

# 这两个条件判断某段文本是否出现在某元素中，一个判断元素的 text，一个判断元素的 value
text_to_be_present_in_element
text_to_be_present_in_element_value

# 这个条件判断 frame 是否可切入，如果可以的话，返回 True 并且 switch 进去，否则返回 False
# 可传入 locator 元组或者直接传入定位方式：id、name、index 或 WebElement
frame_to_be_available_and_switch_to_it

# 这个条件判断是否有 alert 出现
alert_is_present

# 这个条件判断元素是否可点击，传入 locator
element_to_be_clickable

# 这四个条件判断元素是否被选中，第一个条件传入 WebElement 对象，第二个传入 locator 元组
# 第三个传入 WebElement 对象以及状态，相等返回 True，否则返回 False
# 第四个传入 locator 以及状态，相等返回 True，否则返回 False
element_to_be_selected
element_located_to_be_selected
element_selection_state_to_be
element_located_selection_state_to_be

# 最后一个条件判断一个元素是否仍在 DOM 中，传入 WebElement 对象，可以判断页面是否刷新了
staleness_of
```

调用方法原型：
```
WebDriverWait(driver, 超时时长，调用频率，忽略的异常).until（可执行方法，超时时返回的信息)
```
对于 until 或 until_not 中的可执行方法 method 参数：
  - 一定要是可以调用的，即这个对象一定有 __call__() 方法，否则会抛出异常：TypeError: 'xxx' object is not callable；
  - 可以用 selenium 提供的 expected_conditions 模块中的各种条件，也可以用 WebElement 的 is_displayed() 、is_enabled()、is_selected() 方法，或者用自己封装的方法；

例：
```python
driver = webdriver.Firefox()
driver.get("http://somedomain/url_that_delays_loading")
try:
    element = WebDriverWait(driver, 10).until(
        expected_conditions.presence_of_element_located((By.ID, "myDynamicElement"))
    )
finally:
driver.quit()
# 在抛出 TimeoutException 异常之前将等待 10 秒或者在 10 秒内发现了查找的元素。 WebDriverWait 默认情况下会每 500 毫秒调用一次 ExpectedCondition 直到结果成功返回。ExpectedCondition 成功的返回结果是一个布尔类型的 true 或是不为 null 的返回值
```

###### 使用 lambda 表达式

例：
```python
# 10 秒内每隔 500 毫秒扫描 1 次页面变化，当 DOM 加载了 xpath 表达式指定的元素后等待结束
# 超过 10 秒仍未出现目标元素，则抛出超时异常
from selenium.webdriver.support.ui import WebDriverWait
WebDriverWait(driver_item, 10).until(lambda driver: driver.find_element_by_xpath("xpath 表达式"))

# 若元素已被加载，只是暂时不可见，可通过 is_displayed() 进行显式等待
WebDriverWait(driver, 10).until(lambda driver: driver.find_element_by_xpath('xpath 表达式').is_displayed())
```

#### 调用 JavaScript
1. 	
  ```python
  # execute_async_script() 方法是异步方法，它不会阻塞主线程执行
  driver.execute_async_script(js)
  ```

2. 

  ```python
  # execute_script() 是同步方法，用它执行 js 代码会阻塞主线程执行，直到 js 代码执行完毕
  # execute_script() 方法如果有返回值，有以下几种情况：
  # 如果返回一个页面元素（document element), 这个方法就会返回一个 WebElement
  # 如果返回浮点数字，这个方法就返回一个 double 类型的数字
  # 返回非浮点数字，方法返回 Long 类型数字
  # 返回 boolean 类型，方法返回 Boolean 类型
  # 如果返回一个数组，方法会返回一个 List<Object>
  # 其他情况，返回一个字符串
  # 如果没有返回值，此方法就会返回 null
  driver.execute_script(js)

  # 传入 WebElement 作为参数执行 JS
  div = driver.findElemnt(By.id("myDiv")); 
  driver.execute_script("arguments[0].setAttribute('style', arguments[1])", div, "height: 1000px");
  ```
  例：
  ```python
  # 修改标签体内容
  driver.execute_script('arguments[0].innerHTML = arguments[1]', title_element, data[i-1](0))
  ```

#### 关闭浏览器

```python
driver.quit()
或 driver.close()
# 若不主动关闭浏览器，程序会一直运行
```

#### 程序结构

一个页面对象表示在你测试的 WEB 应用程序的用户界面上的区域。

使用页面对象模式的好处：
- 创建可复用的代码以便于在多个测试用例间共享；
- 减少重复的代码量；
- 如果用户界面变化，只需要修改一处；

<!-- TODO: https://selenium-python-zh.readthedocs.io/en/latest/page-objects.html  -->

## PhantomJS

### 介绍

phantomJS 是一个基于 webkit 的“无头浏览器”，原生支持多种 web 标准：DOM 操作，CSS 选择器，JSON，Canvas 以及 SVG。

PhantomJS 的功能，就是提供一个浏览器环境的命令行接口，可以像浏览器解析网页，但不提供图形界面，功能非常强大，比如生成网页的截图、抓取网页数据等。

### 安装

PhantomJS 安装方法有两种，一种是下载源码之后自己来编译，另一种是直接下载编译好的二进制文件。编译需要的时间太长，而且需要挺多的磁盘空间。官方推荐直接下载二进制文件即可。

下载二进制文件 phantomJS.exe，然后将路径加入系统环境变量即可。

安装完成之后命令行输入
```
phantomjs -v
```

如果正常显示版本号，那么证明安装成功了。

### 使用

http://javascript.ruanyifeng.com/tool/phantomjs.html

#### REPL 环境

phantomjs 提供了一个完整的 REPL（交互式编程) 环境，允许用户通过命令行与 PhantomJS 互动。在命令行中键入 phantomjs，进入该环境后便可输入 Javascript 命令进行交互。使用 ctrl+c 可以退出该环境。

#### webpage 模块

webpage 模块是 PhantomJS 的核心模块，用于网页操作。
```javascript
var webPage = require('webpage');
var page = webPage.create();
// 加载 PhantomJS 的 webpage 模块，并创建一个实例
```

webpage 属性和方法：

- open()

  open 方法用于打开具体的网页。
  ```javascript
  var page = require('webpage').create();

  page.open('http://slashdot.org', function (s) {
    console.log(s);
    phantom.exit();
  });
  ```

- evaluate()

  evaluate 方法用于打开网页以后，在页面中执行 JavaScript 代码。
  ```javascript
  var page = require('webpage').create();

  page.open(url, function(status) {
    var title = page.evaluate(function() {
      return document.title;
    });
    console.log('Page title is ' + title);
    phantom.exit();
  });
  ```

- includeJs()

  includeJs 方法用于页面加载外部脚本，加载结束后就调用指定的回调函数。
  ```javascript
  var page = require('webpage').create();
  page.open('http://www.sample.com', function() {
    page.includeJs("http://path/to/jquery.min.js", function() {
      page.evaluate(function() {
        $("button").click();
      });
      phantom.exit()
    });
  });
  // 上面的例子在页面中注入 jQuery 脚本，然后点击所有的按钮。需要注意的是，由于是异步加载，所以 phantom.exit() 语句要放在 page.includeJs() 方法的回调函数之中，否则页面会过早退出。
  ```

- render()

  render 方法用于将网页保存成图片，参数就是指定的文件名。该方法根据后缀名，将网页保存成不同的格式，目前支持 PNG、GIF、JPEG 和 PDF。
  ```javascript
  var webPage = require('webpage');
  var page = webPage.create();

  page.viewportSize = { width: 1920, height: 1080 };
  page.open("http://www.google.com", function start(status) {
    page.render('google_home.jpeg', {format: 'jpeg', quality: '100'});
    phantom.exit();
  });
  ```
  该方法还可以接受一个配置对象，format 字段用于指定图片格式，quality 字段用于指定图片质量，最小为 0，最大为 100。

- viewportSize,zoomFactor

  viewportSize 属性指定浏览器视口的大小，即网页加载的初始浏览器窗口大小；viewportSize 的 Height 字段必须指定，不可省略。

  zoomFactor 属性用来指定渲染时（render 方法和 renderBase64 方法）页面的放大系数，默认是 1（即 100%）。

- onResourceRequested

  scriptonResourceRequested 属性用来指定一个回调函数，当页面请求一个资源时，会触发这个回调函数。它的第一个参数是 HTTP 请求的元数据对象，第二个参数是发出的网络请求对象。

  HTTP 请求包括以下字段。
    - id：所请求资源的编号
    - method：使用的 HTTP 方法
    - url：所请求的资源 URL
    - time：一个包含请求时间的 Date 对象
    - headers：HTTP 头信息数组
  网络请求对象包含以下方法。
    - abort()：终止当前的网络请求，这会导致调用 onResourceError 回调函数。
    - changeUrl(newUrl)：改变当前网络请求的 URL。
    - setHeader(key, value)：设置 HTTP 头信息。

- onResourceReceived

  scriptonResourceReceived 属性用于指定一个回调函数，当网页收到所请求的资源时，就会执行该回调函数。它的参数就是服务器发来的 HTTP 回应的元数据对象，包括以下字段。
    - id：所请求的资源编号
    - url：所请求的资源的 URL r- time：包含 HTTP 回应时间的 Date 对象
    - headers：HTTP 头信息数组
    - bodySize：解压缩后的收到的内容大小
    - contentType：接到的内容种类
    - redirectURL：重定向 URL（如果有的话）
    - stage：对于多数据块的 HTTP 回应，头一个数据块为 start，最后一个数据块为 end。
    - status：HTTP 状态码，成功时为 200。
    - statusText：HTTP 状态信息，比如 OK。
      如果 HTTP 回应非常大，分成多个数据块发送，onResourceReceived 会在收到每个数据块时触发回调函数。

#### system 模块

system 模块可以加载操作系统变量，system.args 就是参数数组。

例：

page.js
```javascript
var page = require('webpage').create(),
    system = require('system'),
    t, address;

// 如果命令行没有给出网址
if (system.args.length === 1) {
    console.log('Usage: page.js <some URL>');
    phantom.exit();
}

t = Date.now();
address = system.args[1];
page.open(address, function (status) {
    if (status !== 'success') {
        console.log('FAIL to load the address');
    } else {
        t = Date.now() - t;
        console.log('Loading time ' + t + ' ms');
    }
    phantom.exit();//phantom.exit(); 这句话非常重要，否则程序将永远不会终止。
});
```
命令行
```shell
$ phantomjs page.js http://www.google.com
```
#### 应用

详见 http://cuiqingcai.com/2577.html

##### 过滤资源

##### 截图

##### 抓取图片

##### 生成网页

## Refer Links

http://cuiqingcai.com/2599.html

http://ftopia.cn/2016/06/20/Selenium%20+%20Python%20%E8%87%AA%E5%8A%A8%E5%8C%96%E6%B5%8B%E8%AF%95%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA.html 
