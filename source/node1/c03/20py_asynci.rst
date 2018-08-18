.. figure:: http://p20tr36iw.bkt.clouddn.com/py_asycio.png
   :alt: 

.. raw:: html

   <!--more-->

python进阶之异步任务
====================

1.yield关键字
-------------

yield类似于return, 但不同之处在于它返回的是生成器！

生成器 -
生成器是通过一个或多个yield表达式构成的函数。每一个生成器都是一个迭代器(但迭代器不一定是生成器)。
-
生成器并不会一次返回所有结果，而是每次遇到yield关键字后返回相应结果，并保留函数当前的运行状态。等待下一次的调用。
- 由于生成器也是一个迭代器，那么它就应该支持next方法来获取下一个值。
除此之外，生成器还支持send函数，该函数可以向生成器传递参数。

.. code:: python


    # 基本操作
    def fun():
        for i in range(10):
            yield i
    f = fun()
    # 通过for循环+next进行调用
    for i in range(10):
        print(next(f))

    print('-----------------------------')
    def func():
        n = 0
        while 1:
            n = yield n

    f = func()
    print(next(f))
    print('------------------------------')
    print(f.send(5)) # n赋值为5

    def func(number):
        while True:
            yield number
            number += 1

    f = func(1)
    print('------------------------------')

    # 取出10个数
    for i in range(10):
        print(next(f))


    # 生产消费模式
    print('------------------------------')
    def consumer():
        r = ''
        while True:
            n = yield r
            if not n:
                return
            print('[CONSUMER] Consuming %s...' % n)
            r = '200 OK'

    def produce(c):
        c.send(None)
        n = 0
        while n < 5:
            n = n + 1
            print('[PRODUCER] Producing %s...' % n)
            r = c.send(n)
            print('[PRODUCER] Consumer return: %s' % r)
        c.close()

    c = consumer()
    produce(c)

2.异步加载asyncio
-----------------

asyncioasyncio可以实现单线程并发IO操作
async和await是针对coroutine的新语法，要使用新的语法，只需要做两步简单的替换：
把@asyncio.coroutine替换为async； 把yield from替换为await。

第一种方式
~~~~~~~~~~

.. code:: python

    import asyncio

    @asyncio.coroutine
    def hello():
        print("Hello world!")
        r = yield from asyncio.sleep(1)
        print("Hello again!")

    loop = asyncio.get_event_loop()
    loop.run_until_complete(hello())
    loop.close()

第二种方式
~~~~~~~~~~

.. code:: python

    async def hello2():
        print('Hello world2!')
        r = await asyncio.sleep(1)
        print('Hello again2!')

    loop = asyncio.get_event_loop()
    loop.run_until_complete(hello2())
    loop.close()

