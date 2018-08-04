.. figure:: http://p20tr36iw.bkt.clouddn.com/dom.jpg
   :alt: 
   
Dom
=======================

1.找到标签
----------

.. code:: html

    -直接找
    获取单个元素document.getElementById('i1')
    获取指定标签名的元素子元素集合document.getElementsByName('***')
    获取多个元素(列表)document.getElementsByTagName('div')
    获取多个元素(列表)document.getElementsByClassgName('c1')
    -间接找
    parentElement 父节点标签元素
    children 所有子元素
    lastElementChild 最后一个子标签元素
    nextElementChile 下一个兄弟标签元素
    previousElementSibling 上一兄弟标签元素

2.操作标签
----------

.. code:: html

    a.innerText
        获取标签中的文本内容，而innerHTML全内容包含标签
        标签.innerText #仅文本
        对标签内部文本进行重新复制
        标签.innerText=""
    b.classname
        tag.className直接整体做操作
        tag.classList.add('样式名') 添加指定样式
        tag.classList.remove('样式名') 删除指定样式
        PS:
        <idv onclick='unc();'>点我</div>
        <script>
            function func(){
            }
        </script>

    c.value
        input value获取当前标签中的值
        select 获取选中的value的值
        selectedIndex 获取选中的index
        textarea value获取当前标签中的值

