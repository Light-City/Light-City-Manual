.. figure:: http://p20tr36iw.bkt.clouddn.com/py_pandas1.jpg
   :alt: 

.. raw:: html

   <!--more-->

数据处理之Pandas(一)
====================

Pandas是基于Numpy构建的，让Numpy为中心的应用变得更加简单。
要使用pandas，首先需要了解他主要两个数据结构：\ **Series和DataFrame**\ 。

1.Series
--------

.. code:: python

    import pandas as pd
    import numpy as np

    # Series
    s = pd.Series([1,3,6,np.nan,44,1])
    print(s)
    '''
    0     1.0
    1     3.0
    2     6.0
    3     NaN
    4    44.0
    5     1.0
    dtype: float64
    '''
    # 默认index从0开始,如果想要按照自己的索引设置，则修改index参数,如:index=[3,4,3,7,8,9]

2.DataFrame
-----------

2.1 DataFrame的简单运用
~~~~~~~~~~~~~~~~~~~~~~~

.. code:: python

    # DataFrame
    dates = pd.date_range('2018-08-19',periods=6)
    # dates = pd.date_range('2018-08-19','2018-08-24') # 起始、结束  与上述等价
    '''
    numpy.random.randn(d0, d1, …, dn)是从标准正态分布中返回一个或多个样本值。
    numpy.random.rand(d0, d1, …, dn)的随机样本位于[0, 1)中。
    (6,4)表示6行4列数据
    '''
    df = pd.DataFrame(np.random.randn(6,4),index=dates,columns=['a','b','c','d'])
    print(df)
    # DataFrame既有行索引也有列索引， 它可以被看做由Series组成的大字典。
    print(df['b'])
    '''
    2018-08-19   -0.217424
    2018-08-20   -1.421058
    2018-08-21   -0.424589
    2018-08-22    0.534675
    2018-08-23   -0.018525
    2018-08-24    0.635075
    Freq: D, Name: b, dtype: float64
    '''
    # 未指定行标签和列标签的数据
    df1 = pd.DataFrame(np.arange(12).reshape(3,4))
    print(df1)
    # 另一种方式
    df2 = pd.DataFrame({
        'A': [1,2,3,4],
        'B': pd.Timestamp('20180819'),
        'C': pd.Series([1,6,9,10],dtype='float32'),
        'D': np.array([3] * 4,dtype='int32'),
        'E': pd.Categorical(['test','train','test','train']),
        'F': 'foo'
    })
    print(df2)
    '''
       A          B     C  D      E    F
    0  1 2018-08-19   1.0  3   test  foo
    1  2 2018-08-19   6.0  3  train  foo
    2  3 2018-08-19   9.0  3   test  foo
    3  4 2018-08-19  10.0  3  train  foo
    '''
    print(df2.dtypes)
    '''
    A             int64
    B    datetime64[ns]
    C           float32
    D             int32
    E          category
    F            object
    dtype: object
    '''
    print(df2.index)
    # RangeIndex(start=0, stop=4, step=1)
    print(df2.columns)
    # Index(['A', 'B', 'C', 'D', 'E', 'F'], dtype='object')
    print(df2.values)
    '''
    [[1 Timestamp('2018-08-19 00:00:00') 1.0 3 'test' 'foo']
     [2 Timestamp('2018-08-19 00:00:00') 6.0 3 'train' 'foo']
     [3 Timestamp('2018-08-19 00:00:00') 9.0 3 'test' 'foo']
     [4 Timestamp('2018-08-19 00:00:00') 10.0 3 'train' 'foo']]
    '''
    # 数据总结
    print(df2.describe())
    '''
                  A          C    D
    count  4.000000   4.000000  4.0
    mean   2.500000   6.500000  3.0
    std    1.290994   4.041452  0.0
    min    1.000000   1.000000  3.0
    25%    1.750000   4.750000  3.0
    50%    2.500000   7.500000  3.0
    75%    3.250000   9.250000  3.0
    max    4.000000  10.000000  3.0
    '''
    # 翻转数据
    print(df2.T)
    # print(np.transpose(df2))等价于上述操作
    '''
    axis=1表示行
    axis=0表示列
    默认ascending(升序)为True
    ascending=True表示升序,ascending=False表示降序
    下面两行分别表示按行升序与按行降序
    '''
    print(df2.sort_index(axis=1,ascending=True))
    print(df2.sort_index(axis=1,ascending=False))
    '''
       A          B     C  D      E    F
    0  1 2018-08-19   1.0  3   test  foo
    1  2 2018-08-19   6.0  3  train  foo
    2  3 2018-08-19   9.0  3   test  foo
    3  4 2018-08-19  10.0  3  train  foo
         F      E  D     C          B  A
    0  foo   test  3   1.0 2018-08-19  1
    1  foo  train  3   6.0 2018-08-19  2
    2  foo   test  3   9.0 2018-08-19  3
    3  foo  train  3  10.0 2018-08-19  4
    '''
    # 表示按列降序与按列升序
    print(df2.sort_index(axis=0,ascending=False))
    print(df2.sort_index(axis=0,ascending=True))
    '''
       A          B     C  D      E    F
    3  4 2018-08-19  10.0  3  train  foo
    2  3 2018-08-19   9.0  3   test  foo
    1  2 2018-08-19   6.0  3  train  foo
    0  1 2018-08-19   1.0  3   test  foo
       A          B     C  D      E    F
    0  1 2018-08-19   1.0  3   test  foo
    1  2 2018-08-19   6.0  3  train  foo
    2  3 2018-08-19   9.0  3   test  foo
    3  4 2018-08-19  10.0  3  train  foo
    '''
    # 对特定列数值排列
    # 表示对C列降序排列
    print(df2.sort_values(by='C',ascending=False))

