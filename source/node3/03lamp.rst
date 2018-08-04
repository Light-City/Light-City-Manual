第三节:燃火搭建CetOS7.3 LAMP服务器
======================================

.. figure:: http://p20tr36iw.bkt.clouddn.com/lamp.png
   :alt: 

1.前言
------

系统：CetOS7.3(CentOS-7-x86\_64-DVD-1611)

LAMP:Linux+Apache+MySQL+PHP，是用于搭建web服务器的一种解决方案。虽然从RHEL
7开始Red
Hat公司推荐使用MariaDB而不是MySQL，但在我这篇文章当中，我还是决定继续使用MySQL。

2.LAMP搭建
----------

2.1 Apache
~~~~~~~~~~

1)安装Apache及启动状态设置
^^^^^^^^^^^^^^^^^^^^^^^^^^

``yum install httpd``

Apache软件的软件包名称叫做httpd,根据红帽官方文档说明，RHEL 7 (或CentOS
7)上可用的Apache版本是2.4版。我安装的便是version
2.4。安装完成后，Apache是以httpd服务的形式存在的。因此，要启动Apache并将其设置为开机启动，就使用命令：

``systemctl start httpd.service``

``systemctl enable httpd.service``

然后，检查httpd服务状态：

``systemctl status httpd.service``

终端出现“enabled”表示httpd服务已设为开机启动，“active（running）”则表示httpd服务正在运行中。

2）防火墙开放80端口
^^^^^^^^^^^^^^^^^^^

这样的话，HTTP协议就已被启动起来了，由于HTTP协议使用到tcp端口80，因此防火墙要放通tcp端口80：
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

::

    firewall-cmd --zone=public --add-port=80/tcp --permanent

重启防火墙以让更改立刻生效：
''''''''''''''''''''''''''''

::

     firewall-cmd --reload

使用以下命令检查配置是否成功：
''''''''''''''''''''''''''''''

::

    firewall-cmd --list-all

终端中出现ports:80/tcp表示成功！

3）测试localhost，显示默认页面
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

然后，就可以在本机上使用浏览器来访问刚刚搭建的web服务器了。不过，因为这个时候还未创建任何页面，所以它显示的是Apache软件自带的测试页面.
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

浏览器顶端输入\ ``localhost``\ ，显示如下界面：
'''''''''''''''''''''''''''''''''''''''''''''''

|image0| #### 4)设置Apache配置文件
Apache软件的主配置文件为\ ``/etc/h ttpd/conf/httpd.conf``

``vi /etc/httpd/conf/httpd.conf``

我只修改了下面这行，其他的没变，更多的请到在网页\ http://httpd.apache.org/docs/2.4/en/\ 中查阅到！

**提醒：修改之前，多备份几个原配置文件！**

AddDefaultCharset Off

//AddDefaultCharset会强制客户端浏览器使用指定的字符集编码方式。这可能会有问题，所以要将它关闭。

根据配置文件,可以知道，默认情况下，\ *网页文档可以放置在/var/www/html目录下*\ ，\ *CGI脚本可以放置在/var/www/cgi-bin目录下；错误日志在/etc/httpd/logs/error\_log，访问日志在/etc/httpd/logs/access\_log。*
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

5)测试配置文件
^^^^^^^^^^^^^^

::

    [root@localhost ~]#apachectl configtest
    然后，重启httpd服务：
    [root@localhost ~]# systemctl restart httpd

Q:vi如何替换多个字符串?

A::n,$s/my/you/g 替换第 n 行开始到最后一行中每一行所有 my 为 you

2.2 PHP
~~~~~~~

1)安装PHP
^^^^^^^^^

::

    yum install php

2)配置文件
^^^^^^^^^^

安装完成后，PHP会生成配置文件/etc/httpd/conf.d/php.conf，因为该配置文件在/etc/httpd/conf.d目录下，所以它会被Apache所读取。PHP还会生成配置文件/etc/httpd/conf.modules.d/10-php.conf，该配置文件也会被Apache所读取，它的设定让Apache可以加载PHP模块。不过，PHP软件本身的配置文件其实是/etc/php.ini。

