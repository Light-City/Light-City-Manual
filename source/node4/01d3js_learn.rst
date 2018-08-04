|image0|

第一节:d3.js初识&绘制柱形图
=============================

1.d3.js初识
-----------

d3.js引用
~~~~~~~~~

.. code:: javascript

    <script src="https://d3js.org/d3.v5.min.js" charset="utf-8"></script>

选择器
~~~~~~

.. code:: javascript

    <P>Hello World1</P>
    <P>Hello World2</P>
    <script src="https://d3js.org/d3.v5.min.js" charset="utf-8"></script>
    <script>
        /*
        选择器：d3.select()或d3.selectAll()
        像下面d3.select().selectAll().text()这种称为链式语法
        */
        //d3.select("body").selectAll("p").text("Hello D3js")
        var c = d3.select("body")
              .selectAll("p")
              .text("您好，d3.js!")
        c.style("color","red")
          .style("font-size","72px")

    </script>

选择器进阶
~~~~~~~~~~

.. code:: javascript

    <p>Apple</p>
    <p id="sec" class="twlas">Pear</p>
    <p class="twlas">hah</p>
    <h1>Banana</h1>
    <script src="https://d3js.org/d3.v5.min.js" charset="utf-8"></script>
    <script type="text/javascript">
        /*
      1.选择所有的p
        var p = d3.selectAll("p")
      */
        /*
      2.选择第一个p
        var p = d3.select("p")
      */
        /*
      3.选择第二个p元素 给第二个设置个id,通过id查找
        var p = d3.select("#sec")
      */
      /*
      4.选择最后两个p元素 给最后两个设置相同的class，在查找时通过记得加.
        var p = d3.selectAll(".twlas")
        p.style("color","red")
         .style("font-size","32px")
         .style("font-weight","bold")
      */
      /*
      5.通过function(d,i)设置第一和第三个style
        p.style("color", function(d, i) {
            if(i!=1) {
                return "red"
            }
        })
      */
        //插入元素
        var body = d3.select("body")
        body.append("p")
            .text("append p element")

        //在id为sec前面插入一个p标签
        body.insert("p","#sec")
            .text("insert p element")


        // 删除元素
        var p = body.select("#sec")
        p.remove()
    </script>

选择元素和绑定数据
~~~~~~~~~~~~~~~~~~

.. code:: javascript

    <p>Apple</p>
    <p>Pear</p>
    <p>Banana</p>
    <script src="https://d3js.org/d3.v5.min.js" charset="utf-8"></script>
    <script type="text/javascript">
        /*
            选择元素和绑定数据
            选择元素
                ---d3.select();d3.selectAll() 函数返回结果为选择集
            绑定数据
                ---datum() 绑定一个数据到选择集上
                ---data()  绑定一个数组到选择集上，数组的各项值分别与选择集的各元素绑定
        */

        var p = d3.select("body")
                  .selectAll("p")

        //------datum()练习------

        var str = "China"
        p.datum(str)
         .text(function(d, i){
            return "第 " + (i+1) + " 个元素绑定的数据是 " + d
        })
        /*
            第 1 个元素绑定的数据是 China
            第 2 个元素绑定的数据是 China
            第 3 个元素绑定的数据是 China
        */
        //------data()练习------
        var dataset = ["I like dogs", "I like cats", "I like snakes"]
        p.data(dataset)
         .text(function(d, i) {
            return "第 " + (i+1) + " 个元素绑定的数据是 " + d
         })
         /*
            第 1 个元素绑定的数据是 I like dogs
            第 2 个元素绑定的数据是 I like cats
            第 3 个元素绑定的数据是 I like snakes
         */
    </script>

绘制柱形图
~~~~~~~~~~

