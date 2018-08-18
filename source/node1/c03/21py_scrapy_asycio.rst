.. figure:: http://p20tr36iw.bkt.clouddn.com/py_scarpy_asycio.png
   :alt: 

.. raw:: html

   <!--more-->

python爬虫系列之异步加载
========================

1.不是异步的
------------

.. code:: python

    import time
    def job(t):
        print('Strat job',t)
        time.sleep(t)
        print('Job',t,'takes',t,'s')

    def main():
        [job(t) for t in range(1,3)]

    t1 = time.time()
    main()
    print("NO async total time:", time.time() - t1)

输出结果：

.. code:: python

    Strat job 1
    Job 1 takes 1 s
    Strat job 2
    Job 2 takes 2 s
    NO async total time: 3.0002732276916504

从上面可以看出, 我们的 job 是按顺序执行的, 必须执行完 job 1 才能开始执行
job 2, 而且 job 1 需要1秒的执行时间, 而 job 2 需要2秒. 所以总时间是 3
秒多.

而如果我们使用 asyncio 的形式, job 1 在等待 time.sleep(t) 结束的时候,
比如是等待一个网页的下载成功, 在这个地方是可以切换给 job 2,
让它开始执行.

2.异步处理
----------

.. code:: python

    # 是异步的
    import asyncio
    import time


    async def job(t):
        print('Strat job',t)
        await asyncio.sleep(t)
        print('Job',t,'takes',t,'s')

    async def main(loop):
        tasks =[
            loop.create_task(job(t)) for t in range(1,3)
        ] # 创建任务,但是不执行
        await asyncio.wait(tasks) # 执行并等待所有任务完成

    t1 = time.time()
    loop = asyncio.get_event_loop() # 建立loop
    loop.run_until_complete(main(loop)) # 执行loop
    loop.close()

    print("Async total time:", time.time() - t1)

输出结果:

.. code:: python

    Strat job 1
    Strat job 2
    Job 1 takes 1 s
    Job 2 takes 2 s
    Async total time: 2.005455255508423

从结果可以看出, 我们没有等待 job 1 的结束才开始 job 2, 而是 job 1 触发了
await 的时候就切换到了 job 2 了. 这时, job 1 和 job 2 同时在等待 await
asyncio.sleep(t), 所以最终的程序完成时间, 取决于等待最长的 t, 也就是
2秒. 这和上面用普通形式的代码相比(3秒), 的确快了很多.

3.异步爬虫
----------

莫烦官网方式:

.. code:: python

    import aiohttp
    import time
    import asyncio
    URL = 'https://morvanzhou.github.io/'

    async def job(session):
        response = await session.get(URL)
        return str(response.url)

    async def main(loop):
        async with aiohttp.ClientSession() as session:      # 官网推荐建立 Session 的形式
            tasks = [loop.create_task(job(session)) for _ in range(2)]
            finished, unfinished = await asyncio.wait(tasks)
            all_results = [r.result() for r in finished]    # 获取所有结果
            print(all_results)

    t1 = time.time()
    loop = asyncio.get_event_loop()
    loop.run_until_complete(main(loop))
    loop.close()
    print("Async total time:", time.time() - t1)

AIOHTTP官网方式:

.. code:: python

    import aiohttp
    import time
    import asyncio
    URL = 'https://morvanzhou.github.io/'

    async def job(session):
        async with session.get(URL) as resp:
            print(resp.status)
            return str(resp.url)

    async def main(loop):
        async with aiohttp.ClientSession() as session:      # 官网推荐建立 Session 的形式
            tasks = [loop.create_task(job(session)) for _ in range(2)]
            finished, unfinished = await asyncio.wait(tasks)
            all_results = [r.result() for r in finished]    # 获取所有结果
            print(all_results)

    t1 = time.time()
    loop = asyncio.get_event_loop()
    loop.run_until_complete(main(loop))
    loop.close()
    print("Async total time:", time.time() - t1)

4.参考文章
----------

`Welcome to
AIOHTTP <https://aiohttp.readthedocs.io/en/stable/client_quickstart.html>`__

`加速爬虫: 异步加载
Asyncio <https://morvanzhou.github.io/tutorials/data-manipulation/scraping/4-02-asyncio/>`__
