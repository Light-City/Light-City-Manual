.. figure:: http://p20tr36iw.bkt.clouddn.com/py_scrapy_cqhouse.png
   :alt: 

.. raw:: html

   <!--more-->

Scrapy框架之爬取城市房价
========================

1.创建项目
----------

-  创建项目

.. code:: python

    scrapy startproject cqhouse

.. code:: python

    scrapy genspider cqHouseSpider cq.newhouse.fang.com
    # 修改name = 'CQSpider'

-  运行项目

.. code:: python

    scrapy crawl CQSpider

2.项目架构
----------

-  说在前面

本次项目以爬取重庆市大学城、渝中区、江北区房价为例，\ `数据来源戳这里! <cq.newhouse.fang.com>`__

-  项目架构

采用scrapy进行爬取，并将数据存储至MongoDB及Mysql。

-  具体代码

``settings.py``

.. code:: python

    # 开启USER_AGENT
    USER_AGENT = 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/68.0.3440.106 Safari/537.36'
    # 关闭机器人
    ROBOTSTXT_OBEY = False
    # Pipelines定义
    ITEM_PIPELINES = {
       'cqhouse.pipelines.MongoPipeline': 300,
       'cqhouse.pipelines.MysqlPipeline': 301,
    }
    # MongoDB设置
    MONGO_URI = 'localhost'
    MONGO_DB = 'test'

``items.py``

-  数据定义

.. code:: python

    class CqhouseItem(scrapy.Item):
        # define the fields for your item here like:
        # name = scrapy.Field()
        collection = 'cqhouse' # MongoDB设置
        lpname = scrapy.Field()
        address = scrapy.Field()
        price = scrapy.Field()

``cqHpuseSpider.py``

.. code:: python

    # -*- coding: utf-8 -*-
    import re
    import scrapy
    from cqhouse import items
    class CqhosespiderSpider(scrapy.Spider):
        name = 'CQSpider'
        allowed_domains = ['cq.newhouse.fang.com']
        start_urls = [
                   'http://cq.newhouse.fang.com/house/s/yuzhong/?ctm=1.cq.xf_search.lpsearch_area.2',
                   'http://cq.newhouse.fang.com/house/s/jiangbei/?ctm=1.cq.xf_search.lpsearch_area.3',
                   'http://cq.newhouse.fang.com/house/s/jiangbei/b92/?ctm=1.cq.xf_search.page.2',
                   'http://cq.newhouse.fang.com/house/s/jiangbei/b93/?ctm=1.cq.xf_search.page.4',]
        '''
        http://cq.newhouse.fang.com/house/s/daxuecheng/?ctm=1.cq.xf_search.lpsearch_area.15'
        大学城的爬取有点特殊,因为第一个房源没有定义价格，得自己加上,不然存储数据到数据库就报错了。
        price.insert(0,'价格待定')
        '''
        def parse(self, response):
            f = open('./wr.txt','w',encoding='utf8')
            f.write(response.text)
            # print(response.text)
            raw_lpname = response.xpath('//div[@class="nhouse_list"]/div[@id="newhouse_loupai_list"]/ul/li/div/div[@class="nlc_details"]/div[@class="house_value clearfix"]/div[@class="nlcd_name"]/a/text()').extract()
            lpname = self.parseBlank(raw_lpname)
            print(lpname)
            print(len(lpname))

            address_data = response.xpath('//div[@class="relative_message clearfix"]/div[@class="address"]')
            raw_address = address_data.xpath('string(.)').extract()
            address = self.parseBlank(raw_address)
            print(address)
            print(len(address))

            price_data = response.xpath('//div[@class="nlc_details"]/div[@class="nhouse_price"]')
            raw_price = price_data.xpath('string(.)').extract()
            price = self.parseBlank(raw_price)
            # price.insert(0,'价格待定')  # 爬起大学城时添加!
            print(price)
            print(len(price))
            item = items.CqhouseItem()
            item['lpname'] = lpname
            item['address'] = address
            item['price'] = price
            yield item
        def parseBlank(self,ls):
            lp = []
            for each in ls:
                name = re.sub(r'\s+', '', each)
                lp.append(name)
            return lp

``pipelines.py``

.. code:: python

    # -*- coding: utf-8 -*-

    # Define your item pipelines here
    #
    # Don't forget to add your pipeline to the ITEM_PIPELINES setting
    # See: https://doc.scrapy.org/en/latest/topics/item-pipeline.html
    import pymongo
    import pymysql


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

    class MysqlPipeline(object):
        def process_item(self, item, spider):
            '''
            将爬取的信息保存到mysql
            '''

            connection = pymysql.connect(host='localhost', user='root', password='xxxx', db='scrapydb',
                                         charset='utf8mb4')
            try:

                with connection.cursor() as cursor:
                    for i in range(len(item['lpname'])):
                        sql = "insert into `cqhouse`(`lpname`,`address`,`price`)values(%s,%s,%s)"
                        cursor.execute(sql, (
                            item['lpname'][i], item['address'][i], item['price'][i]))

                        connection.commit()
            # except pymysql.err.IntegrityError as e:
            #     print('重复数据，勿再次插入!')
            finally:
                connection.close()
            return item

3.项目地址
----------

`戳这里!!! <https://github.com/Light-City/cqhouse>`__
