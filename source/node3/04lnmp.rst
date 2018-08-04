.. figure:: http://p20tr36iw.bkt.clouddn.com/lnmp.png
   :alt: 

第四节:Cetos7.2搭建LNMP与WordPress
===================================

一、前言
--------

  本文基于腾讯云校园活动，1元购买的服务器，进行操作实战，服务器端安装Cetos7.2，利用XShell自行搭建LNMP，并使用WordPress搭建网站。
之前写过一篇Cetos7.3下搭建LAMP,不懂的可以参考：\ `燃火搭建CetOS7.3
LAMP服务器 <http://blog.csdn.net/guangcheng0312q/article/details/54176549>`__\ ，同理LNMP搭建原理大同小异。于是，进行本次操作实战。

<font
color=red注意：具体的购买服务器跟XShell远程连接服务器本文不做阐述，重点放在LNMP搭建及WordPress相关问题上。</font

二、LNMP搭建
------------

1.Linux:Cetos7.2
~~~~~~~~~~~~~~~~

2.Nginx
~~~~~~~

1)安装
^^^^^^

::

    yum install nginx

2)配置
^^^^^^

::

    vi /etc/nginx/nginx.conf

修改配置文件

::

    server {
        listen       80 default_server;
        listen       [::]:80 default_server;
        server_name  blog.codingplayboy.com;
        root         /usr/share/nginx/html;

        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;

        location / {
            root /usr/share/nginx/html/blog-wp;
            index index.php index.html index.htm;
        }

        error_page 404 /404.html;
            location = /40x.html {
            root /usr/share/nginx/html;
        }

        error_page 500 502 503 504 /50x.html;
            location = /50x.html {
            root /usr/share/nginx/html;
        }

        rewrite /wp-admin$ $scheme://$host$uri/ permanent;
        location ~ \.php$ {
            root /usr/share/nginx/html/blog-wp;
            fastcgi_pass 127.0.0.1:9000;  //php-fpm
            fastcgi_index index.php;
            fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
            include fastcgi_params;
        }
    }

3)启动
^^^^^^

::

    systemctl start nginx

3.MySQL
~~~~~~~

1)安装MySQL级启动状态设置
^^^^^^^^^^^^^^^^^^^^^^^^^

查资料发现是CentOS 7
版本将MySQL数据库软件从默认的程序列表中移除，用mariadb代替了。

为了要安装MySQL，我选择的是去官网http://dev.mysql.com/downloads/repo/yum/下载安装包。我下载的是mysql57-community-release-el7-9.noarch.rpm文件。

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

2) 配置文件
^^^^^^^^^^^

/etc/my.cnf：这是MySQL的配置文件 /var/lib/mysql：这是数据库实际存放目录
/var/log/mysqld.log：这是MySQL的错误日志文件

3)实际操作
^^^^^^^^^^

::

    [root@localhost ~]# mysql -u root -p
    Enter password:
    mysql create database wordpress;
    mysql grant all on wordpress.* to username identified by "password";
    mysql exit

4)权限
^^^^^^

修改数据库文件权限：

::

    chown mysql:mysql /var/lib/mysql -R

4.PHP
~~~~~

1）安装
^^^^^^^

::

    sudo yum -y install php php-fpm php-mysql php-gd php-ldap php-odbc php-pear php-xml php-xmlrpc php-mbstring php-snmp php-soap curl

    sudo yum -y install epel-release

2) 配置
^^^^^^^

::

    vi /etc/php-fpm.d/www.conf   //打开php-fpm配置文件
    找到对应owner,user,group，修改为nginx用户和nginx用户组：

    listen.owner = nginx
    listen.group = nginx
    listen.mode = 0666
    ...
    user = nginx
    group = nginx

3) 启动
^^^^^^^

::

    systemctl start php-fpm
    systemctl enable php-fpm
    //设置开机自启动php-fpm

三、WordPress
-------------

::

    cd /usr/share/nginx/html/    //进入网站根目录
    mkdir blog-wp                //创建blog-wp
    wget https://cn.wordpress.org/wordpress-4.7.2-zh_CN.zip  //下载WordPress
    unzip wordpress-4.7.2-zh_CN.zip   //解压wordpress-4.7.2-zh_CN.zip
    权限设置:解决"Access Denied，403Forbidden禁止访问"错误
    chown -R nginx:nginx /usr/share/nginx/html/blog-wp
    chmod -R 774 /usr/share/nginx/html/blog-wp
    服务器公网IP+wordpress即可进入WordPress安装。