.. code:: javascript

    <script src="https://d3js.org/d3.v5.min.js" charset="utf-8"></script>
    <script type="text/javascript">
        var width = 300
        var height = 300
        var svg = d3.select("body")
                    .append("svg")
                    .attr("width",width)
                    .attr("height",height)
        var dataset = [250, 210, 170, 130, 90];  //数据（表示矩形的宽度）
        var rectHeight = 25;   //每个矩形所占的像素高度(包括空白)

        //在 SVG 中，x 轴的正方向是水平向右，y 轴的正方向是垂直向下的
        svg.selectAll("rect")
         .data(dataset) //绑定数组
         .enter() //指定选择集的enter部分
         .append("rect") // 添加足够数量的举行元素
         .attr("x",20)
         .attr("y",function(d,i){
             return i * rectHeight;
         })
         .attr("width",function(d){
             return d;
         })
         .attr("height",rectHeight-2) //减去2表示每个柱子之间留空白
         .attr("fill","#09F");
         /*
            svg.selectAll("rect")   //选择svg内所有的矩形
               .data(dataset)  //绑定数组
               .enter()        //指定选择集的enter部分
               .append("rect") //添加足够数量的矩形元素
            当有数据，而没有足够图形元素的时候，使用此方法可以添加足够的元素。
         */
    </script>

比例尺
~~~~~~

.. code:: javascript

    <script src="https://d3js.org/d3.v5.min.js" charset="utf-8"></script>
    <script type="text/javascript">
      /*
      var dataset = [ 250 , 210 , 170 , 130 , 90 ];
      绘图时，直接使用 250 给矩形的宽度赋值，即矩形的宽度就是 250 个像素。此方式非常具有局限性，如果数值过大或过小。于是，我们需要一种计算关系，能够：将某一区域的值映射到另一区域，其大小关系不变。这就是比例尺（Scale）。
      */
      /*
      1.线性比例尺
      线性比例尺，能将一个连续的区间，映射到另一区间。要解决柱形图宽度的问题，就需要线性比例尺。
      */
      var  dataset = [1.2, 2.3, 0.9, 1.5, 3.3]
      var min = d3.min(dataset)
      var max = d3.max(dataset)
      /*
        比例尺的定义域 domain 为：[0.9, 3.3]
        比例尺的值域 range 为：[0, 300]
      */
      var linear = d3.scaleLinear()
               .domain([min,max])
               .range(0,300)
      linear(0.9);    //返回 0
      console.log(linear(2.3));    //返回 175
      console.log(linear(3.3));    //返回 300

      /*
      2.序数比例尺
      有时候，定义域和值域不一定是连续的
      */
      var index = [0, 1, 2, 3, 4];
      var color = ["red", "blue", "green", "yellow", "black"];

      var ordinal = d3.scaleOrdinal()
              .domain(index)
              .range(color);

      ordinal(0); //返回 red
      ordinal(2); //返回 green
      ordinal(4); //返回 black
    </script>

给柱状图添加比例尺
~~~~~~~~~~~~~~~~~~

.. code:: javascript

    <script src="https://d3js.org/d3.v5.min.js" charset="utf-8"></script>
    <script>
    var width = 300;    //画布的宽度
    var height = 300;   //画布的高度
    var svg = d3.select("body")             //选择文档中的body元素
          .append("svg")                //添加一个svg元素
          .attr("width", width)     //设定宽度
          .attr("height", height);  //设定高度
    var dataset = [ 2.5 , 2.1 , 1.7 , 1.3 , 0.9 ];
    var linear = d3.scaleLinear()
            .domain([0, d3.max(dataset)])
            .range([0, 250]);
    var rectHeight = 25;    //每个矩形所占的像素高度(包括空白)
    svg.selectAll("rect")
        .data(dataset)
        .enter()
        .append("rect")
        .attr("x",20)
        .attr("y",function(d,i){
          return i * rectHeight;
        })
        .attr("width",function(d){
            return linear(d);
        })
        .attr("height",rectHeight-2)
        .attr("fill","steelblue");
    </script>

svg中添加坐标轴
~~~~~~~~~~~~~~~

.. code:: javascript

    /*第一种方式*/
    var xaxis = d3.axisBottom(linearx)
          .ticks(7);            //指定刻度的数量
    svg.append("g")
       .call(xaxis);
    /*第二种方式*/
    function xaxis(selection) {
      selection
          .attr("name1", "value1")
          .attr("name2", "value2");
    }
    foo(d3.selectAll("div"))
    //因此
    xaxis(svg.append("g"))

添加x与y坐标轴
~~~~~~~~~~~~~~