.. code:: html

    实例：

     <div id="i1">我是i1</div>
     <a>sad</a>
     <a>23</a>
     <a>kse</a>

    输入：document.getElementById('i1')
    输出：<div id="i1">我是i1</div>
    输入：document.getElementById('i1').innerText
    输出："我是i1"
    输入：targs=document.getElementsByTagName('a');
    输出：HTMLCollection(3) [a, a, a]
    输入：targs[0].innerText;
    输出："sad"
    输入：targs[0].innerText=777
    输出：777
    输入：for(var i=0;i<targs.length;i++){targs[i].innerText=777;}
    输出：777
    <div>
        <div></div>
        <div>
            c1
        </div>
    </div>
    <div>
        <div></div>
        <div id="i1">
            c2
        </div>
    </div>
    <div>
        <div></div>
        <div>
            c3
        </div>
    </div>

    tag=document.getElementById('i1')
     <div id="i1">
         c2
    </div>
    tag.parentElement
    <div>
        <div></div>
        <div id="i1">
            c2
        </div>
    </div>
    tag.parentElement.children
    HTMLCollection(2) [div, div#i1, i1: div#i1]
    tag.parentElement.previousElementSibling
    <div>
        <div></div>
        <div>
            c1
        </div>
    </div>
    tag.parentElement.nextElementSibling
    <div>
        <div></div>
        <div>
            c3
        </div>
    </div>
    tag.className="c2"
    "c2"
    tag
    <div id="i1" class="c2">
            c2
        </div>
    tag.classList
    DOMTokenList ["c2", value: "c2"]
    tag.classList.add('c3')
    <div id="i1" class="c2 c3">
            c2
        </div>

3.实例之模态对话框
------------------

.. code:: html

        <style>
            .hide{
                display: none;
            }
            .c1 {
                position: fixed;
                left: 0;
                right: 0;
                top: 0;
                bottom: 0;
                background-color: black;
                opacity: 0.6;
                z-index: 9;
            }
            .c2{
                width: 500px;
                height: 400px;
                background-color: white;
                position: fixed;
                left: 50%;
                top: 50%;
                margin-left: -250px;
                margin-top: -200px;
                z-index: 10;
            }
        </style>

        <body style="margin:0;">
            <div>
                <input type="button" value="添加" onclick="ShowModel();"/>
                <table>
                    <thead>
                        <tr>
                            <th>主机名</th>
                            <th>端口</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr>
                            <td>1.1.1.1</td>
                            <td>190</td>
                        </tr>
                        <tr>
                            <td>1.1.1.2</td>
                            <td>192</td>
                        </tr>
                        <tr>
                            <td>1.1.1.3</td>
                            <td>193</td>
                        </tr>
                    </tbody>
                </table>
            </div>
            <!--遮罩层开始-->
            <div class="c1 hide" id="i1"></div>
            <!--遮罩层结束-->
            <!--弹出框开始-->
            <div class="c2 hide" id="i2">
                <p><input type="text"/></p>
                <p><input type="text"/></p>
                <p>
                    <input type="button" value="取消" onclick="HideModel()"/>
                    <input type="button" value="确定"/>
                </p>
            </div>
            <!--弹出框结束-->
            <script>
                /* 点击添加出现模态对话框*/
                function ShowModel() {
                    document.getElementById('i1').classList.remove('hide');
                    document.getElementById('i2').classList.remove('hide');
                }
                /*点击取消去掉模态对话框*/
                function HideModel() {
                    document.getElementById('i1').classList.add('hide');
                    document.getElementById('i2').classList.add('hide');
                }
            </script>
        </body>

4.实例之模态对话框+全选反选及取消
---------------------------------

.. code:: html

    checkbox
        获取值
        checkbox对象.checked
        设置值
        checkbox对象.checked = true


    <head>
        <meta charset="UTF-8">
        <title>Title</title>
        <style>
            .hide{
                display: none;
            }
            .c1 {
                position: fixed;
                left: 0;
                right: 0;
                top: 0;
                bottom: 0;
                background-color: black;
                opacity: 0.6;
                z-index: 9;
            }
            .c2{
                width: 500px;
                height: 400px;
                background-color: white;
                position: fixed;
                left: 50%;
                top: 50%;
                margin-left: -250px;
                margin-top: -200px;
                z-index: 10;
            }
        </style>
    </head>
    <body style="margin:0;">
        <div>
            <input type="button" value="添加" onclick="ShowModel();"/>
            <input type="button" value="全选" onclick="ChooseAll();"/>
            <input type="button" value="取消" onclick="CancelAll();"/>
            <input type="button" value="反选" onclick="ReverseAll();"/>
            <table>
                <thead>
                    <tr>
                        <th>选择</th>
                        <th>主机名</th>
                        <th>端口</th>
                    </tr>
                </thead>
                <tbody id="tb">
                    <tr>
                        <td><input type="checkbox"/></td>
                        <td>1.1.1.1</td>
                        <td>190</td>
                    </tr>
                    <tr>
                        <td><input type="checkbox"/></td>
                        <td>1.1.1.2</td>
                        <td>192</td>
                    </tr>
                    <tr>
                        <td><input type="checkbox"/></td>
                        <td>1.1.1.3</td>
                        <td>193</td>
                    </tr>
                </tbody>
            </table>
        </div>
        <!--遮罩层开始-->
        <div class="c1 hide" id="i1"></div>
        <!--遮罩层结束-->
        <!--弹出框开始-->
        <div class="c2 hide" id="i2">
            <p><input type="text"/></p>
            <p><input type="text"/></p>
            <p>
                <input type="button" value="取消" onclick="HideModel()"/>
                <input type="button" value="确定"/>
            </p>
        </div>
        <!--弹出框结束-->
        <script>
            /* 点击添加出现模态对话框*/
            function ShowModel() {
                document.getElementById('i1').classList.remove('hide');
                document.getElementById('i2').classList.remove('hide');
            }
            /*点击取消去掉模态对话框*/
            function HideModel() {
                document.getElementById('i1').classList.add('hide');
                document.getElementById('i2').classList.add('hide');
            }
            function ChooseAll() {
                var tbody = document.getElementById('tb');
                //获取所有的tr
                var tr_list=tbody.children;
                for (var i=0;i<tr_list.length;i++){
                    //循环所有的tr，current_tr
                    var current_tr=tr_list[i];
                    var checkbox=current_tr.children[0].children[0];
                    checkbox.checked=true;
                }
            }

            function CancelAll() {
                var tbody = document.getElementById('tb');
                var tr_list=tbody.children;
                for (var i=0;i<tr_list.length;i++){
                    var current_tr=tr_list[i];
                    var checkbox=current_tr.children[0].children[0];
                    checkbox.checked=false;
                }
            }
            function ReverseAll() {
                var tbody = document.getElementById('tb');
                var tr_list=tbody.children;
                for (var i=0;i<tr_list.length;i++){
                    var current_tr=tr_list[i];
                    var checkbox=current_tr.children[0].children[0];
                    if (checkbox.checked==true){
                        checkbox.checked=false;
                    }else{
                          checkbox.checked=true;
                    }
                }
            }
        </script>
    </body>

5.实例之后台管理左侧菜单
------------------------

.. code:: html


    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
        <style>
            .hide{
                display: none;
            }
            .item .header{
                height: 35px;
                background-color: #2459a2;
                color: white;
                line-height: 35px;
            }
        </style>
    </head>
    <body>
        <div style="height: 48px"></div>

        <div style="width: 300px">

            <div class="item">
                <div id='i1' class="header" onclick="ChangeMenu('i1');">菜单1</div>
                <div class="content">
                    <div>内容1</div>
                    <div>内容1</div>
                    <div>内容1</div>
                </div>
            </div>
            <div class="item">
                <div id='i2' class="header" onclick="ChangeMenu('i2');">菜单2</div>
                <div class="content hide">
                    <div>内容2</div>
                    <div>内容2</div>
                    <div>内容2</div>
                </div>
            </div>
            <div class="item">
                <div id='i3' class="header" onclick="ChangeMenu('i3');">菜单3</div>
                <div class="content hide">
                    <div>内容3</div>
                    <div>内容3</div>
                    <div>内容3</div>
                </div>
            </div>
            <div class="item">
                <div id='i4' class="header" onclick="ChangeMenu('i4');">菜单4</div>
                <div class="content hide">
                    <div>内容4</div>
                    <div>内容4</div>
                    <div>内容4</div>
                </div>
            </div>
        </div>
        <script>
            function ChangeMenu(nid){
                var current_header = document.getElementById(nid);
                var item_list = current_header.parentElement.parentElement.children;
                for(var i=0;i<item_list.length;i++){
                    var current_item = item_list[i];
                    current_item.children[1].classList.add('hide');
                }
                current_header.nextElementSibling.classList.remove('hide');
            }
        </script>
    </body>
    </html>

6.实例之input输入框默认显示请输入关键字，tab键/鼠标点击后，这些文字消失，移出输入框，则又重新显示请输入关键字
-------------------------------------------------------------------------------------------------------------

.. code:: html

    方法一：js+dom实现
    <div style="width: 600px;margin: 0 auto;">
        <input id="i1" onfocus="Focus();" onblur="Blur()" type="text" value="请输入关键字"/>

    </div>
    <script>
        function Focus() {
            var tag=document.getElementById('i1');
            var val=tag.value;
            if(val=="请输入关键字"){
                tag.value="";
            }
        }
        function Blur() {
            var tag=document.getElementById('i1');
            var val=tag.value;
            if(val==""){
                tag.value="请输入关键字";
            }
        }
    </script>
    方法二：针对最新浏览器，HTML input标签中placeholder属性自带这种效果
    <input placeholder="请输入关键字">

7.Dom样式操作
-------------

.. code:: html

    >代表输入，<代表输出
    >obj=document.getElementById('i1')
    <input id="i1" onfocus="Focus();" onblur="Blur()" type="text" value="请输入关键字">
    >obj.className="c1 c2"
    <"c1 c2"
    >obj
    < <input id=​"i1" onfocus=​"Focus()​;​" onblur=​"Blur()​" type=​"text" value=​"请输入关键字" class=​"c1 c2">​
    >obj.classList
    <DOMTokenList(2) ["c1", "c2", value: "c1 c2"]
    >obj.classList.add('c3')
    <undefined
    >obj
    <<input id="i1" onfocus="Focus();" onblur="Blur()" type="text" value="请输入关键字" class="c1 c2 c3">
    >obj.classList.remove('c2')
    <undefined
    >obj
    <<input id="i1" onfocus="Focus();" onblur="Blur()" type="text" value="请输入关键字" class="c1 c3">
    >obj.style.fontSize='16px'
    <<input id="i1" onfocus="Focus();" onblur="Blur()" type="text" value="请输入关键字" class="c1 c3" style="font-size: 16px;">

8.属性操作
----------

.. code:: html

    >obj=document.getElementById('i1')
    < <input id="i1" onfocus="Focus();" onblur="Blur()" value="请输入关键字" type="text">
    >obj.setAttribute('new_attr','new_value')
    <undefined
    >obj
    < <input id="i1" onfocus="Focus();" onblur="Blur()" value="请输入关键字" new_attr="new_value" type="text">
    >obj.removeAttribute('new_attr')
    <undefined
    >obj
    < <input id="i1" onfocus="Focus();" onblur="Blur()" value="请输入关键字" type="text">
    >obj.attributes
    <NamedNodeMap [ id="i1", onfocus="Focus();", onblur="Blur()", value="请输入关键字", type="text" ]

9.创建标签，并添加到HTML中
--------------------------

.. code:: html

    <input type="button" onclick="AddEle();" value="+"/>
        <div id="i1">
                <p><input type='text'/></p>
        </div>
        <script>
                    //方法一
    //        function AddEle() {
    //            //创建一个标签
    //            //将标签添加大i1里面
    //            var tag = "<p><input type='text'/></p>";
    // 注意第一个参数只能是'beforeEnd'、'beforeBegin'、'afterBegin'、'afterEnd'
    //            document.getElementById('i1').insertAdjacentHTML('beforeEnd',tag);
    // tag为div中的内容，根据tag位置区别上述四个区别。
        beforeBegin text
        <div>beforeBegin html</div>
        <div id="tag">
            afterBegin text
            <div>afterBegin html</div>
            tag
            <div>beforeEnd html</div>
            beforeEnd text
        </div>
        afterEnd text
        <div>afterEnd html</div>
    //        }
                //方法二
                function AddEle() {
                        var tag = document.createElement('input');
                        tag.setAttribute('type', 'text');
                        document.getElementById('i1').appendChild(tag);
                }
        </script>
    上述两者区别，方法一中insertAdjacentHTML第二个参数传递的是字符串，方法二中appendChild中传递的是标签对象。

10.提交表单
-----------

.. code:: html

    任何标签通过DOM都可提交表单
    <form id="f1" action="http://www.baidu.com">
        <input type="text"/>
        <input type="submit" value="提交"/>
        <a onclick="submitForm()">提交另一种方式</a>
    </form>
    <script>
        function submitForm() {
            document.getElementById('f1').submit()
        }
    </script>

11.其他
-------

.. code:: html

    console.log 输出框
    alert 弹出框
    confirm 确认框
    location.href 获取url
    location.href="http://www.baidu.com" 重定向，跳转
    location.href=location.href<<=>>location.reload() 页面刷新
    var obj = setInterval(function(){
        },5000) 设置定时器 一直执行
    clearInterval(obj) 清除定时器
    setTimeout()用法同上，定时器只执行一次。
    setTimeout实例：qq邮箱删除邮件上面提示已删除，倒计时几秒后自动取消。
    clearTimeou() 清除
    <div id="status"></div>
    <input type="button" value="删除" onclick="deleteEle();">
    <script>
        function deleteEle() {
            document.getElementById('status').innerText='已删除';
            setTimeout(function () {
                document.getElementById('status').innerText='';
            },5000);
        }
    </script>

12.事件
-------

.. code:: html

    onclick,onblur,onfocus,onmouseover,onmouseout
    去掉div中的事件
    在script标签中获取div
    例如:var mydiv=document.getElementById("test");然后在后面为获得的div添加onclick事件：
    mydiv.onclick=function(){
        console.log("asd");
    }，这样做的好处是做好了行为、样式、结构相分离。
    <!--代码分离实例-->
    <table border="1" width="300px">
        <tr><td>1</td><td>2</td><td>3</td></tr>
        <tr><td>1</td><td>2</td><td>3</td></tr>
        <tr><td>1</td><td>2</td><td>3</td></tr>
    </table>
    <script>
        var myTrs = document.getElementsByTagName("tr");
        var len=myTrs.length;
        for(var i=0;i<len;i++){
            myTrs[i].onmouseover=function () {
                //谁调用这个函数，this就指向谁,这里不能写myTrs[i].style.backgroundColor="red";作用域问题(当触发此事件时i一直都是2)
                this.style.backgroundColor = "red";
            }
            myTrs[i].onmouseout=function () {
                //谁调用这个函数，this就指向谁
                this.style.backgroundColor = "";
            }
        }
    </script>

    绑定事件两种方式：
        a.直接标签绑定 onclick='xxx()' onfocus
        b.先获取Dom对象，然后进行绑定
        document.getElementById('xx').onclick
        document.getElementById('xx').onfocus
        this,当前触发事件的标签
            a.第一种绑定方式
                <input type='button' onclick='ClickOn(this)'>
                function ClickOn(self){
                    //self当前点击的标签
                }
            b.第二种绑定方式
                <input id='i1' type='button'>
                doucument.getElementById('i1').onclick=function{
                    this 代指当前点击的标签
                }

.. |image0| image:: http://p20tr36iw.bkt.clouddn.com/dom.jpg

