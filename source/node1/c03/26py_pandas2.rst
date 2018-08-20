.. figure:: http://p20tr36iw.bkt.clouddn.com/py_pandas.png
   :alt: 

.. raw:: html

   <!--more-->

数据处理之Pandas(二)
====================

1.\ ``Pandas``\ 合并\ ``concat``
--------------------------------

.. code:: python

    import pandas as pd
    import numpy as np

    # 定义资料集
    df1 = pd.DataFrame(np.ones((3,4))*0, columns=['a','b','c','d'])
    df2 = pd.DataFrame(np.ones((3,4))*1, columns=['a','b','c','d'])
    df3 = pd.DataFrame(np.ones((3,4))*2, columns=['a','b','c','d'])
    print(df1)
    '''
         a    b    c    d
    0  0.0  0.0  0.0  0.0
    1  0.0  0.0  0.0  0.0
    2  0.0  0.0  0.0  0.0
    '''
    print(df2)
    '''
         a    b    c    d
    0  1.0  1.0  1.0  1.0
    1  1.0  1.0  1.0  1.0
    2  1.0  1.0  1.0  1.0
    '''
    print(df3)
    '''
         a    b    c    d
    0  2.0  2.0  2.0  2.0
    1  2.0  2.0  2.0  2.0
    2  2.0  2.0  2.0  2.0
    '''
    # concat纵向合并
    res = pd.concat([df1,df2,df3],axis=0)

    # 打印结果
    print(res)
    '''
         a    b    c    d
    0  0.0  0.0  0.0  0.0
    1  0.0  0.0  0.0  0.0
    2  0.0  0.0  0.0  0.0
    0  1.0  1.0  1.0  1.0
    1  1.0  1.0  1.0  1.0
    2  1.0  1.0  1.0  1.0
    0  2.0  2.0  2.0  2.0
    1  2.0  2.0  2.0  2.0
    2  2.0  2.0  2.0  2.0
    '''
    # 上述合并过程中，index重复，下面给出重置index方法
    # 只需要将index_ignore设定为True即可
    res = pd.concat([df1,df2,df3],axis=0,ignore_index=True)

    # 打印结果
    print(res)
    '''
         a    b    c    d
    0  0.0  0.0  0.0  0.0
    1  0.0  0.0  0.0  0.0
    2  0.0  0.0  0.0  0.0
    3  1.0  1.0  1.0  1.0
    4  1.0  1.0  1.0  1.0
    5  1.0  1.0  1.0  1.0
    6  2.0  2.0  2.0  2.0
    7  2.0  2.0  2.0  2.0
    8  2.0  2.0  2.0  2.0
    '''
    # join 合并方式
    #定义资料集
    df1 = pd.DataFrame(np.ones((3,4))*0, columns=['a','b','c','d'], index=[1,2,3])
    df2 = pd.DataFrame(np.ones((3,4))*1, columns=['b','c','d','e'], index=[2,3,4])
    print(df1)
    print(df2)
    '''
    join='outer',函数默认为join='outer'。此方法是依照column来做纵向合并，有相同的column上下合并在一起，
    其他独自的column各自成列，原来没有值的位置皆为NaN填充。
    '''
    # 纵向"外"合并df1与df2
    res = pd.concat([df1,df2],axis=0,join='outer')

    print(res)

    '''
         a    b    c    d    e
    1  0.0  0.0  0.0  0.0  NaN
    2  0.0  0.0  0.0  0.0  NaN
    3  0.0  0.0  0.0  0.0  NaN
    2  NaN  1.0  1.0  1.0  1.0
    3  NaN  1.0  1.0  1.0  1.0
    4  NaN  1.0  1.0  1.0  1.0
    '''
    # 修改index
    res = pd.concat([df1,df2],axis=0,join='outer',ignore_index=True)

    print(res)
    '''
         a    b    c    d    e
    0  0.0  0.0  0.0  0.0  NaN
    1  0.0  0.0  0.0  0.0  NaN
    2  0.0  0.0  0.0  0.0  NaN
    3  NaN  1.0  1.0  1.0  1.0
    4  NaN  1.0  1.0  1.0  1.0
    5  NaN  1.0  1.0  1.0  1.0
    '''
    # join='inner'合并相同的字段
    # 纵向"内"合并df1与df2
    res = pd.concat([df1,df2],axis=0,join='inner')
    # 打印结果
    print(res)
    '''
         b    c    d
    1  0.0  0.0  0.0
    2  0.0  0.0  0.0
    3  0.0  0.0  0.0
    2  1.0  1.0  1.0
    3  1.0  1.0  1.0
    4  1.0  1.0  1.0
    '''
    # join_axes(依照axes合并)
    #定义资料集
    df1 = pd.DataFrame(np.ones((3,4))*0, columns=['a','b','c','d'], index=[1,2,3])
    df2 = pd.DataFrame(np.ones((3,4))*1, columns=['b','c','d','e'], index=[2,3,4])
    print(df1)
    '''
         a    b    c    d
    1  0.0  0.0  0.0  0.0
    2  0.0  0.0  0.0  0.0
    3  0.0  0.0  0.0  0.0
    '''
    print(df2)
    '''
         b    c    d    e
    2  1.0  1.0  1.0  1.0
    3  1.0  1.0  1.0  1.0
    4  1.0  1.0  1.0  1.0
    '''
    # 依照df1.index进行横向合并
    res = pd.concat([df1,df2],axis=1,join_axes=[df1.index])
    print(res)
    '''
         a    b    c    d    b    c    d    e
    1  0.0  0.0  0.0  0.0  NaN  NaN  NaN  NaN
    2  0.0  0.0  0.0  0.0  1.0  1.0  1.0  1.0
    3  0.0  0.0  0.0  0.0  1.0  1.0  1.0  1.0
    '''
    # 移除join_axes参数,打印结果
    res = pd.concat([df1,df2],axis=1)
    print(res)
    '''
         a    b    c    d    b    c    d    e
    1  0.0  0.0  0.0  0.0  NaN  NaN  NaN  NaN
    2  0.0  0.0  0.0  0.0  1.0  1.0  1.0  1.0
    3  0.0  0.0  0.0  0.0  1.0  1.0  1.0  1.0
    '''
    # append(添加数据)
    # append只有纵向合并，没有横向合并
    #定义资料集
    df1 = pd.DataFrame(np.ones((3,4))*0, columns=['a','b','c','d'])
    df2 = pd.DataFrame(np.ones((3,4))*1, columns=['a','b','c','d'])
    df3 = pd.DataFrame(np.ones((3,4))*2, columns=['a','b','c','d'])
    s1 = pd.Series([1,2,3,4], index=['a','b','c','d'])
    # 将df2合并到df1下面,以及重置index,并打印出结果
    res = df1.append(df2,ignore_index=True)
    print(res)
    '''
         a    b    c    d
    0  0.0  0.0  0.0  0.0
    1  0.0  0.0  0.0  0.0
    2  0.0  0.0  0.0  0.0
    3  1.0  1.0  1.0  1.0
    4  1.0  1.0  1.0  1.0
    5  1.0  1.0  1.0  1.0
    '''
    # 合并多个df,将df2与df3合并至df1的下面,以及重置index,并打印出结果
    res = df1.append([df2,df3], ignore_index=True)
    print(res)
    '''
         a    b    c    d
    0  0.0  0.0  0.0  0.0
    1  0.0  0.0  0.0  0.0
    2  0.0  0.0  0.0  0.0
    3  1.0  1.0  1.0  1.0
    4  1.0  1.0  1.0  1.0
    5  1.0  1.0  1.0  1.0
    6  2.0  2.0  2.0  2.0
    7  2.0  2.0  2.0  2.0
    8  2.0  2.0  2.0  2.0
    '''
    # 合并series,将s1合并至df1，以及重置index，并打印结果
    res = df1.append(s1,ignore_index=True)
    print(res)
    '''
         a    b    c    d
    0  0.0  0.0  0.0  0.0
    1  0.0  0.0  0.0  0.0
    2  0.0  0.0  0.0  0.0
    3  1.0  2.0  3.0  4.0
    '''
    # 总结:两种常用合并方式
    res = pd.concat([df1, df2, df3], axis=0, ignore_index=True)
    res1 = df1.append([df2, df3], ignore_index=True)
    print(res)
    print(res1)