3.pandas选择数据
----------------

3.1 实战筛选
~~~~~~~~~~~~

.. code:: python

    import pandas as pd
    import numpy as np
    dates = pd.date_range('20180819', periods=6)
    df = pd.DataFrame(np.arange(24).reshape((6,4)),index=dates, columns=['A','B','C','D'])
    print(df)
    # 检索A列
    print(df['A'])
    print(df.A)
    # 选择跨越多行或多列
    # 选取前3行
    print(df[0:3])
    print(df['2018-08-19':'2018-08-21'])
    '''
                A  B   C   D
    2018-08-19  0  1   2   3
    2018-08-20  4  5   6   7
    2018-08-21  8  9  10  11
    '''
    # 根据标签选择数据
    # 获取特定行或列
    # 指定行数据
    print(df.loc['20180819'])
    '''
    A    0
    B    1
    C    2
    D    3
    Name: 2018-08-19 00:00:00, dtype: int32
    '''
    # 指定列
    # 两种方式
    print(df.loc[:,'A':'B'])
    print(df.loc[:,['A','B']])
    '''
                 A   B
    2018-08-19   0   1
    2018-08-20   4   5
    2018-08-21   8   9
    2018-08-22  12  13
    2018-08-23  16  17
    2018-08-24  20  21
    '''
    # 行与列同时检索
    print(df.loc['20180819',['A','B']])
    '''
    A    0
    B    1
    Name: 2018-08-19 00:00:00, dtype: int32
    '''
    # 根据序列iloc
    # 获取特定位置的值
    print(df.iloc[3,1])
    print(df.iloc[3:5,1:3]) # 不包含末尾5或3，同列表切片
    '''
                 B   C
    2018-08-22  13  14
    2018-08-23  17  18
    '''
    # 跨行操作
    print(df.iloc[[1,3,5],1:3])
    '''
                 B   C
    2018-08-20   5   6
    2018-08-22  13  14
    2018-08-24  21  22
    '''
    # 混合选择
    print(df.ix[:3,['A','C']])
    '''
                A   C
    2018-08-19  0   2
    2018-08-20  4   6
    2018-08-21  8  10
    '''
    print(df.iloc[:3,[0,2]]) # 结果同上

    # 通过判断的筛选
    print(df[df.A>8])
    '''
                 A   B   C   D
    2018-08-22  12  13  14  15
    2018-08-23  16  17  18  19
    2018-08-24  20  21  22  23
    '''

