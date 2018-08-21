.. figure:: http://p20tr36iw.bkt.clouddn.com/c++_pin1.png
   :alt: 

.. raw:: html

   <!--more-->

c++引用
=======

1.基本数据类型的引用
--------------------

-  原型：\ ``int &b = a;``

::

    #include<iostream>
    using namespace std;
    //基本数据类型的引用
    int main(int argc, char const *argv[])
    {
        /* code */
        int a = 3;
        int &b = a;
        b = 10;
        cout << a << endl; //10
        system("pause");
        return 0;
    }

2.结构体的引用
--------------

-  原型: 结构体类型 &结构体引用名 = 结构体变量

::

    #include <iostream>
    using namespace std;
    typedef struct
    {
        int x;
        int y;
    }Coor;
    int main(int argc, char const *argv[])
    {
        /* code */
        Coor c1;
        Coor &c = c1; // 别名
        c.x = 10;
        c.y = 20;
        cout << c1.x << endl <<c1.y << endl;
        system("pause");
        return 0;
    }

3.指针引用
----------

-  原型: 类型 ``*&指针引用名 = 指针``

::

    #include<iostream>
    using namespace std;
    /*
    指针别名:
    类型  *&指针引用名 = 指针
    */
    int main(int argc, char const *argv[])
    {
        /* code */
        int a = 10;
        int *p = &a;
        cout << *p << endl;
        int *&q = p; // 指针别名
        *q = 20;
        cout << a << endl;
        system("pause");
        return 0;
    }

4.引用做函数参数
----------------

-  原始方式函数定义及调用

::

    #include<iostream>
    using namespace std;
    void fun(int *a, int *b)
    {
        int c = 0;
        c = *a;
        *a = *b;
        *b = c;
    }

    int main(int argc, char const *argv[])
    {
        /* code */
        int x = 10,y = 20;
        fun(&x,&y);
        cout << x << endl << y << endl;
        system("pause");
        return 0;
    }

-  引用做函数参数定义及调用

-  原型: ``void fun(int &a, int &b)`` ``fun(x,y)``

::

    #include<iostream>
    using namespace std;
    void fun(int &a, int &b)
    {
        int c = 0;
        c = a;
        a = b;
        b = c;
    }
    int main(int argc, char const *argv[])
    {
        /* code */
        int x = 10,y = 20;
        fun(x,y);//调用时候 a是x的别名,b是y的别名,对别名操作就是对x，y操作。
        cout << x << endl << y << endl;
        system("pause");
        return 0;
    }
