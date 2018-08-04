|image0|

matlibplot绘制各种图形
======================

1.散点图
--------

    code

.. code:: python

    import matplotlib.pyplot as plt
    import numpy as np

    n=1024
    X=np.random.normal(0,1,n)
    Y=np.random.normal(0,1,n)
    T=np.arctan2(Y,X) # for color value
    plt.scatter(X,Y,s=75,c=T,alpha=0.5)
    plt.xlim(-1.5,1.5)
    plt.ylim(-1.5,1.5)
    plt.xticks(()) # 隐藏x轴内容
    plt.yticks(()) # 隐藏y轴内容
    plt.show()

    output

.. figure:: http://p20tr36iw.bkt.clouddn.com/matlibplt_scatter.png
   :alt: 

2.3D图
------

    code

.. code:: python

    import numpy as np
    import matplotlib.pyplot as plt
    from mpl_toolkits.mplot3d import Axes3D
    fig=plt.figure()
    ax=Axes3D(fig)
    X=np.arange(-4, 4, 0.25)
    Y = np.arange(-4, 4, 0.25)
    X,Y=np.meshgrid(X,Y)
    R=np.sqrt(X**2+Y**2)
    # height value
    Z=np.sin(R)
    # rstride行跨，cstride列跨
    ax.plot_surface(X,Y,Z,rstride=1,cstride=1,cmap=plt.get_cmap('rainbow'))
    # 投影 offset表示把图形压缩到xoy面，z=-2的位置，zdir换成x,y类似
    ax.contourf(X,Y,Z,zdir='z',offset=-2,cmap='rainbow')
    ax.set_zlim(-2,2)
    plt.show()

    output

.. figure:: http://p20tr36iw.bkt.clouddn.com/matlibplot_3d.png
   :alt: 

.. |image0| image:: http://p20tr36iw.bkt.clouddn.com/matlibplt_scatter.png

