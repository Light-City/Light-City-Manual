.. figure:: http://p20tr36iw.bkt.clouddn.com/urls.jpg
   :alt: 

Python Web之Django-urls
=======================

1.将statics文件放在应用文件夹下：
---------------------------------

::

    STATICFILES_DIRS=(
        # os.path.join(BASE_DIR, 'blog/statics'),
        os.path.join(BASE_DIR, 'blog','statics'),
    )

2.无命名分组
------------

    urls.py

.. code:: python

    from django.contrib import admin
    from django.conf.urls import url
    from blog import views
    '''
       r 开头的python字符串是 raw 字符串，所以里面的所有字符都不会被转义，
       比如r'\n'这个字符串就是一个反斜杆加上一字母n，而'\n'我们知道这是个换行符。
       因此，上面的'\\\\'你也可以写成r'\\'，这样，应该就好理解很多了。可以看下面这段：
      '''
    urlpatterns = [
        url(r'admin/',admin.site.urls),
        # r代表raw string，()代表分组，\d代表数字,{4}代表重复4次，后面同理。例如article/2018/02 通过分组(),将相应的2018传给相应视图函数的参数year，02传给相应试图的month。
        url(r'article/(\d{4})/(\d{2})',views.article_year),
    ]

    views.py

.. code:: python

    def article_year(request,y,m):
        return HttpResponse("year:%s month:%s"%(y,m))

3.有命名分组
------------

    urls.py

.. code:: python

    from django.contrib import admin
    from django.conf.urls import url
    from blog import views
    urlpatterns = [
        url(r'admin/',admin.site.urls),
        url(r'show_time/',views.show_time),
        # 无命名分组
        # 访问 http://127.0.0.1:8088/article/2018/02 year:2018  我是想进入2018/02，最后页面返回year:2018 month:02，可却返回了year:2018
        # 原因:article/2018/02前面的article/2018全部匹配上了，就直接进入了article_year视图，不会进入下面的article_year_month。此时正则匹配出加一个$，表示结尾，此时就不会出现上述情况了。
        # url(r'article/(\d{4})',views.article_year),
        url(r'article/(\d{4})$',views.article_year),
        # 有命名分组
        # ?P没有正则意义，只是代表要起一个命名分组
        url(r'article/(?P<year>\d{4})/(?P<month>\d{2})', views.article_year_month),
    ]

    views.py

.. code:: python

    # 无命名分组 y可随意
    def article_year(request,y):
        return HttpResponse("year:%s"%(y))

    # 有命名分组 year month 是定死的，根据前面urls中来定
    def article_year_month(request,year,month):
        return HttpResponse("year:%s month:%s"%(year,month))

4.别名及注册，get/post如何获取信息
----------------------------------

.. code:: python

    别名使用：
    1.urls.py中添加别名name='reg'
    2.修改form表单的action属性值action="{% url 'reg' %}

    urls.py

.. code:: python

    url(r'register',views.register,name='reg'),

    views.py

.. code:: python

    def register(request):
        if request.method=='POST':
            print(request.POST.get('user'))
            return HttpResponse('success!')
        return render(request,'register.html')

    register.html

.. code:: html

    <h1>学生注册</h1>
    <hr/>
    <form action="{% url 'reg' %}" method="post">
        <p>姓名<input type="text" name="user"/></p>
        <p>年龄<input type="text" name="age"/></p>
        <p>爱好
            <input type="checkbox" name="hobby" value="1"/>篮球
            <input type="checkbox" name="hobby" value="2"/>足球
            <input type="checkbox" name="hobby" value="3"/>乒乓球
        </p>
        <p><input type="submit"></p>
    </form>

5.路由分发
----------

.. code:: python

    1.在blog应用里面新建一个urls.py

    ------urls.py-------
    from django.contrib import admin
    from django.conf.urls import url,include
    from blog import views

    urlpatterns=[
        # 无命名分组
        # 访问 http://127.0.0.1:8088/article/2018/02 year:2018  我是想进入2018/02，最后页面返回year:2018 month:02，可却返回了year:2018
        # 原因:article/2018/02前面的article/2018全部匹配上了，就直接进入了article_year视图，不会进入下面的article_year_month。此时正则匹配出加一个$，表示结尾，此时就不会出现上述情况了。
        # url(r'article/(\d{4})',views.article_year),
        url(r'article/(\d{4})$', views.article_year),
        # 有命名分组
        # ?P没有正则意义，只是代表要起一个命名分组
        url(r'article/(?P<year>\d{4})/(?P<month>\d{2})', views.article_year_month),
        url(r'register', views.register, name='reg'),
    ]
    2.在全局urls.py里面写入url(r'blog/',include('blog.urls'))
    ------全局urls.py-------
    from django.contrib import admin
    from django.conf.urls import url,include
    from blog import views
    urlpatterns = [
        url(r'admin/',admin.site.urls),
        url(r'show_time/',views.show_time),
        # 路由分发
        url(r'blog/',include('blog.urls')),
    ]

    3.必须加上blog才可访问到，访问：http://127.0.0.1:8088/blog/register

