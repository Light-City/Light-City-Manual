|image0|

NodeJs模块
==========

1.Url模块
---------

    查看Url构成

.. code:: javascript

    url.parse('http://www.baidu.com:7/assa/s=12?from=123&a=node#12')

.. code:: javascript

    // output
    Url {
      protocol: 'http:',
      slashes: true,
      auth: null,
      host: 'www.baidu.com:7',
      port: '7',
      hostname: 'www.baidu.com',
      hash: '#12',
      search: '?from=123&a=node',
      query: 'from=123&a=node',
      pathname: '/assa/s=12',
      path: '/assa/s=12?from=123&a=node',
      href: 'http://www.baidu.com:7/assa/s=12?from=123&a=node#12' }

    生成一个url

.. code:: javascript

    url.format({   protocol: 'http:',   slashes: true,   auth: null,   host: 'www.baidu.com:7',   port: '7',   hostname: 'www.baidu.com',   hash: '#12',   search: '?from=123&a=node',   query: 'from=123&a=node',
    pathname: '/assa/s=12',   path: '/assa/s=12?from=123&a=node',   href: 'http://www.baidu.com:7/assa/s=12?from=123&a=node#12' })

.. code:: javascript

    'http://www.baidu.com:7/assa/s=12?from=123&a=node#12'

    为URL或 href 插入 或 替换原有的标签

