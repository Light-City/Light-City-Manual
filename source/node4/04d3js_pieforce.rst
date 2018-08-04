.. figure:: http://p20tr36iw.bkt.clouddn.com/d3js_force_show.jpg
   :alt: 

第四节:d3.js布局及实战
=======================

1.布局
------

布局的作用是：将不适合用于绘图的数据转换成了适合用于绘图的数据。

布局的作用解释成：数据转换。

2.布局之饼图
------------

.. figure:: http://p20tr36iw.bkt.clouddn.com/d3js_pie.jpg
   :alt: 

查看饼图布局的数据
~~~~~~~~~~~~~~~~~~

.. code:: javascript

    <script type="text/javascript" src="d3.min.js" charset="utf-8"></script>
    <script type="text/javascript">
        var width = 400
        var height = 400
        var dataset = [30, 10, 43, 55, 13]

        var svg = d3.select("body")
                    .append("svg")
                    .attr("width", width)
                    .attr("height", height)
        //定义布局
        var pie = d3.pie()
        //将dataset数据按照饼图布局转换为piedata
        console.log(pie)
        var piedata  = pie(dataset)
        console.log(piedata)
    </script>

.. raw:: html

   <p>

布局不是要直接绘图，而是为了得到绘图所需的数据。

.. raw:: html

   </p>

.. figure:: http://p20tr36iw.bkt.clouddn.com/d3js_data.jpg
   :alt: 

绘制图形
~~~~~~~~

.. raw:: html

   <p>

以上得到绘图的数据，则需要用到生成器，由于饼图的每一部分都是一段弧，所以这里用到的是弧生成器(弧的路径)。

.. raw:: html

   </p>

.. code:: javascript

    var outerRadius = 150 //外半径，越大，圆越大
    var innerRadius = 0 //内半径,这里不为0的话，在大圆中心嵌套个小圆
    var arc = d3.arc() //弧生成器
                .innerRadius(innerRadius) //设置内半径
                .outerRadius(outerRadius) //设置外半径
    //<svg>元素中添加足够的分组元素g，每一个分组用于存放一段弧的相关元素。
    var arcs = svg.selectAll("g")
                  .data(piedata)
                  .enter()
                  .append("g")
                  .attr("transform", "translate(" + (width/2) + "," + (width/2) + ")")
    // 为每个<g>元素，添加<path>
    //v3 d3.scale.category10()
    var color = d3.scaleOrdinal(d3.schemeCategory10)
    arcs.append("path")
        .attr("fill", function(d, i) {
            return color(i)
        })
        .attr("d", function(d) {
            return arc(d) //调用弧生成器，得到路径值
        })
    arcs.append("text")
        .attr("transform",function(d) {
            return "translate(" + arc.centroid(d) + ")"
        })
        .attr("text-anchor","middle")
        .text(function(d) {
            return d.data
        })

.. code:: javascript

    arc.centroid(d) 能算出弧线的中心。要注意，text() 里返回的是 d.data ，而不是 d 。因为被绑定的数据是对象，里面有 d.startAngle、d.endAngle、d.data 等，其中 d.data 才是转换前的整数的值。

3.力向图
--------

.. code:: javascript

    <script type="text/javascript" src="d3.min.js" charset="utf-8"></script>
    <script type="text/javascript">
        var width = 400
        var height = 400

        var nodes = [ { name: "桂林" }, { name: "广州" },
                { name: "厦门" }, { name: "杭州" },
                { name: "上海" }, { name: "青岛" },
                { name: "天津" } ];

            var edges = [ { source : 0 , target: 1 } , { source : 0 , target: 2 } ,
                 { source : 0 , target: 3 } , { source : 1 , target: 4 } ,
                 { source : 1 , target: 5 } , { source : 1 , target: 6 } ];
        var svg = d3.select("body")
                    .append("svg")
                    .attr("width", width)
                    .attr("height", height)
        //定义布局
        var force = d3.forceSimulation().alphaDecay(0.1) // 设置alpha衰减系数
                      .force("link", d3.forceLink().distance(100)) // distance为连线的距离设置
                      .force("Centering", d3.forceCenter(width / 2, height / 2))
                        .force('collide', d3.forceCollide().radius(() => 30))  // collide 为节点指定一个radius区域来防止节点重叠。
                      .force("charge", d3.forceManyBody().strength(-400))  // 节点间的作用力
                      .nodes(nodes)
                      .force('link', d3.forceLink(edges))
        force.restart()
        console.log(nodes)
        console.log(edges)
    </script>

结点数据
~~~~~~~~

.. figure:: http://p20tr36iw.bkt.clouddn.com/d3js_data_nodes.jpg
   :alt: 

边数据
~~~~~~

.. figure:: http://p20tr36iw.bkt.clouddn.com/d3js_force_links.jpg
   :alt: 

绘制力向图
~~~~~~~~~~

.. figure:: http://p20tr36iw.bkt.clouddn.com/d3js_force_show.jpg
   :alt: 

