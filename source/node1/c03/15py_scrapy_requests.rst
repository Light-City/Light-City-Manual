.. figure:: http://p20tr36iw.bkt.clouddn.com/py_req.png
   :alt: 

.. raw:: html

   <!--more-->

python爬虫系列之多功能的 Requests
=================================

-  python 自带urllib提交网页请求
-  Python 外部模块 requests,向网页发送信息, 上传图片等等。

本节学习requests的基本用法及示例

0.安装模块
----------

.. code:: python

    import requests
    import webbrowser

-  requests用于向url提交请求及数据的外部python模块
-  webbrowser用于打开一个url页面

1. webbrowser简单实用
---------------------

.. code:: python

    # 使用默认浏览器打开
    b.open('http://light-city.me')
    # 使用非默认浏览器打开
    b = webbrowser.get('C:/Program Files (x86)/Google/Chrome/Application/chrome.exe %s')
    b.open('http://light-city.me')
    # 除了上述方法,还可以用下面方法,比如用IE浏览器
    b = webbrowser.get(webbrowser.iexplore)

2.requests使用
--------------

2.1 requests的get方式
~~~~~~~~~~~~~~~~~~~~~

.. code:: python

    param = {'wd':'莫烦python'}
    r = requests.get('http://www.baidu.com/s',params=param)
    print(r.url)
    webbrowser.open(r.url)

2.2 requests的post方式
~~~~~~~~~~~~~~~~~~~~~~

2.2.1 post简单网页
^^^^^^^^^^^^^^^^^^

-  `简单网页直达 <http://pythonscraping.com/pages/files/form.html>`__

**post提交需要关注以下三点信息(打开浏览器inspect)**

-  Request URL (post 要用的 URL)
-  Request Method (post)
-  Form Data (post 去的信息)

.. code:: python

    data={'firstname':'z','lastname':'x'}
    r = requests.post('http://pythonscraping.com/pages/files/processing.php',data=data)
    print(r.text)

2.2.2 post上传图片
^^^^^^^^^^^^^^^^^^

-  `上传图片直达 <http://pythonscraping.com/files/form2.html>`__

.. code:: python

    # 点击选择图片按钮，实际上post提交的是这个按钮对应的值,那么从源代码里卖弄看相应的key,value设为图片地址即可。
    file = {'uploadFile': open('./image.png', 'rb')}
    r = requests.post('http://pythonscraping.com/pages/files/processing2.php', files=file)
    print(r.text)

2.2.3 post简单的登陆
^^^^^^^^^^^^^^^^^^^^

**从登陆学习Cookie**

-  `简单登陆页面直达 <http://pythonscraping.com/pages/cookies/login.html>`__

.. code:: python

    # 打开网页时, 每一个页面都是不连续的, 没有关联的, Cookie 就是用来衔接一个页面和另一个页面的关系。
    # 本节实例就是通过post提交信息，然后通过cookie调用登陆后页面的内容

    # 注意:username为自定义填写，password为password,否则在inspect中查看不到相应的cookie
    payload = {'username': 'z', 'password': 'password'}
    r = requests.post('http://pythonscraping.com/pages/cookies/welcome.php', data=payload)
    print(r.cookies.get_dict())
    r = requests.get('http://pythonscraping.com/pages/cookies/profile.php', cookies=r.cookies)
    print(r.text)

**使用Session登陆(Session管理Cookie)**

上述示例,每次都要传递 cookies 是很麻烦的, 好在有个Session. 在一次会话中,
我们的 cookies 信息都是相连通的, 它自动帮我们传递这些 cookies 信息。

同样是执行上面的登录操作, 下面就是使用 session 的版本. 创建完一个
session 过后, 我们直接只用 session 来 post 和 get. 而且这次 get 的时候,
我们并没有传入 cookies. 但是实际上 session 内部就已经有了之前的 cookies
了。

.. code:: python

    session = requests.Session()
    payload = {'username': 'a', 'password': 'password'}
    r = session.post('http://pythonscraping.com/pages/cookies/welcome.php', data=payload)
    print(r.cookies.get_dict())
    r = session.get("http://pythonscraping.com/pages/cookies/profile.php")
    print(r.text)

3.参考文章
----------

`多功能的
Requests <https://morvanzhou.github.io/tutorials/data-manipulation/scraping/3-01-requests/>`__