.. code:: html

    ...
    <head>
        ...
        <style type="text/css">
            .xaxis,.yaxis text {
                font-family: sans-serif;
                font-size: 10px;
            }
        </style>
    </head>
    <body>
        <script src="https://d3js.org/d3.v5.min.js" charset="utf-8"></script>
        <script type="text/javascript">
            var width = 300;    //画布的宽度
            var height = 300;   //画布的高度
            var svg = d3.select("body")             //选择文档中的body元素
                        .append("svg")              //添加一个svg元素
                        .attr("width", width)       //设定宽度
                        .attr("height", height);    //设定高度
            var datasetx = [ 2.5 , 2.1 , 1.7 , 1.3 , 0.9 ];

            var linearx = d3.scaleLinear()
                    .domain([0, d3.max(datasetx)])
                    .range([0, 250]);
            var lineary = d3.scaleLinear()
                    .domain([0, 125])
                    .range([0, 125]);

            var rectHeight = 25;    //每个矩形所占的像素高度(包括空白)
            datasety = [25,50,75,100,125]

            svg.selectAll("rect")
                .data(datasetx)
                .enter()
                .append("rect")
                .attr("x",30)
                .attr("y",function(d,i){
                    return i * rectHeight;
                })
                .attr("width",function(d){
                    return linearx(d);
                })
                .attr("height",rectHeight-2)
                .attr("fill","steelblue");

        //若axisBottom改为axisTop()则横坐标的ticks朝上
            var xaxis = d3.axisBottom(linearx)
                        .ticks(7);          //指定刻度的数量
            var yaxis = d3.axisLeft(lineary)
                        .tickValues(datasety)
                        .ticks(7)
        //在 SVG 中添加一个分组元素 <g>，再将坐标轴的其他元素添加到这个 <g> 里即可
            svg.append("g")
                .attr("class","xaxis")
                .attr("transform","translate(30,0)")
                .call(xaxis);
            svg.append("g")
                .attr("class","yaxis")
                .attr("transform","translate(30,-2)")
                .call(yaxis);
        </script>
    </body>
    </html>

原理分析
~~~~~~~~

.. code:: html

    <!--通过以上代码，在谷歌浏览器上可以看出svg里面
    就添加好坐标轴的分组g元素，里面又含有line与text元素，
    分组元素<g>，是 SVG 画布中的元素，意思是 group。
    此元素是将其他元素进行组合的容器，在这里是用于将坐标轴的其他元素分组存放。如果需要手动添加这些元素就太麻烦了，为此，D3 提供了一个组件：d3.axisBottom()。它为我们完成了以上工作。-->
    <g>
    <!-- 第一个刻度 -->
    <g>
    <line></line>   <!-- 第一个刻度的直线 -->
    <text></text>   <!-- 第一个刻度的文字 -->
    </g>
    <!-- 第二个刻度 -->
    <g>
    <line></line>   <!-- 第二个刻度的直线 -->
    <text></text>   <!-- 第二个刻度的文字 -->
    </g>
    ...
    <!-- 坐标轴的轴线 -->
    <path></path>
    </g>

.. figure:: http://p20tr36iw.bkt.clouddn.com/d3js_axis.jpg
   :alt: 

``scaleOrdinal``\ 使用
~~~~~~~~~~~~~~~~~~~~~~

.. code:: javascript

    //在上述代码中添加下面即可
    var xTexts = [ "我", "你", "他" ];
    var oridnal = d3.scaleOrdinal()
                    .domain(xTexts)
                    .range([0,100,250])

.. figure:: http://p20tr36iw.bkt.clouddn.com/d3js_board.jpg
   :alt: 

2.绘制完整的柱形图
------------------