::

    所需元素：
      line，线段，表示连线。
      circle，圆，表示节点。
      text，文字，描述节点。

.. code:: javascript

    //定义布局
    var force = d3.forceSimulation().alphaDecay(0.1) // 设置alpha衰减系数
                  .force("link", d3.forceLink().distance(200)) // distance为连线的距离设置
                  .force("Centering", d3.forceCenter(width / 2, height / 2))
                  .force('collide', d3.forceCollide().radius(() => 70))  // collide 为节点指定一个radius区域来防止节点重叠。
                  .force("charge", d3.forceManyBody().strength(-400))  // 节点间的作用力
                  .nodes(nodes)
                  .force('link', d3.forceLink(edges))
                  .on("tick", function(){ //对于每一个时间间隔
                //更新连线坐标
                svg_edges.attr("x1",function(d){ return d.source.x; })
                    .attr("y1",function(d){ return d.source.y; })
                    .attr("x2",function(d){ return d.target.x; })
                    .attr("y2",function(d){ return d.target.y; });

                //更新节点坐标
                svg_nodes.attr("cx",function(d){ return d.x; })
                    .attr("cy",function(d){ return d.y; });

                //更新文字坐标
                svg_texts.attr("x", function(d){ return d.x; })
                   .attr("y", function(d){ return d.y; });
          });



    force.restart()
    console.log(nodes)
    console.log(edges)

    //添加连线
    var svg_edges = svg.selectAll("line")
                       .data(edges)
                       .enter()
                       .append("line")
                       .style("stroke","#ccc")
                       .style("stroke-width",1)

    var color = d3.scaleOrdinal(d3.schemeAccent)
    //添加节点
    var svg_nodes = svg.selectAll("circle")
                       .data(nodes)
                       .enter()
                       .append("circle")
                       .attr("r",20)
                       .style("fill", function(d, i) {
                            return color(i)
                       })
                       .call(d3.drag()
                            .on("start", dragstarted)
                            .on("drag", dragged)
                            .on("end", dragended))
    //添加描述节点的文字
    var svg_texts = svg.selectAll("text")
                       .data(nodes)
                       .enter()
                       .append("text")
                       .style("fill", "black")
                       .attr("dx", 20)
                       .attr("dy", 8)
                       .text(function(d){
                            return d.name;
                        });


    function dragstarted(d) {
            if (!d3.event.active) force.alphaTarget(0.3).restart();
            d.vx = d.x;
            d.vy = d.y;
        }

        function dragged(d) {
            d.vx = d3.event.x;
            d.vy = d3.event.y;
        }

        function dragended(d) {
            if (!d3.event.active) force.alphaTarget(0);
            d.vx = null;
            d.vy = null;
        }

力向图进阶
~~~~~~~~~~

.. code:: javascript

    //1.结点显示图片
    var nodes_img = svg.selectAll("image")
                .data(root.nodes)
                .enter()
                .append("image")
                .attr("width",img_w)
                .attr("height",img_h)
                .attr("xlink:href",function(d){
                return d.image;
                })
                                    //更新结点图片和文字
    nodes_img.attr("x",function(d){ return d.x - img_w/2; });
    nodes_img.attr("y",function(d){ return d.y - img_h/2; });
    {
    "nodes":[
    { "name": "云天河"   , "image" : "./test/tianhe.png" },
    { "name": "xx"   , "image" : "./test/xx.png" },
    .....
    ],
    "edges":[
    { "source": 0 , "target": 1 , "relation":"挚友" },
    { "source": 0 , "target": 2 , "relation":"xx" },
    ........
    ]
    }
    //2.绘制箭头
    var svg = d3.select("body")
                .append("svg")
                .attr("width", width)
                .attr("height", height)
                .call(d3.zoom().scaleExtent([1, 40])
                               .translateExtent([
                                  [-100, -100],
                                  [width + 90, height + 100]
                                ])
                                //添加点击结点放大效果
                               .on("zoom", function () {
                                  svg.attr("transform", d3.event.transform)
                               })).append("g");
    //箭头
    svg.append('defs').append('marker')
            .attr('id','end')
            .attr('viewBox','-0 -5 10 10')
            .attr('refX',25)
            .attr('refY',0)
            .attr('orient','auto')
            .attr('markerWidth',10)
            .attr('markerHeight',10)
            .attr('xoverflow','visible')
            .append('svg:path')
            .attr('d', 'M 0,-5 L 10 ,0 L 0,5')
            .attr('fill', '#f0f')
            .attr('stroke','#f0f');

4.弦图
------

.. figure:: http://p20tr36iw.bkt.clouddn.com/d3js_chord_pic.jpg
   :alt: 

弦图（Chord），主要用于表示两个节点之间的联系。
两点之间的连线，表示谁和谁具有联系。 线的粗细表示权重。

数据
~~~~