可根据自己的需要去修改配置文件，注意：多备份！
''''''''''''''''''''''''''''''''''''''''''''''

::

    然后，重启httpd服务：
    [root@localhost~]# systemctl restart httpd

3)测试Apache调用PHP
^^^^^^^^^^^^^^^^^^^

为了测试Apache能不能正常调用PHP，在/var/www/html目录下新建一个phpinfo.php文档(\ ``vim /var/www/html/phpinfo.php``)，内容如下所示：
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

::

    <?php phpinfo();?

其中的\ ``<?php   ?``\ 是PHP程序的语法，phpinfo
();则是PHP程序提供的一个函式库，该函式库可以显示出你这个web服务器的相关信息。然后，使用浏览器来访问服务器的这个文件，看看页面能不能正常打开。
浏览器输入

::

    localhost/phpinfo.php

出现下面界面表示正常：
''''''''''''''''''''''

|image1| ### 2.3 MySQL

1)安装MySQL级启动状态设置
^^^^^^^^^^^^^^^^^^^^^^^^^

查资料发现是CentOS 7
版本将MySQL数据库软件从默认的程序列表中移除，用mariadb代替了。

为了要安装MySQL，我选择的是去官网http://dev.mysql.com/downloads/repo/yum/下载安装包。我下载的是mysql57-community-release-el7-9.noarch.rpm文件。
'''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

::

    mv ...(下载文件的地方) /root
    # 表示将下载包移动到root目录
    yum localinstall mysql57-community-release-el7-9.noarch.rpm
    # 将MySQL Yum Repository添加到系统的软件库列表（repositorylist）
    yum repolist enabled | grep mysql
    # 检查添加是否成功
    yum install mysql-community-server
    # 安装MySQL
    安装完成后
    systemctl start mysqld
    # 启动mysqld服务
    systemctl enable mysqld
     # 设为开机启动
    systemctl status mysqld
    # 检查mysqld服务状态

    netstat -atulpn | grep mysqld
     # 查看mysqld服务侦听端口

MySQL侦听tcp端口3306。但因为防火墙并未放通该端口，所以从其它设备上是无法访问本服务器的MySQL数据库的。但因为这里的MySQL也仅是提供给本机的PHP使用的，所以也就不必放通tcp端口3306。

2) 配置文件
^^^^^^^^^^^

/etc/my.cnf：这是MySQL的配置文件 /var/lib/mysql：这是数据库实际存放目录
/var/log/mysqld.log：这是MySQL的错误日志文件

3)实际操作
^^^^^^^^^^

::

    [root@localhost ~]# mysql -u root -p
    Enter password:
    mysql create user'myuser'@'localhost' identified by '1234';  //新建本地用户myuser，密码为1234
    mysql create database mydb;   //新建数据库mydb
    mysql grant all privileges on mydb.*to myuser@localhost;  //将数据库mydb的所有权限授权给本地用户myuser
    mysql flush privileges;  //刷新系统权限表
    mysql use mysql; //进入数据库mysql（该数据库为系统自带）
    mysql select * from user where user ='myuser'; //查询数据库mysql中是否存在用户myuser
    mysql show databases;   //显示所有已有的数据库
    mysql exit

4)问题解决
^^^^^^^^^^

::

    Q:在创建本地用户时，ERROR 1819 (HY000): Your password does not satisfy the current policy requirements，怎么解决？

    A:默认情况下MySQL 5.7+拥有密码验证系统。如果你不想严格遵守政策，并需要分配自己的，那么只是禁用密码验证和重新启动mysqld进程。
    先编辑my.cnf文件
    `vi /etc/my.cnf`

    in [mysqld]

    `validate-password=off`

    保存文件，并重启mysql

    `sudo service mysqld restart` or `systemctl restart mysqld`

    Q:上个问题解决后，继续创建本地用户，遇到错误ERROR 1054 (42S22): Unknown column 'password_last_changed' in 'mysql.user'

    A:字段'password_last_changed'在MySQL <5.7的版本中存在, 但是在5.7，给删除了。

    所以升级了mysql server之后，你还有运行’mysql_upgrade’ 脚本把tables从老版本中迁移到新版本。

    `mysql_upgrade -u root -p`

    然后`systemctl restart mysqld`或者`service mysql restart`重启MySQL服务器，问题就解决了。

    Q:如果您是第一次进行安装并想要知道临时密码，请使用以下方法查找第一次密码

    A:`grep 'temporary password' /var/log/mysqld.log`

    Q:更改root密码

    A:`mysql_secure_installation` or `/usr/bin/mysql_secure_installation`

2.4 PHP-MySQL
~~~~~~~~~~~~~

PHP-MySQL是一个用于让PHP程序使用MySQL数据库的模块


1)安装PHP-MySQL
^^^^^^^^^^^^^^^

::

    yum install php-mysql

2)重启httpd服务
^^^^^^^^^^^^^^^

::

    systemctl restart httpd

3)测试PHP能否连接到MySQL数据库
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

在/var/www/html目录下新建一个文档test.php。因为之前已经在MySQL中新建了一个数据库mydb，并给这个数据库建了个用户myuser，密码是1234，所以test.php的内容是这样的：
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

::

    <?php
    $link=mysql_connect("localhost","myuser","1234");
    if(!$link) echo "FAILD!fail......";
    else echo "OK!make it";
    ?

.. figure:: https://github.com/asdfghjklqqq2/asdfghjklqqq2.github.io/blob/master/img/lamp/2.png?raw=true
   :alt: 

Q:测试成功后，查看Apache的错误日志文件，还是发现有报错“mysql\_connect():Headers
and client library minor version mismatch”，该怎么解决？ |image2|
|image3| A:这样的错误是由于高版本的MySQL，低版本的MySQL Client
API引起的，我在CentOS
7上安装MySQL-Server的时候出现了这个错误，解决办法： 卸载PHP-mysql

::

     yum remove php-mysql -y
     安装php-mysqlnd

     yum install php-mysqlnd -y
     重启httpd

     systemctl restart httpd.service

4)Apache联机，修改SELinux规则
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

    让SELinux规则放行：
    [root@www ~]# setsebool -P httpd_can_network_connect=1
    然后确认一下修改是否生效：
    [root@www ~]# getsebool httpd_can_network_connect

至此为止，基本的LAMP平台已经架设好了！
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

3.XCache配置
------------

3.1 XCache简介
~~~~~~~~~~~~~~

为了优化LAMP平台，所以还需要进行一些工作。
XCache是一款开源的PHP缓存器/优化器，它通过把编译 PHP
后的数据缓冲到共享内存从而避免重复的编译过程,
使客户端访问时服务器能够直接使用缓冲区已编译的代码从而提高速度，同时降低服务器负载。到我写这篇文章为止，XCache最新的版本为3.2.0。我选择使用官网提供的源码来安装XCache。

3.2 安装XCache
~~~~~~~~~~~~~~

源码编译安装需要安装gcc：

::

    yum install gcc

再安装php-devel，它用于让PHP可以支持扩展工具（如XCache）：

::

    yum install php-devel

然后，使用XCache官网提供的源码来安装XCache：

::

    然后，使用XCache官网提供的源码来安装XCache：
    [root@www ~]# wget http://xcache.lighttpd.net/pub/Releases/3.2.0/xcache-3.2.0.tar.gz
    [root@www ~]# tar -zxfxcache-3.2.0.tar.gz
    [root@www ~]# cd xcache-3.2.0
    [root@www xcache-3.2.0]# phpize --clean
    [root@www xcache-3.2.0]# phpize
    [root@www xcache-3.2.0]# ./configure--enable-xcache
    [root@www xcache-3.2.0]# make
    [root@www xcache-3.2.0]# make install
    [root@www xcache-3.2.0]# cat xcache.ini /etc/php.ini
    //千万注意，有两个号，表示把xcache.ini的配置追加到php.ini后面。如果只有一个号，会把php.ini原有的配置覆盖掉。
    //建议在操作前可以先备份/etc/php.ini文件。

然后，重启httpd服务：

::

    [root@www ~]# systemctl restart httpd

打开phpinfo.php页面页面，搜索xcache，应该可以搜索到关于XCache的相关信息，可以看到XCache是enabled的：

|image4| ### 3.3 XCache配置管理后台

-  1)在/var/www/html目录下新建一个档案，假设为account.php，将这个档案的内容该成如下所示：

   [root@www ~]# vim/var/www/html/account.php <?php echo md5("1234");
   //双引号内可以填入你想要使用的密码 ?

-  2)浏览器访问这个页面，可以得到一串数字和字母的组合，我得到的是81dc9bdb52d04dc20036dbd8313ed055
   #### |image5|

-  3)修改配置文件

   修改/etc/php.ini中的这两个设定的值： xcache.admin.user ="admin"
   //双引号内可以填入你想要使用的用户名 xcache.admin.pass
   ="81dc9bdb52d04dc20036dbd8313ed055" //双引号内填入刚刚得到的那串组合

-  4)将XCache安装包里面的htdocs整个目录复制到/var/www/html目录下：

   cp -a /root/xcache-3.2.0/htdocs /var/www/html

-  5)修改目录htdocs及其子目录和档案的所属用户和组，我选择将其都改为root：
   chown -R root:root /var/www/html/htdocs

-  6)使用restorecon命令来重置目录htdocs及其子目录和档案的SELinux相关设定：
   restorecon -Rv/var/www/html/htdocs

-  7)最后，使用浏览器访问地址localhost/htdocs/cacher/index.php，然后输入你设定的用户名和密码，就可以打开XCache的管理后台了：
   |image6| |image7|

4.参考文章
----------

4.1 `CentOS
7系统上yum搭建LAMP <http://www.centoscn.com/CentosServer/www/2015/0414/5183.html>`__

4.2 `ERROR 1819 (HY000): Your password does not satisfy the current
policy
requirements <http://stackoverflow.com/questions/34913266/create-a-six-character-password-in-mysql-5-7>`__

4.3（同4.2问题，总共2篇） `ERROR 1819 (HY000): Your password does not
satisfy the current policy
requirements <http://www.cnblogs.com/ivictor/p/5142809.html>`__

4.4 `ERROR 1054 (42S22): Unknown column 'password\_last\_changed' in
'mysql.user' <http://blog.csdn.net/qiyueqinglian/article/details/52778106>`__

4.5
`测试PHP连接MYSQL成功与否的代码 <http://www.jb51.net/article/40653.htm>`__

4.6 `mysql\_connect(): Headers and client library minor version
mismatch.
Headers <http://blog.csdn.net/techkuki/article/details/49362041>`__

.. |image0| image:: https://github.com/asdfghjklqqq2/asdfghjklqqq2.github.io/blob/master/img/lamp/10.png?raw=true
.. |image1| image:: https://github.com/asdfghjklqqq2/asdfghjklqqq2.github.io/blob/master/img/lamp/5.png?raw=true
.. |image2| image:: https://github.com/asdfghjklqqq2/asdfghjklqqq2.github.io/blob/master/img/lamp/4.png?raw=true
.. |image3| image:: https://github.com/asdfghjklqqq2/asdfghjklqqq2.github.io/blob/master/img/lamp/1.png?raw=true
.. |image4| image:: https://github.com/asdfghjklqqq2/asdfghjklqqq2.github.io/blob/master/img/lamp/6.png?raw=true
.. |image5| image:: https://github.com/asdfghjklqqq2/asdfghjklqqq2.github.io/blob/master/img/lamp/7.png?raw=true
.. |image6| image:: https://github.com/asdfghjklqqq2/asdfghjklqqq2.github.io/blob/master/img/lamp/8.png?raw=true
.. |image7| image:: https://github.com/asdfghjklqqq2/asdfghjklqqq2.github.io/blob/master/img/lamp/9.png?raw=true

