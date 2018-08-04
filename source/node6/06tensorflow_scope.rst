.. figure:: http://p20tr36iw.bkt.clouddn.com/tensor_scope.jpg
   :alt: 

第六节:Tensorflow之Scope命名
================================

1.tf.name\_scope()
-----------------------

    两种途径生成变量 variable:

一种是 tf.get\_variable()

另一种是 tf.Variable()

.. code:: python

    import tensorflow as tf
    with tf.name_scope('a_name_scope') as scope:
        initializer=tf.constant_initializer(value=1) # 也可以简写为tf.Constant()
        var1=tf.get_variable(name='var1',shape=[1],dtype=tf.float32,initializer=initializer)
        var2=tf.Variable(name='var2',initial_value=[2],dtype=tf.float32)
        var21=tf.Variable(name='var2',initial_value=[2.1],dtype=tf.float32)
        var22=tf.Variable(name='var2',initial_value=[2.2],dtype=tf.float32)


    with tf.Session() as sess:
        sess.run(tf.global_variables_initializer())
        print(var1.name) # var1:0
        print(sess.run(var1)) # [1.]
        print(var2.name) # a_name_scope/var2:0
        print(sess.run(var2)) # [2.]
        print(var21.name) # a_name_scope/var2_1:0
        print(sess.run(var21)) # [2.1]
        print(var22.name) # a_name_scope/var2_2:0
        print(sess.run(var22)) # [2.2]

2.tf.variable\_scope()
----------------------

    重复利用变量

1.使用\ ``tf.variable_scope()``

2.使用之前声明\ ``tf.variable_scope().reuse_variables()``

3.采用\ ``tf.get_variable()``\ 生成变量

.. code:: python

    tf.Variable() 每次都会产生新的变量(参照两个例子),
    tf.get_variable() 如果遇到了同样名字的变量时,
    它会单纯的提取这个同样名字的变量(避免产生新变量)(参考本例).
    故要重复利用变量, 一定要在代码中强调 scope.reuse_variables(),
    否则系统将会报错。

.. code:: python

    # 修改部分为：增加了一个var1_reuse变量，并打印输出
    import tensorflow as tf
    with tf.variable_scope('a_variable_scope') as scope:
        initializer=tf.constant_initializer(value=1) # 也可以简写为tf.Constant()
        var1=tf.get_variable(name='var1_1',shape=[1],dtype=tf.float32,initializer=initializer)
        scope.reuse_variables()
        var1_reuse=tf.get_variable(name='var1_1')
        var2=tf.Variable(name='var2',initial_value=[2],dtype=tf.float32)
        var21=tf.Variable(name='var2',initial_value=[2.1],dtype=tf.float32)
        var22=tf.Variable(name='var2',initial_value=[2.2],dtype=tf.float32)


    with tf.Session() as sess:
        sess.run(tf.global_variables_initializer())
        print(var1.name) # a_variable_scope/var1_1:0
        print(sess.run(var1)) # [1.]
        print(var1_reuse.name)  # a_variable_scope/var1_1:0
        print(sess.run(var1_reuse))  # [1.]
        print(var2.name) # a_name_scope/var2:0
        print(sess.run(var2)) # [2.]
        print(var21.name) # a_name_scope/var2_1:0
        print(sess.run(var21)) # [2.1]
        print(var22.name) # a_name_scope/var2_2:0
        print(sess.run(var22)) # [2.2]