四、MySql及WordPress相关Q&A
---------------------------

::

    Q:初次登录回车失败报错：ERROR 1045 (28000): Access denied for user 'mysql'@'localhost' (using password: NO)
    A:解决办法：`vi /etc/my.cnf`看到mysql的日志文件目录，查看日志目录文件more /var/log/mysqld.log，看到Note] A temporary passwor
    d is generated for root@localhost:` *******`，`*****代表密码`。然后在输入mysql -u root -p，后面填入以上密码，回车即可。

    Q:免密码登录mysql或者初次登陆MySQL修改密码是出现Unknown column 'password' in 'field list'的解决方法

    A:`vim /etc/my.cnf` 加入`skip-grant-tables`
    在symoblic-links=0下面添加本行代码，然后使用命令`systemctl restart mysqld`重启mysql,终端输入mysql或者mysql -u root -p登录mysql，然后use mysql,下来`update mysql.user set authentication_string=password('root') where user='password';`进行上述操作后，使用mysql -u root -p
    进入mysql，输入上述配置的密码登录进去即可。

    Q:进行上述操作后，登录至mysql，但是输入命令，出现MySQL 报错 *ERROR 1820 (HY000): You must reset your password using ALTER USER statement before executing this statement*

    A:set password = password('123456');

    Q:在创建本地用户或者如上述操作设置密码时，*ERROR 1819 (HY000): Your password does not satisfy the current policy requirements*，怎么解决？

    A:默认情况下MySQL 5.7+拥有密码验证系统。如果你不想严格遵守政策，并需要分配自己的，那么只是禁用密码验证和重新启动mysqld进程。
    先编辑my.cnf文件
    `vi /etc/my.cnf`
    plugin-load=validate_password.so
    validate-password=OFF

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

    Q:启动mysql错误解决方案，mysql.sock丢失，mysqld_safe启动报错
    重启了一次服务器后，使用`mysql -u root -p`登陆是出现下面的错误:
    *ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/tmp/mysql.sock' (2)*

    A:使用重启命令init 6解决，同样，如果在wordpress访问时出现数据库连接错误，先`systemctl restart mysqld`，如果出现错误，mysql服务未重启，则采取init 6便可解决！
    Q:“您的 PHP 似乎没有安装运行 WordPress 所必需的 MySQL 扩展”处理方法

    A:第一步：先用SSH登录，打开PHP.ini
    #vi /etc/php.ini
    第二步：php.ini中 添加
    extension=mysql.so
    第三步：在PHP.ini 中找到如下
    extension_dir = "XXXXXXX"
    注：XXX指扩展安装目录，centos64位的主机一般安装在extension_dir = "/usr/lib64/php/modules"
    第四步：找到这个扩展安装目录，确认是否有mysql.so这个文件，如果没有，下载
    重启服务

五、参考链接
------------

1.\ `LNMP搭建WordPress博客 <http://blog.codingplayboy.com/2017/06/04/lnmp_wordpress/>`__

2.\ `linux重启命令init
6和reboot的区别 <http://moper.me/linux-restart-init-6-reboot.html>`__

3.\ `MySQL 报错 ERROR 1820 (HY000): You must reset your password using
ALTER USER statement before executing this
statement <http://www.jianshu.com/p/53ac2d55b279>`__

4.\ `“您的PHP似乎没有安装运行WordPress所必需的MySQL扩展。”
的解决方法 <http://2233426.blog.51cto.com/2223426/1264128>`__

5.\ `修改MySQL
5.7.9版本的root密码方法以及一些新变化整理 <http://bbs.bestsdk.com/detail/762.html>`__

6.\ `初次登陆MySQL修改密码是出现Unknown column 'password' in 'field
list'的解决方法 <http://www.itwendao.com/article/detail/140724.html>`__

7.\ `启动mysql错误解决方案，学会查看错误日志：mysql.sock丢失，mysqld\_safe启动报错 <http://www.cnblogs.com/super-lucky/p/superlucky.html>`__
