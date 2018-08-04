第六节:Win7下安装CetOS 7
============================

.. figure:: http://p20tr36iw.bkt.clouddn.com/win7.png
   :alt: 

1.前言
------

记实战Win7+CetOS7双系统真机折腾录

1)原因：2015年12月，是初次上手双系统，并且当初是成功安装上CetOs7,但是由于自身盲目及不会Google原因未能恢复Win7系统，故只剩下单系统，回到现在安装成功，其实就差最后一步，哎，一步之遥，相差1年，是本次缘分让我重拾双系统实战。初次安装是使用U盘安装，Win7+CetOs7,而本次则为另外一种装机方式(硬盘安装)

2)真机安装CetOS学习;

3)linux下C或Java开发等;

4)CetOS命令回顾及服务器高级进阶;

5)Python深入开发，爬虫实战。

故基于以上五点，抛去虚拟机的限制，直接真机上手，打破当初U盘装机习惯，采用硬盘装机体验，玩转真机，重拾自信，找回当初的最后一步！

2.工具准备
----------

-  1)\ `CetOS
   7下载 <http://mirror.bit.edu.cn/centos/7/isos/x86_64/CentOS-7-x86_64-DVD-1611.iso>`__

-  2)\ `EasyBCD <https://dw11.uptodown.com/dwn/L6XMPSGOa3q0w9d1oARYrmOWYo_NSLHspM5G5BpGinSzIuArJ22P0Ia8DQIRrKfeSG7TGuoKTQaxLLzUxWhjF4l4GnSFwfUlkwYbZwuix1aZWuK_f4p2Ifs3OILrm05N/qDncqc4ANsHq_WmOouLwDmFmLSqxSKiXB2imn3moMAhxtcIubulN1B834I1P3HZgYa5E5145uEbKMu9u45zKXJ2EpSiUIahnD-9ODa_im_p57u6NumUDf_t-zGCUf6Up/5nkCVrndyrNl9601367dDxLMICdPdK34CKHU9tlFTJ5oQ6v48oWvYy-kNu5TdyMjqYKUCj8BTqQHZXzNzaXYz2ZQiDkCuCsp9vpB4sSmhgssbkg7d2t_Us1J6Ks_5K-k/5uFEmjgr9zo2aGu7pPuR3w==/>`__

-  3)\ `EaseUS Partition Master 10.1 <https://dw.uptodown.com/dwn/Xy_EcYUXNEq4aKtPHKNhu8Q-B9N4TM4i5GVRXBtAege8mp4jmIOxOU49DDLt3SnA7gFhXrRt2hFouJr47c0gXuQKUJ5OekYnV2eF7sOYfoc3c0AJnpe2Q2pfz6gXf8k3/17N6EePyBx5lhs91DroGRApgg2FGRguYbS4RYjhbnFy33a-W_5u9svvKjXt8WSQrkN9MyFEtLIuK4WEEOKpvc17N1VvrHFp_bVc9ppnYPKDEHOzSvjOrKrvo1I6KetmZ/1opCpkMea4flmGcq_tvBKmy_sPWpX4ewmuzwX8SOJGFGqoqqbQbqF8hfeG_ASnTPtIO_oWQqgQ5Vsim-045wXSViPJoLu6_-s3T7L6bxIHuwy9xbKBMDxUcV7vpvilGn/RcjmC5GYXqgJJ3EwnsJwVbJQce8Lo_Zgu-ul1WklfDiiAhQI3ZfngpP0sJvtDf1yeLzxZ82DkVph4FXqXlMmR3R-ECn7moMK0dUbiMBQCTtncRHFef2vTxk2mHGIk_Hn/>`__

-  4)\ `Ext2Fsd <http://xiazai.xiazaiba.com/Soft/E/Ext2Fsd_0.51_XiaZaiBa.zip?pcid=24821&filename=Ext2Fsd_0.51_XiaZaiBa.zip&downloadtype=xiazaiba_seo>`__

-  5)\ `wingrub中文版 <http://d1.rsdown.cn/soft2/wingrub%E4%B8%AD%E6%96%87%E7%89%88.zip>`__

3.难点解释及注意事项
--------------------

难点解释
~~~~~~~~

-  1) Linux系统能识别windows下FAT32文件系统，不能识别NTFS文件系统，所以在linux安装时，选择任何sda都不行，FAT32可以
-  2）FAT32文件系统单个文件不能超过4G
-  3）CentOS 7文件大于4G