.. code:: javascript

    var city_name = [ "北京" , "上海" , "广州" , "深圳" , "香港"  ];
    //数据 表达意思为
    /*
         北京  上海
    北京 1000  3045　
    上海 3214  2000　
    统计人口来源
    来自北京本地1000人，3045人来自上海
    */
    var population = [
      [ 1000,  3045　 , 4567　, 1234 , 3714 ],
      [ 3214,  2000　 , 2060　, 124  , 3234 ],
      [ 8761,  6545　 , 3000　, 8045 , 647  ],
      [ 3211,  1067  , 3214 , 4000  , 1006 ],
      [ 2146,  1034　 , 6745 , 4764  , 5000 ]
    ];
    /*
    v3写法
    var chord_layout = d3.layout.chord()
             .padding(0.03) //节点之间的间隔
             .sortSubgroups(d3.descending) //排序
             .matrix(population); //输入矩阵

    */
    //v4及v5写法
    var chord_layout = d3.chord()
                         .padAngle(0.03) //节点之间的间隔
                         .sortSubgroups(d3.descending)//排序
    /*
    matrix实际分成了两个部分，groups 和 chords，
    其中groups表示弦图上的弧，称为外弦，groups中的各个元素都被计算用添加上了angle、startAngle、endAngle、index、value等字段；
    chords 称为内弦，表示弦图中节点的连线及其权重。chords 里面分为 source 和 target ，分别标识连线的两端。
    v3写法
    var groups = chord_layout.groups();
    var chords = chord_layout.chords();
    console.log( groups );
    console.log( chords );
    */
    //v4写法
    chord_layout = chord_layout(population) //输入矩阵
    var groups = chord_layout.groups
    var chords = chord_layout
    console.log(groups)
    console.log(chords)

output
~~~~~~

.. figure:: http://p20tr36iw.bkt.clouddn.com/d3js_chord.jpg
   :alt: 

定义相关变量
~~~~~~~~~~~~

.. code:: javascript

    var width = 600
    var height = 600
    var innerRadius = width/2 * 0.7;
    var outerRadius = innerRadius * 1.1;
    var color = d3.scaleOrdinal(d3.schemeAccent)
    var svg = d3.select("body").append("svg")
                               .attr("width", width)
                               .attr("height", height)
                               .append("g")
                               .attr("transform", "translate(" + width/2 + "," + height/2 + ")")

绘制节点
~~~~~~~~

.. code:: javascript

    //绘制节点（即分组，有多少个城市画多少个弧形），及绘制城市名称。
    //节点位于弦图的外部。节点数组 groups 的每一项，都有起始角度和终止角度，因此节点其实是用弧形来表示的，
    var outer_arc = d3.arc()
                      .innerRadius(innerRadius)
                      .outerRadius(outerRadius);

    var g_outer = svg.append("g");

    g_outer.selectAll("path")
           .data(groups)
           .enter()
           .append("path")
           .style("fill", function(d) { return color(d.index); })
           .style("stroke", function(d) { return color(d.index); })
           .attr("d", outer_arc );

    /*
    节点的文字（即城市名称），有两个地方要特别注意。

    each()：表示对任何一个绑定数据的元素，都执行后面的无名函数 function(d,i) ，函数体里做两件事：
    计算起始角度和终止角度的平均值，赋值给 d.angle 。
    将 city_name[i] 城市名称赋值给 d.name 。
    transform 的参数：用 translate 进行坐标变换时，要注意顺序： rotate -> translate（先旋转再平移）。 此外，
    ( ( d.angle > Math.PI*3/4 && d.angle < Math.PI*5/4 ) ? "rotate(180)" : "")
    意思是，当角度在 135° 到 225° 之间时，旋转 180°。不这么做的话，下方的文字是倒的。
    */
    g_outer.selectAll("text")
           .data(groups)
           .enter()
           .append("text")
           .each(function(d,i) {
               d.angle = (d.startAngle + d.endAngle) / 2;
               d.name = city_name[i];
           })
           .attr("dy",".35em")
           .attr("transform", function(d){
              return "rotate(" + ( d.angle * 180 / Math.PI ) + ")" + "translate(0,"+ -1.0*(outerRadius+10) +")" + ( ( d.angle > Math.PI*3/4 && d.angle < Math.PI*5/4 ) ? "rotate(180)" : "");
           })
           .text(function(d){
              return d.name;
           });

绘制连线（弦）
~~~~~~~~~~~~~~

.. code:: javascript

    var inner_chord = d3.ribbon()
                        .radius(innerRadius);
    svg.append("g")
        .attr("class", "chord")
        .selectAll("path")
        .data(chords)
        .enter()
        .append("path")
        .attr("d", inner_chord )
        .style("fill", function(d) { return color(d.source.index); })
        .style("opacity", 1)
        .on("mouseover",function(d,i){
            d3.select(this)
              .style("fill","yellow");
        })
        .on("mouseout",function(d,i) {
            d3.select(this)
              .transition()
              .duration(1000)
              .style("fill",color(d.source.index));
        });