2.Pandas 合并 merge
-------------------

2.1 定义资料集并打印出
~~~~~~~~~~~~~~~~~~~~~~

.. code:: python

    import pandas as pd
    # 依据一组key合并
    # 定义资料集并打印出
    left = pd.DataFrame({'key' : ['K0','K1','K2','K3'],
                         'A' : ['A0','A1','A2','A3'],
                         'B' : ['B0','B1','B2','B3']})

    right = pd.DataFrame({'key': ['K0', 'K1', 'K2', 'K3'],
                          'C' : ['C0', 'C1', 'C2', 'C3'],
                          'D' : ['D0', 'D1', 'D2', 'D3']})
    print(left)
    '''
        A   B key
    0  A0  B0  K0
    1  A1  B1  K1
    2  A2  B2  K2
    3  A3  B3  K3
    '''
    print(right)
    '''
        C   D key
    0  C0  D0  K0
    1  C1  D1  K1
    2  C2  D2  K2
    3  C3  D3  K3
    '''

2.2 依据key column合并,并打印
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: python

    # 依据key column合并,并打印
    res = pd.merge(left,right,on='key')
    print(res)
    '''
        A   B key   C   D
    0  A0  B0  K0  C0  D0
    1  A1  B1  K1  C1  D1
    2  A2  B2  K2  C2  D2
    3  A3  B3  K3  C3  D3
    '''
    # 依据两组key合并
    #定义资料集并打印出
    left = pd.DataFrame({'key1': ['K0', 'K0', 'K1', 'K2'],
                          'key2': ['K0', 'K1', 'K0', 'K1'],
                          'A': ['A0', 'A1', 'A2', 'A3'],
                          'B': ['B0', 'B1', 'B2', 'B3']})
    right = pd.DataFrame({'key1': ['K0', 'K1', 'K1', 'K2'],
                           'key2': ['K0', 'K0', 'K0', 'K0'],
                           'C': ['C0', 'C1', 'C2', 'C3'],
                           'D': ['D0', 'D1', 'D2', 'D3']})
    print(left)
    '''
        A   B key1 key2
    0  A0  B0   K0   K0
    1  A1  B1   K0   K1
    2  A2  B2   K1   K0
    3  A3  B3   K2   K1
    '''
    print(right)
    '''
        C   D key1 key2
    0  C0  D0   K0   K0
    1  C1  D1   K1   K0
    2  C2  D2   K1   K0
    3  C3  D3   K2   K0
    '''

