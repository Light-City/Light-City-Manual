.. figure:: http://p20tr36iw.bkt.clouddn.com/py_multiprocess.png
   :alt: 

.. raw:: html

   <!--more-->

python爬虫系列之多进程分布式爬虫
================================

说在前面:本节用来解决同时下载多个网页, 同时分析多个网页,
这样就有种事倍功半的效用。本节学习来源于莫烦Python,目标是爬取莫烦Python的所有Url。

1.导包
------

.. code:: python

    import multiprocessing as mp
    import time
    from urllib.request import urlopen, urljoin
    from bs4 import BeautifulSoup
    import re

2.爬取网页及解析网页
--------------------

一个爬取网页的(crawl), 一个是解析网页的(parse)

.. code:: python

    base_url = "https://morvanzhou.github.io/"

    def crawl(url):
        response = urlopen(url)
        # time.sleep(0.1)
        return response.read().decode()

.. code:: python

    def parse(html):
        soup = BeautifulSoup(html, 'lxml')
        '''
        ^ 匹配输入字符串的开始位置
        $ 匹配输入字符串的结尾位置
        . 匹配输入字符串的结尾位置
        + 匹配前面的子表达式一次或多次
        ? 匹配前面的子表达式零次或一次，或指明一个非贪婪限定符
        .+?表示最小匹配
        举例说明.+?与.+的区别
        <a href="xxx"><span>
        如果用<.+>匹配，则匹配结果是
        <a href="xxx"><span>
        如果用<.+?>匹配，则匹配结果是
        <a href="xxx">
        也就是.+?只要匹配就返回了，不会再接着往下找了
        '''
        urls = soup.find_all('a', {'href':re.compile('^/.+?/$')})
        title = soup.find('h1').get_text().strip()
        page_urls = set([urljoin(base_url, url['href']) for url in urls])   # 去重
        url = soup.find('meta', {'property': "og:url"})['content']
        return title, page_urls, url

3.多进程+进程池爬取url
----------------------

注意:多进程操作必须放在\ ``__name__ == '__main__'``\ 里面，否则报错！

.. code:: python

    if __name__ == '__main__':
        unseen = set([base_url,])  # {'http://.............',}
        seen = set()

        pool = mp.Pool(4) # CPU核数为4
        count, t1 = 1, time.time()
        while len(unseen) != 0:  # still get some url to visit
            if len(seen) > 20:
                break
            print('\nDistributed Crawling...')
            crawl_jobs = [pool.apply_async(crawl, args=(url,)) for url in unseen]
            htmls = [j.get() for j in crawl_jobs]    # request connection

            print('\nDistributed Parsing...')
            parse_jobs = [pool.apply_async(parse, args=(html,)) for html in htmls]
            results = [j.get() for j in parse_jobs]                  # parse html

            print('\nAnalysing...')
            seen.update(unseen)         # seen the crawled
            unseen.clear()              # nothing unseen

            for title, page_urls, url in results:
                print(count, title, url)
                count += 1
                unseen.update(page_urls - seen)     # get new url to crawl
        print('Total time: %.1f s' % (time.time()-t1, ))   

4.参考文章
----------

`加速爬虫:
多进程分布式 <https://morvanzhou.github.io/tutorials/data-manipulation/scraping/4-01-distributed-scraping/>`__
