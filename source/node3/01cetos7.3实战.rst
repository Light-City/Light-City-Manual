.. figure:: http://p20tr36iw.bkt.clouddn.com/2.png
   :alt: 


第一节:CetOS 7.3配置初实战
============================


一、rpm/yum/tar实战
-------------------

1.rpm命令
~~~~~~~~~

::

    rpm包，由“-”、“.”构成，包名、版本信息、版本号、运行平台

    对已安装软件信息的查询
    rpm -qa 查询已安装的软件
    rpm -qf 文件名绝对路径  文件名的绝对路径
    rpm -ql 软件名查询已安装的软件包都安装到何处

    软件包的安装、升级、删除
    rpm -ivh rpm文件安装rpm包
    rpm -Uvh rpm文件  更新rpm包
    rpm -e 软件名删除rpm包
    rpm -e 软件名 --nodeps不管依赖关系，强制删除软件

    rpm --import 签名文件导入签名
    rpm --import RPM-GPG-KEY

2.yum命令
~~~~~~~~~

    ``yum= yellow dog updater, modified``
    主要功能更方便添加、删除、更新rpm包，自动解决依赖性问题，便于管理大量系统的更新问题

    同时配置多个资源库（repository）简介的配置文件（\ ``/etc/yum.conf``\ 自动解决增加或删除rpm包时遇到的依赖性问题，方便保持rpm数据库的一致性）

    yum安装，\ ``rpm -ivh yum-*.noarch.rpm``\ 在第一次启用yum之前要先导入系统的RPM-GPG-KEY

    第一次使用yum管理软件时，yum会自动下载需要的headers放置在\ ``/var/cache/yum``\ 目录下

::

    rpm包更新
    yum check-update 查看可以更新的软件包
    yum update更新所有的软件包
    yum update kernel 更新指定的软件包
    yum upgrade大规模更新升级

    rpm包安装和删除
    yum install xxx[服务名]安装rpm包
    yum remove xxx[服务名]  删除rpm包

3.tar命令
~~~~~~~~~

::

    -c: 建立压缩档案

    -x：解压

    -t：查看内容

    -r：向压缩归档文件末尾追加文件

    -u：更新原压缩包中的文件

    这五个是独立的命令，压缩解压都要用到其中一个，可以和别的命令连用但只能用其中一个。

    下面的参数是根据需要在压缩或解压档案时可选的。

    -z：有gzip属性的

    -j：有bz2属性的

    -Z：有compress属性的

    -v：显示所有过程

    -O：将文件解开到标准输出

    参数-f是必须的

    -f: 使用档案名字，切记，这个参数是最后一个参数，后面只能接档案名。
    常用举例：
    1、*.tar 用 tar –xvf 解压

    2、*.gz 用 gzip -d或者gunzip 解压

    3、*.tar.gz和*.tgz 用 tar –xzf 解压

    4、*.bz2 用 bzip2 -d或者用bunzip2 解压

    5、*.tar.bz2用tar –xjf 解压

    6、*.Z 用 uncompress 解压

    7、*.tar.Z 用tar –xZf 解压

二、Pycharm/Eclipse配置实战
---------------------------

1.Python3.6.0安装
~~~~~~~~~~~~~~~~~

1)获取Python3.6.0
^^^^^^^^^^^^^^^^^

``wget http://www.python.org/ftp/python/3.6.0/Python-3.6.0.tgz``

2)解压压缩文件
^^^^^^^^^^^^^^

::

    tar xzvf Python-3.6.0.tgz

3)进入文件
^^^^^^^^^^

::

    cd Python-3.6.0

4)新加一个python3的安装目录防止覆盖到老版本的python
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

    mkdir /opt/python3.6.0

5)开始编译安装
^^^^^^^^^^^^^^

::

    ./configure --prefix=/opt/python3.6.0
    make
    make install

6)建立一个新版本python的连接
''''''''''''''''''''''''''''

::

    ln -s /opt/python3.6.0/bin/python3 /usr/local/bin/python3

7)使用python3
^^^^^^^^^^^^^

::

    [root@localhost Python-3.6.0]# python3

2.PyCharm for python
~~~~~~~~~~~~~~~~~~~~

1)首先下载pycharm for Python最新版本2016.3.2
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

http://www.jetbrains.com/pycharm/
'''''''''''''''''''''''''''''''''

2)解压
^^^^^^

::

    tar zxvf *.tar.gz     //*改为自己的版本

3)移动到/usr/local/pycharm目录
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

    mv pycharm-* /usr/local/pycharm  //pycharm-*表示第二步解压出来的目录

4)运行pycharm
^^^^^^^^^^^^^

::

    /usr/local/pycharm/bin/pycharm.sh

    然后根据安装提示，一步一步安装即可，后续过程跟Windws版本安装一样，此处就不多阐述了。
    需要注意的是python2.7是系统自带的版本，我前面装的是Python3.6.0，在选择的时候可以根据自己使用版本，填入目录，选择相应版本！

