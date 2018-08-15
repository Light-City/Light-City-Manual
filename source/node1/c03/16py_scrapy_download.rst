.. figure:: http://p20tr36iw.bkt.clouddn.com/py_req_download.png
   :alt: 

.. raw:: html

   <!--more-->

python爬虫系列之下载文件
========================

0.导包
------

.. code:: python

    from urllib.request import urlretrieve
    import os
    os.makedirs('./img/',exist_ok=True)

1.使用urlretrieve下载文件
-------------------------

.. code:: python

    # 使用urlretrieve
    IMAGE_URL = 'https://morvanzhou.github.io/static/img/description/learning_step_flowchart.png'
    urlretrieve(IMAGE_URL,'./img/image1.png')

2.使用requests下载文件
----------------------

.. code:: python

    # 使用requests
    import requests
    r = requests.get(IMAGE_URL)
    with open('./img/image2.png','wb') as f:
        f.write(r.content)
    with open('./img/image3.png','wb') as f:
        for chunk in r.iter_content(chunk_size=32):
            f.write(chunk)

3.上述两种下载方式对比
----------------------

-  urlretrieve: 小文件
-  requests：小或大文件

如果要下载的是大文件, 比如视频等. requests 能让你下一点, 保存一点,
而不是要全部下载完才能保存去另外的地方。 这就是一个 chunk 一个 chunk
的下载. 使用r.iter\_content(chunk\_size) 来控制每个 chunk 的大小,
然后在文件中写入这个 chunk 大小的数据。
