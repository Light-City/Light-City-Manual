|image0|

Python Web之Django初识
======================

1.安装及配置
------------

.. code:: python

    #0  安装:
    pip3 install django
    #1  创建project:
    django-admin startproject mysite
           ---mysite
              ---settings.py # 包含了项目的默认设置，包括数据库信息，调试标志以及其他一些工作的变量。
              ---url.py # 路由  负责把URL模式映射到应用程序。
              ---wsgi.py # 协议
           ---- manage.py(启动文件)  # Django项目里面的工具，通过它可以调用django shell和数据库等。
    #2  在mysite目录下创建blog应用:
        python manage.py startapp blog
            ---blog
              ---__init__.py
              ---admin.py
              ---apps.py
              ---migrations
                ---__init__.py
              ---models.py
              ---tests.py
              ---views.py
    #3  启动django项目:
         python manage.py runserver 8080
    #4  生成同步数据库的脚本:
         python manage.py makemigrations
           同步数据库:
         python manage.py migrate
    #5 访问后台管理系统:
            为进入这个项目的后台创建超级管理员：
         python manage.py createsuperuser，设置好用户名和密码后便可登录啦！
         http://127.0.0.1:8080/admin/
    #6  清空数据库:
         python manage.py  flush
    #7  查询某个命令的详细信息:
         django-admin.py  help  startapp
    #8  启动交互界面:
         python manage.py  shell
                这个命令和直接运行 python 进入 shell 的区别是：你可以在这个 shell 里面调用当前项目的 models.py 中的 API，对于操作数据，还有一些小测试非常方便。
    #9  查看详细的列表:
         python manage.py   
        

2.hello django及显示时间实例
----------------------------

.. code:: python

    # urls.py
    添加path('show_time/', views.show_time),
    # 修改views(视图)
    # **每一个视图必须有一个形参，客户端/浏览器发送服务器之后，服务器返回浏览器打包的信息对象，全在request里面**
    1.效果一：访问页面显示hello
    # **导入HttpResponse，HttpResponse('hello')返回给前端的实例对象**
    def show_time(request):
        return HttpResponse('hello')
    2.效果二：访问页面显示hello，hello封装到模板index.html中。
    def show_time(request):
        return render(request,"index.html")
    3.效果三：访问页面显示hello django，并显示当前时间
    def show_time(request):
        t=time.ctime()
    # 将字符串time以键值对绑定当前时间点，并发送给前端，前端index.html中{{time}}将time对应的内容渲染出来
        return render(request,"index.html",{'time':t})
    # 新增index.html(放置templates下面)：
    <div>
        <!--两个大括号去渲染一个变量-->
        hello {{ time }}
    </div>

    访问记得：http://localhost：端口/show_time

3.引用资源文件(例如引用jquery)
------------------------------

方法一：settings别名
~~~~~~~~~~~~~~~~~~~~

    settings.py

.. code:: python


    # 前端用的这个别名(虚拟路径)，是对后面statics的替换，为了维护方便
    STATIC_URL = '/static/' # 别名

    # 以下为添加的内容，注意元组/列表填写路径，否则报错
    # 物理路径/绝对路径
    STATICFILES_DIRS = (
        os.path.join(BASE_DIR,'statics'),
    )

    /templates/index.html

.. code:: html


    <div id="div1">
        <!--两个大括号去渲染一个变量-->
       hello {{ time }}
    </div>
    // 注意这里访问jquery文件时，必须用别名访问，否则报错，资源找不到。
    <script src="/static/jquery-3.3.1.js"></script>
    <script>
        $('#div1').css("color",'red')
    </script>

    总结

.. code:: html

    先在根目录下定义一个statics包文件夹，然后在下面放置jquery资源文件，为了让django读取到此文件，则必须更改settings.py中相关设置，在settings.py把statics添加进去，看上述代码，注意别名问题，所谓别名就是为了维护方便，在所有HTML处引用时只需使用别名访问，而不管资源文件(比如jquery)文件名的不断改变。若用资源文件名(例如将上述/statci改为/statics)则报错，资源文件找不到！！！

    #django对引用名和实际名进行映射,引用时,只能按照引用名来,不能按实际名去找
    #<script src="/statics/jquery-3.1.1.js"></script>
    #------error－－－－－不能直接用，必须用STATIC_URL = '/static/':
    #<script src="/static/jquery-3.1.1.js"></script>

