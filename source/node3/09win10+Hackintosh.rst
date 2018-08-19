|image0|

第九节:单硬盘安装win10及Hackintosh双系统
===========================================

1.gpt+efi安装win10
------------------

1.1 进入PE分区
~~~~~~~~~~~~~~

-  刻一个efi的U盘，在bios中选择efi引导，此时不需要往U盘里面放系统。
   只是进入PE中进行磁盘分区！
-  磁盘工具及命令分区

::

    安装mac一般efi分区得设置为300M+,win10所分配的efi只有100M,
    这里采用dos创建efi与msr保留分区。
    dos输入:diskpart
    sel disk 0 (选择0号磁盘)
    clean (清除磁盘)
    convert gpt (磁盘转gpt格式)
    create partition efi size=300 (必须)
    create partition msr size=128 (可选)
    exit (退出)
    最后用DiskGenius直接快速创建分区，左下角记得把保留efi分区与创建msr分区勾选上。

1.2 U盘刻win10原版镜像
~~~~~~~~~~~~~~~~~~~~~~

解决用 UEFI 引导 U盘启动映像文件大于 4G

::

    原版镜像中的 install.wim 没有超过 4G 的，如果是我们自己备份的 wim 映像，
    一般都会超过 4G。FAT32 文件系统不支持大于 4G 的文件，而如果用 NTFS 格式的
    文件系统又不支持 UEFI 启动。如果 wim 文件超过 4G 不是太大，可以转换为 esd
    格式；否则，要是想两者兼备，那只能对 U盘分区了。
    U盘分两个区，一个是 FAT32 用来存放引导文件，另一个就是 NTFS 来放置 wim 文件了。
    分区有很多种方式，为了便于操作，我们用软碟通来操作。

`下载原版系统msdn <https://msdn.itellyou.cn/>`__

::

    用软碟通打开镜像 (iso)，进行以下操作：
    boot 文件夹仅保留 boot.sdi，其他删除；
    sources 文件夹仅保留 boot.wim，其他删除；
    efi 文件夹全部保留。
    其他文件和文件夹全部删除。

    然后保存为一个新的iso文件
    插上U盘，点击菜单栏上的“启动” - “写入硬盘映像”。
    写入完成后U盘分为可见分区与UEFI 启动分区。将可见分区格式fat32转化为NTFS格式。
    将下列内容复制进 U盘：
    boot 文件夹以及里面的全部文件；(原iso)
    bootmgr 文件；(原iso)
    sources 文件夹（注意：里面的 install.wim 是自己备份的，名字必须是 install.wim (原iso)

1.3 直接安装
~~~~~~~~~~~~

插入U盘安装即可！

2.mac系统安装
-------------

下载一个mac 10.13.4原版镜像
~~~~~~~~~~~~~~~~~~~~~~~~~~~

准备一个存放mac启动的U盘
~~~~~~~~~~~~~~~~~~~~~~~~

用win10自带的磁盘管理压缩C分区用于安装黑苹果
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

推荐45G左右

用TransMac制作黑苹果安装U盘
~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

    右键U盘先选择formatDisk for mac,再选择Restore with Disk Image,选择下载好的黑苹果镜像dmg文件,会弹出窗口,提示将要格式化USB磁盘,点击OK按钮继续。写入完成，系统弹出将其格式化，点击取消。

替换U盘EFI分区文件
~~~~~~~~~~~~~~~~~~

::

    下载远景论坛的efi配置,然后替换掉U盘EFI分区文件即可！
    复制方法如下:
      打开DiskGenius分区软件删除U盘的EFI目录
      在DiskGenius分区软件(在点击相应盘符后中间位置会有个浏览文件)下用快捷键ctrl+v 把新的EFI复制进去

格式化分区
~~~~~~~~~~

::

    运行Paragon Hard Disk Manage 12，将第三点中压缩出来的未使用分区格式化为Apple HFS

合拼双系统的EFI启动分区
~~~~~~~~~~~~~~~~~~~~~~~

::

    合拼黑苹果EFI和Win10的EFI启动分区
    运行diskpart命令
    按照下面命令挂载自带硬盘的EFI分区
    list disk  //列出目前连接的磁盘
    sel disk 0 //选中disk0磁盘
    list par //列出选中磁盘的所有分区
    sel par 1 //选中第一个分区，默认第一个是ESP/EFI分区，如果不一样按照实际修改。
    ass letter=Z //挂载选中的第一分区为Z盘
    exit  //退出diskpart
    将刚刚做好的U盘EFI分区（也叫做ESP分区）和硬盘里的EFI分区合拼
    以管理员权限运行命令台（开始菜单找到命令行，右键以管理员运行）。输入以下命令，自动将安装U盘里的EFI分区（含有clover启动文件和黑苹果引导文件）和硬盘的EFI分区（含有WIN10引导文件）合拼。
    XCOPY E:\EFI Z:\EFI /s /e /h
    上面的Z：代表你刚刚给硬盘ESP分区分配的盘符，这个大家都一定相同的。
    E：代表你的U盘的EFI分区，这个可能每个人都不一样，自己按照实际修改。
    默认情况下硬盘上的EFI分区是由WIN10自动生成的，卷标名称为ESP。U盘的EFI分区卷标为EFI

添加主板UEFI启动项
~~~~~~~~~~~~~~~~~~

::

    点击UEFI选项，修改启动顺序，添加一个启动项。在打开的窗口选中你硬盘的EFI分区（默认卷标名称为ESP），注意别选错了，经过刚刚的合拼，clover启动和黑苹果引导已经在硬盘的EFI分区（默认卷标为ESP）必须是硬盘上的。

mac安装
~~~~~~~

由于之前已经用Paragon Hard Disk Manage
12工具对磁盘进行操作了，这里不需要使用磁盘工具进行抹盘操作，直接安装即可，注意安装过程中有自动重启过程，请不必担心。

3.驱动问题
----------

由于之前的efi已经配置很好了，使用的现成的，所以这里只需要解决两个问题:
第一：无线无法连接，这个问题无解！
第二：声卡问题，通过淘宝援助解决！之前使用的是万能声卡，开机噪声很大，卸载了，网上教程未深入尝试！

4.参考资料
----------

-  `小米笔记本Air 13.3 Win10+黑苹果macOS
   Sierra10.12.3安装教程 <http://www.miui.com/forum.php?mod=viewthread&tid=7601066&extra=page=1&mobile=2>`__

-  `解决用 UEFI 引导 U盘启动映像文件大于 4G
   时不得不分 <https://tieba.baidu.com/p/4750680504?red_tag=1574076497>`__

-  `ge60 2pl 269
   可用efi分享，希望大家共同完善 <http://bbs.pcbeta.com/viewthread-1782781-1-1.html>`__

-  `黑苹果VoodooHDA开机爆音完美解决方案- <https://www.jikemac.com/drive/audio/3524.html>`__

-  `ALC声卡的最新完美无脑解决方案-Voodoo2.8.8 <http://bbs.pcbeta.com/viewthread-1672934-1-3.html>`__

-  `声卡 <https://github.com/acidanthera/AppleALC/wiki/Supported-codecs>`__

.. |image0| image:: http://p20tr36iw.bkt.clouddn.com/win10_mac.jpg
