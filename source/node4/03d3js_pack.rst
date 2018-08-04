.. figure:: http://p20tr36iw.bkt.clouddn.com/d3js_pack.jpg
   :alt: 

第三节:d3.js布局之打包图
==========================

.. code:: javascript

    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Document</title>
    </head>
    <body>
        <script type="text/javascript" src="d3.min.js" charset="utf-8"></script>
        <script type="text/javascript">
            var width = 800
            var height = 800
            var svg = d3.select("body")
                        .append("svg")
                        .attr("width",width)
                        .attr("height",height)
                        .append("g")
                        .attr("transform", "translate(80,0)")
            var pack = d3.pack()
                        .size([width, height])
                        .padding(0)
            d3.json("test.json").then(function(data) {
                var root = d3.hierarchy(data);
              root.sum(function(d) { return d.value; });

          var pack = d3.pack()
                        .size([width, height])
                        .padding(0); //指定圆的切线之间的距离。默认值为0。
              console.log(pack(root))
              pack(root);

              // 3. svg要素の配置
          var node = d3.select("svg").selectAll(".node")
                        .data(root.descendants())
                        .enter()
                        .append("g")
                        .attr("transform", function(d) { return "translate(" + d.x + "," + (d.y) + ")"; });

          var color = d3.scaleOrdinal(d3.schemeAccent)
          node.append("circle")
                .attr("r", function(d) { return d.r; })
                .attr("stroke", "black")
                .attr("fill", function(d,i) { return color(i); });

          node.append("text")
                .style("text-anchor", function(d) { return d.children ? "end" : "middle"; })
                .attr("font-size", "150%")
                .text(function(d) { return d.children ? "" : d.data.name; });

        })
        </script>
    </body>
    </html>

