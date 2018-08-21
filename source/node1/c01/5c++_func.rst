.. figure:: http://p20tr36iw.bkt.clouddn.com/c++_func.png
   :alt: 

.. raw:: html

   <!--more-->

c++之函数特性及内存管理
=======================

一、c++之函数特性
-----------------

1.函数参数默认值
~~~~~~~~~~~~~~~~

.. code:: cpp

    void fun(int i, int j=5, int k=10); //正确
    void fun(int i, int j=5, int k);  //错误

有默认参数值的参数必须放在参数表的最右端

.. code:: cpp

    void fun(int i, int j=5, int k=10); //声明
    void fun(int i, int j, int k) //定义
    {
      cout<<i<<j<<k;
    }

声明给默认值,定义不给默认值,采用这种方式,则所有编译器都可以通过,如果在定义时候加上默认值,部分编译器通不过！

声明和定义的i,j,k可以不一样，可以换成其他字母,比如:

.. code:: cpp

    void fun(int i, int j=5, int k=10);//声明
    void fun(int a, int b, int c) //定义
    {
      cout<<a<<b<<c;
    }

.. code:: cpp

    int main()
    {
      fun(20);
      fun(20,30);
      fun(20,30,40);
      return 0;
    }

无实参则用默认值,否则实参覆盖默认值。

2.函数重载
~~~~~~~~~~

在相同作用域内,用同意函数名定义的多个函数,参数个数和参数类型不同。

.. code:: cpp

    int getMax(int x, int y, int z)
    {
      //to do
    }
    double getMax(double x, double y)
    {
      //to do
    }

编译器如何区别重载函数:
如果函数名称相同,参数不同,则会形成函数名\_参数的新函数getMax\_int\_int\_int域getMax\_double\_double来区分同名函数。

重载带来的好处:
比如求最大值,根据求2个最大值,3个最大值,...到n个最大值，可以用同一个函数名，通过重载方式解决。

-  示例

::

    #include<iostream>
    using namespace std;
    void fun(int i, int j=10, int k=20);
    void fun(double i, double j);
    void fun(int i, int j, int k) {
        cout<<i<<','<<j<<','<<k<<endl;
    }
    void fun(double i, double j){
        cout<<i<<','<<j<<endl;
    }
    int main(int argc, char const *argv[])
    {
        /* code */
        fun(5);
        fun(6.1,5.2);
        system("pause");
        return 0;
    }

以上例子,函数在调用时候会自动选择相应的函数。

3.内联函数
~~~~~~~~~~

编译时将函数体代码和实参替代函数调用语句

内联函数关键字:inline

.. code:: cpp

    inline int max(int a, int b, int c){
      //to do
    }

为什么不所有函数都使用内联方式呢?

1. 内联编译是建议性的，由编译器决定。
2. 逻辑简单(无for、while等)，调用频繁的函数建议使用内联。
3. 递归函数无法使用内联方式

-  示例

::

    inline fun(int i, int j=10, int k=20);
    inline fun(double i, double j);
    inline fun(int i, int j, int k) {
        cout<<i<<','<<j<<','<<k<<endl;
    }
    inline fun(double i, double j){
        cout<<i<<','<<j<<endl;
    }

二、c++之内存管理
-----------------

1.什么示内存管理
~~~~~~~~~~~~~~~~

-  1.内存的本质:资源
-  2.谁掌控内存资源:操作系统
-  3.我们能做什么：申请/归还

申请/归还内存管理就是内存管理

2.内存的申请和释放
~~~~~~~~~~~~~~~~~~

-  申请内存:\ ``new``
-  释放内存:\ ``delete``

``new``\ 与\ ``delete``\ 都是运算符

3.申请和释放内存的方法
~~~~~~~~~~~~~~~~~~~~~~

-  申请内存:\ ``int *p = new int;``
-  释放内存:\ ``delete p;``

4.如何申请和释放块内存呢?
~~~~~~~~~~~~~~~~~~~~~~~~~

-  申请块内存:\ ``int *arr = new int [10]``
-  释放块内存:\ ``delete []arr;``

5.内存操作注意事项
~~~~~~~~~~~~~~~~~~

new 与 delete必须配套使用

申请完内存必须释放

申请内存完，不一定成功

.. code:: cpp

    int *p = new int[1000];
    if(NULL==p)
    {
      //内存分配失败
    }

6.释放内存需要注意什么?
~~~~~~~~~~~~~~~~~~~~~~~

释放完指针后,要将指针变为空。

-  第一种

.. code:: cpp

    int *p = new int;
    if(NULL == p)
    {
      //内存分配失败
      //异常处理
    }
    delete p;
    p = NULL;

-  第二种:块内存

.. code:: cpp

    int *p = new int[1000];
    if(NULL == p)
    {
      //内存分配失败
      //异常处理
    }
    delete []p;
    p = NULL;

7.示例
~~~~~~

::

    #include<iostream>
    using namespace std;
    int main(int argc, char const *argv[])
    {
        /* code */
        int *p = new int(30);
        if(NULL == p)
        {
            system("pause");
            return 0;
        }
        //*p =20;
        cout<<*p<<endl;
        delete p;
        p=NULL;
        system("pause");
        return 0;
    }

上述两种方式初始化赋值:

.. code:: cpp

    //第一种:
    int *p = new int(30);
    //第二种:
    int *p = new int;
    *p =20;

如果要申请块内存:

::

    #include<iostream>
    using namespace std;
    int main(int argc, char const *argv[])
    {
        /* code */
        int *p = new int[1000];
        if(NULL == p)
        {
            system("pause");
            return 0;
        }
        p[0] = 10;
        p[1] = 20;

        cout<<p[0]<<","<<p[1]<<endl;
        delete []p;
        p=NULL;
        system("pause");
        return 0;
    }
