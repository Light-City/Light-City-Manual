.. figure:: http://p20tr36iw.bkt.clouddn.com/matlibplot_bar.png
   :alt: 

.. raw:: html

   <!--more-->

matlibplot绘制条形图
====================

1.内容
------

-  ``numpy.ndarray``
-  ``numpy.random.uniform``
-  ``zip``
-  ``bar``\ 绘制

2.详情
------

2.1 ``np.arange()``
-------------------

.. code:: python

    np.arange()返回的是numpy.ndarray()
    三个参数(first,last,step) last必须提供,默认first从0开始,step为1,
    生成的ndarray(),只包含first,不包含last
    np.arange(3.0)
    Out[6]: array([0., 1., 2.])
    np.arange(1,3.0,.5)
    Out[7]: array([1. , 1.5, 2. , 2.5])

2.2 ``numpy.random.uniform()``
------------------------------

.. code:: python

    函数原型：numpy.random.uniform(low,high,size)
    功能：从一个均匀分布[low,high)中随机采样，注意定义域是左闭右开，即包含low，不包含high.
    参数介绍:  
        low: 采样下界，float类型，默认值为0；
        high: 采样上界，float类型，默认值为1；
        size: 输出样本数目，为int或元组(tuple)类型，例如，size=(m,n,k), 则输出m*n*k个样本，缺省时输出1个值。
    返回值：ndarray类型，其形状和参数size中描述一致。
    这里顺便说下ndarray类型，表示一个N维数组对象，其有一个shape（表维度大小）和dtype（说明数组数据类型的对象），
    使用zeros和ones函数可以创建数据全0或全1的数组，原型：
    numpy.ones(shape,dtype=None,order='C'),
    其中，shape表数组形状（m*n）,dtype表类型,order表是以C还是fortran形式存放数据。

2.3 ``zip()``
-------------

.. code:: python

    zip() 函数用于将可迭代的对象作为参数，将对象中对应的元素打包成一个个元组，然后返回由这些元组组成的列表。
    如果各个迭代器的元素个数不一致，则返回列表长度与最短的对象相同，利用 * 号操作符，可以将元组解压为列表。
    zip([iterable, ...])

    iterabl -- 一个或多个迭代器;
    返回值
    返回元组列表.

    >>>a = [1,2,3]
    >>> b = [4,5,6]
    >>> c = [4,5,6,7,8]
    >>> zipped = zip(a,b)     # 打包为元组的列表
    [(1, 4), (2, 5), (3, 6)]
    >>> zip(a,c)              # 元素个数与最短的列表一致
    [(1, 4), (2, 5), (3, 6)]
    >>> zip(*zipped)          # 与 zip 相反，可理解为解压，返回二维矩阵式
    [(1, 2, 3), (4, 5, 6)]

2.4 bar绘制
-----------

.. code:: python

    import matplotlib.pyplot as plt
    import numpy as np

    # 生成n个柱状图
    n = 12

    X = np.arange(n) # [0,12)范围步长为1的array
    '''
    print(X)
    print(X/float(n))
    print(1-X/float(n))
    '''
    Y1 = (1-X/float(n)) * np.random.uniform(0.5, 1.0, n)
    Y2 = (1-X/float(n)) * np.random.uniform(0.5, 1.0, n)

    plt.bar(X, +Y1, facecolor='#9999ff', edgecolor='white')
    plt.bar(X, -Y2, facecolor='#ff9999', edgecolor='white')
    for x,y in zip(X,Y1):
        # ha: horizontal alignment
        # va: vertical alignment
        plt.text(x, y, '%.2f' % y, ha='center', va='bottom')

    for x,y in zip(X,Y2):
        # ha: horizontal alignment
        # va: vertical alignment
        plt.text(x, -y, '%.2f' % y, ha='center', va='top')

    plt.xlim(-1,n)
    plt.xticks(())
    plt.ylim(-1.25, 1.25)
    plt.yticks(())
    plt.show()

