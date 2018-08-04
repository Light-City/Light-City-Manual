.. figure:: http://p20tr36iw.bkt.clouddn.com/tensot_numpy.jpg
   :alt: 

Numpy中维度问题
===============

1.导包
------

.. code:: python

    import numpy as np

2.创建类似于4行2列的矩阵
------------------------

.. code:: python

    # 看作4行2列矩阵
    X = np.array([[0, 1], [2, 3], [4, 5], [6, 7]])

3.取相应列数据\ ``X[:,n]``
--------------------------

.. code:: python

    # X[:,n]
    print(X[:,0]) # [0 2 4 6]

4.取相应行数据 ``X[n,:]``
-------------------------

.. code:: python

    # X[n,:]
    print(X[1,:]) # [2,3]

5.取多列数据
------------

.. code:: python

    # X[:,  m:n]，即取所有数据的第m到n-1列数据，含左不含右
    print(X[:,0:1])
    '''
    [[0]
     [2]
     [4]
     [6]]
    '''

6.取多行数据
------------

.. code:: python

    # X[m:n,:]，即取所有数据的第m到n-1行数据，含左不含右
    print(X[0:2,:])
    '''
    [[0 1]
     [2 3]]
    '''

7.参考文章
----------

    `Python中的X[:,0]和X[:,1] <https://blog.csdn.net/csj664103736/article/details/72828584>`__
