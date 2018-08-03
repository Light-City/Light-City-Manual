.. figure:: http://p20tr36iw.bkt.clouddn.com/anaconda.png
   :alt: 

Win10上Anaconda的python2与python3切换
=====================================



一、Conda配置
-------------

1.Path配置
~~~~~~~~~~


配置系统变量

.. code:: python

    D:\Anaconda
    D:\Anaconda\Library\bin
    D:\Anaconda\Scripts


上述操作失败

.. code:: python

    在Scripts中找不到activate.bat/deactivate.bat/conda.exe

.. code:: python

    1.进入 D:\Anaconda\pkgs\conda-4.4.10-py36_0\Scripts
    2.将所有文件复制到 D:\Anaconda\Scripts
    3.重启cmd，输入conda，正常，到此圆满配置好conda。

2.Anaconda prompt配置
~~~~~~~~~~~~~~~~~~~~~

anaconda-prompt报错

.. code:: python

    '"D:\Anaconda\Scripts\..\Library\bin\conda.bat"' 不是内部或外部命令，也不是可运行的程序
    或批处理文件。

解决办法

.. code:: python

    1.进入D:\Anaconda\pkgs\conda-4.4.10-py36_0\Library\bin
    2.复制conda.bat至D:\Anaconda\Library\bin
    3.再次运行，成功！

3.Anaconda Navigator
~~~~~~~~~~~~~~~~~~~~

.. code:: python

    1.升级navigator
    conda update anaconda-navigator
    2.重置navigator
    anaconda-navigator --reset
    3.升级客户端
    conda update anaconda-client
    4.升级安装依赖包
    conda update -f anaconda-client
    5.此时左下角开始所有程序中就会出现Anaconda Navigator，打开正常运行

二、Python与Python3切换
-----------------------

第一种方式：Anaconda切换
~~~~~~~~~~~~~~~~~~~~~~~~

打开Anaconda Navigator，如下图所示，点击create创建一个新的配置环境，Python3与Python2切换，则需要创建Python3与Python2环境。
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. figure:: http://p20tr36iw.bkt.clouddn.com/anaconda.png
   :alt: 

以上创建操作就相当于conda create -n py2 python=2.7，此时不管是上述可视化操作还是命令操作，会在D:\Anaconda\envs生成相应的文件夹`(例如:py2)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

    activate py2
    deactivate
    activate py3
    deactivate

.. figure:: http://p20tr36iw.bkt.clouddn.com/py2_py3.gif
   :alt: 

第二种方式：Pycharm切换
~~~~~~~~~~~~~~~~~~~~~~~

三、参考文章
------------

`1.windows 环境下在anaconda 3中安装python2和python3两个环境（python2和python3共存） <https://blog.csdn.net/u010801439/article/details/78459920>`__
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

`2.解决Anaconda navigator闪退问题 <https://blog.csdn.net/u012318074/article/details/78844789>`__
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

`3.Windows下Anaconda2(Python2)和Anaconda3(Python3)的共存 <https://blog.csdn.net/infin1te/article/details/50445217>`__
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

`4.安装Anaconda3 后，怎样使用 Python 2.7？ <https://blog.csdn.net/u013313168/article/details/67828746>`__
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