方法二：
~~~~~~~~

    不能去掉settings.py上面加的STATICFILES\_DIRS

    meta标签下加

.. code:: python

    {% load staticfiles %}

    form表单里面加

.. code:: html

    <script src={% static "jquery-3.3.1.js" %}></script>

.. code:: html


    位置如下：
    <div id="div1">
        <!--两个大括号去渲染一个变量-->
       hello {{ time }}
    </div>
    <script src={% static "jquery-3.3.1.js" %}></script>
    <script>
        $('#div1').css("color",'red')
    </script>

4.提交数据并展示
----------------

    userInfor.html

.. code:: html

    <h1>创建个人信息</h1>

    <form action="/userInfor/" method="post">

        <p>姓名<input type="text" name="username"></p>
        <p>性别<input type="text" name="sex"></p>
        <p>邮箱<input type="text" name="email"></p>
        <p><input type="submit" value="submit"></p>

    </form>

    <hr>

    <h1>信息展示</h1>

    <table border="1">

        <tr>
            <td>姓名</td>
            <td>性别</td>
            <td>邮箱</td>
        </tr>
        {% for i in info_list %}

            <tr>
                <td>{{ i.username }}</td>
                <td>{{ i.sex }}</td>
                <td>{{ i.email }}</td>
            </tr>

        {% endfor %}

    </table>

    url.py

.. code:: python

    url(r'^userInfor/', views.userInfor)

    views.py

.. code:: python

    info_list=[]

    def userInfor(req):

        if req.method=="POST":
            username=req.POST.get("username",None)
            sex=req.POST.get("sex",None)
            email=req.POST.get("email",None)

            info={"username":username,"sex":sex,"email":email}
            info_list.append(info)

        return render(req,"userInfor.html",{"info_list":info_list})

    dos下运行python manage.py runserver 8000
    http://localhost:8000/userInfor/
    在使用Django提交Post表单时遇到如下错误：

.. code:: python

    Forbidden (403)
    CSRF verification failed. Request aborted.

    解决方法:

.. code:: python


    1、在表单Form里加上{% csrf_token %}
    <form action="/index/" method="post">
        {% csrf_token %}
    ......

    2、在Settings里的MIDDLEWARE增加配置：(一般默认就有)

        'django.middleware.csrf.CsrfViewMiddleware',
    我的版本是Django2.0.3,如果是以前版本，则为MIDDLEWARE_CLASSES配置。
    3.在views中的方法上面加上@csrf_exempt(记得引入包)注解
    from django.views.decorators.csrf import csrf_exempt
    @csrf_exempt
    def userInfor(req):
    .............

    另一种办法就是直接注释掉settings.py >MIDDLEWARE>'django.middleware.csrf.CsrfViewMiddleware',

5.提交数据至数据库，并在后台管理操作
------------------------------------

5.1原版
~~~~~~~

::

    # 首先创建django项目，其项目目录如下：
    exa
      ---dbreq
        ---migrations
          ---__init__.py
        ---admin.py
        ---apps.pyy
        ---models.py
        ---tests.py
        ---views.py
      ---exa
        ---__init__.py
        ---settings.py
        ---urls.py
        ---wsgi.py
      ---templates
        ---userInfor.html
      ---db.sqlites
      ---manage.py

    /templates/userInfor.html

.. code:: html

    <h1>创建个人信息</h1>
    <form action="/userInfor/" method="post">
        {% csrf_token %}
        <p>姓名<input type="text" name="username"></p>
        <p>性别<input type="text" name="sex"></p>
        <p>邮箱<input type="text" name="email"></p>
        <p><input type="submit" value="submit"></p>
    </form>
    <hr>
    <h1>信息展示</h1>
    <table border="1">
        <tr>
            <td>姓名</td>
            <td>性别</td>
            <td>邮箱</td>
        </tr>
        {% for i in info_list %}
            <tr>
                <td>{{ i.username }}</td>
                <td>{{ i.sex }}</td>
                <td>{{ i.email }}</td>
            </tr>
        {% endfor %}
    </table>

    /exa/urls.py

.. code:: python

    # 添加以下代码
    path('userInfor/', views.userInfor),

    /dbreq/views.py

