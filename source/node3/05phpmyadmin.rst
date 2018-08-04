.. figure:: http://p20tr36iw.bkt.clouddn.com/phpmyadmin.jpg
   :alt: 

第五节:CetOS 7.3 配置phpMyAdmin
==================================

1.设置EPEL
----------

::

    1.安装epel仓库：
    在CentOS6和CentOS7都可以执行下面的命令安装epel仓库
    yum -y install epel-release
    这条命令的好处是可以自动安装不同版本的epel，
    比如在CentOS6上面安装的就是epel6，在CentOS7上面安装的就是epel7。
    2.移除epel仓库：
    在CentOS6和CentOS7都可以执行下面的命令移除epel仓库
    yum -y remove epel-release
    3.查看仓库信息：
    yum repolist

或者

::

    wget http://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm

    rpm -ivh epel-release-latest-7.noarch.rpm

    yum repolist      ##检查是否已添加至源列表

OK，检查好已添加至源后就可以进行yum安装了

2.yum安装phpMyAdmin
-------------------

::

    yum install phpmyadmin

3.配置phpMyAdmin
----------------

    默认情况下，CentOS 7上的phpMyAdmin只允许从回环地址(127.0.0
    .1)访问。为了能远程连接，你需要改动它的配置。

::

    编辑phpMyAdmin的配置文件(路径：/etc/httpd/conf.d/phpMyAdmin.conf)，
    找出并注释掉带有"Require ip XXXX"字样的代码行。会有四处这样的代码行，
    用"Require all granted"取而代之。
    vi /etc/httpd/conf.d/phpMyAdmin.conf

    . . . . .
    <Directory /usr/share/phpMyAdmin/>
       AddDefaultCharset UTF-8

       <IfModule mod_authz_core.c>
         # Apache 2.4
         <RequireAny>
           #Require ip 127.0.0.1
           #Require ip ::1
           Require all granted
         </RequireAny>
       </IfModule>
       <IfModule !mod_authz_core.c>
         # Apache 2.2
         Order Deny,Allow
         Deny from All
         Allow from 127.0.0.1
         Allow from ::1
       </IfModule>
    </Directory>

    <Directory /usr/share/phpMyAdmin/setup/>
       <IfModule mod_authz_core.c>
         # Apache 2.4
         <RequireAny>
           #Require ip 127.0.0.1
           #Require ip ::1
           Require all granted
         </RequireAny>
       </IfModule>
       <IfModule !mod_authz_core.c>
         # Apache 2.2
         Order Deny,Allow
         Deny from All
         Allow from 127.0.0.1
         Allow from ::1
       </IfModule>
    </Directory>
    . . . . .

**最后，重启httpd使改动生效。**

::

    systemctl restart httpd

4.测试phpMyAdmin
----------------

    测试phpMyAdmin是否设置成功，访问这个页面：http:///phpmyadmin
    我的是localhost/phpmyadmin，如下图:

5.Q&A
-----

::

    > Q:你在浏览器里尝试连接phpMyAdmin页面的时候，你看到"403 Forbidding"错误：
    You don't have permission to access /phpMyAdmin on this server.
    发生这种错误是因为phpMyAdmin默认阻止了IP地址远程连接。要修复这种错误.

    > A:你需要编辑它的配置文件来允许远程连接。具体操作见上。
    >
    > Q:当你连接phpMyAdmin页面时，你看见"Cannot load mcrypt extension. Please check your PHP configuration"错误信息。
    >
    > A:先`yum install php-mcrypt`然后重启httpd,`systemctl restart httpd`.
    >
    > Q:浏览器输入localhost/phpmyadmin，访问页面为空白.
    >
    > A:有些IE版本会出现这样的问题,换了一下浏览器解决本问题，如果需要根除解决，参考[http://blog.csdn.net/keenx/article/details/6021028](http://blog.csdn.net/keenx/article/details/6021028)
    >
    > Q:登录页面下面出现
    >
        Warning in ./libraries/session.inc.php#105  session_start():open(/var/lib/php/session/sess_lgnlj0ie5b0tv0h6gl4qubbku3bftrlf, O_RDWR) failed: Permission denied (13)

    > A:权限问题：终端输入`chmod -R 777 /var/lib/php/session` ，便可解决问题！

6.参考文章
----------

1.\ `如何在CentOS上安装phpMyAdmin <http://www.lupaworld.com/portal.php?mod=view&aid=248940&page=all>`__
2.\ `访问phpmyadmin时出现空白的解决方案 <http://blog.csdn.net/keenx/article/details/6021028>`__