5)破解
^^^^^^

百度以后找到如下注册码，填入即可用：
''''''''''''''''''''''''''''''''''''

::

    B1c2Ugb25seSIsImNoZWNrQ29uY3VycmVudFVzZSI6ZmFsc2UsInByb2R1Y3RzIjpbeyJjb2RlIjoiSUkiLCJwYWlkVXBUbyI6IjIwMTctMDItMjUifSx7ImNvZGUiOiJBQyIsInBhaWRVcFRvIjoiMjAxNy0wMi0yNSJ9LHsiY29kZSI6IkRQTiIsInBhaWRVcFRvIjoiMjAxNy0wMi0yNSJ9LHsiY29kZSI6IlBTIiwicGFpZFVwVG8iOiIyMDE3LTAyLTI1In0seyJjb2RlIjoiRE0iLCJwYWlkVXBUbyI6IjIwMTctMDItMjUifSx7ImNvZGUiOiJDTCIsInBhaWRVcFRvIjoiMjAxNy0wMi0yNSJ9LHsiY29kZSI6IlJTMCIsInBhaWRVcFRvIjoiMjAxNy0wMi0yNSJ9LHsiY29kZSI6IlJDIiwicGFpZFVwVG8iOiIyMDE3LTAyLTI1In0seyJjb2RlIjoiUEMiLCJwYWlkVXBUbyI6IjIwMTctMDItMjUifSx7ImNvZGUiOiJSTSIsInBhaWRVcFRvIjoiMjAxNy0wMi0yNSJ9LHsiY29kZSI6IldTIiwicGFpZFVwVG8iOiIyMDE3LTAyLTI1In0seyJjb2RlIjoiREIiLCJwYWlkVXBUbyI6IjIwMTctMDItMjUifSx7ImNvZGUiOiJEQyIsInBhaWRVcFRvIjoiMjAxNy0wMi0yNSJ9XSwiaGFzaCI6IjMzOTgyOTkvMCIsImdyYWNlUGVyaW9kRGF5cyI6MCwiYXV0b1Byb2xvbmdhdGVkIjpmYWxzZSwiaXNBdXRvUHJvbG9uZ2F0ZWQiOmZhbHNlfQ==-keaxIkRgXPKE4BR/ZTs7s7UkP92LBxRe57HvWamu1EHVXTcV1B4f/KNQIrpOpN6dgpjig5eMVMPmo7yMPl+bmwQ8pTZaCGFuLqCHD1ngo6ywHKIQy0nR249sAUVaCl2wGJwaO4JeOh1opUx8chzSBVRZBMz0/MGyygi7duYAff9JQqfH3p/BhDTNM8eKl6z5tnneZ8ZG5bG1XvqFTqWk4FhGsEWdK7B+He44hPjBxKQl2gmZAodb6g9YxfTHhVRKQY5hQ7KPXNvh3ikerHkoaL5apgsVBZJOTDE2KdYTnGLmqxghFx6L0ofqKI6hMr48ergMyflDk6wLNGWJvYHLWw==-MIIEPjCCAiagAwIBAgIBBTANBgkqhkiG9w0BAQsFADAYMRYwFAYDVQQDDA1KZXRQcm9maWxlIENBMB4XDTE1MTEwMjA4MjE0OFoXDTE4MTEwMTA4MjE0OFowETEPMA0GA1UEAwwGcHJvZDN5MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAxcQkq+zdxlR2mmRYBPzGbUNdMN6OaXiXzxIWtMEkrJMO/5oUfQJbLLuMSMK0QHFmaI37WShyxZcfRCidwXjot4zmNBKnlyHodDij/78TmVqFl8nOeD5+07B8VEaIu7c3E1N+e1doC6wht4I4+IEmtsPAdoaj5WCQVQbrI8KeT8M9VcBIWX7fD0fhexfg3ZRt0xqwMcXGNp3DdJHiO0rCdU+Itv7EmtnSVq9jBG1usMSFvMowR25mju2JcPFp1+I4ZI+FqgR8gyG8oiNDyNEoAbsR3lOpI7grUYSvkB/xVy/VoklPCK2h0f0GJxFjnye8NT1PAywoyl7RmiAVRE/EKwIDAQABo4GZMIGWMAkGA1UdEwQCMAAwHQYDVR0OBBYEFGEpG9oZGcfLMGNBkY7SgHiMGgTcMEgGA1UdIwRBMD+AFKOetkhnQhI2Qb1t4Lm0oFKLl/GzoRykGjAYMRYwFAYDVQQDDA1KZXRQcm9maWxlIENBggkA0myxg7KDeeEwEwYDVR0lBAwwCgYIKwYBBQUHAwEwCwYDVR0PBAQDAgWgMA0GCSqGSIb3DQEBCwUAA4ICAQC9WZuYgQedSuOc5TOUSrRigMw4/+wuC5EtZBfvdl4HT/8vzMW/oUlIP4YCvA0XKyBaCJ2iX+ZCDKoPfiYXiaSiH+HxAPV6J79vvouxKrWg2XV6ShFtPLP+0gPdGq3x9R3+kJbmAm8w+FOdlWqAfJrLvpzMGNeDU14YGXiZ9bVzmIQbwrBA+c/F4tlK/DV07dsNExihqFoibnqDiVNTGombaU2dDup2gwKdL81ua8EIcGNExHe82kjF4zwfadHk3bQVvbfdAwxcDy4xBjs3L4raPLU3yenSzr/OEur1+jfOxnQSmEcMXKXgrAQ9U55gwjcOFKrgOxEdek/Sk1VfOjvS+nuM4eyEruFMfaZHzoQiuw4IqgGc45ohFH0UUyjYcuFxxDSU9lMCv8qdHKm+wnPRb0l9l5vXsCBDuhAGYD6ss+Ga+aDY6f/qXZuUCEUOH3QUNbbCUlviSz6+GiRnt1kA9N2Qachl+2yBfaqUqr8h7Z2gsx5LcIf5kYNsqJ0GavXTVyWh7PYiKX4bs354ZQLUwwa/cG++2+wNWP+HtBhVxMRNTdVhSm38AknZlD+PTAsWGu9GyLmhti2EnVwGybSD2Dxmhxk3IPCkhKAK+pl0eWYGZWG3tJ9mZ7SowcXLWDFAk0lRJnKGFMTggrWjV8GYpw5bq23VmIqqDLgkNzuoog==

