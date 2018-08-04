.. figure:: http://p20tr36iw.bkt.clouddn.com/file.jpg
   :alt: 

第二节:Hadoop HDFS命令
=======================

本节所有命令
------------

+----------------------------+---------------------------------------------------+
| 命令                       | 说明                                              |
+============================+===================================================+
| hadoop fs -mkdir           | 创建HDFS目录                                      |
+----------------------------+---------------------------------------------------+
| hadoop fs -ls              | 列出HDFS目录                                      |
+----------------------------+---------------------------------------------------+
| hadoop fs -copyFromLocal   | 使用-copyFromLocal 复制本地(local)文件到HDFS      |
+----------------------------+---------------------------------------------------+
| hadoop fs -put             | 使用-put 复制本地(local)文件到HDFS                |
+----------------------------+---------------------------------------------------+
| hadoop fs -cat             | 列出HDFS目录下的文件内容                          |
+----------------------------+---------------------------------------------------+
| hadoop fs -copyToLocal     | 使用-copyToLocal将HDFS上的文件复制到本地(local)   |
+----------------------------+---------------------------------------------------+
| hadoop fs -get             | 使用-get将HDFS上的文件复制到本地(local)           |
+----------------------------+---------------------------------------------------+
| hadoop fs -cp              | 复制HDFS文件                                      |
+----------------------------+---------------------------------------------------+
| hadoop fs -rm              | 删除HDFS文件                                      |
+----------------------------+---------------------------------------------------+

一、启动Hadoop Multi-Node Cluster
---------------------------------

-  1.开启master、data1、data2、data3的虚拟主机
-  2.start-all.sh启动Hadoop Multi-Node Cluster

二、创建与查看HDFS目录
----------------------

.. code:: 

    # 创建user目录
    hadoop fs -mkdir /user

.. code:: 

    # 在user目录下，创建hduser子目录
    hadoop fs -mkdir /user/hduser

.. code:: 

    # 在hduser目录下，创建test子目录
    hadoop fs -mkdir /user/hduser/test

.. code:: 

    # 查看之前创建的HDFS目录
    hadoop fs -ls

.. code:: 

    # 查看HDFS根目录
    hadoop fs -ls /
    # 查看HDFS的/user目录
    # 查看HDFS的/user/hduser目录
    hadoop fs -ls /user/hduser

.. code:: 

    # 查看所有HDFS子目录
    # 一次查看所有子目录
    hadoop fs -ls -R /
    #创建多级HDFS子目录
    hadoop fs -p /dir1/dir2/dir3
    # 查看所有HDFS子目录
    hadoop fs -ls -R /

三、从本地计算机复制文件到HDFS
------------------------------

.. code:: 

    # 复制本地文件到HDFS目录
    hadoop fs -copyFromLocal /usr/local/hadoop/README.txt /user/hduser/test
    # 复制本地文件到HDFS目录的test.txt
    hadoop fs -copyFromLocal /usr/local/hadoop/README.txt /user/hduser/test/test1.txt
    # 列出HDFS目录下的文件
    hadoop fs -ls /user/hduser/test
    # 列出HDFS目录下的文件内容
    hadoop fs -cat /user/hduser/test/README.txt
    # 针对大文件，不一次显示，分多页显示，使用|more命令.
    hadoop fs -cat /user/hduser/test/README.txt|more
    # 强制复制本地文件到HDFS的目录
    hadoop fs -copyFromLocal -f /usr/local/hadoop/README.txt /user/hduser/test
    # 复制多个本地文件到HDFS目录
    hadoop fs -copyFromLocal /usr/local/hadoop/NOTICE.txt /usr/local/hadoop/LICENSE.txt /user/hduser/test
    # 复制目录到HDFS目录
    hadoop fs -copyFromLocal /usr/local/hadoop/etc /user/hduser/test
    # 列出HDFS目录下的文件
    hadoop fs -ls /user/hduser/test
    # 使用put复制文件到HDFS目录，会直接覆盖文件
    hadoop fs -put /usr/local/hadoop/README.txt /user/hduser/test/test2.txt
    # 将原本显示在屏幕上的内容存储到HDFS文件，“|”即pipe管道
    echo abc | hadoop fs -put - /usr/hduser/test/echoin.txt
    # 显示在HDFS的文件echion.txt的内容
    hadoop fs -cat /usr/hduser/test/echoin.txt
    # 本地目录的列表存储HDFS文件
    ls /usr/local/hadoop | hadoop fs -put - /usr/hduser/test/hadooplist.txt
    # 显示HDFS的文件hadooplist.txt
    hadoop fs -cat /user/hduser/test/hadooplist.txt

四、将HDFS上的文件复制到本地计算机
----------------------------------

::

    # 本地计算机创建test测试目录
    mkdir test
    # 切换到test目录
    cd test
    # 查看本地目录
    ll
    # 将整个目录上的目录复制到本地计算机
    hadoop fs -copyToLocal /user/hduser/test/etc
    # 将HDFS上的文件复制到本地计算机
    hadoop fs -get /user/hduser/test/README.txt localREADME.txt

五、复制与删除HDFS文件
----------------------

::

    # 在HDFS上创建测试目录/user/hduser/test/temp
    hadoop fs -nkdir /user/hduser/test/temp
    # 复制HDFS文件到HDFS测试目录
    hadoop fs -cp /user/hduser/test/README.txt /user/hduser/test/temp
    # 查看HDFS测试目录
    hadoop fs -ls /user/hduser/test/temp
    # 先查看准备要被删除的文件
    hadoop fs -ls /user/hduser/test
    # 删除HDFS文件
    hadoop fs -rm /user/hduser/test/test2.txt
    # 查看准备要被删除的HDFS目录
    hadoop fs -ls /user/hduser/test
    # 删除HDFS目录
    hadoop fs -rm -R /user/hduser/test/etc

六、在Hadoop HDFS Web 用户界面浏览HDFS
--------------------------------------

::

    # 查看我们之前创建的HDFS目录与文件
    http://master:50070

.. figure:: http://p20tr36iw.bkt.clouddn.com/file.jpg
   :alt: 

