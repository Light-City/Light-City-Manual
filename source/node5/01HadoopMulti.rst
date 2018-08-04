.. figure:: http://p20tr36iw.bkt.clouddn.com/main.jpg
   :alt: 

第一节:Vmware-Ubuntu 16.04(LTS版)实战Hadoop Multi Node Cluster
===============================================================

一、前言
--------

  本文操作是在Hadoop Single Node
Cluster基础上的进阶，由于Single与Multi大同小异，不多作解释，本文着重阐述Hadoop
Multi Node Cluster。   Hadoop Multi Node
Cluster架构有一台主要的计算机master,在HDFS担任NameNode角色、在MapReduce2(YARN)担任ResourceManager角色。除此之外，添加三个虚拟主机data1、data2，data3.

.. figure:: http://p20tr36iw.bkt.clouddn.com/network.png
   :alt: 

  注：Hadoop Multi Node
Cluster架构，必须有4台实体服务器才能建立，才能够发挥多台计算机并行处理的优势，此处选用虚拟主机进行实验。

二、准备工作
------------

1.Vmware station--虚拟机安装
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  也可以换成VirtualBox。

2.Linux系统一个
~~~~~~~~~~~~~~~

-  这里选用Ubuntu 16.04(LTS版)。

3.Hadoop二进制文件一个
~~~~~~~~~~~~~~~~~~~~~~

-  这里选用hadoop-2.8.3.tar.gz

注：虚拟机安装Ubuntu略，Ubuntu16.04安装Hadoop Single Node Cluster略！

三、开始实操
------------

  由于之前Hadoop Single Node Cluster已经安装好了，现在进入下列操作：

1.克隆主机
~~~~~~~~~~

通过Vmware克隆模式，克隆出4个独立的虚拟主机，并且命名为data1,data2,data3,master。然后通过ifconfig得出各个虚拟主机的IP。这里为

.. code:: 

    master 192.168.56.100
    data1 192.168.56.101
    data1 192.168.56.102
    data1 192.168.56.103

2.data1修改
~~~~~~~~~~~

编辑interfaces网络配置文件---保证虚拟主机IP地址不变

.. code:: 

    sudo gedit /etc/network/interfaces

.. code:: 

    # interfaces(5) file used by ifup(8) and ifdown(8)
    auto lo
    iface lo inet loopback

    # 以下为新添加内容
    # NAT interface
    auto eth0
    iface eth1 inet dhcp

    # host only interface
    auto eth1
    iface eth1 inet static
    address 192.168.56.101
    netmask 255.255.255.0
    network 192.168.56.0
    broadcast 192.168.56.255

1.设置hostname---修改主机名
^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: 

    sudo gedit /etc/hostname

.. code:: 

    # 清空文件内容，改为以下内容
    data1

编辑完成后，先保存，再关闭gedit.

2.编辑hosts文件
^^^^^^^^^^^^^^^

.. code:: 

    # 添加下面内容，注意输入每一台主机对应的IP地址
    master 192.168.56.100
    data1 192.168.56.101
    data1 192.168.56.102
    data1 192.168.56.103

3.编辑core-site.xml
^^^^^^^^^^^^^^^^^^^

.. code:: 

    sudo gedit /usr/local/hadoop/etc/hadoop/core-site.xml

.. code:: xml

    <!--在Hadoop Single Node Cluster安装后，下面master为localhost，此时改为master即可-->
    <property>
        <name>fs.default.name</name>
        <value>hdfs://master:9000</value>
    </property>

4.编辑yarn-site.xml
^^^^^^^^^^^^^^^^^^^

.. code:: 

    sudo gedit /usr/local/hadoop/etc/hadoop/yarn-site.xml

.. code:: xml

    <configuration>

    <!-- 设置ResourceManager主机与NodeManager的连接地址 -->
    <property>
        <name>yarn.resourcemanager.resource-tracker.address</name>
        <value>master:8025</value>
    </property>
    <!-- 设置ResourceManager与ApplicationMaster的连接地址 -->
    <property>
        <name>yarn.resourcemanager.scheduler.address</name>
        <value>master:8030</value>
    </property>
    <!-- 设置ResourceManager与客户端的连接地址 -->
    <property>
        <name>yarn.resourcemanager.address</name>
        <value>master:8050</value>
    </property>

    </configuration>

5.编辑mapred-site.xml
^^^^^^^^^^^^^^^^^^^^^

.. code:: 

    sudo gedit /usr/local/hadoop/etc/hadoop/mapred-site.xml

.. code:: xml

    <configuration>
    <property>
        <name>mapred.job.tracker</name>
        <value>master:54311</value>
    </property>
    </configuration>