3. Centos7下安装eclipse进行C/C++开发
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1)下载eclipse
^^^^^^^^^^^^^

Eclipse下载网址： https://www.eclipse.org/downloads/
''''''''''''''''''''''''''''''''''''''''''''''''''''

    **注意选择的时候选择Eclipse IDE for C/C++ Developers**

2)下载cdt
^^^^^^^^^

cdt下载网址：\ `www.eclipse.org/cdt/ <www.eclipse.org/cdt/>`__
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

3)安装
^^^^^^

进入要“安装软件”的目录然后，解压eclipse-jee-kepler-RC3-Linux-gtk.tar.gz压缩包命令是 tar –zxvf eclipse-jee-kepler-RC3-linux-gtk.tar.gz得到：eclipse文件夹
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

4)安装cdt
^^^^^^^^^

在终端输入：unzip cdt-master-8.1.2.zip –d cdt，可以把cdt-master-8.1.2.zip解压并且它的内容存放在cdt文件夹下。再输入：cp –r cdt/plugins/ eclipse/，则将cdt下plugins的内容拷贝到eclipse下plugins文件夹。最后，输入cp –r cdt/features/ eclipse/，则将cdt下features的内容拷贝到eclipse下features文件夹。
'''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

到现在，安装已经完成。
''''''''''''''''''''''

5)测试Eclipse是否可以工作
^^^^^^^^^^^^^^^^^^^^^^^^^

直接打开eclipse文件下的eclipse文件，运行弹出窗口，测试成功，否则，不成功，请验证是否安装JDK。按照本章节操作，cetos系统必须提前安装好JDK之类的，否则会出错。
'''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

6)eclipse cdt运行c程序报错“launch failed，binary not found”
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

在CDT中每一次新项目建成后，系统一般默认会进行第一次的构建，也就是自动生成可执行文件。可是事实我们在刚新建的项目甚至还没有源码文 件，所以当然不 会生成可执行的文件了。当我们新建了一个源码文件时，点击执行按钮，就会弹出所说的"launch failed.Binary not found "提示说明（找不到可运行的二进制文件）。
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

    **解决办法：窗口左面的项目文件夹上右键鼠标，在弹出的菜单中选择Build
    Configurations --->Build
    selected，选择其中的debug或者release进行构建。**

7)在应用菜单中添加菜单项-----把eclipse添加到应用菜单中
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

gnome桌面的所有菜单项都存储如下位置：
'''''''''''''''''''''''''''''''''''''

::

    /usr/share/applications/

新建一个菜单项，直接在该目录下新建一个后缀名为.desktop的文件即可。
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

::

    vim /usr/share/applications/c-eclipse.desktop

文件内容如下：
''''''''''''''

::

    [Desktop Entry]
    Version=1.0
    Name=C-Eclipse
    Icon=/opt/eclipse/icon.xpm
    Exec=/opt/eclipse/eclipse
    Terminal=false
    Type=Application
    StartupNotify=true
    Categories=Application;Development;IDE

三、参考文章
------------

1.\ http://www.centoscn.com/image-text/install/2016/0529/7296.html

2.\ http://www.360kb.com/kb/2_140.html

3.\ http://blog.csdn.net/u011345885/article/details/51871435

4.\ http://www.centoscn.com/CentOS/config/2015/1106/6391.html

