.. figure:: http://p20tr36iw.bkt.clouddn.com/py_scrapy_selenium.png
   :alt: 

.. raw:: html

   <!--more-->

python爬虫系列之Selenium控制浏览器操作
======================================

1.弹出浏览器运行
----------------

.. code:: python

    from selenium import webdriver
    driver = webdriver.Chrome()
    driver.get('https://morvanzhou.github.io/')
    driver.find_element_by_xpath(u"//img[@alt='强化学习 (Reinforcement Learning)']").click()
    driver.find_element_by_link_text('About').click()
    driver.find_element_by_link_text(u'赞助').click()
    driver.find_element_by_link_text(u'教程 ▾').click()
    driver.find_element_by_link_text(u'数据处理 ▾').click()
    driver.find_element_by_link_text(u'网页爬虫').click()

    # 页面html源码
    html = driver.page_source

    # 截图
    driver.get_screenshot_as_file('./img/screenshot1.png')

    driver.close()

2.静默状态运行
--------------

每次都要看着浏览器执行这些操作, 有时候有点不方便. 我们可以让 selenium
不弹出浏览器窗口, 让它”安静”地执行操作. 在创建 driver
之前定义几个参数就能摆脱浏览器的身体了.

.. code:: python

    # 在上述第一行代码后面加入下面代码，并按照加入chrome_options参数
    from selenium.webdriver.chrome.options import Options
    chrome_options = Options()
    chrome_options.add_argument("--headless")
    driver = webdriver.Chrome(chrome_options=chrome_options)

