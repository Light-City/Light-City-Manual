.. figure:: http://p20tr36iw.bkt.clouddn.com/py_scrapy.png
   :alt: 

.. raw:: html

   <!--more-->

python爬虫系列之Scrapy框架
==========================

1.创建项目
----------

.. code:: python

    scrapy startproject jianshu_article

2.创建爬虫程序
--------------

2.1 自动创建
~~~~~~~~~~~~

.. code:: python

    cd jianshu_article
    scrapy genspider jianshu jianshu.com

2.2 手动创建
~~~~~~~~~~~~

.. code:: python

    为了创建一个Spider，您必须继承 scrapy.Spider 类， 且定义以下三个属性:
    name: 用于区别Spider。 该名字必须是唯一的，您不可以为不同的Spider设定相同的名字。
    start_urls: 包含了Spider在启动时进行爬取的url列表。 因此，第一个被获取到的页面将是其中之一。 后续的URL则从初始的URL获取到的数据中提取。
    parse() 是spider的一个方法。 被调用时，每个初始URL完成下载后生成的 Response 对象将会作为唯一的参数传递给该函数。 该方法负责解析返回的数据(response data)，提取数据(生成item)以及生成需要进一步处理的URL的 Request 对象。

3.设置数据模型
--------------

Item 是保存爬取到的数据的容器；其使用方法和python字典类似，
并且提供了额外保护机制来避免拼写错误导致的未定义字段错误。

.. code:: python

    class JianshuArticleItem(scrapy.Item):
        # define the fields for your item here like:
        # name = scrapy.Field()
        avatar = scrapy.Field()  # 头像
        nickname = scrapy.Field()  # 昵称
        write_love = scrapy.Field() # 字数及喜欢
        fan = scrapy.Field()  # 粉丝
        force = scrapy.Field()  # 关注

4.分析页面源码，编写爬虫
------------------------

``jianshu.py``

.. code:: python

    # -*- coding: utf-8 -*-
    import scrapy
    from jianshu_article.items import JianshuArticleItem


    class JianshuSpider(scrapy.Spider):
        name = 'jianshu'
        allowed_domains = ['jianshu.com']

        user_id = "1b4c832fb2ca"
        url = "https://www.jianshu.com/u/{0}?page=1".format(user_id)
        start_urls = [
            url,
        ]


        def parse(self, response):

            '''
            不写extract()打印出来就是Selector对象,加上extract()
            就是返回一个list对象,里面为对应的value,
            取值可通过.extract()[index]来取得。
            '''
            # 用户头像
            avatar = response.xpath('//div[@class="avatar"]/img/@src').extract()
            # 昵称
            nickname = response.xpath('//div[@class="author-info"]/div[@class="name"]/text()').extract()
            # 关注及粉丝
            force_fan = response.xpath('//div[@class="author-info"]/div[@class="follow-meta"]/span/text()').extract()
            # 字数及喜欢
            write_love = response.xpath('normalize-space(//div[@class="author-meta"]/text())').extract() # normalize-space去除所有回车换行
            item = JianshuArticleItem()
            item['avatar'] = avatar[0]
            item['nickname'] = nickname[0]
            item['write_love'] = write_love[0]
            item['force'] = force_fan[0]
            item['fan'] = force_fan[1]
            print(item)
            yield item

5.运行Scrapy
------------

.. code:: python

    scrapy crawl spider的name
    这里是:
    scrapy crawl jianshu

6.运行后错误解决
----------------

.. code:: python

    USER_AGENT = 'Mozilla/5.0 (Linux; Android 6.0; Nexus 5 Build/MRA58N) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/68.0.3440.106 Mobile Safari/537.36'
    CONCURRENT_REQUESTS = 1 #并发数
    DOWNLOAD_DELAY = 5  #为了防止IP被封，我们5秒请求一次
    HTTPERROR_ALLOWED_CODES = [403] #上面报的是403，就把403加入
    # Obey robots.txt rules
    ROBOTSTXT_OBEY = False

7.数据存储至csv
---------------

7.1 修改\ ``pipelines.py``
~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: python

    import json
    class JianshuArticlePipeline(object):
        def open_spider(self, spider):
            self.file = open('jianshu.json','w',encoding='utf-8') # encoding设置编码格式，否则保存的中文会乱码

        def process_item(self, item, spider):
            if(type(item)!=dict):
                item = dict(item)
            str_data = json.dumps(item,ensure_ascii=False) + ',\n'
            self.file.write(str_data)
            return item

        def close_spider(self, spider):
            self.file.close()

7.2 setting.py设置
~~~~~~~~~~~~~~~~~~

.. code:: python

    ITEM_PIPELINES = {
        'jianshu_article.pipelines.JianshuArticlePipeline': 300,
    }

8.项目地址
----------

`戳这里! <https://github.com/Light-City/jianshu_article>`__

9.参考文章
----------

`Scrapy 0.24
文档 <https://scrapy-chs.readthedocs.io/zh_CN/0.24/index.html>`__

`Python--Scrapy爬虫获取简书作者ID的全部文章列表数据 <https://www.jianshu.com/p/daaa65c07af1>`__
