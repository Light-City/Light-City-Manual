.. figure:: http://p20tr36iw.bkt.clouddn.com/kg_main.png
   :alt: 

第七节:高血压知识图谱部署云服务器
==================================

一、LNMP配置
------------

参考之前csdn上写过的一篇文章 `Cetos
7.2+LNMP+WordPress <https://blog.csdn.net/guangcheng0312q/article/details/54926549>`__

二、python3
-----------

安装编译Python时所需要的库

.. code:: 

    yum -y install gcc gcc-c++

下载python 3.6.5的源码包(y提前下载wget工具)

.. code:: 

    wget https://www.python.org/ftp/python/3.6.5/Python-3.6.5.tgz

解压

.. code:: 

    tar -zxvf Python-3.6.5.tgz

配置

.. code:: 

    cd python-3.6.5
    ./configure

编译安装

.. code:: 

    make && make install

最后查看版本

.. code:: 

    python3 -V  # 输出版本为python3.6.5

系统自带python2.7，现在需要设置默认python3

.. code:: python3

    # 备份python
    mv /usr/bin/python /usr/bin/python2.7.bak
    # 链接 python3.6.5
    ln –s /usr/local/bin/python3 /usr/bin/python
    python -V # 此时输出python3.6.5

yum此时用不了了

.. code:: 

    # 这是yum用的是原来2.7的版本，而刚刚我们把python改为3.6.5了，所以用不了了，需要编辑yum文件
    vim usr/bin/yum
    把第一行的usr/bin/python改成/usr/bin/python2.7

python命令行无法使用退格键、方向键

.. code:: python3

    # 安装readline模块
    yum -y install readline-devel
    # 重装Python3即可解决

三、pip安装
-----------

.. code:: python3

    setuptool默认自动装
    到python官网找pip tar.gz文件
    wget http:/xxxxxx.tar.gz
    tar -zxvf xxxxx.tar.gz
    cd xxxx-version/
    python setup.py install

四、使用pip安装django
---------------------

.. code:: python3

    # 安装
    pip install django
    # 检查
    python   进入命令行
    import django
    print(django.get_version())

五、远程传输文件及配置
----------------------

传输KG项目及Apache Jena Tdb数据 ### 使用FlashFXP传输文件至服务器 KG项目
requirement.txt里面的库安装 ###
``pip install --upgrade -r requirements.txt`` KG项目问题之Django
Question

::

    1.python manage.py runserver 0.0.0.0:8000
    如果没有加0.0.0.0:8000的话就是通过localhost或127.0.0.1访问，加上后可以在我们非服务器电脑上访问。
    2.Django运行访问项目出现的问题:DisallowedHost at / Invalid HTTP_HOST header
    找到项目的setting.py文件，然后设置ALLOWED_HOSTS = ['*'] # *号用服务器公网ip替换掉或者根据页面报错提示输入
    3.页面报错socket.error : (113 , 'No route to host ')，提示有防火墙，然后根据以下两项设置：
      关闭防火墙：sudo systemctl stop firewalld.service
      关闭开机启动：sudo systemctl disable firewalld.service
    4.记得在settings.py中，INSTALLED_APPS中添加项目中的应用

六、Apache Jena在Linux端配置
----------------------------

::

    上述通过FlashFXP传输了Apache Jena文件，这里完成了药物本体及相应规则的拷贝配置后，运行./fuseki-server --config=ConfigFile   启动服务，最后一行有具体的端口号，打开浏览器，查看 ip:3030/，未打开，提示权限不够,执行以下操作：
      chmod 777 fuseki-server
      chmod 777 fuseki-server.bat
    再次刷新，发现页面有信息，但是无数据，打开f12查看原因
    看到server 报错 403 Access denied : only localhost access allowed
    此时在执行启动服务同级目录下 有个run文件夹，vi shiro.ini 
    将/$/** = localhostFilter改为/$/** = anon

七、Linux SSH 客户端断开后如何保持进程继续运行配置方法?
-------------------------------------------------------

这里使用nohup命令

::

    就是在相应命令前加上nohup，结尾处加上&
    运行后会显示PID，然后再回车就会到命令行，默认会将进程的任务放本项目的nohup.out文件中

八、参考文章
------------

`1.Django的安装与服务器的搭建 <https://blog.csdn.net/a249900679/article/details/51527200>`__

`2.python命令行无法使用退格键、方向键 <https://blog.csdn.net/xushuai110/article/details/50525232>`__

`3.Python命令行下退格、删除、方向键乱码问题解决（亲测有效） <https://blog.csdn.net/robin_star_/article/details/78774004>`__

`4.Django运行访问项目出现的问题:DisallowedHost at / Invalid HTTP\_HOST
header <https://blog.csdn.net/will5451/article/details/53861092>`__

`5.Linux 安装 Apache Jena Fuseki 3.6 \| add
one <https://www.jianshu.com/p/380a768e4aea>`__

`6.CentOS7
防火墙关闭 <https://blog.csdn.net/laotoumo/article/details/67632618>`__

`7.云服务器 ECS Linux SSH
客户端断开后保持进程继续运行配置方法 <https://help.aliyun.com/knowledge_detail/42523.html>`__
