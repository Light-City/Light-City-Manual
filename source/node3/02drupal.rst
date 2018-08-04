
.. figure:: http://p20tr36iw.bkt.clouddn.com/drup.png
   :alt: 

第二节:CetOS 7.3安装Drupal 8踩坑之路
=========================================

1. 实战
-------

1.1 drupal 8.x需求
~~~~~~~~~~~~~~~~~~

DataBase
^^^^^^^^

-  MySQL 5.5.3/MariaDB 5.5.20/Percona Server 5.5.8 or higher with PDO
   and an InnoDB-compatible primary storage engine,

-  PostgreSQL 9.1.2 or higher with PDO,

-  SQLite 3.7.11 or higher

PHP
^^^

PHP 5.5.9 or higher

1.2 部署LAMP
~~~~~~~~~~~~

本节参考之前文章\ `燃火搭建CetOS7.3
LAMP服务器 <http://blog.csdn.net/guangcheng0312q/article/details/54176549>`__
由于我当初部署LAMP的PHP是5.4.\* 版本的，根据drupal
8安装需求，需要升级成PHP 5.5.9+，以下就是我在升级过程中的一些Q&A！

2.PHP5.4-PHP5.6
---------------

2.1 Centos7 中php5.4升级到php5.6
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

2.1.1 添加EPEL和Remi源
^^^^^^^^^^^^^^^^^^^^^^

::

    wget http://dl.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-7-5.noarch.rpm

    wget http://rpms.famillecollet.com/enterprise/remi-release-7.rpm

    rpm -Uvh remi-release-7*.rpm epel-release-7*.rpm

2.1.2 使用Remi repo
^^^^^^^^^^^^^^^^^^^

::

    vim /etc/yum.repos.d/remi.repo

    [remi]

        name=Les RPM de remi pour Enterprise Linux 6 - $basearch
        #baseurl=http://rpms.famillecollet.com/enterprise/6/remi/$basearch/
        mirrorlist=http://rpms.famillecollet.com/enterprise/6/remi/mirror
        enabled=1
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-remi

        [remi-php56]

        name=Les RPM de remi de PHP 5.5 pour Enterprise Linux 6 - $basearch
        #baseurl=http://rpms.famillecollet.com/enterprise/6/php55/$basearch/
        mirrorlist=http://rpms.famillecollet.com/enterprise/6/php55/mirror
        # WARNING: If you enable this repository, you must also enable "remi"

        enabled=1
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-remi

*把enabled设为1.使之起作用。
注意：如果想吧PHP5.4升级为PHP5.5，则把［remi-php55］的enabled设置为1*

2.1.3 停止相关服务
^^^^^^^^^^^^^^^^^^

::

    service httpd stop
    service mysqld stop

2.1.4 更新及查看
^^^^^^^^^^^^^^^^

::

    yum update -y
    yum update php
    这个时候会提醒更新php到5.6，通过以下命令查看是否完成升级
    php -v
    然后启动服务
    service httpd start
    service mysqld start
    这个时候就可以用了。

2.2 PHP升级 Q&A
~~~~~~~~~~~~~~~

2.2.1 PHP升级方法一
^^^^^^^^^^^^^^^^^^^

Q：上述php-v后出现如下错误，PHP Startup:Xache:Unble to initalize module
|image0|

A:1)stackoverflow上解决办法：使用pecl 去更新Xcache模块，例:pecl upgrade
Xcache,但是我使用出现错误，故没采用此方法!具体的参考链接详细解释，请看参考文章！

2）输入\ ``yum -y install php-xcache``\ ，然后php
-v,没有出现此错误，但是出现PHP Warning: Module 'modulename' already
loaded in Unknown on line 0，

3）PHP Warning: Module 'modulename' already loaded in Unknown on line 0
|image1| A: 原因：

在PHP中加载大多数扩展有两种方法。 一个是通过将扩展直接编译成PHP二进制。
另一种是通过ini文件动态加载共享扩展。
错误表明动态扩展正在通过.ini文件加载，即使它们已经编译成PHP二进制文件。

解决：

