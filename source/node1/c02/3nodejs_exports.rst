.. figure:: http://p20tr36iw.bkt.clouddn.com/nodejs_exports.jpg
   :alt: 

exportst与module.exports
========================

1.exports和module.exports指向同一对象，
------------------------------------------------------------------------------------------------------------------
两者随便添加属性/方法，都会接收到变化，因为都指向同一个对象。故--->结果相同

.. code:: javascript

    exports.tfunc = function(){}
    module.exports.name = 'zha'
    console.log(exports)  // { tfunc: [Function], name: 'zha' }
    console.log(module.exports) // { tfunc: [Function], name: 'zha' }

2.exports直接赋值
------------------------------------------------------------------------------------------------------------------------------
此时切断了exports同module.exports一同指向对象的引用，真正输出的是module.exports指向的对象，而exports赋值无效

.. code:: javascript

    exports.name = 'wan'
    var tfunc = function(){}
    exports = tfunc
    console.log(exports) // [Function: tfunc]
    console.log(module.exports) // { tfunc: [Function], name: 'wan' }

3.修正上述两者输出
-----------------------------------------------------------------------
注意下面代码不能同时运行，两部分切换调整输出，看结果

.. code:: javascript

    /*-----------------test1-----------------*/
    exports = module.exports
    console.log(exports) // { tfunc: [Function], name: 'wan' }

    // 那如果想让module.exports同exports输出一样，怎么调整呢？
    /*-----------------test2-----------------*/
    // module.exports =  exports
    // console.log(module.exports)  // [Function: tfunc]

4.module.exports直接赋值
----------------------------------------------------------------------------------------------
同2一样切断了exports的引用。上述的module.exports值会被现在的对象覆盖

.. code:: javascript

    /*
    1.上述为test1时
        [Function: tfunc]
        11
    2.上述为test2时
        { tfunc: [Function], name: 'wan' }
        11
    */
    module.exports = "li"
    console.log(exports)
    console.log(module.exports)

5.更直观的module.exports覆盖问题
--------------------------------

.. code:: javascript

    add=function(arg1,arg2){
         return arg1+arg2;
    };
    minus=function(arg1,arg2){
         return arg1-arg2;
    };
    module.exports=add;
    module.exports=minus;
    console.log(module.exports); // [Function: minus] 最后一个会将前面的全部覆盖掉

6.总结
------

.. code:: javascript

    总结：
        真正输出总是 module.exports。
        如果两者同时出现或被修改，
        只有 module.exports 返回，exports 被忽略。
