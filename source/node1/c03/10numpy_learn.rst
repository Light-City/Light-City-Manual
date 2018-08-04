.. figure:: http://p20tr36iw.bkt.clouddn.com/numpy_learn.jpg
   :alt: 

Python科学计算Numpy导包及定义数组
==================================

.. code:: python

    import numpy as np
    A=np.array([1,1,1])
    B=np.array([2,2,2])

垂直合并
-----------------

.. code:: python

    C=np.vstack((A,B))

水平合并
-----------------

.. code:: python

    '''
    [[1 1 1]
     [2 2 2]]
    '''
    D=np.hstack((A,B))
    print(C,D) # [1 1 1 2 2 2]
    # (3,) (3,) (2, 3) (6,)
    # A、B仅仅是一个拥有3项元素的数组（数列）
    # C是一个2行3列的矩阵
    # D是一个拥有4项元素的数组（数列）
    print(A.shape,B.shape,C.shape,D.shape)

增加维度
-----------------

.. code:: python

    # 前面加了一个维度变成(1,3)
    print(A[np.newaxis,:].shape)
    # 后面加了一个维度变成(3,1)
    print(A[:,np.newaxis].shape)

利用维度对矩阵做转置
----------------------
.. code:: python

    '''
    [[1]
     [1]
     [1]]
    '''
    # 将A变为纵向(即类似转置)
    print(A[:,np.newaxis])

.. code:: python

    A=np.array([1,1,1])[:,np.newaxis]
    B=np.array([2,2,2])[:,np.newaxis]
    # 垂直合并
    C=np.vstack((A,B))
    '''
    # 水平合并
    [[1 2]
     [1 2]
     [1 2]]
    (3, 1) (3, 2)
    '''
    D=np.hstack((A,B))
    print(D)
    print(A.shape,D.shape)

合并操作
-----------------

需要针对多个矩阵或序列时，可借助concatenate函数,axis=0上下合并，axis=1水平合并

.. code:: python

    '''
    [[1 2 2 1]
     [1 2 2 1]
     [1 2 2 1]]
    '''
    C=np.concatenate((A,B,B,A),axis=1)
    print(C)

参考文章
-----------------

`Numpy array
合并 <https://morvanzhou.github.io/tutorials/data-manipulation/np-pd/2-6-np-concat/>`_