.. code:: javascript

    > url.resolve('/one/two/three', 'four')
    '/one/two/four'
    > url.resolve('/one/two/three', '/four')
    '/four'
    > url.resolve('http://www.as.com/one/two','/four')
    'http://www.as.com/four'
    > url.resolve('http://www.as.com/one/two','four')
    'http://www.as.com/one/four'

    \`url.parse(urlString[, parseQueryString[, slashesDenoteHost]])\`\`

.. code:: javascript

    //对于想把query也即查询条件变为字典格式的，则需要设定第二个参数为true
    > url.parse('http://www.baidu.com:7/assa/s=12?from=123&a=node#12')
    Url {
      protocol: 'http:',
      slashes: true,
      auth: null,
      host: 'www.baidu.com:7',
      port: '7',
      hostname: 'www.baidu.com',
      hash: '#12',
      search: '?from=123&a=node',
      query: 'from=123&a=node',
      pathname: '/assa/s=12',
      path: '/assa/s=12?from=123&a=node',
      href: 'http://www.baidu.com:7/assa/s=12?from=123&a=node#12' }
    > url.parse('http://www.baidu.com:7/assa/s=12?from=123&a=node#12',true)
    Url {
      protocol: 'http:',
      slashes: true,
      auth: null,
      host: 'www.baidu.com:7',
      port: '7',
      hostname: 'www.baidu.com',
      hash: '#12',
      search: '?from=123&a=node',
      query: { from: '123', a: 'node' },
      pathname: '/assa/s=12',
      path: '/assa/s=12?from=123&a=node',
      href: 'http://www.baidu.com:7/assa/s=12?from=123&a=node#12' }


    //对于不清楚http还是https等协议的url，想要解析出host/hostname等就需要追加最后一个参数为true
    > url.parse('//imca/learn/node',true)
    Url {
      protocol: null,
      slashes: null,
      auth: null,
      host: null,
      port: null,
      hostname: null,
      hash: null,
      search: '',
      query: {},
      pathname: '//imca/learn/node',
      path: '//imca/learn/node',
      href: '//imca/learn/node' }
    > url.parse('//imca/learn/node',true,true)
    Url {
      protocol: null,
      slashes: true,
      auth: null,
      host: 'imca',
      port: null,
      hostname: 'imca',
      hash: null,
      search: '',
      query: {},
      pathname: '/learn/node',
      path: '/learn/node',
      href: '//imca/learn/node' }

2.querystring模块
-----------------

    querystring序列化

.. code:: javascript

    第一个参数：字典
    > querystring.stringify({name:'gx',course:['a','b'],from:''})
    'name=gx&course=a&course=b&from='

    第二个参数：连接符
    > querystring.stringify({name:'gx',course:['a','b'],from:''},',')
    'name=gx,course=a,course=b,from='
    第三个参数：将默认的=号替换成相应的字符
    > querystring.stringify({name:'gx',course:['a','b'],from:''},',',':')
    'name:gx,course:a,course:b,from:'
    最后一个参数options，options（可省）传入一个对象，该对象可设置encodeURIComponent这个属性：encodeURIComponent:值的类型为function，可以将一个不安全的url字符串转换成百分比的形式，默认值为querystring.escape()。

    querystring反序列化

.. code:: javascript

    querystring.stringify(obj,separator,eq,options)
    > querystring.parse('name=gx&course=a&course=b&from=')
    { name: 'gx', course: [ 'a', 'b' ], from: '' }
    > querystring.parse('name=gx,course=a,course=b,from=',',')
    { name: 'gx', course: [ 'a', 'b' ], from: '' }
    > querystring.parse('name:gx,course:a,course:b,from:',',',':')
    { name: 'gx', course: [ 'a', 'b' ], from: '' }
    最后一个参数options，可设置最多key,以字典形式传入
    >querystring.parse('name:gx,course:a,course:b,from:',',',':',{maxKeys:2})
    { name: 'gx', course: 'a' }

    escape对字符串进行编码

.. code:: javascript

    > querystring.escape('哈哈')
    '%E5%93%88%E5%93%88'
    > querystring.unescape('%E5%93%88%E5%93%88')
    '哈哈'

3.补充
------

.. code:: javascript

    http协议
    http客户端发起请求，创建端口
    http服务器在端口监听客户端请求
    http服务器向客户端返回状态和内容

4.cheerio(爬虫模块)
-------------------

    以慕课网视频教程列表为例

.. code:: javascript

    var http = require('http')
    var cheerio = require('cheerio')

    var url = 'http://www.imooc.com/learn/348'

    function filterChapters(html) {
        var $ = cheerio.load(html)
        var chapters = $('.chapter')
        var courseData = []
      /*
      爬出数据格式为
      courseData =[
        chapterTitle: chapterTitle,
        videos: [
              {
                title:videoTitle,
                id:id
              }
          ]
        ]
      */
        chapters.each(function(item) {
            var chapter = $(this)
            var chapterTitle = chapter.find('h3').text()
            var videos = chapter.find('.video').children('li')
            var chapterData = {
                chapterTitle: chapterTitle,
                videos: []
            }
            videos.each(function(item) {
                var video = $(this).find('.J-media-item')
                var videoTitle = video.text()
                var id =video.attr('href').split('video/')[1]

                chapterData.videos.push({
                    title: videoTitle,
                    id: id
                })
            })
            courseData.push(chapterData)
        })
        return courseData
    }
    function printCoutseInfo(courseData) {
        courseData.forEach(function(item) {
            var chapterTitle = item.chapterTitle
            console.log(chapterTitle+ '\n')
            item.videos.forEach(function(video) {
                console.log(' [' + video.id + ']' + video.title + '\n' )
            })
        })

    }
    http.get(url, function(res) {
        var html = ''
        res.on('data',function(data) {
            html += data
        })
        res.on('end',function() {
            var courseData = filterChapters(html)
            printCoutseInfo(courseData)
        })
    }).on('error',function() {
        console.log('获取课程数据出错!')
    })

.. figure:: http://p20tr36iw.bkt.clouddn.com/node_scrapwer.jpg
   :alt: 

5.\ ``events``\ 模块
--------------------

.. code:: javascript

    var EventEmitter = require('events').EventEmitter
    var life = new EventEmitter()
    life.setMaxListeners(11)
    life.on('求安慰', function(who) {
        console.log('给' + who + '揉脚')
    })
    life.on('求安慰', function(who) {
        console.log('给' + who + '洗衣')
    })
    life.on('求安慰', function(who) {
        console.log('给' + who + '做饭')
    })
    life.on('求安慰', function(who) {
        console.log('给' + who + '5')
    })
    life.on('求安慰', function(who) {
        console.log('给' + who + '6')
    })
    life.on('求安慰', function(who) {
        console.log('给' + who + '7')
    })
    life.on('求安慰', function(who) {
        console.log('给' + who + '8')
    })
    life.on('求安慰', function(who) {
        console.log('给' + who + '9')
    })
    life.on('求安慰', function(who) {
        console.log('给' + who + '10')
    })
    life.on('求安慰', function(who) {
        console.log('给' + who + '你想累死我啊。。。')
    })
    life.on('求溺爱', function(who) {
        console.log('给' + who + '交工资')
    })
    life.on('求溺爱', function(who) {
        console.log('给' + who + '买衣服')
    })
    /*---删除监听事件开始---*/
    function water(who) {
        console.log('给' + who + '倒水')
    }

    life.on('求安慰', water)
    //单个移除
    life.removeListener('求安慰', water)
    //下面方法不传参，会移除所有事件监听，传参，只会移除某一类的事件函数
    life.removeAllListeners('求安慰')
    /*---删除监听事件结束---*/
    // var hasConforListener = life.emit('求安慰','汉子')
    // var hasLoveListener = life.emit('求溺爱','妹子')
    // var hasPlayListener = life.emit('求玩坏','汉子')
    // console.log(hasConforListener)
    // console.log(hasLoveListener)
    // console.log(hasPlayListener)
    //打印监听事件个数

    console.log(life.listeners('求安慰').length)
    console.log(EventEmitter.listenerCount(life,'求安慰'))

.. |image0| image:: http://p20tr36iw.bkt.clouddn.com/node_module.jpg

