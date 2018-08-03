Java增强的for循环
=================

.. figure:: http://img.zcool.cn/community/03320dd554c75c700000158fce17209.jpg
   :alt: 

JDK1.5中增加了增强的for循环。 缺点： 对于数组，不能方便的访问下标值；
对于集合，与使用Interator相比，不能方便的删除集合中的内容（在内部也是调用Interator）.
除了简单遍历并读取其中的内容外，不建议使用增强的for循环。

一、遍历数组
------------

语法为:

.. code:: java

    for (Type value : array) {
        expression value;
    }

::

    `//现在我们只需这样写（和以上写法是等价的）：`

.. code:: java

    void someFunction ()
    {
        int[] array = {1,2,5,8,9};
        int total = 0;
        for (int n : array)
       {
           total += n;
        }
        System.out.println(total);
    }

这种写法的缺点： 显而易见，for/in(for
each)循环自动控制一次遍历数组中的每一个元素，然后将它赋值给一个临时变量（如上述代码中的int
n），然后在循环体中可直接对此临时变量进行操作。这种循环的缺点是： 1.
只能顺次遍历所有元素，无法实现较为复杂的循环，如在某些条件下需要后退到之前遍历过的某个元素；
2.
循环变量（i）不可见，如果想知道当前遍历到数组的第几个元素，只能这样写:

.. code:: java

    int i = 0;
     for (int n : array) {
         System.out.println("This " + i + "-th element in the array is " + n);
         i++;
    }

二、遍历集合
------------

语法为: ``java  for (Type value : Iterable) {    expression value; }``

** 注意：for/in循环遍历的集合必须是实现Iterable接口的。**
//以前我们这样写：

.. code:: java

    void someFunction ()
    {
      List list = new ArrayList();
      list.add("Hello ");
      list.add("Java ");
      list.add("World!");
      String s = "";
      for (Iterator iter = list.iterator(); iter.hasNext();)
      {
         String temp= (String) iter.next();
         s += temp;
      }
      System.out.println(s);
    }

    //现在我们这样写：
    void someFunction (){List list = new ArrayList();list.add("Hello ");list.add("Java ");list.add("World!");String s = "";for (Object o : list){String temp = (String) o;s += temp; }System.out.println(s);}
    // 如果结合“泛型”，那么写法会更简单，如下：`
    void someFunction ()
    {
    List list = new ArrayList();
    list.add("Hello ");
    list.add("Java ");
    list.add("World!");
    String s = "";
    for (String temp : list)
       {
       s += temp; //省去了对强制类型转换步骤
       }
       System.out.println(s);
    }

    //上述代码会被编译器转化为：
     void someFunction ()
    {
    List list = new ArrayList();
    list.add("Hello ");
    list.add("Java ");
    list.add("World!");
    String s = "";
    for (Iterator iter = list.iterator(); iter.hasNext(); )
       {
       String temp = iter.next();
       s += temp;
    }
    System.out.println(s);
     }

这种写法的缺点：
虽然对集合进行的for/in操作会被编译器转化为Iterator操作，但是使用for/in时，Iterator是不可见的，所以如果需要调用Iterator.remove()方法，或其他一些操作，
for/in循环就有些力不从心了。 综上所述，Java
5.0中提供的增强的for循环——for/in(for
each)循环能让我们的代码更加简洁，让程序员使用时更加方便，但是也有它的局限性，所以一定要根据实际需要有选择性地使用，不要盲目追求所谓的“新特性”。
