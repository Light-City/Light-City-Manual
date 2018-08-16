.. figure:: http://p20tr36iw.bkt.clouddn.com/py_multi_process.png
   :alt: 

.. raw:: html

   <!--more-->

python进阶之多进程
==================

1.进程与线程初识
----------------

1.1 导包
~~~~~~~~

.. code:: python

    # 导入线程进程标准模块
    import multiprocessing as mp
    import threading as td

1.2 定义被调函数
~~~~~~~~~~~~~~~~

.. code:: python

    # 定义一个被线程和进程调用的函数
    def job(a,d):
        print('aaaaa')

1.3 创建线程和进程
~~~~~~~~~~~~~~~~~~

.. code:: python

    # 创建线程和进程
    t1 = td.Thread(target=job, args=(1,2)) # (1,2,)与(1,2)一样效果
    p1 = mp.Process(target=job, args=(1,2)) # (1,2,)与(1,2)一样效果

1.4 启动线程和进程
~~~~~~~~~~~~~~~~~~

.. code:: python

    if __name__ == '__main__':
        # 启动线程和进程
        t1.start()
        p1.start()
        # 连接线程和进程
        t1.join()
        p1.join()

2.输出结果存放至Queue
---------------------

2.1 导包
~~~~~~~~

.. code:: python

    import multiprocessing as mp

2.2 定义被调函数
~~~~~~~~~~~~~~~~

这里传入Queue对象

.. code:: python

    def job(q):
        res = 0
        for i in range(1000000):
            res += i + i ** 2 + i ** 3
        q.put(res)

2.3 启动多进程，存放结果
~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: python

    if __name__ == '__main__':
        q = mp.Queue()
        p1 = mp.Process(target=job, args=(q,))
        p2 = mp.Process(target=job, args=(q,))
        p1.start()
        p2.start()
        p1.join()
        p2.join()
        res1 = q.get()
        res2 = q.get()
        print(res1+res2)

3.进程与线程效率对比
--------------------

3.1 导入多进程包
~~~~~~~~~~~~~~~~

.. code:: python

    import multiprocessing as mp

3.2 定义被调函数
~~~~~~~~~~~~~~~~

.. code:: python

    def job(q):
        res = 0
        for i in range(1000000):
            res += i + i ** 2 + i ** 3
        q.put(res)

3.3 封装多进程
~~~~~~~~~~~~~~

.. code:: python

    # 多核/多进程
    def multicore():
        q = mp.Queue()
        p1 = mp.Process(target=job, args=(q,))
        p2 = mp.Process(target=job, args=(q,))
        p1.start()
        p2.start()
        p1.join()
        p2.join()
        res1 = q.get()
        res2 = q.get()
        print('multicore:', res1 + res2)

3.4 导入线程包
~~~~~~~~~~~~~~

.. code:: python

    import threading as td

3.5 封装多线程
~~~~~~~~~~~~~~

.. code:: python

    # 多线程
    def multithread():
        q = mp.Queue()
        t1 = td.Thread(target=job, args=(q,))
        t2 = td.Thread(target=job, args=(q,))
        t1.start()
        t2.start()
        t1.join()
        t2.join()
        res1 = q.get()
        res2 = q.get()
        print('multithread:', res1 + res2)

3.6 封装普通方法
~~~~~~~~~~~~~~~~

.. code:: python

    def normal():
        res = 0
        for _ in range(2):
            for i in range(1000000):
                res += i + i ** 2 + i ** 3
        print('normal:',res)

3.7 主函数调用
~~~~~~~~~~~~~~

三种方法对比效率

.. code:: python

    import time
    if __name__ == '__main__':
        st = time.time()
        normal()
        st1 = time.time()
        print('normal time:', st1 - st)
        multithread()
        st2 = time.time()
        print('multithread time:', st2 - st1)
        multicore()
        st3 = time.time()
        print('multicore time:', st3 - st2)

3.8 输出结果
~~~~~~~~~~~~

