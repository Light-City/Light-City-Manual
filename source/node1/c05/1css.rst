|image0|

**CSS杂谈**
===========

1.CSS text-indent属性
---------------------

定义文本首行的缩进(在首行文字之前插入指定的长度)
可以是具体的值，如：20px;也可以是继承父类缩进：inherit。

2.background
------------

简写属性在一个声明中设置所有的背景属性。

可以设置如下属性：

background-color

background-position

background-size

background-repeat

background-origin

background-clip

background-attachment

background-image

3.text-decoration
-----------------

定义文本是否有划线以及划线的方式

::

    取值:none | [ underline || overline || line-through || blink ] | inherit
    none: 定义正常显示的文本
    [underline || overline || line-through || blink]: 四个值中的一个或多个
    underline: 定义有下划线的文本
    overline: 定义有上划线的文本
    line-through: 定义直线穿过文本
    blink: 定义闪烁的文本
    inherit: 继承

4.字体加粗属性
--------------

font-weight：blod或者具体值，例：200;

5.CSS A link hover active visited伪类超链接锚文本样式
-----------------------------------------------------

介绍这4个常见伪类作用与解释

::

    1、a:link
    设置a对象在未被访问前（未点击过和鼠标未经过）的样式表属性。也就是html a锚文本标签的内容初始样式。
    2、a:hover
    设置对象在其鼠标悬停时的样式表属性，也就是鼠标刚刚经过a标签并停留在A链接上时样式。
    3、a:active
    设置A对象在被用户激活（在鼠标点击与释放之间发生的事件）时的样式表属性。也就是鼠标左键点击html A链接对象与释放鼠标右键之间很短暂的样式效果。
    4、a:visited
    设置a对象在其链接地址已被访问过时的样式表属性。也就是html a超链接文本被点击访问过后的CSS样式效果。

6.字体样式
----------

font-style:italic/\ *斜体*/

7.div#top-----------div跟多个id选择器连在一块写，中间不能有空格。
-----------------------------------------------------------------

8.CSS white-space 属性
----------------------

::

    p
    {
    white-space:nowrap;
    }

    normal  默认。空白会被浏览器忽略。
    pre 空白会被浏览器保留。其行为方式类似 HTML 中的 <pre> 标签。
    nowrap  文本不会换行，文本会在在同一行上继续，直到遇到 <br> 标签为止。
    pre-wrap    保留空白符序列，但是正常地进行换行。
    pre-line    合并空白符序列，但是保留换行符。
    inherit 规定应该从父元素继承 white-space 属性的值。

9.CSS display 属性
------------------

常用属性：

::

    none    此元素不会被显示。
    block   此元素将显示为块级元素，此元素前后会带有换行符。
    inline  默认。此元素会被显示为内联元素，元素前后没有换行符。
    inline-block    行内块元素。（CSS2.1 新增的值）

10.form表单补充
---------------

::

    <form action="" method="post">
     <fieldset>/*对form表单添加了一个边框*/
        <legend>用户高级选项</legend> /*对边框进行了赋值*/
        <p><label>生日</label><input type="text"/></p>
        <p><label>住址</label><input type="password"/></p>
        <p><label>电话号码</label><input type="password"/></p> 
      </fieldset>
    </form>

11.label
--------

    标签定义及使用说明 <label标签为 input 元素定义标注（标记）。 label
    元素不会向用户呈现任何特殊效果。不过，它为鼠标用户改进了可用性。如果您在
    label
    元素内点击文本，就会触发此控件。就是说，当用户选择该标签时，浏览器就会自动将焦点转到和标签相关的表单控件上。
    <label标签的 for 属性应当与相关元素的 id 属性相同。

::

    <form action="demo_form.asp">
      <label for="male">Male</label>
      <input type="radio" name="sex" id="male" value="male"><br>
      <label for="female">Female</label>
      <input type="radio" name="sex" id="female" value="female"><br>
      <input type="submit" value="提交">
    </form>

12.Textarea 对象

Textarea 对象代表 HTML 表单中的一个文本域 (text-area)。HTML 表单的每一个
``<textarea>`` 标签，都能创建一个Textarea 对象。

13.list-style:none
------------------

取消列表风格

14.百分比控制宽度
-----------------

width属性在设置宽度值时，直接表示居中效果，用百分比设置可以理解为相对于父布局所占的宽度。

15.CSS在HTML中的引用方式
------------------------

在HTML中引入CSS共有3种方式： > > （1）外部样式表； > （2）内部样式表； >
（3）内联样式表；

**CSS的3种引用方式**

*1、外部样式表*

外部样式表是最理想的CSS引用方式，在实际开发当中，为了提升网站的性能和维护性，一般都是使用外部样式表。所谓的“外部样式表”，就是把CSS代码和HTML代码都单独放在不同文件中，然后在HTML文档中使用link标签来引用CSS样式表。

当样式需要被应用到多个页面时，外部样式表是最理想的选择。使用样式表，你就可以通过更改一个CSS文件来改变整个网站的外观。

外部样式表在单独文件中定义，并且在

.. raw:: html

   <head>

.. raw:: html

   </head>

标签对中使用link标签来引用。

举例：

::

    <!DOCTYPE html> 
    <html xmlns="http://www.w3.org/1999/xhtml">
    <head>
    <title></title>
    <!--在HTML页面中引用文件名为index的css文件-->
    <link href="index.css" rel="stylesheet" type="text/css" />
    </head>
    <body>
    <div></div>
    </body>
    </html>

说明：

外部样式表都是在head标签内使用link标签来引用的。

*2、内部样式表*
内部样式，指的就是把CSS代码和HTML代码放在同一个文件中，其中CSS代码放在

.. raw:: html

   <style></style>

标签对内，并且

.. raw:: html

   <style></style>

标签对是放在

.. raw:: html

   <head>

.. raw:: html

   </head>

标签对内的。

举例：

::

    <!DOCTYPE html> 
    <html xmlns="http://www.w3.org/1999/xhtml">
    <head>
    <title></title>
    <!--这是内部样式表，CSS样式在style标签中定义-->
    <style type="text/css">
    p{color:Red;}
    </style>
    </head>
    <body>
    <p>哒哒</p>
    <p>哒哒</p>
    <p>哒哒</p>
    </body>
    </html>

*3、内联样式表*

内联样式表，也是把CSS代码和HTML代码放在同一个文件中，但是跟内部样式表不同，CSS样式不是在\ ``<style></style>``
标签对中定义，而是在标签的style属性中定义。

举例：

::

    <!DOCTYPE html> 
    <html xmlns="http://www.w3.org/1999/xhtml">
    <head>
    <title></title>
    </head>
    <body>
    <p style="color:Red; ">哒哒</p>
    <p style="color:Red; ">哒哒</p>
    <p style="color:Red; ">哒哒</p>
    </body>
    </html>

.. |image0| image:: http://p20tr36iw.bkt.clouddn.com/han.jpg