.. code:: javascript

    <body>
        <script src="https://d3js.org/d3.v5.min.js" charset="utf-8"></script>
        <script type="text/javascript">
            //画布大小
        var width = 400;
        var height = 400;

        //在 body 里添加一个 SVG 画布
        var svg = d3.select("body")
            .append("svg")
            .attr("width", width)
            .attr("height", height);

        //画布周边的空白
        var padding = {left:20, right:30, top:50, bottom:20};

        //定义一个数组
        var dataset = [10, 20, 30, 40, 33, 24, 12, 5];

        //x轴的比例尺
        var xScale = d3.scaleBand()
            .domain(d3.range(dataset.length))
            .rangeRound([0, width - padding.left - padding.right]);

        //y轴的比例尺
        var yScale = d3.scaleLinear()
            .domain([0,d3.max(dataset)])
            //[height - padding.top - padding.bottom, 0]这样可以使得y轴正方向向上
            .range([ 0,height - padding.top - padding.bottom]);

        //定义x轴
        var xAxis = d3.axisTop(xScale)
        //定义y轴
        var yAxis = d3.axisLeft(yScale)

        //矩形之间的空白
        var rectPadding = 4;

        //添加矩形元素
        var rects = svg.selectAll(".MyRect")
            .data(dataset)
            .enter()
            .append("rect")
            .attr("class","MyRect")
        //
            .attr("transform","translate(" + padding.left + "," + padding.top + ")")
            /*
            柱子绘制位置
            */
            .attr("x", function(d,i){
                return xScale(i) + rectPadding/2;
            } )
            .attr("y",function(d){
                return 0;
            })
            .attr("width", xScale.bandwidth() - rectPadding)
            .attr("height", function(d){
                return yScale(d);
            });

        //添加文字元素
        var texts = svg.selectAll(".MyText")
            .data(dataset)
            .enter()
            .append("text")
            .attr("class","MyText")
            .attr("transform","translate(" + padding.left + "," + padding.top + ")")
            .attr("x", function(d,i){
                return xScale(i) + rectPadding/2;
            } )
            .attr("y",function(d){
                return yScale(d);
            })
            .attr("dx",function(){
                return (xScale.bandwidth() - rectPadding)/2;
            })
            .attr("dy",function(d){
                return 20;
            })
            .text(function(d){
                return d;
            });

        //添加x轴
        svg.append("g")
            .attr("class","axis")
            .attr("transform","translate(" + padding.left + "," + padding.top + ")")
            .call(xAxis);

        //添加y轴
        svg.append("g")
            .attr("class","axis")
            .attr("transform","translate(" + padding.left + "," + padding.top + ")")
            .call(yAxis);
        </script>
    </body>

.. figure:: http://p20tr36iw.bkt.clouddn.com/d3js_axis_rev.jpg
   :alt: 

图形修改及润色
~~~~~~~~~~~~~~~~~~~~~~

.. code:: javascript

    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Document</title>
        <style type="text/css">

            .MyText {
                fill: #37C743FF;
                text-anchor: middle;
            }
            /*css修改柱子颜色*/
            .MyRect {
                fill: steelblue;
            }
        </style>
    </head>
    <body>
        <script src="https://d3js.org/d3.v5.min.js" charset="utf-8"></script>
        <script type="text/javascript">
            //画布大小
        var width = 400;
        var height = 400;

        //在 body 里添加一个 SVG 画布
        var svg = d3.select("body")
            .append("svg")
            .attr("width", width)
            .attr("height", height);

        //画布周边的空白
        var padding = {left:20, right:30, top:20, bottom:20};

        //定义一个数组
        var dataset = [10, 20, 30, 40, 33, 24, 12, 5];

        //x轴的比例尺
        var xScale = d3.scaleBand()
            .domain(d3.range(dataset.length))
            .rangeRound([0, width - padding.left - padding.right]);

        //y轴的比例尺
        var yScale = d3.scaleLinear()
            .domain([0,d3.max(dataset)])
            //[height - padding.top - padding.bottom, 0]这样可以使得y轴正方向向上
            .range([height - padding.top - padding.bottom, 0]);

        //定义x轴
        var xAxis = d3.axisBottom(xScale)
        //定义y轴
        var yAxis = d3.axisLeft(yScale)

        //矩形之间的空白
        var rectPadding = 4;

        //添加矩形元素
        var rects = svg.selectAll(".MyRect")
            .data(dataset)
            .enter()
            .append("rect")
            .attr("class","MyRect")
            .attr("transform","translate(" + padding.left + "," + padding.top + ")")

            .attr("x", function(d,i){
                return xScale(i) + rectPadding/2;
            } )
            .attr("y",function(d){
                return yScale(d);
            })
            .attr("width", xScale.bandwidth() - rectPadding)
            .attr("height", function(d){
                //y轴朝上写法与上述y轴比例尺的.range([height - padding.top - padding.bottom, 0])配合使用
                return height - padding.top - padding.bottom - yScale(d);
            });

        //添加文字元素
        var texts = svg.selectAll(".MyText")
            .data(dataset)
            .enter()
            .append("text")
            .attr("class","MyText")
            .attr("transform","translate(" + padding.left + "," + padding.top + ")")
            //text的x
            .attr("x", function(d,i){
                return xScale(i) + rectPadding/2;
            } )
            //text的y
            .attr("y",function(d){
                return yScale(d);
            })
            //dx为text的位移，向左为负，向右为正
            .attr("dx",function(){
                return (xScale.bandwidth() - rectPadding)/2 ;
            })
            //根据图形自己调整,dy为text相对于柱子的位移，向下为正，向上为负号
            //当y坐标向下时，为默认情况，此时这里dy为正值时，则正常显示，但当y坐标为上，由于height - padding.top - padding.bottom - yScale(d),此时会出现覆盖情况，text不显示，需手动调整
            .attr("dy",function(d){
                return -5;
            })
            .text(function(d){
                return d;
            });

        //添加x轴
        svg.append("g")
            .attr("class","axis")
            .attr("transform","translate(" + padding.left + "," + (height - padding.bottom) + ")")
            .call(xAxis);

        //添加y轴
        svg.append("g")
            .attr("class","axis")
            .attr("transform","translate(" + padding.left + "," + padding.top + ")")
            .call(yAxis);
        </script>
    </body>
    </html>

