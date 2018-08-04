.. figure:: http://p20tr36iw.bkt.clouddn.com/c++_learn.jpg
   :alt: 

c++初识
=======

-  c++新特性
-  c++输入输出
-  c++之namespace
-  综合练习

1.c++新特性
-----------

    新的数据类型：c中没有bool型，而c++有

    新的初始化方式：c中只可以复制初始化（int
    x=100），而c++除此之外还可以直接初始化（ int x(100) ）

    随用随定义：c要在最开始定义所有变量，而c++要用的时候定义就可以了

2.c++输入输出
-------------

.. code:: cpp

    #include <iostream>
    using namespace std;
    int main(int argc, char* argv[])
    {

        cout << "请输入一个整数：" << endl;
        int x = 0;
        cin >> x;
        cout << oct << x << endl; //八进制
        cout << dec << x << endl; //十进制
        cout << hex << x << endl; //十六进制
        cout << "请输入一个布尔值(0、1)：" << endl;
        bool y = false;
        cin >> y;
        cout << boolalpha << y << endl; //打印出布尔类型 true/false
        return 0;
    }

3.c++之namespace
----------------

-  1、概念：划片取名字，每个命名空间下可再细分名字以方便引用
-  2、设定命名空间的原因：解决重名问题
-  3、定义命名空间的方法：
   ``c++   namespace A{int x=0;   void f1();}   namespace B{int x=2;   void f1();}``
-  4.引用命名空间的相关变量函数 ``c++   cout<<A::x<<endl;   B::f1()``
-  5.实例

.. code:: cpp

	#include <stdlib.h>
	#include <iostream>
	using namespace std;
	namespace A
	{
	  int x = 1;
	  void fun()
	  {
		  cout << "A" <<endl;
	  }

	}
	namespace B
	{
	  int x = 2;
	  void fun()
	  {
		  cout << "B" <<endl;
	  }
	  void fun2()
	  {
		  cout << "B-fun2" <<endl;
	  }

	}
	using namespace B;
	int main(int argc, char * argv[])
	{

	  cout << A::x << endl;
	  B::fun();
	  fun2();
	  return 0;
	}
	
4.综合练习
-----------

* 获取数组中的最大值最小值

.. code:: cpp

	#include<iostream>
	using namespace std;
	namespace Acompany
	{
	  int getMaxOrMin(int *arr, int count, bool isMax)
	  {
		  int temp = arr[0];
		  for (int i = 1; i < count; i++)
		  {
			  if(isMax) {
				  if (temp < arr[i])
					  temp = arr[i];
			  }
			  else
			  {
				  if (temp > arr[i])
					  temp = arr[i];
			  }
		  }
		  return temp;
	  }
	}
	int main(int argv, char* argc[])
	{
	  int arr[4] = {9,1,3,7};
	  bool isMax = true;
	  int val = Acompany::getMaxOrMin(arr, 4, isMax);
	  if(isMax)
	  {
		  cout << val << "是最大值" << endl;
	  }
	  else
	  {
		  cout << val << "是最小值" << endl;
	  }
	  return 0;
	}