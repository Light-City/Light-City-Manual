.. figure:: http://p20tr36iw.bkt.clouddn.com/py_scrapy_db.png
   :alt: 

.. raw:: html

   <!--more-->

Scrapy框架之批量下载360图片
===========================

1.项目初始化
------------

-  创建项目

.. code:: python

    scrapy startproject images360

-  创建Spider

::

    scrapy genspider images images.so.com

2.定义存储结构
--------------

查看数据:

.. figure:: http://p20tr36iw.bkt.clouddn.com/py_scrapy_res.png
   :alt: 

``items.py``

.. code:: python

    # 提取数据
    class Images360Item(Item):
        # define the fields for your item here like:
        # name = scrapy.Field()
        collection = table = 'images' # 设置MongoDB的表名为images
        id = Field()
        url = Field()
        title = Field()
        thumb = Field()

3.Spider核心代码
----------------

-  ``settings.py``

.. code:: python

    MAX_PAGE = 50 # 爬取 50 页，每页 30 张，一共 1500 张图片
    ROBOTSTXT_OBEY = False # 设为False,否则无法抓取

-  ``images.py``

分析网页(http://images.so.com/z?ch=beauty)

注意先打开 http://images.so.com/
然后点美女,紧接着,打开浏览器的检查页面,点击Network的XHR,向下滑动鼠标,动态加载出如下图一所示的左边一栏Name值,再往下滑动鼠标会发现，Name里面的sn不断发生变化,其他的保持不变。

.. figure:: http://p20tr36iw.bkt.clouddn.com/py_scrapy_show.png
   :alt: 

.. figure:: http://p20tr36iw.bkt.clouddn.com/py_scrapy_res.png
   :alt: 

.. code:: python

    from scrapy import Spider, Request
    from urllib.parse import urlencode
    import json
    from images360.items import Images360Item

    class ImagesSpider(Spider):
        name = 'images'
        allowed_domains = ['images.so.com']
        start_urls = ['http://images.so.com/']

        def start_requests(self):
            data = {'ch': 'beauty', 'listtype': 'new', 'temp': '1'}
            base_url = 'https://image.so.com/zj?'
            for page in range(1, self.settings.get('MAX_PAGE') + 1):
                data['sn'] = page * 30
                params = urlencode(data)
                url = base_url + params
                yield Request(url, self.parse)

        def parse(self, response):
            result = json.loads(response.text) # 字符串转dict类型
            for image in result.get('list'):
                item = Images360Item()
                item['id'] = image.get('imageid')
                item['url'] = image.get('qhimg_url')
                item['title'] = image.get('group_title')
                item['thumb'] = image.get('qhimg_thumb_url')
                yield item

4.pipeline下载及存储
--------------------

-  修改\ ``settings.py``

启用item Pipeline组件
每个pipeline后面有一个数值，这个数组的范围是0-1000，这个数值确定了他们的运行顺序，数字越小越优先

.. code:: python

    ITEM_PIPELINES = {
       # 下载图片到本地
       'images360.pipelines.ImagePipeline': 300,
       # 存储至MongoDB
       'images360.pipelines.MongoPipeline': 301
    }

    BOT_NAME = 'images360'
    MAX_PAGE = 50
    MONGO_URI = 'localhost'
    MONGO_DB = 'test'

-  设置图片存储路径

``settings.py``

.. code:: python

    import os
    # 配置数据保存路径，为当前工程目录下的 images 目录中
    project_dir = os.path.abspath(os.path.dirname(__file__))
    print(project_dir)
    IMAGES_STORE = os.path.join(project_dir, 'images')

-  修改pipelines.py

``process_item(self,item,spider)``

每个item
piple组件是一个独立的pyhton类，必须实现以process\_item(self,item,spider)方法;每个item
pipeline组件都需要调用该方法，这个方法必须返回一个具有数据的dict,或者item对象，或者抛出DropItem异常，被丢弃的item将不会被之后的pipeline组件所处理

.. code:: python

    import pymongo
    from scrapy import Request
    from scrapy.exceptions import DropItem
    from scrapy.pipelines.images import ImagesPipeline

    class MongoPipeline(object):
        def __init__(self, mongo_uri, mongo_db):
            self.mongo_uri = mongo_uri
            self.mongo_db = mongo_db

        @classmethod
        def from_crawler(cls, crawler):
            return cls(
                mongo_uri=crawler.settings.get('MONGO_URI'),
                mongo_db=crawler.settings.get('MONGO_DB')
            )

        def open_spider(self, spider):
            self.client = pymongo.MongoClient(self.mongo_uri)
            self.db = self.client[self.mongo_db]

        def process_item(self, item, spider):
            self.db[item.collection].insert(dict(item))
            return item

        def close_spider(self, spider):
            self.client.close()

    class ImagePipeline(ImagesPipeline):
        def file_path(self, request, response=None, info=None):
            url = request.url
            file_name = url.split('/')[-1]
            return file_name

        def item_completed(self, results, item, info):
            image_paths = [x['path'] for ok, x in results if ok]
            print(image_paths)
            if not image_paths:
                raise DropItem('Image Downloaded Failed')
            return item

        def get_media_requests(self, item, info):
            yield Request(item['url'])

上述代码解释:

.. code:: python

    # 存储至MongoDB实现
    open_spider(self,spider)
    表示当spider被开启的时候调用这个方法
    close_spider(self,spider)
    当spider关掉时这个方法被调用
    from_crawler(cls,crawler)
    必须设置为类方法@classmethod
    # 下载至本地实现
    file_path(self, request, response=None, info=None)
    根据request的url得到图片的原始xx.jpg(即获得图片名)
    get_media_requests(self,item, info)：
    ImagePipeline根据image_urls中指定的url进行爬取，
    可以通过get_media_requests为每个url生成一个Request
    item_completed(self, results, item, info)：
    图片下载完毕后，处理结果会以二元组的方式返回给item_completed()函数。这个二元组定义如下：
    (success, image_info_or_failure)
    其中，第一个元素表示图片是否下载成功；第二个元素是一个字典
    image_paths = [x['path'] for ok, x in results if ok] #打印图片path,比如xx.jpg
    等价于
    for ok, x in results:
        if ok:
            image_paths = [x['path']]

5.json知识
----------

.. code:: python

    a={
        'asd':'12',
        'as':'4',
        'asd12':'s12',
        'list': [{'a':'12'},{'b':'123'}]
    }
    import json
    print(type(a))
    a = json.dumps(a)
    print(a)
    print(type(a))
    a = json.loads(a)
    print(a)
    print(type(a))
    a = a.get('list')
    print(a)

6.参考资料
----------

`Scrapy
实战之爬取妹子图 <https://mp.weixin.qq.com/s/zbeG3JNpexe5i4bT-yXgwA>`__

`Python爬虫从入门到放弃（十六）之 Scrapy框架中Item
Pipeline用法 <https://www.cnblogs.com/zhaof/p/7196197.html?utm_source=itdadao&utm_medium=referral>`__

`Scrapy框架之利用ImagesPipeline下载图片 <https://www.jianshu.com/p/c12d2ac7d55f>`__