.. figure:: http://p20tr36iw.bkt.clouddn.com/d3js_axis_col.jpg
   :alt: 

3.让图表动起来
--------------

图表动起来
~~~~~~~~~~

::

    动态的图表，是指图表在某一时间段会发生某种变化，可能是形状、颜色、位置等，而且用户是可以看到变化的过程的。
    transition() 启动过渡效果
    duration() 指定过渡的持续事件 单位为毫秒
    ease() 指定过渡的方式 d3.easeBounce d3.easeLinear等
    调用：ease(d3.easeBounce)
    delay() 指定延迟的时间，表示一定时间后才开始转变，单位同样为毫秒。此函数可以对整体指定延迟，也可以对个别指定延迟。
    对整体指定时：
      .transition()
      .duration(1000)
      .delay(500)
    如此，图形整体在延迟 500 毫秒后发生变化，变化的时长为 1000 毫秒。因此，过渡的总时长为1500毫秒。
    又如，对一个一个的图形（图形上绑定了数据）进行指定时：
    .transition()
    .duration(1000)
    .delay(funtion(d,i){
        return 200*i;
    })
    如此，假设有 10 个元素，那么第 1 个元素延迟 0 毫秒（因为 i = 0），第 2 个元素延迟 200 毫秒，第 3 个延迟 400 毫秒，依次类推….整个过渡的长度为 200 * 9 + 1000 = 2800 毫秒。

    为上述图形添加动态效果

.. figure:: http://p20tr36iw.bkt.clouddn.com/d3js_dyn.gif
   :alt: 

.. code:: javascript

    //添加矩形元素
        var rects = svg.selectAll(".MyRect")
            .data(dataset)
            .enter()
            .append("rect")
            .attr("class","MyRect")
            .attr("transform","translate(" + padding.left + "," + padding.top + ")")

            .attr("x", function(d,i){
                return xScale(i) + rectPadding/2;
            } )

            .attr("width", xScale.bandwidth() - rectPadding)
            .attr("y",function(d){
                var min = yScale.domain()[0];
                return yScale(min);
            })
            .attr("height", function(d){
                return 0;
            })
            .transition()
            .delay(function(d,i){
                return i * 200;
            })
            .duration(2000)
            .ease(d3.easeBounce)
            .attr("y",function(d){
                return yScale(d);
            })
            .attr("height", function(d){
                return height - padding.top - padding.bottom - yScale(d);
            });


        //添加文字元素
        var texts = svg.selectAll(".MyText")
            .data(dataset)
            .enter()
            .append("text")
            .attr("class","MyText")
            .attr("transform","translate(" + padding.left + "," + padding.top + ")")
            //text的x
            .attr("x", function(d,i){
                return xScale(i) + rectPadding/2;
            } )
            //dx为text的位移，向左为负，向右为正
            .attr("dx",function(){
                return (xScale.bandwidth() - rectPadding)/2 ;
            })
            //根据图形自己调整,dy为text相对于柱子的位移，向下为正，向上为负号
            //当y坐标向下时，为默认情况，此时这里dy为正值时，则正常显示，但当y坐标为上，由于height - padding.top - padding.bottom - yScale(d),此时会出现覆盖情况，text不显示，需手动调整
            .attr("dy",function(d){
                return -5;
            })

            //text的y
            .attr("y",function(d){
                //获取y的最小值
                var min = yScale.domain()[0];
                return yScale(min);
            })
            .transition()
            .delay(function(d,i) {
                return i*200
            })
            .duration(2000)
            .ease(d3.easeCubic)
            .attr("y",function(d){
                return yScale(d)
            })
            //必须放在最后，否则报错！
            .text(function(d){
                return d;
            });

