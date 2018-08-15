.. figure:: http://p20tr36iw.bkt.clouddn.com/py_showdownloadimg.png
   :alt: 

.. raw:: html

   <!--more-->

python爬虫系列之Senium反爬虫
============================

1.反爬虫方案
------------

说在前面:爬取的是国家地理中文网上最新一栏的三张图片，点击查看更多又会出现三张图片，总共六张。

**第一个难点:获取真实的html**

-  selenium + chromdriver

通过url直接访问这个网站，获得的html并不是想要的，会发现里面提示:
浏览器正在安全检查中....

对于上述并未爬到想要的html解决方案是，发现该网站通过js来运行，倒计时后将字符串拼接请求，进入相应网站，如果能够模拟浏览器自动执行js,那么就实现了我们想要的效果了。

于是，这里采用selenium通过chromdriver调用chrome浏览器，模拟操作，自动运行js，(这里注意，倒计时5s，那么get
url后，设置时间得大于5s,用time模块的sleep方法模拟即可)进而直接获得相应的html，随后进行正常的爬虫。

**第二个难点:获得html后，并通BeautifulSoup获取到了6张图片的url,如何下载url对应的图片**

-  requests.get + cookies + headers

这里下载采用requests.get方法来下载图片,但是直接这样操作会出现503错误(Service
Unavailable)，下载出来的图片也无法查看，那么就要解决这个问题。
解决办法：通过webdriver获得cookies，并对cookie进行下载与格式化为字典形式，传递给requests的get方法，除此之外，需要将User-Agent传递给requests的get方法，这里写入headers中进行传递。

**第三个难点:如何将这些下载的图片进行呈现，并合并到一张图中集体展示**

-  matplotlib.pyplot + matplotlib.image

先通过matplotlib.image的imread方法读取图片,再通过matplotlib.pyplot绘制一个figure，然后在绘制子图放入figure中即可。

2.完整代码
----------

2.1 导入相关的库
~~~~~~~~~~~~~~~~

.. code:: python

    import time
    from bs4 import BeautifulSoup as bs
    from selenium import webdriver
    import requests
    import matplotlib.pyplot as plt
    import matplotlib.image as mping

2.2 selenium + chromdriver
~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: python

    # 反爬虫应对代码

    driver = webdriver.Chrome() #注意需要将chromedriver放入代码中方可运行
    URL = 'http://www.ngchina.com.cn/animals/'
    driver.get(URL)
    time.sleep(7) # 要大于5s
    html=driver.page_source # 获取实际页面的html
    # print(html)

2.2 BeautifulSoup下载图片+打开图片
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  保存下载cookies操作

.. code:: python

    # 获取cookies，保存到本地，读取进行格式化
    driver_cookie = driver.get_cookies()
    cookies = [item["name"] + "=" + item["value"] for item in driver_cookie]
    cookiesStr = ' '.join(cookies)
    # print cookiesStr
    with open('cookies.txt', 'w') as f:
        f.write(cookiesStr)

    with open('cookies.txt') as f:
        str_cookies = f.read()
    print("str_cookies..................."+str_cookies)
    list_cookies = str_cookies.split(' ')  # 对字符串切片,返回分割后的字符串列表

    cookies = {}
    for cookie in list_cookies:
        '''
        robots=1
        cookie.split('=')
        变为
        ['robots', '1']
        key = cookie.split('=')[0]
        value = cookie.split('=')[-1]
        '''
        key = cookie.split('=')[0]
        value = cookie.split('=')[-1]
        '''
        dict.update(dict2)
        # 把字典dict2的键/值对更新到dict里
        '''
        cookies.update({key : value}) # 变为字典类型，如：{'robots': '1'}
    print(cookies)

-  BeautifulSoup根据真实Html,获取图片Url

.. code:: python

    soup = bs(html,'lxml')
    img_ul = soup.find_all('ul',{"class":"img_list"})
    print(img_ul)


    headers = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/68.0.3440.106 Safari/537.36',
    }

    i=1
    for ul in img_ul:
        imgs = ul.find_all('img')
        for img in imgs:
            url = img['src']

            r = requests.get(url, headers=headers, cookies=cookies) # cookies与headers一起解决503错误
            print(r.status_code)
            image_name = url.split('/')[-1]
            with open('./img/%s' % image_name, 'wb') as f:
                for chunk in r.iter_content(chunk_size=128):
                    f.write(chunk)
            print('Saved %s' % image_name)

            # 使用Matlibplot展示图片
            with open('./img/%s' % image_name, 'r') as f:
                _img = mping.imread('./img/%s' % image_name)
            if i==1:
                plt.figure()
            plt.subplot(2,3,i) # 2行三列显示在第i个位置
            plt.imshow(_img)
            plt.title(image_name)
            plt.axis('off')
            i=i+1
    plt.show()

3.项目地址
----------

`戳我star!!! <https://github.com/Light-City/py_scrapy/blob/master/scrapy_downloadexample/sc_scr_down.py>`__

4.参考文章
----------

`Requests开发接口 <http://docs.python-requests.org/zh_CN/latest/api.html#requests.cookies.RequestsCookieJar>`__

`selenium-webdriver(python) (十三) --
cookie处理 <https://www.cnblogs.com/fnng/p/3269450.html>`__

`下载美图 <https://morvanzhou.github.io/tutorials/data-manipulation/scraping/3-03-practice-download-image/>`__

`使用Python +
Selenium打造浏览器爬虫 <https://www.jianshu.com/p/2263d023b559>`__

`一种爬虫绕过百度云加速检测的方法 <https://delcoding.github.io/2017/12/scrapy-bypass/>`__

`Selenium控制Chrome初探 <http://gohom.win/2016/02/02/Selenium-Chrome/>`__

`SeleniumHQ Browser
Automation <https://docs.seleniumhq.org/docs/03_webdriver.jsp#introducing-the-selenium-webdriver-api-by-example>`__
