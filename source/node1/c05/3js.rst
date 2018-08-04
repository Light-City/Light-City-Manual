.. figure:: http://p20tr36iw.bkt.clouddn.com/js1.jpg
   :alt: 

javascript基础
==============

1.js初识
--------

.. code:: javascript

        JavaScript
        独立的语言，浏览器具有js解释器
        JavaScript代码存在形式：
        -Head中
            # 默认为javascript代码，也可以添加type="text/javascript"
            <script>
                //javascript代码
            </script>
        -body中
        -文件
            <script src='js文件路径'></script>

        解释器从上到下依次执行，如果scrip标签放在head中，若加载不出来，则body中标签内容不会显示，为了避免出现这种问题，则采用：js代码放置在<body>标签内部的最下方
        存在于HTML中
        <script>标签中内容单行注释用//，多行注释用/* */
        -写js代码：
        可以在HTML文件中编写；临时，浏览器的终端console

2.变量
------

.. code:: javascript

    name='***'  # 全局变量
    var name='***' # 局部变量

3.数据类型
----------

.. code:: javascript

    3.1数字
    a="18" #顶一个全局变量a，存储字符串18
    i=parseInt(a) #将字符串转化为int，并保存至i变量
    a=18; #定义了一个int型数据
    3.2字符串
    a="asq"
    a.charAt(0) #获取到了字符
    a.substring(起始位置，结束位置 ) #取出大于等于起始位置且小于结束位置的字符串
    a.lenght #获取当前字符串长度
    3.2.1定时器实例
     <script>
            function f1() {
                console.log(1)
            }
             //创建一个定时器
            //setInterval("alert(123);",2000)
            setInterval("f1();",2000);
     </script>
    3.2.2跑马灯实例
        <div id="i1">欢迎老男孩莅临指导</div>
        <script>
            function change(){
                // 根据ID获取指定标签的内容，定义局部变量接收
                var tag=document.getElementById('i1');
                var content=tag.innerText;
                // 获取标签内部的内容
                var f=content.charAt(0);
                var l=content.substring(1,content.length);
                var new_content=l+f;
                tag.innerText=new_content;

            }
            setInterval("change()",2000)
        </script>

        字符串的split()函数
        >a="5456630"
        <"5456630"
        >a.split('6')
        <["545", "", "30"]

    3.3布尔
        python大写 TRUE、FALSE
        js小写 true、false
    3.4列表(数组)
        python中称列表，而js中称数组
        a=[11,22,33]
        a.splice(start,deleteCount,value,..)插入、删除或替换数组的元素
        a.splice(1),则删除index=1及以后的所有元素，只剩下index为0的元素
        a.splice(1,0,99)在index=1处删除0个元素，并插入99

        a.join('分隔符')
        例如：a.join('-') #11-22-33 将数组转化为字符串

    3.5字典
    a={'k1':'v1','k2':'v2'}

    >

4.for循环
---------

.. code:: javascript

    1.循环输出值
    a=[11,22,33]
    for(var item in a){
        #循环输出索引console.log(item);
        console.log(a[item]);
    }

    2.循环输出key
    a={'k1':'v1','k2':'v2'}
    for(var item in a){
        console.log(item);
        #循环输出valueconsole.log(a[item]);
    }
    3.有序的循环，不支持字典
    a=[11,22,33]
    for(var i=0;i<a.length;i=i+1){
        console.log(a[i])
    }

    # 条件语句
    if(条件){
    }else if(条件){
    }else{
    }
    ==与===的区别
    ==表示只需要满足值相等，而===表示类型与值均要相等。
    例如：
    1=="1"
    true
    1==="1"
    false
    ## 函数
    function 函数名(a,b,c){
    }
    函数名(1,2,3)

5.函数
------

.. code:: javascript

    普通函数：
        function func(){
        }
    匿名函数：
        function func(arg){
            return arg+1
        }
        setInterbal("func()",5000);
        setInterbal(function(){
        console.log(123);
        },5000);
    自执行函数:
        function func(arg){
            console.log(arg);
        }
        func()
        //自动创建并执行
        (function func(arg){
            console.log(1);
        })(1)

6.js序列化及转义
----------------

.. code:: javascript

    //JSON.stringify 将对象转换为字符串
    //列表转字符串
    li=[11,22,33,4,5]
    s=JSON.stringify(li)

    //JSON.parse 将字符串转换为对象类型
    //字符串转列表
    newLi=JSON.parse(s)

7.eval
------

.. code:: javascript

    python:
        val=eval(表达式)
                exec(执行代码)
    JavaScript:
        eval  # 既能跟表达式又能跟执行代码

8.时间
------

.. code:: javascript

        Date
        var d=new Date()
        d.getxxx 获取
        d.setxxx 设置

9.作用域
--------

.. code:: javascript

        其他语言：以代码块作为作用域
        python中以函数作为作用域
        1.javascript以函数作为作用域;
        2.函数的作用域在函数未被调用之前,已经创建;
        3.函数的作用域存在作用域链(一层套一层)，并且也是在被调用之前创建。
        4.函数内局部变量提前声明
        function func(){
            console.log(xxoo);
        }
        func();
        //程序直接报错
        function func(){
            console.log(xxoo);
            var xxoo=''alex;
        }
        func();
        //undefined
        在JavaScript中国var n未赋值，则为undefined
        在进入函数中，会生成作用链，找到内部所有的局部变量，执行局部变量声明。例如第二个代码，生成作用链后，找到了局部变量xxoo，则在函数外面会声明var xxoo，此时未赋值，则结果为undefined

10.JavaScript面向对象
---------------------

.. code:: javascript


        function foo(){
            var xo='alex';
        }
        foo()
        function foo(){
            this.name='alex';
        }
        var obj=new foo();
        obj.name

        原型：
            function Foo(n){
                this.name=n;
            }
            # Foo的原型
            Foo.prototype={
                'sayName'：function(){
                    console.log(this.name)
                }
            }
        obj1=new Foo('we');
        obj1.satName();

        obj2=new Foo('wee');

11.词法分析
-----------

.. code:: javascript

    <script>
        function t1(age){
            console.log(age); // function age()
            var age=27;
            console.log(age); //27
            function age(){}
            console.log(age); //27
        }
        t1(3);
    </script>
    JavaScript在创建的时候会对function进行词法分析，函数会在创建时形成一个活动对象，ActiveObject，简称AO

    1.分析形式参数:此例为age，即生成一个AO.age=undefined的活动对象==>然后根据进行赋值AO.age=3。
    分析局部变量;
    2.在第三行有一个局部变量，跟age重名，此时会生成一个AO.age=undefined的活动对象，AO.age=3会被覆盖;
    3.分析函数声明表达式：此时声明了一个age的函数。所以此时的活动变量为：AO.age=function。优先级最高。
    词法分析结束后函数开始执行，此时的AO.age=function,函数执行时会首先从AO中获取值，所以会首先将function赋给age，所以整个函数的输出为：
    function
    27
    27