要解决这个问题，您必须编辑您的php.ini（或extensions.ini）文件，并注释掉已经编译的扩展。
例如，编辑后，您的ini文件可能看起来像下面的行：

::

    ;extension=pcre.so
    ;extension=spl.so
    ;extension=simplexml.so
    ;extension=session.so
    ;extension=exif.so

你也可以删除这些行，而不是注释掉。 一旦你禁用了这些行，运行php
-v，看看警告是否消失。

2.2.2 PHP升级方法二(建议采用本方法解决)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1)CentOS上PHP完全卸载
'''''''''''''''''''''

-  step 1:首先查看机器上安装的所有php相关的rpm包

::

    [root@localhost nginx]# rpm -qa | grep php
    php-cli-5.3.3-22.el6.x86_64
    .....
    php-pear-1.9.4-4.el6.noarch

-  step 2:按依赖顺序进行删除

::

    rpm -e php-fpm-5.3.3-22.el6.x86_64
    rpm-e php-pdo-5.3.3-22.el6.x86_64
    rpm -e php-pear-1.9.4-4.el6.noarch
    rpm-e php-cli-5.3.3-22.el6.x86_64
    rpm -e php-5.3.3-22.el6.x86_64
    rpm-e php-xml-5.3.3-22.el6.x86_64
    rpm -e php-gd-5.3.3-22.el6.x86_64
    rpm-e php-common-5.3.3-22.el6.x86_64

Q:rpm卸载软件忽略循环依赖
当卸载到php-pecl-jsonc-1.3.10-1.el6.remi.5.6.x86\_64和php-pecl-zip-1.13.4-1.el6.remi.5.6.x86\_64的时候出现以下的错误：

::

    [iteblog@iteblog.com ~] $ rpm -e php-pecl-jsonc-1.3.10-1.el6.remi.5.6.x86_64
    error: Failed dependencies:php-pecl-jsonc(x86-64) is needed by (installed) php-common-5.6.25-0.1.RC1.el6.remi.x86_64

    [iteblog@iteblog.com ~] $ rpm -e php-pecl-zip-1.13.4-1.el6.remi.5.6.x86_64
    error: Failed dependencies: php-pecl-zip(x86-64) is needed by (installed) php-common-5.6.25-0.1.RC1.el6.remi.x86_64

A:此时用rpm --nodeps -e

::

    [root@iteblog.com ~] $ rpm --nodeps -e php-common-5.6.25-0.1.RC1.el6.remi.x86_64
    [root@iteblog.com ~] $ rpm --nodeps -e php-pecl-zip-1.13.4-1.el6.remi.5.6.x86_64
    [root@iteblog.com ~] $ rpm --nodeps -e php-pecl-jsonc-1.3.10-1.el6.remi.5.6.x86_64
    [root@iteblog.com ~] $ rpm -qa|grep php

终于卸载干净了！有困难找man啊！

2)CentOS上安装PHP5.5
''''''''''''''''''''

::

    yum install –enable-opcache  php55w php55w-opcache php55w-mbstring php55w-gd php55w-xml php55w-pear php55w-fpm php55w-mysql

**说明：–enable-opcache，安装PHP5.5同时开启**

3.安装Drupal 8
--------------

3.1 Drupal 8下载
~~~~~~~~~~~~~~~~

下载

::

    ## 1.wget下载
    # wget https://ftp.drupal.org/files/projects/drupal-8.1.1.tar.gz
    ## 2.解压缩到apache
    # tar xvfz drupal-8.1.1.tar.gz -C /var/www/html
    ### 3.重命名
    # cd /var/www/html
    # mv drupal-8.1.1 drupal
    ## 4.修改权限
    # chown -R apache:apache /var/www/html/drupal/
    ## 5.复制配置文件
    # cd /var/www/html/drupal/sites/default
    # cp -p default.settings.php settings.php

新建数据库

::

    # mysql -u root -p
    create database drupal_db;
    CREATE USER db_user@localhost IDENTIFIED BY 'Durpal@123#';
    GRANT ALL PRIVILEGES ON drupal_db.* TO db_user@localhost;
    FLUSH PRIVILEGES;
    exit;

打开浏览器http://your.ip/drupal

3.2安装drupal 8
~~~~~~~~~~~~~~~

在浏览器输入：localhost/drupal,接着傻瓜式安装，注意数据库配置，使用上面创建的数据库drupal\_db和用户db\_user以及相应的密码！

3.3 安装Q&A
~~~~~~~~~~~

Q:安装时检查安装需求时，opcache和clean url(简洁链接)问题解决

A:1）启用opcache(上述已经安装过opcache) 在php.ini文件中添加如下配置：

::

    zend_extension=opcache.so
    [opcache]
    opcache.memory_consumption=128
    opcache.interned_strings_buffer=8
    opcache.max_accelerated_files=4000
    opcache.revalidate_freq=60
    opcache.fast_shutdown=1
    opcache.enable=1
    opcache.enable_cli=1

然后检查下phpinfo,这个参考之前文章\ `燃火搭建CetOS7.3
LAMP服务器 <http://blog.csdn.net/guangcheng0312q/article/details/54176549>`__

2)drupal的clean url(简洁链接)问题解决 在apache目錄下修改httpd.conf.
文件:/apache/conf/httpd.conf.

在文件中 确定开启mod\_rewrite模块 如果尚未开放把前面的#号去掉 LoadModule
rewrite\_module modules/mod\_rewrite.so
在httpd.conf文件中的AllowOverride none代碼 全部替换成 AllowOverride All
这个是以保证重写可以启用 其他的不用改了，最后 重启apache服务：

::

    systemctl restart httpd

Q:DateTime::createFromFormat(): It is not safe to rely on the system's
timezone settings. You are required to use the date.timezone setting or
the date\_default\_timezone\_set() function. In case you used any of
those methods and you are still getting this warning, you most likely
misspelled the timezone identifier. We selected the timezone 'UTC' for
now, but please set date.timezone to select your timezone. in 



A:修改php.ini文件，设置为date.timezone = Asia/XX
(XX为自己的用户名，随意！)

4.参考文章
----------

-  `CentOS 7.x 安装drupal
   8 <http://www.centoscn.com/image-text/install/2016/0515/7229.html>`__

-  `PHP Warning: Module 'modulename' already loaded in Unknown on line
   0 <http://www.somacon.com/p520.php>`__

-  `PHP Warning: PHP Startup: ????????: Unable to initialize
   module <http://stackoverflow.com/questions/3130910/php-warning-php-startup-unable-to-initialize-module>`__

-  `Apache is “Unable to initialize module” because of module's and
   PHP's API don't match after changing the PHP
   configuration <http://stackoverflow.com/questions/2394532/apache-is-unable-to-initialize-module-because-of-modules-and-phps-api-dont>`__

-  `在CentOS 7上安装Drupal
   8 <http://blog.topspeedsnail.com/archives/3090>`__

-  `Centos7
   把php5.4升级到php5.6 <https://my.oschina.net/volash86/blog/494175>`__

-  `CentOS上PHP完全卸载 <http://blog.csdn.net/dc_726/article/details/9519293>`__

-  `rpm卸载软件忽略循环依赖 <https://www.iteblog.com/archives/1731>`__

-  `PHP5.5+启用OPCache <http://blog.csdn.net/iefreer/article/details/38349057>`__

-  `drupal的clean
   url(简洁链接)问题解决 <http://blog.csdn.net/gaogao0603/article/details/7238721>`__

-  `PHP DateTime throws timezone warning even though date.timezone
   set <http://stackoverflow.com/questions/20061861/php-datetime-throws-timezone-warning-even-though-date-timezone-set>`__

-  `drupal8提示主机信任问题解决方法 <http://www.fourye.com/?p=423>`__

.. |image0| image:: https://github.com/asdfghjklqqq2/asdfghjklqqq2.github.io/blob/master/img/drupal/2.png?raw=true
.. |image1| image:: https://github.com/asdfghjklqqq2/asdfghjklqqq2.github.io/blob/master/img/drupal/1.png?raw=true
.. |image2| image:: https://github.com/asdfghjklqqq2/asdfghjklqqq2.github.io/blob/master/img/drupal/3.png?raw=true

