|image0|

Node.Js初识
===========

1.HelloWorld程序
----------------

.. code:: javascript

    const http = require('http');

    const hostname = '127.0.0.1';
    const port = 3000;

    // 调用http的createServer回调函数，用于处理请求与响应
    const server = http.createServer((req, res) => {
      res.statusCode = 200;
      res.setHeader('Content-Type', 'text/plain');
      res.end('Hello World\n');
    });

    //调用listen回调函数用于监听本机端口,最后浏览器便可以访问到，并呈现出Hello World
    server.listen(port, hostname, () => {
      console.log(`Server running at http://${hostname}:${port}/`);
    });

2.模块流程
----------

创建模块

.. code:: javascript

    teacher.js

导出模块

.. code:: javascript

    exports.add = function() {}

加载模块

.. code:: javascript

    var teacher = require('./teacher.js')

使用模块

.. code:: javascript

    teacher.add('xxx')

3.一个模块练习实例
------------------

.. code:: javascript

    //项目目录
    school
      -teacher.js
      -student.js
      -class.js
      -school.js
      -index.js

    teacher.js

.. code:: javascript

    function add(teacher) {
        console.log('Add teacher:' + teacher)
    }

    exports.add = add

    student.js

.. code:: javascript

    function add(student) {
        console.log('Add student:' + student)
    }

    exports.add = add

    class.js

.. code:: javascript

    var student = require('./student')
    var teacher = require('./teacher')



    function add(teacherName, students) {
        teacher.add(teacherName)
        students.forEach(function(item, index) {
            student.add(item)
        })
    }


    exports.add = add
    // module.exports.add = add

    school.js

.. code:: javascript

    //此处名不能为class，会报错
    var klass = require('./class')

    exports.add = function(classes) {
        console.log('-------本学校的班级信息如下-------')
        classes.forEach(function(item, index) {
            var teacherName = item[0]
            var students = item[1]
            console.log('class' + (index+1))
            klass.add(teacherName, students)
        })
    }

index.js

.. code:: javascript

    var school = require('./school')

    school.add([['美',['白富美','白美']],['富',['高富帅','帅气']]])

    output

.. figure:: http://p20tr36iw.bkt.clouddn.com/node_output.jpg
   :alt: 

4.回调函数学习
--------------

.. code:: javascript

    // 回调函数学习
    function learn(something) {
        console.log(something)
    }
    function we(callback,something) {
        something += ' is cool'
        callback(something)
    }

传递具名函数

.. code:: javascript

    we(learn, 'NodeJs')

传递匿名函数

.. code:: javascript

    we(function(something) {
        console.log(something)
    },'Python')

5.作用域、上下文
----------------

作用域

.. code:: javascript

    var globalVariable = 'This is global variable'

    function globalFunction() {
        var localVariable = 'This is local variable'

        console.log('Visit global/local variable')
        console.log(globalVariable)
        console.log(localVariable)

        globalVariable = 'This is changed variable'
        console.log(globalVariable)
        function localFunction() {
            var innerLocalVariable = 'This is inner local variable'
            console.log('Visit global/local/innerLocal variable')
            console.log(globalVariable)
            console.log(localVariable)
            console.log(innerLocalVariable)
        }
        localFunction()
    }
    globalFunction()

    上下文(this指代对象详解)

.. code:: javascript

    // learn1example
    // 通常把拥有者称为上下文(如下面的pet),this指向函数拥有者
    // var pet = {
    //  words: '...',
    //  speak: function() {
    //      console.log(this.words) // ...
    //      console.log(this === pet) // true
    //  }
    // }

    // pet.speak()

    // function pet(words) {
    //  this.words = words
    //  console.log(this.words) // ...
    //  console.log(this === global)// 指向顶层的global对象 true
    // }

    // pet('...')


    function Pet(words) {
        this.words = words
        this.speak = function() {
            console.log(this.words)
            console.log(this)
        }
    }
    var cat = new Pet('Miao')
    /*
    Miao
    Pet { words: 'Miao', speak: [Function] }
    此时的this所指向的对象是此处的cat
    */
    cat.speak()

    var pet = {
        words: '...',
        speak: function(say) {
            console.log(say + ' ' + this.words)
        }
    }

    var dog = {
        words: 'Wang'
    }
    // call方法让本来speak指向pet，现在指向了dog，让dog拥有了speak
    pet.speak.call(dog,'Speak') // Speak

.. code:: javascript

    // learn2example
    function Pet(words) {
        this.words = words
        this.speak = function() {
            console.log(this.words)
        }
    }
    function Dog(words) {
        Pet.call(this,words)
        // Pet.apply(this, arguments)
    }

    var dog = new Dog('Wang')
    dog.speak() // Wang

6.同步与异步
------------

同步模式

