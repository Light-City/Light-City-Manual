.. figure:: http://p20tr36iw.bkt.clouddn.com/timg.jpg
   :alt: 
   
第三节:Ubuntu 16.04安装Spark
==============================

一、安装Scala
-------------

scala 下载与安装
~~~~~~~~~~~~~~~~

.. code:: 

    wget http://scala-lang.org/files/archive/scala-2.11.12.tgz

.. code:: 

    tar xcf scala-2.11.12.tgz

.. code:: 

    sudo mv scala-2.11.12  /usr/local/scala

.. code:: 

    # 编辑~/.bashrc文件
    sudo gedit ~/.bashrc
    # 末尾添加Scala配置
    export SCALA_HOME=/usr/local/scala
    export PATH=$PATH:${SCALA_HOME}/bin
    # ~/.bashrc修改生效
    source ~/.bashrc

启动scala
~~~~~~~~~

.. figure:: http://p20tr36iw.bkt.clouddn.com/cat.png
   :alt: 

issue:cat /usr/lib/jvm/java-8-openjdk-amd64//release:没有那个文件或目录

solution:更换openjdk为oracle jdk

二、在Ubuntu 16.04上正确安装Oracle Java
---------------------------------------

1. 将webupd8team存储库添加到apt
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

    sudo apt-get install python-software-properties
    sudo add-apt-repository ppa:webupd8team/java
    sudo apt-get update

-  注：运行第二个命令出现No module named "apt\_pkg"错误
-  解决方法：由于更换python版本3.6，所以出现以下问题，依次运行以下代码即可解决

::

    sudo apt install python3-apt
    cd /usr/lib/python3/dist-packages
    sudo cp apt_pkg.cpython-35m-x86_64-linux-gnu.so  apt_pkg.cpython-36m-x86_64-linux-gnu.so

2.安装 Java
~~~~~~~~~~~

::

    sudo apt-get install oracle-java8-installer

3.选择Oracle Java作为默认JVM
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

    sudo update-alternatives --config java

|image0| ### 4.检查此命令的输出，以验证是否正确

::

    java -version

5.自此完全正常
~~~~~~~~~~~~~~

.. figure:: http://p20tr36iw.bkt.clouddn.com/scala.png
   :alt: 

三、安装&配置Spark
------------------

1.下载与安装
~~~~~~~~~~~~

::

    # 下载
    wget http://mirrors.hust.edu.cn/apache/spark/spark-2.2.1/spark-2.2.1-bin-hadoop2.7.tgz
    # 解压
    tar zxf spark-2.2.1-bin-hadoop2.7.tgz
    # 目录移动
    sudo mv spark-2.2.1-bin-hadoop2.7 /usr/local/spark
    # 编辑~/.bashrc
    sudo gedit ~/.bashrc
    # 配置Spark--复制以下两行至~/.bashrc文件中
    export SPARK_HOME=/usr/local/spark
    export PATH=$PATH:${SPARK_HOME}/bin
    # ~/.bashrc配置生效
    source ~/.bashrc
    # 进入spark-shell交互界面
    spark-shell

.. figure:: http://p20tr36iw.bkt.clouddn.com/spark.png
   :alt: 

2.设置spark-shell显示信息
~~~~~~~~~~~~~~~~~~~~~~~~~

::

    # 切换到spark配置文件目录
    cd /us/local/spark/conf
    # 复制log4j模板文件到log4j.properties
    cp log4j.properties.template log4j.properties
    # 编辑log4j.properties---将log4j.rootCategory=INFO,console改为log4j.rootCategory=WARN,console
    sudo gedit log4j.properties
    # 启动spark-shell

此时会发现信息少了很多，只列出重要部分的信息

.. figure:: http://p20tr36iw.bkt.clouddn.com/sparkw.png
   :alt: 

四、构建Spark Standalone Cluster执行环境
----------------------------------------

::

    # 复制模板文件来创建spark-env.sh
    cp /usr/local/spark/conf/spark-env.sh.template /usr/local/spark/conf/spark-env.sh
    # 编辑spark-env.sh
    sudo gedit /usr/local/spark/conf/spark-env.sh
    # 添加以下内容
    export SPARK_MASTER_IP=master
    export SPARK_WORKER_CORES=1
    export SPARK_WORKER_MEMORY=800m
    export SPARK_WORKER_INSTANCES=2

::

    # 将master的spark程序复制到data1、data2、data3,以下五行重复，注意更换data服务器
    ssh data1 # ssh连接至data1
    sudo mkdir /usr/local/spark # 连接成功后，创建spark目录
    sudo chown hduser:hadoop /usr/local/spark # 更换所有者为hduser
    exit # 注销data1
    sudo scp -r /usr/local/spark hduser@data1:/usr/local

五、在Spark Standalone运行spark-shell
-------------------------------------

::

    # 启动Spark Standalone Cluster
    /usr/local/spark/sbin/start-all.sh
    # 在Spark Standalone运行spark-shell
    spark-shell --master spark://master:7077

::

    http://master:8080/

.. figure:: http://p20tr36iw.bkt.clouddn.com/sparkMas.png
   :alt: 

六、参考文章
------------

1.\ `python3.6 下提示No module named
"apt\_pkg" <http://www.fantansy.cn/index.php/python/311.html>`__

2.\ `如何在Ubuntu 16.04上正确安装Oracle
Java <http://www.linuxidc.com/Linux/2017-07/145652.htm>`__

.. |image0| image:: http://p20tr36iw.bkt.clouddn.com/alt.png