.. code:: python

    normal: 499999666667166666000000
    normal time: 1.779979944229126
    multithread: 499999666667166666000000
    multithread time: 1.8090195655822754
    multicore: 499999666667166666000000
    multicore time: 1.2929792404174805

**结论:多进程 < 普通 < 多线程**

4.进程池
--------

说在前面:有了池子之后，就可以让池子对应某一个函数，我们向池子里丢数据，池子就会返回函数返回的值。

Pool和之前的Process的不同点是丢向Pool的函数有返回值，而Process的没有返回值。

4.1 导入进程包
~~~~~~~~~~~~~~

.. code:: python

    import multiprocessing as mp

4.2 定义被调函数
~~~~~~~~~~~~~~~~

.. code:: python

    def job(x):
        return x*x

4.3 封装函数
~~~~~~~~~~~~

map() 与 apply\_async() 两种方式 返回结果

.. code:: python

    def multicore():
        '''
        Pool默认调用是CPU的核数,传入processes可自定义CPU核数
        map()放入迭代参数,返回多个结果
        apply_async()只能放入一组参数,并返回一个结果,如果想得到map()的效果需要通过迭代
        '''
        pool = mp.Pool(processes=2)
        res = pool.map(job, range(10))
        print(res)
        '''
        apply_async()只能传递一个值，它只会放入一个核进行运算，传入的值因为必须是可迭代的，
        所以在传入值后需要加逗号，同时需要用get()方法获取返回值。
        '''
        res = pool.apply_async(job, (2,))
        multi_res = [pool.apply_async(job, (i,)) for i in range(10)]
        print(res.get()) # 获取单个结果
        print([res.get() for res in multi_res]) # 获取多个结果

4.4 主函数调用
~~~~~~~~~~~~~~

.. code:: python

    if __name__ == '__main__':
        multicore()

5.共享内存
----------

.. code:: python

    import multiprocessing as mp

    '''
    使用Value数据存储在一个共享的内存表中
    d表示一个双精浮点类型，i表示一个带符号的整型
    '''
    value1 = mp.Value('i', 0)
    value2 = mp.Value('d', 3.14)

    '''
    Array类，可以和共享内存交互，来实现在进程之间共享数据。
    这里的Array和numpy中的不同，它只能是一维的，不能是多维的。
    同样和Value 一样，需要定义数据形式，否则会报错。
    '''
    array = mp.Array('i', [1,2,3,4])

6.进程锁
--------

6.1 不同进程争夺资源
~~~~~~~~~~~~~~~~~~~~

.. code:: python

    import multiprocessing as mp
    import time
    def job(v, num):
        for _ in range(5):
            time.sleep(0.1)
            v.value += num
            print(v.value,end="\n")

    def multicore():
        v = mp.Value('i',0) # 定义共享变量
        p1 = mp.Process(target=job, args=(v,1))
        p2 = mp.Process(target=job, args=(v,3)) # 设定不同的number看如何抢夺内存
        p1.start()
        p2.start()
        p1.join()
        p2.join()

    if __name__ == '__main__':
        multicore()

6.2 通过锁机制解决争夺资源问题
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: python

    import multiprocessing as mp
    import time
    def job(v, num, l):
        l.acquire() # 锁住
        for _ in range(5):
            time.sleep(0.1)
            v.value += num
            print(v.value,end="\n")
        l.release() # 释放

    def multicore():
        l = mp.Lock() # 定义一个进程锁
        v = mp.Value('i',0) # 定义共享变量
        p1 = mp.Process(target=job, args=(v,1,l)) # 需要将lock传入
        p2 = mp.Process(target=job, args=(v,3,l)) # 设定不同的number看如何抢夺内存
        p1.start()
        p2.start()
        p1.join()
        p2.join()

    if __name__ == '__main__':
        multicore()

7.参考资料
----------

`多进程 <https://morvanzhou.github.io/tutorials/python-basic/multiprocessing/>`__
