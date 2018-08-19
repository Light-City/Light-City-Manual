.. figure:: http://p20tr36iw.bkt.clouddn.com/py_numpy1.jpg
   :alt: 

.. raw:: html

   <!--more-->

数据处理之Numpy(二)
===================

1.Numpy基础运算
---------------

.. code:: python

    import numpy as np

    A = np.arange(2,14).reshape((3,4))
    print(A)

    # 最小元素索引
    print(np.argmin(A)) # 0
    # 最大元素索引
    print(np.argmax(A)) # 11
    # 求整个矩阵的均值
    print(np.mean(A)) # 7.5
    print(np.average(A)) # 7.5
    print(A.mean()) # 7.5
    # 中位数
    print(np.median(A)) # 7.5
    # 累加
    print(np.cumsum(A))
    # [ 2  5  9 14 20 27 35 44 54 65 77 90]
    # 累差运算
    B = np.array([[3,5,9],
                  [4,8,10]])
    print(np.diff(B))
    '''
    [[2 4]
     [4 2]]
    '''
    C = np.array([[0,5,9],
                  [4,0,10]])
    print(np.nonzero(B))
    print(np.nonzero(C))
    '''
    # 将所有非零元素的行与列坐标分割开，重构成两个分别关于行和列的矩阵
    (array([0, 0, 0, 1, 1, 1], dtype=int64), array([0, 1, 2, 0, 1, 2], dtype=int64))
    (array([0, 0, 1, 1], dtype=int64), array([1, 2, 0, 2], dtype=int64))
    '''
    # 仿照列表排序
    A = np.arange(14,2,-1).reshape((3,4)) # -1表示反向递减一个步长
    print(A)
    '''
    [[14 13 12 11]
     [10  9  8  7]
     [ 6  5  4  3]]
    '''
    print(np.sort(A))
    '''
    # 只是对每行进行递增排序
    [[11 12 13 14]
     [ 7  8  9 10]
     [ 3  4  5  6]]
    '''
    # 矩阵转置
    print(np.transpose(A))
    '''
    [[14 10  6]
     [13  9  5]
     [12  8  4]
     [11  7  3]]
    '''
    print(A.T)
    '''
    [[14 10  6]
     [13  9  5]
     [12  8  4]
     [11  7  3]]
    '''
    print(A)
    print(np.clip(A,5,9))
    '''
    clip(Array,Array_min,Array_max)
    将Array_min<X<Array_max  X表示矩阵A中的数，如果满足上述关系，则原数不变。
    否则，如果X<Array_min，则将矩阵中X变为Array_min;
    如果X>Array_max，则将矩阵中X变为Array_max.
    [[9 9 9 9]
     [9 9 8 7]
     [6 5 5 5]]
    '''

2.Numpy索引
-----------

.. code:: python

    import numpy as np
    A = np.arange(3,15)
    print(A)
    # [ 3  4  5  6  7  8  9 10 11 12 13 14]
    print(A[3])
    # 6
    B = A.reshape(3,4)
    print(B)
    '''
    [[ 3  4  5  6]
     [ 7  8  9 10]
     [11 12 13 14]]
    '''
    print(B[2])
    # [11 12 13 14]
    print(B[0][2])
    # 5
    print(B[0,2])
    # 5
    # list切片操作
    print(B[1,1:3]) # [8 9] 1:3表示1-2不包含3

    for row in B:
        print(row)

    '''
    [3 4 5 6]
    [ 7  8  9 10]
    [11 12 13 14]
    '''
    # 如果要打印列，则进行转置即可
    for column in B.T:
        print(column)
    '''
    [ 3  7 11]
    [ 4  8 12]
    [ 5  9 13]
    [ 6 10 14]
    '''
    # 多维转一维
    A = np.arange(3,15).reshape((3,4))
    # print(A)
    print(A.flatten())
    # flat是一个迭代器，本身是一个object属性
    for item in A.flat:
        print(item)

3.Numpy array合并
-----------------

3.1 数组合并
~~~~~~~~~~~~

.. code:: python

    import numpy as np
    A = np.array([1,1,1])
    B = np.array([2,2,2])
    print(np.vstack((A,B)))
    # vertical stack 上下合并,对括号的两个整体操作。
    '''
    [[1 1 1]
     [2 2 2]]
    '''
    C = np.vstack((A,B))
    print(C)
    print(A.shape,B.shape,C.shape)
    # (3,) (3,) (2, 3)
    # 从shape中看出A,B均为拥有3项的数组(数列)
    # horizontal stack左右合并
    D = np.hstack((A,B))
    print(D)
    # [1 1 1 2 2 2]
    print(A.shape,B.shape,D.shape)
    # (3,) (3,) (6,)
    # 对于A,B这种，为数组或数列，无法进行转置，需要借助其他函数进行转置

