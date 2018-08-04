.. figure:: http://p20tr36iw.bkt.clouddn.com/cytoscape.png
   :alt: 

第四节:Cytoscape.js可视化Neo4J
==================================

说在前面：本次可视化Neo4J实现在Flask与Django两种框架上。关于Neo4J数据如何构建，点击\ `Python操作知识图谱数据库 <http://light-city.me/post/3c5353.html>`__\ 。记得安装\ ``py2neo，pip install py2neo==2.0.8``

1.Flask实现
-----------

index.html

.. code:: html

    <!DOCTYPE html>
    <html>
    <head>
        <title>KGFHBP</title>
        <style type="text/css">
            /* cytoscape graph */
            #cy {
                height: 100%;
                width: 100%;
                position: absolute;
            }
        </style>
        <script src="http://cdn.bootcss.com/jquery/1.11.2/jquery.min.js"></script>
        <script src="http://cdn.bootcss.com/cytoscape/2.3.16/cytoscape.min.js"></script>
        <script>
            $.get('/kg/graph/json', function(result){
                var style = [
                    {selector: 'node[label = "临床表现"]', css: {'background-color': '#6FB1FC','content': 'data(name)'}},
                    {selector: 'node[label = "药物"]', css: {'background-color': '#F5A45D','content': 'data(name)'}},
                    {selector: 'node[label = "诊断方法"]', css: {'background-color': '#bd78f5','content': 'data(name)'}},
                    {selector: 'node[label = "副作用"]', css: {'background-color': '#c0f54a','content': 'data(name)'}},
                    {selector: 'node[label = "疾病"]', css: {'background-color': '#f5739c','content': 'data(name)'}},
                    { selector: 'edge', css: {'content': 'data(relationship)', 'target-arrow-shape': 'triangle'}}
                ];
                cytoscape({
                  container: document.getElementById('cy'),
                  style: style,
                  elements: result.elements,
                  layout: { name: 'grid'}
                });
            }, 'json');
        </script>
    </head>
    <body>
        <div id="cy"></div>

    </body>
    </html>

app.py

.. code:: python


    # coding=utf-8
    from flask import Flask, jsonify, render_template
    from py2neo import Graph, authenticate

    app = Flask(__name__)
    # set up authentication parameters
    authenticate("localhost:7474", "neo4j", "XXXX")

    # connect to authenticated graph database
    graph = Graph("http://localhost:7474/db/data/")

    def buildNodes(nodeRecord):
        data = {"id": str(nodeRecord.n._id), "label": next(iter(nodeRecord.n.labels))}
        data.update(nodeRecord.n.properties)

        return {"data": data}

    def buildEdges(relationRecord):
        data = {"source": str(relationRecord.r.start_node._id),
                "target": str(relationRecord.r.end_node._id),
                "relationship": relationRecord.r.rel.type}

        return {"data": data}

    @app.route('/kg/graph')
    def index():
        return render_template('index.html')

    @app.route('/kg/graph/json')
    def get_graph():
        nodes = list(map(buildNodes, graph.cypher.execute('MATCH (n) RETURN n')))
        edges = list(map(buildEdges, graph.cypher.execute('MATCH ()-[r]->() RETURN r')))
        # 保证中文不乱码
        app.config['JSON_AS_ASCII'] = False
        return jsonify(elements = {"nodes": nodes, "edges": edges})

    if __name__ == '__main__':
        app.run(debug = True)

2.Django实现
------------

views.py

.. code:: python

    # set up authentication parameters
    authenticate("localhost:7474", "neo4j", "XXXX")
    # connect to authenticated graph database
    graph = Graph("http://localhost:7474/db/data/")

    def cyNeo4j_post(request):
        return render(request, "cytoscape_neo4j.html")


    def buildNodes(nodeRecord):
        data = {"id": str(nodeRecord.n._id), "label": next(iter(nodeRecord.n.labels))}
        data.update(nodeRecord.n.properties)
        return {"data": data}


    def buildEdges(relationRecord):
        data = {"source": str(relationRecord.r.start_node._id),
                "target": str(relationRecord.r.end_node._id),
                "relationship": relationRecord.r.rel.type}
        return {"data": data}


    def ghJson_post(request):
        nodes = list(map(buildNodes, graph.cypher.execute('MATCH (n) RETURN n')))
        edges = list(map(buildEdges, graph.cypher.execute('MATCH ()-[r]->() RETURN r')))
        elements = {"nodes": nodes, "edges": edges}
        return HttpResponse(json.dumps(elements, ensure_ascii=False), content_type="application/json")
        #return JsonResponse(elements,safe=False)

urls.py

.. code:: python

    url(r'^kg/graph$', views.cyNeo4j_post),
    url(r'^kg/graph/json$', views.ghJson_post),

3.项目地址
----------

`flask实现 <https://github.com/Light-City/flask_cytoscape>`__\ \|\ `Django实现 <https://github.com/Light-City/KGFBHP>`__\ ，欢迎Start!

4.学习文章
----------

`1.用cytoscape.js展示neo4j网络关系图 - 1.
Flask <https://blog.csdn.net/zhongzhu2002/article/details/45843283>`__

`2.用cytoscape.js展示neo4j网络关系图 - 2.
py2neo <https://blog.csdn.net/zhongzhu2002/article/details/46043047>`__

`3.用cytoscape.js展示neo4j网络关系图 - 3.
cytoscape.js <https://blog.csdn.net/zhongzhu2002/article/details/46049197>`__

`4.Cytoscape.js官网 <http://js.cytoscape.org/?utm_source=javascriptweekly&utm_medium=email>`__

`5.ER\_PARSE\_ERROR: You have an error in your SQL syntax check the
manual that corresponds to your
MySQL <https://blog.csdn.net/baidu_34036884/article/details/78996573>`__

`6.django
返回json格式数据 <https://blog.csdn.net/lanyang123456/article/details/75261105>`__
