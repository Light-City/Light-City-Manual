.. figure:: http://p20tr36iw.bkt.clouddn.com/py_numpy.jpg
   :alt: 

.. raw:: html

   <!--more-->

数据处理之Numpy(一)
===================

1.Numpy基本操作
---------------

1.1 列表转为矩阵
~~~~~~~~~~~~~~~~

.. code:: python

    import numpy as np
    array = np.array([
        [1,3,5],
        [4,6,9]
    ])

    print(array)

输出：

.. code:: python

    [[1 3 5]
     [4 6 9]]

1.2 维度
~~~~~~~~

.. code:: python

    print('number of dim:', array.ndim)

输出:

.. code:: python

    number of dim: 2

1.3 行数和列数()
~~~~~~~~~~~~~~~~

.. code:: python

    print('shape:',array.shape)

输出:

.. code:: python

    shape: (2, 3)

1.4 元素个数
~~~~~~~~~~~~

.. code:: python

    print('size:',array.size)

输出:

::

    size:6

2.Numpy创建array
----------------

2.1 一维array创建
~~~~~~~~~~~~~~~~~

.. code:: python

    import numpy as np
    # 一维array
    a = np.array([2,23,4], dtype=np.int32) # np.int默认为int32
    print(a)
    print(a.dtype)

输出:

.. code:: python

    [ 2 23  4]
    int32

2.1 多维array创建
~~~~~~~~~~~~~~~~~

.. code:: python

    # 多维array
    a = np.array([[2,3,4],
                  [3,4,5]])
    print(a) # 生成2行3列的矩阵

输出:

.. code:: python

    [[2 3 4]
     [3 4 5]]

2.2 创建全零数组
~~~~~~~~~~~~~~~~

.. code:: python

    a = np.zeros((3,4))
    print(a) # 生成3行4列的全零矩阵

输出:

.. code:: python

    [[0. 0. 0. 0.]
     [0. 0. 0. 0.]
     [0. 0. 0. 0.]]

2.3 创建全一数据
~~~~~~~~~~~~~~~~

.. code:: python

    # 创建全一数据，同时指定数据类型
    a = np.ones((3,4),dtype=np.int)
    print(a)

输出:

::

    [[1 1 1 1]
     [1 1 1 1]
     [1 1 1 1]]

2.4 创建全空数组
~~~~~~~~~~~~~~~~

.. code:: python

    # 创建全空数组，其实每个值都是接近于零的数
    a = np.empty((3,4))
    print(a)

输出:

::

    [[0. 0. 0. 0.]
     [0. 0. 0. 0.]
     [0. 0. 0. 0.]]

2.5 创建连续数组
~~~~~~~~~~~~~~~~

.. code:: python

    # 创建连续数组
    a = np.arange(10,21,2) # 10-20的数据，步长为2
    print(a)

输出:

::

    [10 12 14 16 18 20]

2.6 reshape操作
~~~~~~~~~~~~~~~

.. code:: python

    # 使用reshape改变上述数据的形状
    b = a.reshape((2,3))
    print(b)

输出:

::

    [[10 12 14]
     [16 18 20]]

2.7 创建连续型数据
~~~~~~~~~~~~~~~~~~

.. code:: python

    # 创建线段型数据
    a = np.linspace(1,10,20) # 开始端1，结束端10，且分割成20个数据，生成线段
    print(a)

输出:

::

    [ 1.          1.47368421  1.94736842  2.42105263  2.89473684  3.36842105
      3.84210526  4.31578947  4.78947368  5.26315789  5.73684211  6.21052632
      6.68421053  7.15789474  7.63157895  8.10526316  8.57894737  9.05263158
      9.52631579 10.        ]

2.8 linspace的reshape操作
~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: python

    # 同时也可以reshape
    b = a.reshape((5,4))
    print(b)

输出:

::

    [[ 1.          1.47368421  1.94736842  2.42105263]
     [ 2.89473684  3.36842105  3.84210526  4.31578947]
     [ 4.78947368  5.26315789  5.73684211  6.21052632]
     [ 6.68421053  7.15789474  7.63157895  8.10526316]
     [ 8.57894737  9.05263158  9.52631579 10.        ]]

3.Numpy基本运算
---------------

3.1 一维矩阵运算
~~~~~~~~~~~~~~~~

.. code:: python

    import numpy as np
    # 一维矩阵运算
    a = np.array([10,20,30,40])
    b = np.arange(4)
    print(a,b)
    # [10 20 30 40] [0 1 2 3]
    c = a - b
    print(c)
    # [10 19 28 37]
    print(a*b) # 若用a.dot(b),则为各维之和
    # [  0  20  60 120]
    # 在Numpy中，想要求出矩阵中各个元素的乘方需要依赖双星符号 **，以二次方举例，即：
    c = b**2
    print(c)
    # [0 1 4 9]
    # Numpy中具有很多的数学函数工具
    c = np.sin(a)
    print(c)
    # [-0.54402111  0.91294525 -0.98803162  0.74511316]
    print(b<2)
    # [ True  True False False]
    a = np.array([1,1,4,3])
    b = np.arange(4)
    print(a==b)
    # [False  True False  True]

3.2 多维矩阵运算
~~~~~~~~~~~~~~~~

.. code:: python

    a = np.array([[1,1],[0,1]])
    b = np.arange(4).reshape((2,2))
    print(a)
    '''
    [[1 1]
     [0 1]]
    '''
    print(b)
    '''
    [[0 1]
     [2 3]]
    '''
    # 多维度矩阵乘法
    # 第一种乘法方式:
    c = a.dot(b)
    print(c)
    # 第二种乘法:
    c = np.dot(a,b)
    print(c)
    '''
    [[2 4]
     [2 3]]
    '''
    # 多维矩阵乘法不能直接使用'*'号

    a = np.random.random((2,4))

    print(np.sum(a)) # 3.657010765991042
    print(np.min(a)) # 0.10936760904735132
    print(np.max(a)) # 0.9476048882750654

    print("a=",a)
    '''
    a= [[0.16607436 0.94760489 0.59649117 0.22698245]
     [0.66494464 0.23447984 0.10936761 0.71106581]]
    '''

    print("sum=",np.sum(a,axis=1)) # sum= [1.93715287 1.7198579 ]
    print("min=",np.min(a,axis=0)) # min= [0.16607436 0.23447984 0.10936761 0.22698245]
    print("max=",np.max(a,axis=1)) # max= [0.94760489 0.71106581]

    '''
    如果你需要对行或者列进行查找运算，
    就需要在上述代码中为 axis 进行赋值。
    当axis的值为0的时候，将会以列作为查找单元，
    当axis的值为1的时候，将会以行作为查找单元。
    '''