.. code:: python

    from django.shortcuts import render
    from dbreq import models
    from django.views.decorators.csrf import csrf_exempt
    # Create your views here.
    @csrf_exempt
    def userInfor(req):
        if req.method == "POST":
            u = req.POST.get("username", None)
            s = req.POST.get("sex", None)
            e = req.POST.get("email", None)
            info={"username":u,"sex":s,"email":e}
            models.UserInfor.objects.create(**info)
            info_list=models.UserInfor.objects.all()
            return render(req, "userInfor.html", {"info_list":info_list})
        return render(req, "userInfor.html")

    models.py

::

    # 创建数据库
    from django.db import models

    # Create your models here.
    class UserInfor(models.Model):
        username=models.CharField(max_length=64)
        sex=models.CharField(max_length=64)
        email=models.CharField(max_length=64)

    以上.py文件写完后，生成表

.. code:: python

    # 生成相应的表:
    python manage.py makemigrations
    python manage.py migrate

    为项目后台数据库设置账户

.. code:: python

    python manage.py createsuperuser

    此时运行python manage.py runserver
    8088，然后http://localhost:8088/admin
    登录账户后，会发现无表，此时需要对admin.py进行修改

::

    # admin.py
    from django.contrib import admin

    # Register your models here.
    from dbreq import models
    # 把models创建的表添加到admin后台中
    admin.site.register(models.UserInfor)

此时后台如下界面： |image1| >此时进行增加数据操作

.. code:: python

    http://127.0.0.1:8000/userInfor/

页面创建表，后台实时更新成功，如图！

.. figure:: http://p20tr36iw.bkt.clouddn.com/sqli1.png
   :alt: 

.. figure:: http://p20tr36iw.bkt.clouddn.com/sqli2.png
   :alt: 

5.2更新版
~~~~~~~~~

    更新内容

::

    1.数据库后台修改了一行数据并添加了一行；
    2.增加show页面，将原先提交的数据可在另一个页面访问到
    3.删除数据并呈现操作
    4.更新数据并呈现数据

5.2.1show页面
^^^^^^^^^^^^^

    urls.py

.. code:: python

     path('show/', views.show),

    views.py

.. code:: python

    def show(req):
        info_list=models.UserInfor.objects.all() # 取出该表所有数据

        return  render(req,'show.html',{'info_list':info_list})

    show.html

.. code:: html

    <table border="1">
        <thead>
             <tr>
                <td>姓名</td>
                <td>性别</td>
                <td>邮箱</td>
            </tr>
        </thead>
        <tbody>
            {% for i in info_list %}
                <tr>
                    <td>{{ i.username }}</td>
                    <td>{{ i.sex }}</td>
                    <td>{{ i.email }}</td>
                </tr>
            {% endfor %}
        </tbody>
    </table>

    python manage.py runserver

.. figure:: http://p20tr36iw.bkt.clouddn.com/show.png
   :alt: 

5.2.2delData页面
^^^^^^^^^^^^^^^^

    urls.py

.. code:: python

    path('delData/', views.delData),

    views.py

.. code:: python

    # 删除操作
    @csrf_exempt
    def delData(req):
        # 删除数据
        info_list = models.UserInfor.objects.filter(username='哈哈哈')
        return render(req, "show.html", {"info_list": info_list})

    python manage.py runserver

.. figure:: http://p20tr36iw.bkt.clouddn.com/del.png
   :alt: 

5.2.3updateData页面
^^^^^^^^^^^^^^^^^^^

    urls.py

.. code:: python

    path('updateData/', views.updateData),

    views.py

.. code:: python

    # 修改操作
    @csrf_exempt
    def updateData(req):
        models.UserInfor.objects.filter(username='哈哈哈').update(sex='女',email='yixiugai@163.com')
        info_list = models.UserInfor.objects.all()
        return render(req,"show.html",{"info_list":info_list})

    python manage.py runserver

.. figure:: http://p20tr36iw.bkt.clouddn.com/update.png
   :alt: 

6.参考文章
----------

`Django-Model操作数据库(增删改查、连表结构） <https://www.cnblogs.com/yangmv/p/5327477.html>`__

7.项目地址：
------------

`点此出 <https://github.com/Light-City/firstDjangoProject>`__,欢迎star/fork！
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. |image0| image:: http://p20tr36iw.bkt.clouddn.com/django.jpg
.. |image1| image:: http://p20tr36iw.bkt.clouddn.com/sqli3.png