2.3 依据key1与key2 columns进行合并
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: python

    # 依据key1与key2 columns进行合并，并打印出四种结果['left', 'right', 'outer', 'inner']
    res = pd.merge(left, right, on=['key1', 'key2'], how='inner')
    print(res)
    res = pd.merge(left, right, on=['key1', 'key2'], how='outer')
    print(res)
    res = pd.merge(left, right, on=['key1', 'key2'], how='left')
    print(res)
    res = pd.merge(left, right, on=['key1', 'key2'], how='right')
    print(res)
    '''
    ---------------inner方式---------------
        A   B key1 key2   C   D
    0  A0  B0   K0   K0  C0  D0
    1  A2  B2   K1   K0  C1  D1
    2  A2  B2   K1   K0  C2  D2
    ---------------outer方式---------------
         A    B key1 key2    C    D
    0   A0   B0   K0   K0   C0   D0
    1   A1   B1   K0   K1  NaN  NaN
    2   A2   B2   K1   K0   C1   D1
    3   A2   B2   K1   K0   C2   D2
    4   A3   B3   K2   K1  NaN  NaN
    5  NaN  NaN   K2   K0   C3   D3
    ---------------left方式---------------
        A   B key1 key2    C    D
    0  A0  B0   K0   K0   C0   D0
    1  A1  B1   K0   K1  NaN  NaN
    2  A2  B2   K1   K0   C1   D1
    3  A2  B2   K1   K0   C2   D2
    4  A3  B3   K2   K1  NaN  NaN
    --------------right方式---------------
         A    B key1 key2   C   D
    0   A0   B0   K0   K0  C0  D0
    1   A2   B2   K1   K0  C1  D1
    2   A2   B2   K1   K0  C2  D2
    3  NaN  NaN   K2   K0  C3  D3
    '''

