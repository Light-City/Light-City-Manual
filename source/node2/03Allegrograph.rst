.. figure:: http://p20tr36iw.bkt.clouddn.com/Allegrograph.png
   :alt: 

第三节:AllegroGraph初识
=========================

一、下载与安装
--------------

1.\ `安装(AllegroGraph官网) <https://franz.com/agraph/downloads/>`__
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: python

    点击进入官网
    下载AllegroGraph Virtual Machine
    windows上安装VMware,直接加载解压后的虚拟机文件
    运行

2.启动及关闭
~~~~~~~~~~~~

::

    双击Start AG则启动
    双击Stop AG则关闭
    启动后，通过localhost:10035即可进入web页面

3.两个账号信息
~~~~~~~~~~~~~~

    AllegroGraph虚拟机账户及密码

.. code:: python

    Username :franz
    Password:allegrograph

    登录AllegroGraph

.. code:: python

    username:test
    password:xyzzy

二、简单使用
------------

1.利用Gruff建立仓库
~~~~~~~~~~~~~~~~~~~

.. code:: python

    以下操作前记得开启服务！
    击桌面上的gruff连接，右击打开。
    左上角file -> New triple store
    指定triple store 的名字。
    选择->file ->Load->N-triples 选一个可用的N-triple 文件，打开。

2.利用Web页面建立
~~~~~~~~~~~~~~~~~

.. code:: python

    test账户登陆
    Create new repository>Name  填写仓库名字，类似于上述triple store名字
    创建好后，上面Repositories就会出现刚才新建的仓库
    点进刚新建的仓库，选择Load and Delete Date>import RDF>from an uploaded file>File 选择本地文件>OK
    Explore the Repository>View triples>Edit query输入SPARQL语句查询三元组

三、Gah. Your tab just crashed problem
--------------------------------------

    火狐浏览器崩溃

.. code:: python

    更换谷歌浏览器，以下为安装步骤
    sudo wget http://www.linuxidc.com/files/repo/google-chrome.list -P /etc/apt/sources.list.d/
    wget -q -O - https://dl.google.com/linux/linux_signing_key.pub  | sudo apt-key add -
    sudo apt-get update
    sudo apt-get install google-chrome-stable
    /usr/bin/google-chrome-stable
    在浏览器图标上右键，将启动器锁定

四、参考文章
------------

`1.Guide to the AllegroGraph Virtual Machine <https://franz.com/agraph/allegrograph/vm/>`__
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

`2.AllegroGraph下载 <https://franz.com/agraph/downloads/>`__
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

`3.AllegroGraph Virtual Machine 安装教程 <https://blog.csdn.net/sinat_25551085/article/details/62887128>`__
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

`4.ubuntu16.04中安装google chrome <https://blog.csdn.net/HYESC/article/details/78991405>`__
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
