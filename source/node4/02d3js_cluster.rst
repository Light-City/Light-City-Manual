.. figure:: http://p20tr36iw.bkt.clouddn.com/d3js_cluster_show.jpg
   :alt: 

第二节:d3.js布局之集群图与树图
===================================

-  注意：通过d3.json加载json数据文件时，必须通过一个http服务器来访问，不能本地！例如：127.0.0.1/d3\_example.html

1.集群图
--------

.. code:: javascript

    <!DOCTYPE html>
    <html>

    <head>
      <meta charset="utf-8">
      <title>集群图构建</title>
      <style>
      .link {
        fill: none;
        stroke: #555;
        stroke-opacity: 0.4;
        stroke-width: 1.5px;
      }
      circle {
        stroke-width: 1.5px;
        stroke: steelblue;
        fill: #fff;
      }
      </style>
    </head>

    <body>
      <script src="d3.min.js"></script>
      <script>
      // 定义svg宽度与高度
      var width = 800
      var height = 600
      //为body里面添加svg标签
        var svg = d3.select("body").append("svg")
                    .attr("width", width)
                    .attr("height", height)
            //在svg中添加g标签
                  .append("g")
                    .attr("transform", "translate(80,0)");

      // 加载json文件
      // v5使用.then语法(promise)来构建
      d3.json("city1.json").then( function(data){
          root = d3.hierarchy(data);

          var cluster = d3.cluster()
            .size([height, width - 300])
            //  .nodeSize([50,300]) ;
            //  .separation(function(a, b) { return(a.parent == b.parent ? 1 : 2); });
        //打印cluster后的数据
          console.log(cluster(root))
          cluster(root)
        //通过path绘制节点之间的连接
          var link = svg.selectAll(".link")
                      //加载除了第一个结点之后的所有子孙的连接
                      .data(root.descendants().slice(1))
                        .enter()
                        .append("path")
                        .attr("class", "link")
                        .attr("d", function(d) {
                          return "M" + d.y + "," + d.x +
                            "C" + (d.parent.y + 100) + "," + d.x +
                            " " + (d.parent.y + 100) + "," + d.parent.x +
                            " " + d.parent.y + "," + d.parent.x;
                        });
        //在svg中添加g标签，用于存放节点与文字
          var node = svg.selectAll(".node")
                        .data(root.descendants())
                        .enter()
                        .append("g")
                        //簇的深度方向(也就是水平方向)是y，排列方向是x(也就是靠近页面顶部方向)
                        .attr("transform", function(d) { return "translate(" + d.y + "," + d.x + ")"; })
        //绘制节点
          node.append("circle")
                .attr("r", 8)

        //添加文字
          node.append("text")
            //设置文字偏移量
                .attr("dy", 5)
            //设置文字偏移量，根据是否有孩子来判断是向左还是向右
                .attr("x", function(d) { return d.children ? -12 : 12; })
                .style("text-anchor", function(d) { return d.children ? "end" : "start"; })
                .attr("font-size", "150%")
                .text(function(d) { return d.data.name; });
      })

      </script>
    </body>

    </html>

2.树图
------

.. figure:: http://p20tr36iw.bkt.clouddn.com/d3js_tree.jpg
   :alt: 

.. code:: javascript

    //修改代码如下，其余不变
    var tree = d3.tree()
                 .size([height, width - 300])
    console.log(tree(root))
    tree(root)