2.4 Indicator设置合并列名称
~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: python

    # Indicator
    df1 = pd.DataFrame({'col1':[0,1],'col_left':['a','b']})
    df2 = pd.DataFrame({'col1':[1,2,2],'col_right':[2,2,2]})
    print(df1)
    '''
       col1 col_left
    0     0        a
    1     1        b
    '''
    print(df2)
    '''
       col1  col_right
    0     1          2
    1     2          2
    2     2          2
    '''

    # 依据col1进行合并,并启用indicator=True,最后打印
    res = pd.merge(df1,df2,on='col1',how='outer',indicator=True)
    print(res)
    '''
       col1 col_left  col_right      _merge
    0     0        a        NaN   left_only
    1     1        b        2.0        both
    2     2      NaN        2.0  right_only
    3     2      NaN        2.0  right_only
    '''
    # 自定义indicator column的名称,并打印出
    res = pd.merge(df1,df2,on='col1',how='outer',indicator='indicator_column')
    print(res)
    '''
       col1 col_left  col_right indicator_column
    0     0        a        NaN        left_only
    1     1        b        2.0             both
    2     2      NaN        2.0       right_only
    3     2      NaN        2.0       right_only
    '''

2.5 依据index合并
~~~~~~~~~~~~~~~~~

.. code:: python

    # 依据index合并
    #定义资料集并打印出
    left = pd.DataFrame({'A': ['A0', 'A1', 'A2'],
                         'B': ['B0', 'B1', 'B2']},
                         index=['K0', 'K1', 'K2'])
    right = pd.DataFrame({'C': ['C0', 'C2', 'C3'],
                          'D': ['D0', 'D2', 'D3']},
                         index=['K0', 'K2', 'K3'])
    print(left)
    '''
         A   B
    K0  A0  B0
    K1  A1  B1
    K2  A2  B2
    '''
    print(right)
    '''
         C   D
    K0  C0  D0
    K2  C2  D2
    K3  C3  D3
    '''
    # 依据左右资料集的index进行合并,how='outer',并打印
    res = pd.merge(left,right,left_index=True,right_index=True,how='outer')
    print(res)
    '''
          A    B    C    D
    K0   A0   B0   C0   D0
    K1   A1   B1  NaN  NaN
    K2   A2   B2   C2   D2
    K3  NaN  NaN   C3   D3
    '''
    # 依据左右资料集的index进行合并,how='inner',并打印
    res = pd.merge(left,right,left_index=True,right_index=True,how='inner')
    print(res)
    '''
         A   B   C   D
    K0  A0  B0  C0  D0
    K2  A2  B2  C2  D2
    '''

2.6 解决overlapping的问题
~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: python

    # 解决overlapping的问题
    #定义资料集
    boys = pd.DataFrame({'k': ['K0', 'K1', 'K2'], 'age': [1, 2, 3]})
    girls = pd.DataFrame({'k': ['K0', 'K0', 'K3'], 'age': [4, 5, 6]})
    print(boys)
    '''
       age   k
    0    1  K0
    1    2  K1
    2    3  K2
    '''
    print(girls)
    '''
       age   k
    0    4  K0
    1    5  K0
    2    6  K3
    '''
    # 使用suffixes解决overlapping的问题
    # 比如将上面两个合并时,age重复了,则可通过suffixes设置,以此保证不重复,不同名
    res = pd.merge(boys,girls,on='k',suffixes=['_boy','_girl'],how='inner')
    print(res)
    '''
       age_boy   k  age_girl
    0        1  K0         4
    1        1  K0         5
    '''

3.Pandas plot出图
-----------------

.. code:: python

    import pandas as pd
    import numpy as np
    import matplotlib.pyplot as plt

    data = pd.Series(np.random.randn(1000), index=np.arange(1000))
    print(data)
    print(data.cumsum())
    # data本来就是一个数据，所以我们可以直接plot
    data.plot()
    plt.show()

.. figure:: http://p20tr36iw.bkt.clouddn.com/pandas_1.png
   :alt: 

.. code:: python

    # np.random.randn(1000,4) 随机生成1000行4列数据
    # list("ABCD")会变为['A','B','C','D']
    data = pd.DataFrame(
        np.random.randn(1000,4),
        index=np.arange(1000),
        columns=list("ABCD")
    )
    data.cumsum()
    data.plot()
    plt.show()

.. figure:: http://p20tr36iw.bkt.clouddn.com/pandas_2.png
   :alt: 

.. code:: python

    ax = data.plot.scatter(x='A',y='B',color='DarkBlue',label='Class1')
    # 将之下这个 data 画在上一个 ax 上面
    data.plot.scatter(x='A',y='C',color='LightGreen',label='Class2',ax=ax)
    plt.show()

.. figure:: http://p20tr36iw.bkt.clouddn.com/pandas_3.png
   :alt: 

4.参考资料
----------

`Pandas
学习 <https://morvanzhou.github.io/tutorials/data-manipulation/np-pd/>`__
