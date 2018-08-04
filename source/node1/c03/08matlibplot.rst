.. figure:: http://p20tr36iw.bkt.clouddn.com/matlibplot.png
   :alt: 

matplotlib学习之基本使用
========================

1.figure学习
------------

    导包

.. code:: python

    import  matplotlib.pyplot as plt
    import numpy as np

    产生-3到3之间50个点

.. code:: python

    x=np.linspace(-3,3,50)

    定义函数

.. code:: python

    y1=2*x+1
    y2=x**2

    绘制直线

.. code:: python

    plt.figure()
    plt.plot(x,y1)

    曲线与直线绘制一块

.. code:: python

    # num=3表示图片上方标题 变为figure3，figsize=(长，宽)设置figure大小
    plt.figure(num=3,figsize=(8,5))
    plt.plot(x,y2)
    # 红色虚线直线宽度默认1.0
    plt.plot(x,y1,color='red',linewidth=1.0,linestyle='--')
    plt.show()

.. figure:: http://p20tr36iw.bkt.clouddn.com/matlibplot.png
   :alt: 

2.设置坐标轴
------------

    设置x轴范围

.. code:: python

    plt.xlim((-1,2))

    设置轴y范围

.. code:: python

    plt.ylim((-2,3))

    设置坐标轴含义

.. code:: python

    # 注：英文直接写，中文需要后面加上fontproperties属性
    plt.xlabel(u'价格',fontproperties='SimHei')
    plt.ylabel(u'利润',fontproperties='SimHei')

    设置x轴刻度

.. code:: python

    # -1到2区间，5个点，4个区间，平均分：[-1.,-0.25,0.5,1.25,2.]
    new_ticks=np.linspace(-1,2,5)
    print(new_ticks)
    plt.xticks(new_ticks)

    设置y轴刻度

.. code:: python

    '''
    设置对应坐标用汉字或英文表示，后面的属性fontproperties表示中文可见，不乱码，
    内部英文$$表示将英文括起来，r表示正则匹配，通过这个方式将其变为好看的字体
    如果要显示特殊字符，比如阿尔法，则用转意符\alpha,前面的\ 表示空格转意
    '''
    plt.yticks([-2,-1.8,-1,1.22,3.],
               ['非常糟糕','糟糕',r'$good\ \alpha$',r'$really\ good$','超级好'],fontproperties='SimHei')

.. figure:: http://p20tr36iw.bkt.clouddn.com/matlibplot_show.png
   :alt: 

设置边框/坐标轴

.. code:: python

    gca='get current axis/获取当前轴线'
    ax=plt.gca()
    # spines就是脊梁，即四个边框
    # 取消右边与上边轴
    ax.spines['right'].set_color('none')
    ax.spines['top'].set_color('none')

    matlibplot并没有设置默认的x轴与y轴方向，下面就开始设置默认轴

.. code:: python

    ax.xaxis.set_ticks_position('bottom')
    ax.yaxis.set_ticks_position('left')

    设置坐标原点

.. code:: python

    # 实现将(0,-1)设为坐标原点
    # 设置y轴上-1为坐标原点的y点,把x轴放置再-1处
    ax.spines['bottom'].set_position(('data',-1)) # 也可以是('axes',0.1)后面是百分比，相当于定位到10%处
    # 设置x轴上0为坐标原点的x点，将y轴移置0处
    ax.spines['left'].set_position(('data',0))

    再写一遍以下代码，因为以上使用set\_position后，中文会显示不出来

.. code:: python

    plt.yticks([-2,-1.8,-1,1.22,3.],
               ['非常糟糕','糟糕',r'$good\ \alpha$',r'$really\ good$','超级好'],fontproperties='SimHei')

    显示图片

.. code:: python

    plt.show()

.. figure:: http://p20tr36iw.bkt.clouddn.com/matlibplot_axis.png
   :alt: 

3.Legend 图例
-------------

    解释见1

.. code:: python

    import  matplotlib.pyplot as plt
    import numpy as np
    x=np.linspace(-3,3,50)
    y1=2*x+1
    y2=x**2
    # 绘制直线
    plt.figure()
    plt.plot(x,y1)
    # 曲线与直线绘制一块
    # num=3表示图片上方标题 变为figure3，figsize=(长，宽)设置figure大小
    plt.figure(num=3,figsize=(8,5))

    # 设置x轴范围
    plt.xlim((-1,2))
    # 设置轴y范围
    plt.ylim((-2,3))
    # 设置坐标轴含义
    # 注：英文直接写，中文需要后面加上fontproperties属性
    plt.xlabel(u'价格',fontproperties='SimHei')
    plt.ylabel(u'利润',fontproperties='SimHei')

    # -1到2区间，5个点，4个区间，平均分：[-1.,-0.25,0.5,1.25,2.]
    new_ticks=np.linspace(-1,2,5)
    print(new_ticks)
    plt.xticks(new_ticks)

    plt.yticks([-2,-1.8,-1,1.22,3.],
               ['非常糟糕','糟糕',r'$good\ \alpha$',r'$really\ good$','超级好'],fontproperties='SimHei')

    设置legend图例