.. code:: javascript

    var i =0
    while(true) {
        i++
    }
    //或者如下
    var c = 0
    function printIt() {
        console.log(c)
    }
    function plus() {
        c += 1
    }
    plus()
    printIt()

异步模式

.. code:: javascript

    var c = 0
    function printIt() {
        console.log(c)
    }
    function plus(callback) {
        setTimeout(function() {
            c += 1
            callback()
        }, 1000)
    }
    plus(printIt)

7.事件
------

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

8.\ ``stream``\ 流
------------------

文件读写

.. code:: javascript

    var fs = require('fs')
    var source = fs.readFileSync('./image_base.jpg')
    fs.writeFileSync('stream_copy_logo.png',source)

读写流控制

.. code:: javascript

    var fs = require('fs')
    var readStream = fs.createReadStream('1.mp4')
    var writeStream = fs.createWriteStream('1-stream.mp4')
    readStream
        .on('data', function(chunk) {
            // 防爆仓
            if (writeStream.write(chunk) === false) {
                console.log('still cached')
                readStream.pause()
            }
        })
        .on('end', function() {
            writeStream.end()
        })
    writeStream.on('drain',function() {
            console.log('data drains')
            readStream.resume()
        })

``pipe()``\ 使用

.. code:: javascript

    /*--------示例1--------*/
    var http = require('http')
    var fs = require('fs')
    var request = require('request')
    http.
        createServer(function(req, res) {
            // fs.readFile('image_base.jpg', function(err, data) {
            //  if (err) {
            //      res.end('file not existe!')
            //  }
            //  else {
            //      res.writeHead(200, {'Context-Type': 'text/html'})
            //      res.end(data)
            //  }
            // })
            //fs.createReadStream('image_base.jpg').pipe(res)
            request('https://www.imooc.com/static/img/index/logo.png').pipe(res)
        })
        .listen(8090)
    /*--------示例2--------*/
      var fs = require('fs')
      fs.createReadStream('1.mp4').pipe(fs.createWriteStream('1-pipe.mp4'))
    /*--------示例3--------*/
      var fs = require('fs')
      var readStream = fs.createReadStream('stream_copy.js')

      var n =0

      readStream
        .on('data', function(chunk) {
            n++
            console.log('data emits')
            console.log(Buffer.isBuffer(chunk))
            //console.log(chunk.toString('utf8'))
            readStream.pause()
            console.log('data pause')
            setTimeout(function() {
                console.log('data pause end')
                readStream.resume()
            }, 3000)
        })
        .on('readable', function() {
            console.log('data readable')
        })
        .on('end', function() {
            console.log(n)
            console.log('data ends')
        })
        .on('close', function() {
            console.log('data close')
        })
        .on('error', function(e) {
            console.log('data read error' + e)
        })

``Readable``\ 与\ ``Writable``,把Readable流用管道输送到Writable流

.. code:: javascript

    var Readable = require('stream').Readable
    var Writable = require('stream').Writable


    var readStream = new Readable()
    var writeStream = new Writable()

    readStream.push('I')
    readStream.push('Love')
    readStream.push('Immoc\n')
    readStream.push(null)

    //writeStream._write是定义在writeStream下的一个私有方法，一般私有方法都已下划线开头的
    writeStream._write = function(chunk, encode, cb) {
        console.log(chunk.toString())
        cb()
    }
    readStream.pipe(writeStream)

``TransformStream``\ 与继承

.. code:: javascript

    /*
    util模块提供了util.inherits()方法来允许你
    创建一个继承另一个对象的prototype(原形)方法的对象。
    当创建一个新对象时，prototype方法自动被使用。
    */
    var stream = require('stream')
    var util = require('util')

    //ReadStream
    function ReadStream() {
        stream.Readable.call(this)
    }
    //继承
    util.inherits(ReadStream, stream.Readable)

    //重写
    ReadStream.prototype._read = function() {
        this.push('I')
        this.push('Love')
        this.push('Immoc\n')
        this.push(null)
    }

    //
    function WritStream() {
        stream.Writable.call(this)
        this._cached = new Buffer('')
    }
    util.inherits(WritStream, stream.Writable)
    WritStream.prototype._write = function(chunk, encode, cb) {
        console.log(chunk.toString())
        cb()
    }
    function TransformStream() {
        stream.Transform.call(this)
    }
    util.inherits(TransformStream, stream.Transform)
    TransformStream.prototype._transform = function(chunk, encode, cb) {
        this.push(chunk)
        cb()
    }
    TransformStream.prototype._flush = function(cb) {
        this.push('Oh Yeah!')
        cb()
    }
    var rs = new ReadStream()
    var ws = new WritStream()
    var ts = new TransformStream()
    //pipe()把Readable流用管道输送到Transform流再输送到Writable流
    rs.pipe(ts).pipe(ws)

.. |image0| image:: http://p20tr36iw.bkt.clouddn.com/node.jpg