3.2 筛选总结
~~~~~~~~~~~~

.. code:: python

    1.iloc与ix区别
      总结:相同点：iloc可以取相应的值，操作方便,与ix操作类似。
      不同点：ix可以混合选择，可以填入column对应的字符选择，而iloc只能采用index索引，对于列数较多情况下，ix要方便操作许多。
    2.loc与iloc区别
      总结：相同点：都可以索引处块数据
      不同点：iloc可以检索对应值,两者操作不同。
    3.ix与loc、iloc三者的区别
      总结：ix是混合loc与iloc操作
    如下:对比三者操作
    print(df.loc['20180819','A':'B'])
    print(df.iloc[0,0:2])
    print(df.ix[0,'A':'B'])
    输出结果相同，均为:
    A    0
    B    1
    Name: 2018-08-19 00:00:00, dtype: int32

4.Pandas设置值
--------------

4.1 创建数据
~~~~~~~~~~~~

.. code:: python

    import pandas as pd
    import numpy as np
    # 创建数据
    dates = pd.date_range('20180820',periods=6)
    df = pd.DataFrame(np.arange(24).reshape(6,4), index=dates, columns=['A','B','C','D'])
    print(df)
    '''
                 A   B   C   D
    2018-08-20   0   1   2   3
    2018-08-21   4   5   6   7
    2018-08-22   8   9  10  11
    2018-08-23  12  13  14  15
    2018-08-24  16  17  18  19
    2018-08-25  20  21  22  23
    '''

4.2 根据位置设置loc和iloc
~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: python

    # 根据位置设置loc和iloc
    df.iloc[2,2] = 111
    df.loc['20180820','B'] = 2222
    print(df)
    '''
                 A     B    C   D
    2018-08-20   0  2222    2   3
    2018-08-21   4     5    6   7
    2018-08-22   8     9  111  11
    2018-08-23  12    13   14  15
    2018-08-24  16    17   18  19
    2018-08-25  20    21   22  23
    '''

4.3 根据条件设置
~~~~~~~~~~~~~~~~

.. code:: python

    # 根据条件设置
    # 更改B中的数，而更改的位置取决于4的位置，并设相应位置的数为0
    df.B[df.A>4] = 0
    print(df)
    '''
                 A     B    C   D
    2018-08-20   0  2222    2   3
    2018-08-21   4     5    6   7
    2018-08-22   8     0  111  11
    2018-08-23  12     0   14  15
    2018-08-24  16     0   18  19
    2018-08-25  20     0   22  23
    '''

4.4 按行或列设置
~~~~~~~~~~~~~~~~

.. code:: python

    # 按行或列设置
    # 列批处理，F列全改为NaN
    df['F'] = np.nan
    print(df)

4.5 添加Series序列(长度必须对齐)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: python

    df['E'] = pd.Series([1,2,3,4,5,6], index=pd.date_range('20180820',periods=6))
    print(df)

4.6 设定某行某列为特定值
~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: python

    # 设定某行某列为特定值
    df.ix['20180820','A'] = 56
    df.loc['20180820','A'] = 67
    df.iloc[0,0] = 76

4.7 修改一整行数据
~~~~~~~~~~~~~~~~~~

.. code:: python

    # 修改一整行数据
    df.iloc[1] = np.nan # df.iloc[1,:]=np.nan
    df.loc['20180820'] = np.nan # df.loc['20180820,:']=np.nan
    df.ix[2] = np.nan # df.ix[2,:]=np.nan
    df.ix['20180823'] = np.nan
    print(df)

5.Pandas处理丢失数据
--------------------

5.1 创建含NaN的矩阵
~~~~~~~~~~~~~~~~~~~