3.2 数组转置为矩阵
~~~~~~~~~~~~~~~~~~

.. code:: python

    print(A[np.newaxis,:]) # [1 1 1]变为[[1 1 1]]
    print(A[np.newaxis,:].shape) # (3,)变为(1, 3)
    print(A[:,np.newaxis])
    '''
    [[1]
     [1]
     [1]]
    '''

3.3 多个矩阵合并
~~~~~~~~~~~~~~~~

.. code:: python

    # concatenate的第一个例子
    print("------------")
    print(A[:,np.newaxis].shape) # (3,1)
    A = A[:,np.newaxis] # 数组转为矩阵
    B = B[:,np.newaxis] # 数组转为矩阵
    # axis=0纵向合并
    C = np.concatenate((A,B,B,A),axis=0)
    print(C)
    '''
    [[1]
     [1]
     [1]
     [2]
     [2]
     [2]
     [2]
     [2]
     [2]
     [1]
     [1]
     [1]]
    '''
    # axis=1横向合并
    C = np.concatenate((A,B),axis=1)
    print(C)
    '''
    [[1 2]
     [1 2]
     [1 2]]
    '''

.. code:: python

    # concatenate的第二个例子
    print("-------------")
    a = np.arange(8).reshape(2,4)
    b = np.arange(8).reshape(2,4)
    print(a)
    print(b)
    print("-------------")
    # axis=0多个矩阵纵向合并
    c = np.concatenate((a,b),axis=0)
    print(c)
    # axis=1多个矩阵横向合并
    c = np.concatenate((a,b),axis=1)
    print(c)
    '''
    [[0 1 2 3]
     [4 5 6 7]
     [0 1 2 3]
     [4 5 6 7]]
    [[0 1 2 3 0 1 2 3]
     [4 5 6 7 4 5 6 7]]
    '''

4.Numpy array分割
-----------------

4.1 构造3行4列矩阵
~~~~~~~~~~~~~~~~~~

.. code:: python

    import numpy as np
    A = np.arange(12).reshape((3,4))
    print(A)
    '''
    [[ 0  1  2  3]
     [ 4  5  6  7]
     [ 8  9 10 11]]
    '''

4.2 等量分割
~~~~~~~~~~~~

.. code:: python

    # 等量分割
    # 纵向分割同横向合并的axis
    print(np.split(A, 2, axis=1))
    '''
    [array([[0, 1],
           [4, 5],
           [8, 9]]), array([[ 2,  3],
           [ 6,  7],
           [10, 11]])]
    '''
    # 横向分割同纵向合并的axis
    print(np.split(A,3,axis=0))
    # [array([[0, 1, 2, 3]]), array([[4, 5, 6, 7]]), array([[ 8,  9, 10, 11]])]

4.3 不等量分割
~~~~~~~~~~~~~~

.. code:: python

    print(np.array_split(A,3,axis=1))
    '''
    [array([[0, 1],
           [4, 5],
           [8, 9]]), array([[ 2],
           [ 6],
           [10]]), array([[ 3],
           [ 7],
           [11]])]
    '''

4.4 其他的分割方式
~~~~~~~~~~~~~~~~~~

.. code:: python

    # 横向分割
    print(np.vsplit(A,3)) # 等价于print(np.split(A,3,axis=0))
    # [array([[0, 1, 2, 3]]), array([[4, 5, 6, 7]]), array([[ 8,  9, 10, 11]])]
    # 纵向分割
    print(np.hsplit(A,2)) # 等价于print(np.split(A,2,axis=1))
    '''
    [array([[0, 1],
           [4, 5],
           [8, 9]]), array([[ 2,  3],
           [ 6,  7],
           [10, 11]])]
    '''

5.Numpy copy与 ``=``
--------------------

5.1 ``=``\ 赋值方式会带有关联性
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: python

    import numpy as np
    # `=`赋值方式会带有关联性
    a = np.arange(4)
    print(a) # [0 1 2 3]

    b = a
    c = a
    d = b
    a[0] = 11
    print(a) # [11  1  2  3]
    print(b) # [11  1  2  3]
    print(c) # [11  1  2  3]
    print(d) # [11  1  2  3]
    print(b is a) # True
    print(c is a) # True
    print(d is a) # True

    d[1:3] = [22,33]
    print(a) # [11 22 33  3]
    print(b) # [11 22 33  3]
    print(c) # [11 22 33  3]

5.2 copy()赋值方式没有关联性
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: python

    a = np.arange(4)
    print(a) # [0 1 2 3]
    b =a.copy() # deep copy
    print(b) # [0 1 2 3]
    a[3] = 44
    print(a) # [ 0  1  2 44]
    print(b) # [0 1 2 3]

    # 此时a与b已经没有关联

