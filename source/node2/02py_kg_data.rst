|image0|

第二节:RDB、RDF及本体映射
===========================

1.通过PowerDesigner构建关系数据库(RDB)模型。
2.通过RDB模型构建RDB数据库，这里可以选择SQL/MySQL等。
3.通过Python爬虫技术抓取数据，存储至上数据库。
4.现在有了数据了，由于我们构建的是知识图谱，而知识图谱的数据操作不像关系数据那样，KG以三元组为特性，如果采用关系数据库操作，则需要构建大量的连接操作，耗时耗力，故我们得设法得到三元组数据存储样式，那么这里就涉及了RDB转化RDF数据操作，而这一操作则需要通过D2RQ来进行。
5.D2RQ(Accessing Relational Databases as Virtual RDF
Graphs)简单介绍：D2RQ最主要的一个特性是以虚拟RDF图的方式访问关系数据库。它的机理就是通过mapping文件，把对RDF的查询等操作翻译成SQL语句，最终在RDB上实现对应操作。在做知识图谱项目的时候，我们可以灵活地选择数据访问方式。当对外提供服务，查询操作比较频繁的情况下，最好是将RDB的数据直接转为RDF，会节省很多SPARQL到SQL的转换时间。D2RQ有一个比较方便的地方，可以根据你的数据库自动生成预定义的mapping文件，用户可以在这个文件上修改，把数据映射到自己的本体上。通过mapping文件映射本体！
6.通过dump-rdf.bat将RDB转为RDF

.. code:: python

    例如：.\dump-rdf.bat -o rdf.nt .\kg_demo_movie_mapping.ttl
    kg_demo_movie_mapping.ttl是我们修改后的mapping文件。其支持导出的RDF格式有“TURTLE”, “RDF/XML”, “RDF/XML-ABBREV”, “N3”, 和“N-TRIPLE”。“N-TRIPLE”是默认的输出格式。

7.D2RQ，是以虚拟RDF的方式来访问关系数据库中的数据，即我们不需要显式地把数据转为RDF形式。通过默认，或者自己定义的mapping文件，我们可以用查询RDF数据的方式来查询关系数据库中的数据。换个说法，D2RQ把SPARQL查询，按照mapping文件，翻译成SQL语句完成最终的查询，然后把结果返回给用户。通过上述操作我们得到了RDF数据，并通过D2RQ以虚拟RDF图的方式访问关系数据库。那如何做到非虚拟操作，可以将RDF简单地固化到.nt文件，甚至可以直接存储到.txt文件。但若对于RDF有CRUD操作，那么简单地使用文档，是事倍功半的。为了方便用户管理本体或知识图谱，将RDF固化到一个通用的数据库中，才是最优做法。将RDF固化，有多种方法，例如基于图数据库的：AllegroGraph1、Neo4j2；或者基于存储固定结构的数据库：关系型数据库、NoSQL数据库；或者基于索引的ES、基于缓存的Redis等。这里选择的是Apache
Jena TDB。 8.D2RQ与Apache Jena所提供的endpoind模式比较：

.. code:: python

    D2RQ是以虚拟RDF图的方式来访问关系数据库，在访问频率不高，数据变动频繁的场景下，这种方式比较合适。对于访问频率比较高的场景（比如KBQA），将数据转为RDF再提供服务更为合适。利用Apache Jena，创建基于显式RDF数据的SPARQL endpoint；并展示，在加入推理机后，对数据进行本体推理我们可以得到额外的信息。
    最后采用Apache Jena Fuseki组件。

9.Apache Jena 提供TDB、rule
reasoner和Fuseki组件：上述第7点TDB用于固化数据，第8点Fuseki用于提供endpoint与Sparql查询。
10.固化数据操作：

.. code:: python

    将上述第6步得到.nt(RDF文件)进行固化，以下命令解释，利用tdbloader工具直接将rdf数据导入到tdb存储库中，这样在对rdf操作时候直接访问，而不是以虚拟方式访问，速度自然快。
    例如：tdbloader.bat --loc d:\tdb d:\rdf.nt 
    运行完后，数据变成如下图的文件存储。

11.学习文章

`1.【Jena使用手册】Apache
Jena导入N-Triple等RDF文件 <https://blog.csdn.net/rk2900/article/details/38342181>`__

`2.使用Jena-TDB存储RDF本体、知识图谱文件 <https://blog.csdn.net/svenhuayuncheng/article/details/78751300>`__

`3.知识图谱-给AI装个大脑 <https://zhuanlan.zhihu.com/knowledgegraph>`__

.. |image0| image:: http://p20tr36iw.bkt.clouddn.com/kgmain.jpg

