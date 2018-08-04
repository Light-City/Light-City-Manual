第八节:Vmware Ubuntu安装virtual tools
=========================================

一、su认证失败
--------------

.. figure:: http://p20tr36iw.bkt.clouddn.com/su.jpg
   :alt: 

-  

   1. Ubuntu安装后，有两种登录方式，一开始我进入的是guest模式，切换sudo
      passwd输入密码报错，原因需进入桌面用户来进行。

-  

   2. Ubuntu安装后，root用户默认是被锁定了的，不允许登录，也不允许 "su"
      到 root.

::

    francis@francis-virtual-machine:~$ su
    密码：                 #输入安装时root用户的密码
    su：认证失败

二、允许su到root
----------------

::

    francis@francis-virtual-machine:~$ sudo passwd
    密码：                            #输入安装时那个用户的密码
    输入新的 UNIX 密码：               #新的Root用户密码
    重新输入新的 UNIX 密码：            #重复新的Root用户密码
    passwd：                          #已成功更新密码
    francis@francis-virtual-machine:~$ su
    密码：<--输入重置的新密码
    root@francis-virtual-machine:/home/francis/桌面   #已经进入root用户

三、问题
--------

客户机操作系统已将 CD-ROM 门锁定，并且可能正在使用
CD-ROM，这可能会导致客户机无法识别介质的更改。如果可能，请在断开连接之前从客户机内部弹出
CD-ROM。确实要断开连接并覆盖锁定设置吗?

找到VMware安装根目录，寻找linux.iso文件，在虚拟机设置中更换ISO镜像文件为linux.iso，类似的如果是在win系统中则更换win.iso。

四、安装
--------

1.找VmTools
~~~~~~~~~~~

现在再开始进入系统后，在VMware菜单栏找到安装虚拟工具的时候，它会弹出一个文件夹，里面就有VMware
Tools的安装包。

2.拷贝VMwareTools至桌面
~~~~~~~~~~~~~~~~~~~~~~~

3.tar -xzvf VMwareTools-xxxx-xxx.tar.gz
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

4.执行：sudo ./wmware-install.pl
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

5.reboot
~~~~~~~~

现在就可以实现虚拟机系统与本机文件复制了。