.. code:: python

    # Pandas处理丢失数据
    import pandas as pd
    import numpy as np
    # 创建含NaN的矩阵
    # 如何填充和删除NaN数据?
    dates = pd.date_range('20180820',periods=6)
    df = pd.DataFrame(np.arange(24).reshape((6,4)),index=dates,columns=['A','B','C','D']) # a.reshape(6,4)等价于a.reshape((6,4))
    df.iloc[0,1] = np.nan
    df.iloc[1,2] = np.nan
    print(df)
    '''
                 A     B     C   D
    2018-08-20   0   NaN   2.0   3
    2018-08-21   4   5.0   NaN   7
    2018-08-22   8   9.0  10.0  11
    2018-08-23  12  13.0  14.0  15
    2018-08-24  16  17.0  18.0  19
    2018-08-25  20  21.0  22.0  23
    '''

5.2 删除掉有NaN的行或列
~~~~~~~~~~~~~~~~~~~~~~~

.. code:: python

    # 删除掉有NaN的行或列
    print(df.dropna()) # 默认是删除掉含有NaN的行
    print(df.dropna(
        axis=0, # 0对行进行操作;1对列进行操作
        how='any' # 'any':只要存在NaN就drop掉；'all':必须全部是NaN才drop
    ))
    '''
                 A     B     C   D
    2018-08-22   8   9.0  10.0  11
    2018-08-23  12  13.0  14.0  15
    2018-08-24  16  17.0  18.0  19
    2018-08-25  20  21.0  22.0  23
    '''
    # 删除掉所有含有NaN的列
    print(df.dropna(
        axis=1,
        how='any'
    ))
    '''
                 A   D
    2018-08-20   0   3
    2018-08-21   4   7
    2018-08-22   8  11
    2018-08-23  12  15
    2018-08-24  16  19
    2018-08-25  20  23
    '''

5.3 替换NaN值为0或者其他
~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: python

    # 替换NaN值为0或者其他
    print(df.fillna(value=0))
    '''
                 A     B     C   D
    2018-08-20   0   0.0   2.0   3
    2018-08-21   4   5.0   0.0   7
    2018-08-22   8   9.0  10.0  11
    2018-08-23  12  13.0  14.0  15
    2018-08-24  16  17.0  18.0  19
    2018-08-25  20  21.0  22.0  23
    '''

5.4 是否有缺失数据NaN
~~~~~~~~~~~~~~~~~~~~~

.. code:: python

    # 是否有缺失数据NaN
    # 是否为空
    print(df.isnull())
    '''
                    A      B      C      D
    2018-08-20  False   True  False  False
    2018-08-21  False  False   True  False
    2018-08-22  False  False  False  False
    2018-08-23  False  False  False  False
    2018-08-24  False  False  False  False
    2018-08-25  False  False  False  False
    '''
    # 是否为NaN
    print(df.isna())
    '''
                    A      B      C      D
    2018-08-20  False   True  False  False
    2018-08-21  False  False   True  False
    2018-08-22  False  False  False  False
    2018-08-23  False  False  False  False
    2018-08-24  False  False  False  False
    2018-08-25  False  False  False  False
    '''
    # 检测某列是否有缺失数据NaN
    print(df.isnull().any())
    '''
    A    False
    B     True
    C     True
    D    False
    dtype: bool
    '''
    # 检测数据中是否存在NaN,如果存在就返回True
    print(np.any(df.isnull())==True)

6.Pandas导入导出
----------------

6.1 导入数据
~~~~~~~~~~~~

.. code:: python

    import pandas as pd # 加载模块
    # 读取csv
    data = pd.read_csv('student.csv')
    # 打印出data
    print(data)
    # 前三行
    print(data.head(3))
    # 后三行
    print(data.tail(3))

6.2 导出数据
~~~~~~~~~~~~

.. code:: python

    # 将资料存取成pickle
    data.to_pickle('student.pickle')
    # 读取pickle文件并打印
    print(pd.read_pickle('student.pickle'))

7.参考资料
----------

`Pandas
学习 <https://morvanzhou.github.io/tutorials/data-manipulation/np-pd/>`__