.. code:: python

    l1,=plt.plot(x,y2) # 可添加label属性，只不过如果这里添加了，下面legend再添加，下面的就会覆盖此处的！
    # 红色虚线直线宽度默认1.0
    l2,=plt.plot(x,y1,color='red',linewidth=1.0,linestyle='--')
    '''
    prop={'family':'SimHei','size':15}显示中文
    legend(hadles=[,,],labels=[,,],loc='best/upper right/upper left/.../lower right')
    handles就是你给他添加legend的线,如果要用handles，则前面的plt.plot，必须用l1,形式(不要忘记逗号)
    此处labels会覆盖上述的plt.plot()的label
    loc默认是best,给你放在一个合适的位置上，如果你拉伸弹框，位置会跟着变，自动放置合适位置
    '''
    plt.legend(handles=[l1,l2],prop={'family':'SimHei','size':15},loc='lower right',labels=['直线','曲线'])

    绘制

.. code:: python

    plt.show()

.. figure:: http://p20tr36iw.bkt.clouddn.com/plt_legend.png
   :alt: 

4.Annotation 标注
-----------------

    见上述详解

.. code:: python

    import  matplotlib.pyplot as plt
    import numpy as np
    x=np.linspace(-3,3,20)
    y=2*x+1
    # 绘制直线
    plt.figure(num=1,figsize=(8,5),)
    plt.plot(x,y)
    # gca='get current axis/获取当前轴线'
    ax=plt.gca()
    # spines就是脊梁，即四个边框
    # 取消右边与上边轴
    ax.spines['right'].set_color('none')
    ax.spines['top'].set_color('none')
    ax.xaxis.set_ticks_position('bottom')
    ax.spines['bottom'].set_position(('data',-0)) # 也可以是('axes',0.1)后面是百分比，相当于定位到10%处
    ax.yaxis.set_ticks_position('left')
    ax.spines['left'].set_position(('data',0))

    绘制特定散点

.. code:: python

    x0=1
    y0=2*x0+1
    # plot散点图，上述plt.plot(x,y)变为plt.scatter(x,y)绘制出来就是散点图
    # s代表大小，b代表blue
    plt.scatter(x0,y0,s=50,color='b')
    # 把两个点放进去plot一下，画出垂直于x轴的一条线，[x0,x0]表示两个点的x,[0,y0]表示两个点的y

    绘制(x0,y0)垂直于x轴的线

.. code:: python

    # k--表示黑色虚线，k代表黑色，--表示虚线,lw表示线宽
    plt.plot([x0,x0],[0,y0],'k--',lw=2.5)

    添加注释 annotate

.. code:: python

    '''
    其中参数xycoords='data' 是说基于数据的值来选位置, xytext=(+30, -30) 和 textcoords='offset points'
    对于标注位置的描述 和 xy 偏差值, arrowprops是对图中箭头类型的一些设置.
    '''

    plt.annotate(r'$2x+1=%s$'%y0,xy=(x0,y0),xycoords='data',xytext=(+30,-30),textcoords='offset points',fontsize=16,arrowprops=dict(arrowstyle='->',connectionstyle='arc3,rad=.2'))

    添加注释 text

.. code:: python

    # 其中-3.7, 3,是选取text的位置, 空格需要用到转字符\ ,fontdict设置文本字体.
    plt.text(-3.7,3,r'$This\ is\ the\ some\ text.\mu\ \sigma_i\ \alpha_t$',
             fontdict={'size':'16','color':'red'})
    plt.show()

|image0| 

5.tick能见度 
-----------------

参考上面

.. code:: python

    import matplotlib.pyplot as plt
    import numpy as np

    x = np.linspace(-3, 3, 50)
    y = 0.1*x

    plt.figure()
    # 设置 zorder 给 plot 在 z 轴方向排序
    plt.plot(x, y, linewidth=10, zorder=1)
    plt.ylim(-2, 2)
    ax = plt.gca()
    ax.spines['right'].set_color('none')
    ax.spines['top'].set_color('none')
    ax.spines['top'].set_color('none')
    ax.xaxis.set_ticks_position('bottom')
    ax.spines['bottom'].set_position(('data', 0))
    ax.yaxis.set_ticks_position('left')
    ax.spines['left'].set_position(('data', 0))

    调整坐标

.. code:: python

    # 对被遮挡的图像调节相关透明度，本例中设置 x轴 和 y轴 的刻度数字进行透明度设置
    for label in ax.get_xticklabels()+ax.get_yticklabels():
        label.set_fontsize(12)
        '''
        其中label.set_fontsize(12)重新调节字体大小，bbox设置目的内容的透明度相关参，facecolor调节 box 前景色，edgecolor 设置边框， 本处设置边框为无，alpha设置透明度.
        '''
        # 其中label.set_fontsize(12)重新调节字体大小，bbox设置目的内容的透明度相关参，facecolor调节 box 前景色，edgecolor 设置边框， 本处设置边框为无，alpha设置透明度.
        label.set_bbox(dict(facecolor='white',edgecolor='none',alpha=0.7))

    绘制

.. code:: python

    plt.show()

.. figure:: http://p20tr36iw.bkt.clouddn.com/plt_vis.png
   :alt: 

6.参考文章
----------

    `matlibplot基本使用 <https://morvanzhou.github.io/tutorials/data-manipulation/plt/2-1-basic-usage/>`__

.. |image0| image:: http://p20tr36iw.bkt.clouddn.com/plt_annotation.png