结论：在win7下使用FAT32和NTFS系统存放安装ISO都不可行，则要想办法用工具分出一块linux文件系统，如：ext3(linux分区格式)，不受4G的限制

注意事项
~~~~~~~~

EaseUS Partition Master 10.1
软件使用时，建议关闭其他所有软件，不然本软件会卡死！

4.安装过程
----------

-  1)准备一个空的盘符，最好是最后一个，而且不在逻辑分区内。
   (PS:我的是分出100GB,分为两部分：1)40GB--ext3用于存放镜像，启动安装；2)60GB---未分配磁盘，用于在安装过程中，回收剩余的本60GB，去安装，不然的话，如果是一个磁盘，到时候得回收，由于本盘装了镜像，所以安装不太行，有得重来，故一定要在放镜像之外，至少分配一个未分配的空间！具体操作见下面)

-  2)如果最后一个是逻辑分区的话，可以用EaseUS Partition Master
   10.1转化成主分区，然后再删除(用EaseUS Partition Master
   10.1将最后一个磁盘删除)，然后在新建一个40g的ext3分区来存放CentOS 7
   镜像文件。Windows是不识别ext2、3等linux文件系统的，所以创建好ext3分区之后要用ext2fsd工具将ext3文件系统挂载到win7上

-  a:分区（ps：傻瓜式操作，英文不懂的，可以Google翻译哈，或者再不懂大家不懂用这个工具的话可以换其他的，或者找下EaseUS
   Partition Master
   10.1的教程，这里我自己是分了将近40g的空间来放置CentOS 7 镜像文件）
   |image0|
-  b：利用ext2Fsd工具启用支持ext3的功能，并给刚才划分ext3文件系统分配盘符。

**右键刚才分出来的ext3文件系统，添加-加载并推出-返回，看到ext3有盘符就说明可以了，如果没有就只执行刚才的操作。**

-  c：将CentOS 7
   用虚拟光驱加载，将里面的images、isolinux文件夹复制到40g的ext3文件系统中，同时也要把CentOS
   7镜像复制到40gext3里面。
-  
-  3)做完上面的镜像准备工作之后就要开始配置grub了。配置的时候要用winGrub查看下40g
   ext3文件系统盘的分区号，然后使用EasyBCD点击添加新条目，然后选择NeoGrub条目，点击安装后，点击配置，弹出记事本文档，在grub中写下如下配置：
   ####

   title CentOS 7 root (hd0,7) kernel (hd0,7)/isolinux/vmlinuz linux
   repo=hd:/dev/sda8:/ initrd (hd0,7)/isolinux/initrd.img

-  4)然后是安装,安装CentOS过程

重启选择NeoGrub引导

一步步安装

后面安装步骤省略，在安装选择分区是我选择自动分配。安装模式根据自己的需求安装，包括一些软件之类的，自选！

一步步安装后，到此就安装成功CetOS 7了，重启进入CetOS 7系统。

5.安装问题及解决办法
--------------------

::

    问题1：安装过程出现/dev/root does not exist的提示！

    解决办法：EasyBCD配置grub时，文件不要忘了添加(这个是不同于CetOS 6，目前所知安装CetOs 7得添加此句方可解决！)


    root (hd0,7)跟repo=hd:/dev/sda8:/

    问题2： CentOS 7安装好之后，原先的win7启动项就会消失，这时候有两种办法可以找回来。

    解决办法1：第一种暴力直接，但是有效果。（个人比较推荐第二种方法！）
    进入pe重建C盘的主引导记录，然后进入win7，在bcd中添加新条目-Linux/BSD-驱动器选择60GB的linux（/boot）

    第二种就是在CentOS 7中添加Win7的启动项

    修改/etc/grub.d/40_coustom 添加如下内容：
    menuentry "Windows7"{
      set root=(hd0,1)
      chainloader +1
    }
    然后用grub2-mkconfig -o /boot/grub2/grub.cfg重建grub2引导

6.参考链接
----------

-  1)http://www.cnblogs.com/Johness/archive/2012/12/03/2800126.html
-  2)http://bckong.blog.51cto.com/5092126/1574489

.. |image0| image:: https://github.com/asdfghjklqqq2/asdfghjklqqq2.github.io/blob/master/img/win7+CetOS/fen.png?raw=true