4.浅析Update、Enter、Exit
-------------------------

what is Update and Enter?
~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: javascript

    如果数组为 [3, 6, 9, 12, 15]，将此数组绑定到p的选择集上。
    以下分为两种：
      -第一种：数组元素(数据)大于p标签元素个数
      -第二种：数组元素(数据)小于p标签元素个数
      第一种情况中会有几个数组元素没有对应的p标签元素，此时这部分称为enter,而有数据与p元素相对应的称为update。
      第二种情况中会有几个空余的p元素未能与数据相对应，此时没有数据绑定的部分被称为 Exit。

.. figure:: http://p20tr36iw.bkt.clouddn.com/d3js_update.png
   :alt: 

Update和Enter使用
~~~~~~~~~~~~~~~~~

给定一个元素个数为6的数组，3个p标签，分别处理Update与Enter

.. code:: javascript

    <p></p>
    <p></p>
    <p></p>
    <script type="text/javascript" src="d3.min.js" charset="utf-8"></script>
    <script type="text/javascript">
        var dataset = [3,6,9,15,18,20]
        // 选择body中的p元素
        var p =d3.select("body")
                 .selectAll("p")
        //获取enter部分
        var update = p.data(dataset)
        //update部分的处理：更新属性值
        update.text(function(d,i) {
            return "index " + i +" update " + d
        })
        var enter = update.enter()
        //enter部分的处理：添加元素后赋予属性值
        enter.append("p")
             .text(function(d, i){
                return "index " + i +" enter " + d
        });
    </script>

output
~~~~~~

.. code:: javascript

    index 0 update 3
    index 1 update 6
    index 2 update 9
    index 3 enter 15
    index 4 enter 18
    index 5 enter 20

Update 和 Exit 的使用
~~~~~~~~~~~~~~~~~~~~~

当对应的元素过多时 （ 绑定数据数量 < 对应元素 ），需要删掉多余的元素。

.. code:: javascript


    <p></p>
    <p></p>
    <p></p>
    <script type="text/javascript" src="d3.min.js" charset="utf-8"></script>
    <script type="text/javascript">
        var dataset = [3];
        //选择body中的p元素
        var p = d3.select("body").selectAll("p");
        //获取update部分
        var update = p.data(dataset);
        //获取exit部分
        var exit = update.exit();
        //update部分的处理：更新属性值
        update.text(function(d){
            return "update " + d;
        });
        //exit部分的处理：修改p元素的属性
        exit.text(function(d){
                return "exit";
            });
        //exit部分的处理通常是删除元素
        // exit.remove();
    </script>

output
~~~~~~

.. code:: javascript

    update 3
    exit
    exit

5.交互式操作
------------

.. figure:: http://p20tr36iw.bkt.clouddn.com/d3js_inter.png
   :alt: 

用户用于交互的工具一般有三种：鼠标、键盘、触屏
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: javascript

    //添加矩形元素
    var rects = svg.selectAll(".MyRect")
        .data(dataset)
        .enter()
        .append("rect")
        .attr("class","MyRect")   //把类里的 fill 属性清空
        .attr("transform","translate(" + padding.left + "," + padding.top + ")")
        .attr("x", function(d,i){
            return xScale(i) + rectPadding/2;
        } )
        .attr("y",function(d){
            return yScale(d);
        })
        .attr("width", xScale.bandwidth() - rectPadding )
        .attr("height", function(d){
            return height - padding.top - padding.bottom - yScale(d);
        })
        .attr("fill","steelblue")       //填充颜色不要写在CSS里
        .on("mouseover",function(d,i){
            console.log("MouseOver");
            d3.select(this)
                .attr("fill","yellow");
        })
        .on("click",function (d,i) {
            console.log("Click!");
        })
        .on("mouseout",function(d,i){
            console.log("MouseOut")
            d3.select(this)
                .transition()
                .duration(500)
                .attr("fill","steelblue");
        });

.. |image0| image:: http://p20tr36iw.bkt.clouddn.com/d3js_axis_col.jpg