6.编辑hdfs-site.xml
^^^^^^^^^^^^^^^^^^^

.. code:: 

    sudo gedit /usr/local/hadoop/etc/hadoop/hdfs-site.xml

.. code:: xml

    <configuration>
    <property>
        <name>dfs.replication</name>
        <value>3</value>

    </property>

    <property>
        <name>dfs.datanode.data.dir</name>
        <value> file:/usr/local/hadoop/hadoop_data/hdfs/datanode</value>

    </property>

7.重启
^^^^^^

data2、data3、master做上述同样操作即可.

注意相关文件替换，比如data2主机中得修改hostname为data2，举一反三即可。

4.设置master服务器.
~~~~~~~~~~~~~~~~~~~

按照data1操作设置完毕---修改hostname

1.修改hdfs-site.xml文件
^^^^^^^^^^^^^^^^^^^^^^^

.. code:: 

    sudo gedit /usr/local/hadoop/etc/hadoop/hdfs-site.xml

.. code:: xml

    <configuration>
    <property>
        <name>dfs.replication</name>
        <value>3</value>
    </property>
    <property>
        <name>dfs.namenode.name.dir</name>
        <value> file:/usr/local/hadoop/hadoop_data/hdfs/namenode</value>
    </property>
    </configuration>

2.编辑master文件
^^^^^^^^^^^^^^^^

.. code:: 

    # 没有就先自己创建再编辑
    sudo gedit /usr/local/hadoop/etc/hadoop/masters

::

    # 写入以下内容
    master

3.编辑slaves文件
^^^^^^^^^^^^^^^^

.. code:: 

    sudo gedit /usr/local/hadoop/etc/hadoop/slaves

::

    # 设置data1,data2,data3
    data1
    data2
    data3

4.重启
^^^^^^

5.SSH无密码连接.
~~~~~~~~~~~~~~~~

1.master主机操作
^^^^^^^^^^^^^^^^

::

    ssh-keygen

::

    cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys

::

    ssh localhost

::

    # 主机公钥通过ssh连接复制给data1,如果要复制给data2/data3,修改IP即可
    scp ~/.ssh/id_rsa.pub hduser@192.168.56.101:/home/hduser/

2.data1/data2/data3操作
^^^^^^^^^^^^^^^^^^^^^^^

::

    cat ~/id_rsa.pub >> ～/.ssh/authorized_keys

3.最后在master主机中操作
^^^^^^^^^^^^^^^^^^^^^^^^

::

    ssh data1

6.创建并格式化NameNode HDFS.
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

    hadoop namenode -format

7.启动Hadoop Multi Node Cluster
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

    start-all.sh

或者

::

    start-dfs.sh
    start-yarn.sh

8.查看master的进程是否启动
~~~~~~~~~~~~~~~~~~~~~~~~~~

::

    >查看启动的进程

        jps

    >显示

        4646 jps
        4350 ResourceManager
        3998 NameNode
        4185 SecondaryNameNode

    ### &nbsp;&nbsp;8.查看data1/data2/data3的进程是否启动
    >分别输入

        ssh data1
        jps
        exit

    >jps显示

        1956 NodeManager
        1872 DataNode
        2044 Jps

9.打开Hadoop ResourceManager Web界面
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

    http://master:8088/

.. figure:: http://p20tr36iw.bkt.clouddn.com/hadoopq.png
   :alt: 

10.打开NameNode Web界面
~~~~~~~~~~~~~~~~~~~~~~~

::

    http://master:50070/

.. figure:: http://p20tr36iw.bkt.clouddn.com/node.png
   :alt: 

四、问题解决
------------

::

    >1.ssh连接远程主机时，出现 sign_and_send_pubkey: signing failed: agent refused operation 错误，并且还是需要输入密码

    表示ssh-agent 已经在运行了，但是找不到附加的任何keys，就是说你生成的key，没有附加到ssh-agent上，需要附加一下，执行ssh后出现上述错误。

    解决办法：依次执行下列代码

    eval "$(ssh-agent -s)"

    ssh-add

    >2.ssh无密码登录失败，还是需要输入密码登录

    解决办法:清除.ssh文件夹里面所有文件，然后按照上述操作即可。

    >A@B 终端中依次代表 用户名@主机名

    >ssh data1连接后，要记得用exit退出再去操作！

五、参考文章
------------

-  1.\ `SSH无密码 <http://www.linuxidc.com/Linux/2015-01/112032.htm>`__

-  2.\ `sign\_and\_send\_pubkey: signing failed: agent refused
   operation <https://www.cnblogs.com/chjbbs/p/6637519.html>`__
