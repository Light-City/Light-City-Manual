.. figure:: http://p20tr36iw.bkt.clouddn.com/jieba.png
   :alt: 

第六节:Python实现jieba分词
============================

jieba分词初识


1.结巴分词三种模式
------------------

默认模式

.. code:: python

    s_list=jieba.cut("我来到北京清华大学",cut_all=True) # 默认为False
    print("[全模式]:"+'/'.join(s_list))

精确模式

.. code:: python

    s_list=jieba.cut("我来到北京清华大学",) # 默认是精确模式cut_all=False
    print("[默认模式]:"+'/'.join(s_list))

搜索引擎模式

.. code:: python

    s_list=jieba.cut_for_search("小米毕业于中科院计算所，后来在日本东京大学留学深造") # 搜索引擎模式
    print("[默认模式]:"+'/'.join(s_list))

以上结果

.. code:: python

    [全模式]:我/来到/北京/清华/清华大学/华大/大学
    [默认模式]:我/来到/北京/清华大学
    [默认模式]:小米/毕业/于/中科/计算/中科院/计算所/中科院计算所/，/后来/在/日本/东京/大学/日本东京大学/留学/深造

2.自定义字典
------------

jieba默认分词

.. code:: python

    test_string=("医疗卫生事业是强国安民的光荣事业，是为实现中国梦奠定基础的伟大事业。")
    words=jieba.cut(test_string)
    print('jieba默认分词效果')
    print('/'.join(words))

加载自定义字典

.. code:: python

    # 自定义字典 test_string.txt
    光荣事业 4 nz
    中国梦 4 nl
    奠定基础 4 nz
    # 自定义字典实现
    jieba.load_userdict('test_string.txt')
    words=jieba.cut(test_string)
    print("加载自定义字典后，分词效果")
    print('/'.join(words))

以上结果

.. code:: python

    jieba默认分词效果
    医疗卫生/事业/是/强国/安民/的/光荣/事业/，/是/为/实现/中国/梦/奠定/基础/的/伟大事业/。
    加载自定义字典后，分词效果
    医疗卫生/事业/是/强国/安民/的/光荣事业/，/是/为/实现/中国/梦/奠定基础/的/伟大事业/。

3.动态修改字典
--------------

.. code:: python

    t=jieba.suggest_freq(('医疗','卫生'),True)
    print(t)
    print('/'.join(jieba.cut(test_string, HMM=False)))
    t=jieba.suggest_freq(('中国梦'),True)
    print(t)
    print('/'.join(jieba.cut(test_string, HMM=False)))

以上结果

.. code:: python

    0
    医疗/卫生事业/是/强国/安民/的/光荣事业/，/是/为/实现/中国/梦/奠定基础/的/伟大事业/。
    9
    医疗/卫生事业/是/强国/安民/的/光荣事业/，/是/为/实现/中国梦/奠定基础/的/伟大事业/。

4.词性标注及关键字提取
----------------------

.. code:: python

    import jieba.posseg as pseg
    words=pseg.cut(test_string)
    for word, flag in words:
        print('%s %s' % (word, flag))

以上结果

.. code:: python

    医疗 n
    卫生事业 n
    是 v
    强国 n
    安民 nr
    的 uj
    光荣事业 nz
    ， x
    是 v
    为 p
    实现 v
    中国梦 nl
    奠定基础 nz
    的 uj
    伟大事业 nz
    。 x

5.问题及解决办法
----------------

在加载自定义字典时，自定义字典文件内容及有问题代码单独存放文件如下：

.. code:: html


    ----------自定义字典文件test_string.txt--------
    光荣事业 4 nz
    中国梦 4 nl
    奠定基础 4 nz
    ----------issue.py----------
    import jieba
    test_string=("医疗卫生事业是强国安民的光荣事业，是为实现中国梦奠定基础的伟大事业。")
    words=jieba.cut(test_string)
    print('jieba默认分词效果')
    print('/'.join(words))
    # 加载自定义字典
    jieba.load_userdict('test_string.txt')
    words=jieba.cut(test_string)
    print("加载自定义字典后，分词效果")
    print('/'.join(words))
    ----------output----------
    医疗卫生/事业/是/强国/安民/的/光荣/事业/，/是/为/实现/中国/梦/奠定/基础/的/伟大事业/。
    加载自定义字典后，分词效果
    医疗卫生/事业/是/强国/安民/的/光荣事业/，/是/为/实现/中国/梦/奠定基础/的/伟大事业/。

观察以上output会发现，光荣事业跟奠定基础按照了字典文件进行了合并，但是中国梦并没有，这是什么问题呢？

.. code:: html

    于是得出以下结论：
    jieba 分词自定义词典只对长词起作用
    对如果定义的词比jieba自己分的短，则没有用

那如何解决呢？

.. code:: python

    直接改变主字典路径(0.28 之前的版本是不能指定主词典的路径的，有了延迟加载机制后，你可以改变主词典的路径)，不用jieba默认的：
    将issue.py中的jieba.load_userdict('test_string.txt')
    替换为jieba.set_dictionary('test_string.txt')
    此时输出：
    jieba默认分词效果
    医疗卫生/事业/是/强国/安民/的/光荣/事业/，/是/为/实现/中国/梦/奠定/基础/的/伟大事业/。
    加载自定义字典后，分词效果
    医疗/卫生/事业/是/强国安民/的/光荣事业/，/是/为/实现/中国梦/奠定基础/的/伟大事业/。

6.参考资料
----------

`jieba
分词自定义词典问题 <https://blog.csdn.net/u013378306/article/details/64126358>`__

`通过用户自定义词典来增强歧义纠错能力 <https://github.com/fxsjy/jieba/issues/14>`__

7.项目地址
----------

`jieba分词初识 <https://github.com/Light-City/jieba_test>`__
