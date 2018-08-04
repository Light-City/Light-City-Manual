.. figure:: http://p20tr36iw.bkt.clouddn.com/python.jpg
   :alt: 

Python基础
==========

一、Day1
--------

0.print
~~~~~~~

.. code:: python

    name = input("What is your name?")
    print("Hello "+name )
    # 或者print("Hello",name ),print中逗号分隔直接将字符串用空格分隔，若用+号连接，并且想留空格，则在前一字符串留空格即可

1.输入输出
~~~~~~~~~~

.. code:: python

    username=input("username:")
    password=input("password:")
    print(username,password)

2.格式输入输出
~~~~~~~~~~~~~~

.. code:: python

    # 第一种方法
    name=input("Name:")
    age=input("age:")
    job=input("job:")

    info='''---------info of ---------''' + '''
    Name:'''+name+'''
    Age:'''+age+'''
    Job:'''+job
    print(info)

    # 第二种方法

    name=input("Name:")
    age=int(input("age:"))  #如果不用int()就会报错(虽然输入为数字，但是print(type(age))为str型)，因为python如果不强制类型转化，就会默认字符型
    job=input("job:")

    info='''---------info of ---------
    Name:%s
    Age:%d
    Job:%s'''%(name,age,job)
    print(info)


    # 第三种方法

    name=input("Name:")
    age=int(input("age:"))  #如果不用int()就会报错(虽然输入为数字，但是print(type(age))为str型)，因为python如果不强制类型转化，就会默认字符型
    job=input("job:")

    info='''---------info of ---------
    Name:{_name}
    Age:{_age}
    Job:{_job}'''.format(_name=name,_age=age,_job=job)
    print(info)


    # 第四种方法

    name=input("Name:")
    age=int(input("age:"))  #如果不用int()就会报错(虽然输入为数字，但是print(type(age))为str型)，因为python如果不强制类型转化，就会默认字符型
    job=input("job:")

    info='''---------info of ---------
    Name:{0}
    Age:{1}
    Job:{2}'''.format(name,age,job)
    print(info)

3.输入密码不可见 getpass在pycharm中不好使
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: python

    import getpass
    pwd=getpass.getpass("请输入密码:")
    print(pwd)

4.验证，python缩进
~~~~~~~~~~~~~~~~~~

.. code:: python

    _username='Alex Li'
    _password='abc123'
    username=input("username:")
    password=input("password:")
    if _username==username and _password==password:
        print(("Welcome user {name} login...").format(name=username))
    else:
        print("Invalid username or password!")

5.指向---修改字符串
~~~~~~~~~~~~~~~~~~~

.. code:: python

    print("Hello World")
    name = "Alex Li"
    name2=name
    print(name)
    print("My name is", name,name2) # Alex Li Alex Li
    name = "PaoChe Ge"
    # name2=name指的是name2与name一样指向Alex Li的内存地址，name指向改了，但是name2不变
    print("My name is", name,name2) # PaoChe Ge Alex Li
    print("您好，我来了")

6.注释\ ``''' '''``\ 内涵
~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: python

    # 第一种情况就是注释
    '''print("这是一行注释")'''
    #第二种情况就是打印多行字符串
    str='''这是第一行内容
    这是第二行内容'''
    print(str)
    # 3.单套双，双套单都可以
    str1="i'am a student"
    print(str1)

7.模块初始sys与os
~~~~~~~~~~~~~~~~~

.. code:: python

    import sys
    # 打印环境变量
    print(sys.path)

    print(sys.argv)
    print(sys.argv[2])

    # 进度条
    import time
    for i in range(50):
        sys.stdout.write('#')
        sys.stdout.flush()
        time.sleep(0.5)

.. code:: python

    import os
    cmd_res = os.system("dir") # os.system()执行后直接输出到终端，然后结束，最后cmd_res保存的是os.system()执行后的状态码
    print("--->",cmd_res) # ---> 0

    cmd_res1=os.popen("dir")
    print("--->",cmd_res1) # 得到的是内存对象值 ---> <os._wrap_close object at 0x00000000029187B8>
    cmd_res1=os.popen("dir").read()
    print("--->",cmd_res1) # 读取数据必须再后面加个read()

    os.mkdir("new_dir3") # 创建一个目录
    os.removedirs("new_dir") # 删除一个目录

8.三元运算
~~~~~~~~~~

.. code:: python

    # 1.result = 值1 if 条件 else 值2
    d=a if a>b else c
    print(d)

9.python3特性
~~~~~~~~~~~~~

.. code:: python

    python3中最重要的新特性大概是对文本和二进制数据作了更为清晰的区分。文本总是Unicode，由str类型表示,
    二进制数据则由bytes类型表示。Python3不会以任意隐式的方式混用str和bytes，正是这使得两者区分特别清晰。
    即：在python2中类型会自动转化，而在python3中则要么报错，要么不转化
    str与bytes相互转化

10.bytes与str转化
~~~~~~~~~~~~~~~~~

.. code:: python

    msg="我爱北京天安门"

    print(msg)
    print(msg.encode(encoding="utf-8")) # str转bytes,编码
    print(msg.encode(encoding="utf-8").decode(encoding="utf-8")) # bytes转str,解码

11.循环
~~~~~~~

.. code:: python

    print("第一种循环")
    count = 0
    while True:
        print("count:",count)
        count+=1
        if(count==10):
            break
    print("第二种循环")
    count = 0
    for count in range(0,10,2):
        print("count:", count)

    for i in range(0,10):
        if i<5:
            print("loop ",i)
        else:
            continue
        print("hehe....")
    my_age=28
    count = 0
    while count<3:
        user_input=int(input("input your guess num:"))

        if user_input==my_age:
            print("Congratulations,you got it!")
            break
        elif user_input<my_age:
            print("Oops,think bigger!")
        else:
            print("think smaller!")
        count+=1
        print("猜这么多次都不对，你个笨蛋.")

12.练习---三级菜单
~~~~~~~~~~~~~~~~~~

.. code:: python

    data={
        '北京':{
            "昌平":{
                "沙河":["oldboys",'test'],
                "天通苑":["链家地产","我爱我家"]
            },
            "朝阳":{
                "望京":["oldboys",'默陌陌'],
                "国贸":["CICC","HP"],
                "东直门":["Advent","飞信"]
            },
            "海淀":{}
        },
        '山东':{
            "德州":{},
            "青岛":{},
            "济南":{}
        },
        '广东':{
            "德州":{},
            "青岛":{},
            "济南":{}
        },
    }
    exit_flag = False
    while not exit_flag:
        for i in data:
            print(i)
        choice=input("选择进入1>>:")
        if choice in data:
            while not exit_flag:
                for i2 in data[choice]:
                    print("\t",i2)
                choice2=input("选择进入2>>:")
                if choice2 in data[choice]:
                    while not exit_flag:
                        for i3 in data[choice][choice2]:
                            print("\t\t", i3)
                        choice3 = input("选择进入3>>:")
                        if choice3 in data[choice][choice2]:
                            for i4 in data[choice][choice2][choice3]:
                                print(i4)
                            choice4=input("最后一层，按b返回>>:")
                            if choice4=='b':
                                pass # pass可以理解为占位符，表示什么都不做，返回循环起始位置，以后可以在此处添加内容
                            elif choice4=='q':
                                exit_flag=True
                        if (choice3 == 'b'):
                            break
                        elif choice3 == 'q':
                            exit_flag = True
                if (choice2 == 'b'):
                    break
                elif choice2 == 'q':
                    exit_flag = True

        if (choice == 'b'):
            break

二、Day2
--------

1.unicode 与 utf-8/gbk变换
~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: python

    # utf-8与gbk互相转化需要通过Unicode作为中介
    s="我爱北京天安门"  # 默认编码为Unicode
    print(s.encode("gbk")) # Unicode可直接转化为gbk
    print(s.encode("utf-8")) # Unicode可直接转化为utf-8
    print(s.encode("utf-8").decode("utf-8").encode("gb2312")) # 此时s.encode("utf-8")即转为utf-8了，然后转为gb2312，则需要先告诉Unicode你原先的编码是什么，即s.encode("utf-8").decode("utf-8"),再对其进行编码为gb2312，即最终为s.encode("utf-8").decode("utf-8").encode("gb2312")

2.文件
~~~~~~

.. code:: python

    f=open('ly.txt','r',encoding='utf-8') # 文件句柄 'w'为创建文件，之前的数据就没了
    data=f.read()
    print(data)
    f.close()

    f=open('test','a',encoding='utf-8') # 文件句柄 'a'为追加文件 append
    f.write("\n阿斯达所，\n天安门上太阳升")
    f.close()

    f = open('ly.txt', 'r', encoding='utf-8')  # 文件句柄

    for i in range(5):
    print(f.readline().strip())  # strip()去掉空格和回车


    for line in f.readlines():
        print(line.strip())

    # 到第十行不打印

    for index,line in enumerate(f.readlines()):
        if index==9:
            print('----我是分隔符-----')
            continue
        print(line.strip())
    # 到第十行不打印
    count=0
    for line in f:

        if count==9:
            print('----我是分隔符-----')
            count += 1
            continue
        print(line.strip())
        count += 1
    f = open('ly.txt', 'r', encoding='utf-8')  # 文件句柄
    print(f.tell())
    print(f.readline(5))
    print(f.tell())
    f.seek(0)
    print(f.readline(5))
    print(f.encoding)
    print(f.buffer)
    print(f.fileno())
    print(f.flush()) # 刷新缓冲区
    # 进度条
    import sys,time
    for i in range(50):
        sys.stdout.write('#')
        sys.stdout.flush()
        time.sleep(0.5)
    f = open('ly.txt', 'a', encoding='utf-8')  # 文件句柄
    f.seek(10)
    f.truncate(20) # 指定10到20个字符，10个字符前面留着，后面20字符清除
    f = open('ly.txt', 'r+', encoding='utf-8')  # 文件句柄
    print(f.readline().strip())
    print(f.readline().strip())
    print(f.readline().strip())
    f.write("我爱中华")
    f.close()


    # 实现简单的shell sed替换功能

    f=open("ly.txt","r",encoding="utf-8")
    f_new=open("ly2.txt","w",encoding="utf-8")

    for line in f:
        if "肆意的快乐" in line:
            line=line.replace("肆意的快乐","肆意的happy")
        f_new.write(line)

    f.close()
    f_new.close()

    import sys
    f=open("ly.txt","r",encoding="utf-8")
    f_new=open("ly2.txt","w",encoding="utf-8")
    find_str = sys.argv[1]
    replace_str = sys.argv[2]
    for line in f:
        if find_str in line:
            line=line.replace(find_str,replace_str)
        f_new.write(line)
    f.close()
    f_new.close()

    # with语句---为了避免打开文件后忘记关闭，可以通过管理上下文
    with open('ly.txt','r',encoding='utf-8') as f:
        for line in f:
            print(line.strip())
    # python2.7后，with又支持同时对多个文件的上下文进行管理，即:
    with open('ly.txt','r',encoding='utf-8') as f1,open('ly2.txt','r',encoding='utf-8'):
        pass

3.全局变量
~~~~~~~~~~

.. code:: python


    names=["Alex","Jack","Rain"]

    # 除了整数和字符串在函数内不能改，列表，字典这些可以改
    def change_name():
        names[0]="金角大王"
        print("inside func",names
              )

    change_name()
    print(names)

    # 当全局变量与局部变量同名时，在定义局部变量的子程序内，局部变量起作用，在其它地方全局变量起作用。

4.list操作
~~~~~~~~~~

.. code:: python

    __author__="Alex Li"
    names="zhang Gu Xiang Xu"
    names=["zhang","Gu","Xiang","Xu"]
    # 1.切片
    print(names[0],names[1],names[2])
    print(names[1:3])  # 顾头不顾尾，切片
    print(names[-1]) # 在不知道多长情况下，取最后一个位置
    print(names[-1:-3]) # 切片是从左往右，此时不输出
    print(names[-3:-1]) # 顾头顾尾，去最后三个
    print(names[-2:])  # 取最后两个
    print(names[0:3]) # 切片 等价于 print(names[:3])

    # 2.追加
    names.append("Lei")
    print(names)
    # 3.指定位置插入
    names.insert(1,"Chen") # Gu前面插入
    print(names)
    # 4.修改
    names[2]="Xie"
    print(names)
    # 5.删除
    # 第一种删除方法
    names.remove("Chen")
    print(names)
    # 第二种删除方法
    del names[1]
    print(names)
    # 第三种删除方法
    names.pop() # 默认删除最后一个
    print(names)
    names.pop(1) #删除第二个元素
    print(names)
    print(names.index("Xu")) # 1
    print(names[names.index("Xu")]) #打印出找出的元素值3
    # 6.统计
    names.append("zhang") #再加一个用于学习统计"zhang"的个数
    print(names.count("zhang"))
    # 7.排序
    names.sort() #按照ASCII码排序
    print(names)
    names.reverse() # 逆序
    print(names)
    # 8.合并
    names2=[1,2,3,4]
    names.extend(names2)
    print(names,names2)
    # 9.删掉names2
    '''del names2'''
    print(names2) # NameError: name 'names2' is not defined,表示已删除
    # 10.浅copy
    names2=names.copy()
    print(names,names2) # 此时names2与names指向相同
    names[2]="大张"
    print(names,names2) # 此时names改变，names2不变
    # 11.浅copy在列表嵌套应用
    names=[1,2,3,4,["zhang","Gu"],5]
    print(names)
    names2=names.copy()
    names[3]="斯"
    names[4][0]="张改"
    print(names,names2) # copy为浅copy,第一层copy不变，后面的嵌套全部都变,修改names2与names都一样
    # 12.完整克隆
    import copy
    # 浅copy与深copy
    '''浅copy与深copy区别就是浅copy只copy一层，而深copy就是完全克隆'''
    names=[1,2,3,4,["zhang","Gu"],5]
    # names2=copy.copy(names) # 这个跟列表的浅copy一样
    names2=copy.deepcopy(names) #深copy
    names[3]="斯"
    names[4][0]="张改"
    print(names,names2)

    # 13.列表循环
    for i in names:
        print(i)
    print(names[0:-1:2]) # 步长为2进行切片
    # 0与-1都可以省略掉
    print(names[::2]) # 步长为2进行切片

    # 浅拷贝三种方式
    person=['name',['a',100]]
    p1=copy.copy(person)
    p2=person[:]  #其实p2=person[0:-1],0与-1均可以不写
    p3=list(person)
    print(p1,p2,p3)

5.Tuple操作
~~~~~~~~~~~

.. code:: python

    # 元组相当于只读列表,只有两个方法一个是count,一个是index

    names=('alex','jack','alex')

    print(names.count('alex'))
    print(names.index('jack'))
    # 购物篮程序

    product_list=[('Iphone', 5800),
                  ('Mac Pro', 9800),
                  ('Bike', 5800),
                  ('Watch', 10600),
                  ('Coffee', 31),
                  ('Alex Python', 120),]
    shopping_list=[]
    salary=input("Input your salary:")

    if salary.isdigit():
        salary=int(salary)
        while True:
            '''for item in product_list:
                print(product_list.index(item),item)
            '''
            for index,item in enumerate(product_list):
                print(index,item)
            user_choice=input("选择要买嘛？>>:")
            if user_choice.isdigit():
                user_choice=int(user_choice)
                if user_choice<len(product_list) and user_choice>=0:
                    p_item=product_list[user_choice]
                    if p_item[1]<=salary:
                        shopping_list.append(p_item)
                        salary-=p_item[1]
                        print("Added %s into shopping cart, your current balance is \033[31;1m%s\033[0m"%(p_item,salary))
                    else:
                        print("\033[41;1m你的余额只剩[%s]啦，还买个毛线\033[0m"%salary)
                else:
                    print("product code[%s] is not exist!"%user_choice)
            elif user_choice=='q':
                print('-----------shopping list----------------')
                for p in shopping_list:
                    print(p)
                    print("Your current balance:",salary)
                exit()
            else:
                print("invalid option")

6.Set操作
~~~~~~~~~

.. code:: python

    # 集合set  集合关系测试
    list_1=[1,4,5,7,3,6,7,9]
    list_1=set(list_1)
    print(list_1,type(list_1))
    list_2=set([2,6,0,6,22,8,4])
    print(list_2,type(list_2))
    print("--------------------------------")
    # 取交集
    print("方法一")
    print(list_1.intersection(list_2))
    print("方法二")
    print(list_1&list_2)
    print("--------------------------------")
    # 取并集
    print("方法一")
    print(list_1.union(list_2))
    print("方法二")
    print(list_1|list_2)
    print("--------------------------------")
    # 差集 in list_1 but not in list_2
    print(list_1.difference(list_2))
    print(list_1-list_2)
    print("--------------------------------")
    # 子集
    list_3=[1,4,6]
    list_4=[1,4,6,7]
    list_3=set(list_3)
    list_4=set(list_4)
    print(list_3.issubset(list_4))
    print(list_4.issuperset(list_3))
    print("--------------------------------")
    # 对称差集 把list_1与list_2互相都没有的元素放在一块，其实就是去掉重复元素
    print(list_1.symmetric_difference(list_2))
    print(list_1^list_2)
    print("--------------------------------")
    # 是否没有交集 Return True if two sets have a null intersection.
    list_5=set([1,2,3,4])
    list_6=set([5,6,7])
    print(list_5.isdisjoint(list_6))
    print("--------------------------------")
    # 基本操作
    # 添加一项
    list_1.add('x')
    print(list_1)
    # 添加多项
    list_1.update([10,37,42])
    print(list_1)
    # 删除一项
    list_1.remove(10)
    print(list_1)
    # set长度
    print(len(list_1))
    # 测试9是否是list_1的成员
    print(9 in list_1)
    # pop()删除并且返回一个任意的元素
    print(list_1.pop())
    # 删除一个指定的值
    list_1.discard('x')
    print(list_1)

7.字符串操作
~~~~~~~~~~~~

.. code:: python

    name="alex"
    print(name.capitalize()) # 首字母大写
    print(name.count("a")) # 统计字母个数
    print(name.count("a")) # 统计字母个数
    print(name.center(50,"-")) #总共打印50个字符，并把nam放在中间，不够的用-补上
    print(name.endswith("ex")) # 判断字符串以什么结尾
    name="alex \tname is alex"
    print(name.expandtabs(tabsize=30)) # 将name中\t转为30个空格
    print(name.find("x")) # 取索引
    print(name[name.find("x"):]) # 字符串切片
    name="my \tname is {name} and i am {year} old"
    print(name.format(name="alex",year=23))
    print(name.format_map({'name':'alex','year':23}))
    print('ab123'.isalnum()) #isalnum()包含所有字母及数字，如果不是这两个，则为False
    print('ab123'.isalpha()) # False  isalpha()包含纯英文字符
    print('1A'.isdecimal()) # 是否是十进制 False
    print('1A'.isdigit()) # 是否是整数 False
    print('_'.isidentifier()) #判断是否是合法的标识符，实质是否为合法变量名 True
    print('aasd'.islower()) # 判断是否是小写 True
    print(''.isspace()) # 是否是空格 False
    print('My name is'.istitle()) # 字符串首字母大写为title，否则不是
    print('+'.join(['1','2','3'])) # 对一列表中所有元素进行join操作
    print(name.ljust(50,'*')) # 左对齐字符串，多余位用*补全
    print(name.rjust(50,'-')) # 右对齐字符串，多余位用*-补全
    print('\n Alex'.lstrip()) # 去掉左边的空格/回车
    print('\nAlex\n'.rstrip()) # 去掉右边的空格/回车
    print('\nAlex\n'.strip()) # 去掉左边和右边的空格/回车
    print('Alex')

    p=str.maketrans("abcdef","123456")
    print("alex li".translate(p))  #把alex li换成上一行对应的值

    print("alex li".replace('l','L',1)) # 替换 1表示替换几个l,从左到右计算替换个数
    print("alex li".rfind('l')) # 找到的最右边的下标返回
    print("alex li".split('l')) # 默认将字符串按照空格分隔成列表，也可以在()中填写相应的分隔符，比如以字符l分隔，print("alex li".split(‘l’)),而且分隔符在列表中不会出现
    print("1+2+3+4".split('+')) # ['1', '2', '3', '4']
    print("1+2\n+3+4".splitlines()) # ['1+2', '+3+4']
    print("Alex Li".swapcase()) # aLEX lI
    print('lex li'.title()) # Lex Li
    print('lex li'.zfill(50)) #不够以0填充
    print('---')

8.字典
~~~~~~

.. code:: python

    # 字典无序
    info={
        'stu1101':"tengxun",
        'stu1102':"baidu",
        'stu1103':"ali",
    }

    print(info)
    # 0.查找
    # 方法一:确定存在
    print(info["stu1101"]) # 查找若不在，则报错
    # 方法二:不确定存在，安全查找方法
    print(info.get("stu11004")) # 查找不在不会报错，直接返回None，若有直接返回
    print('stu1103' in info) # True
    # 1.修改
    info["stu1101"]="腾讯"
    print(info)
    # 2.增加
    info["stu1104"]="zhubajie"
    print(info)
    # 3.删除
    # 方法一
    del info["stu1101"]
    print(info)
    # 方法二
    info.pop("stu1102")
    print(info)
    '''
    # 随机删除
    info.popitem()
    print(info)
    '''
    # 4.多级字典嵌套及操作
    av_catalog = {
        "欧美":{
            "www.youporn.com": ["很多免费的,世界最大的","质量一般"],
            "www.pornhub.com": ["很多免费的,也很大","质量比yourporn高点"],
            "letmedothistoyou.com": ["多是自拍,高质量图片很多","资源不多,更新慢"],
            "x-art.com":["质量很高,真的很高","全部收费,屌比请绕过"]
        },
        "日韩":{
            "tokyo-hot":["质量怎样不清楚,个人已经不喜欢日韩范了","听说是收费的"]
        },
        "大陆":{
            "1024":["全部免费,真好,好人一生平安","服务器在国外,慢"]
        }
    }
    b={
        'stu1101':"Alex",
        1:3,
        2:5
    }
    info.update(b) #将两个字典合并，存在key,则更新value，不存在key，则合并
    print(info)
    print(info.items()) #把一个字典转成列表
    c=info.fromkeys([6,7,8],"test")
    print(c)
    c=info.fromkeys([6,7,8],[1,{'name':'alex'},444])
    print(c)
    c[7][1]['name']='Jack Chen' # 3个key共用一个value,修改一个则所有的都修改了
    print(c)
    print("--------")
    av_catalog["大陆"]["1024"][1]="可以在国内做镜像" # 二级字典替换
    av_catalog.setdefault("taiwan",{"www.baidu.com":[1,2]}) # 如果不重名，即创建一个新的值，如果重名，将找到的值返回
    print(av_catalog)
    print(info.keys()) # 打印出所有的key
    print(info.values()) # 打印出所有的value

    print("---------------")
    for i in info:
        print(i,info[i])  #效率更高点
    print("---------------")
    for k,v in info.items():
        print(k,v)

9.函数
~~~~~~

.. code:: python

    # 1.无参函数
    # 定义一个函数
    def fun1():
        '''testing'''
        print('in the fun1')
        return 1
    # 定义一个过程 实质就是无返回值的函数
    def fun2():
        '''testing2'''
        print('in the fun2')

    x=fun1()
    y=fun2()
    print(x)
    print(y)  # 没有返回值得情况下，python隐式地返回一个None
    import time
    def logger():
        time_format='%Y-%m-%d %X %A %B %p %I'
        time_current=time.strftime(time_format)
        with open('a.txt','a+')as f:
            f.write('time %s end action\n'%time_current)
    def test1():
        print('in the test1')
        logger()


    def test2():
        print('in the test2')
        logger()
        return 0

    def test3():
        print('in the test3')
        logger()
        return 1,{5:"sda",6:"zad"},[1,2,3]

    x=test1()
    y=test2()
    z=test3()

    print(x) # None
    print(y) # 0
    print(z) # (1, {5: 'sda', 6: 'zad'}, [1, 2, 3])


    '''
    总结：
        返回值数=0:返回None
        返回值数=1:返回object
        返回值数>1:返回tuple
    '''
    # 2.有参函数
    # 默认参数特点：调用函数的时候，默认参数非必须传递
    # 用途：1.默认安装值

    def test(x,y):
        print(x)
        print(y)

    test(1,2)     # 位置参数调用 与形参意义对应
    test(y=1,x=2) # 关键字调用，与形参顺序无关
    test(3,y=2) # 如果既有关键字调用又有位置参数，前面一个一定是位置参数，一句话：关键参数一定不能写在位置参数前面
    '''
    比如加入一个参数z
    '''
    def test1(x,y,z):
        print(x)
        print(y)
        print(z)
    # 关键参数一定不能放在位置参数前面
    test1(3,4,z=6)
    test1(3,z=6,y=4)
    # 默认参数,
    def test(x,y,z=2):
        print(x)
        print(y)
        print(z)

    test(1,2)
    # 用*args传递多个参数，转换成元组的方式 *表示一个功能代号，表示接受的参数不固定，args可以随意起名
    def test(*args):
        print(args)
    test(1,3,4,5,5,6)
    test(*[1,3,4,5,5,6]) # args=tuple([1,2,3,4,5])
    def test(x,*args):
        print(x)
        print(args)
    test(1,2,3,4,5,6,7) # 1 (2,3,4,5,6,7)
    # 字典传值 **kwagrs:把N个关键字参数，转换成字典的方式
    def test(**kwargs):
        print(kwargs)
        print(kwargs['name'],kwargs['age'],kwargs['id'],kwargs['sex'])

    test(name='alex',age=8,id=10,sex='M') # {'name': 'alex', 'age': 8, 'id': 10, 'sex': 'M'}
    test(**{'name':'alex','age':8,'id':10,'sex':'M'})
    def test(name,**kwargs):
        print(name)
        print(kwargs)
    test('alex',age=18,sex='M') # 字典 {'age': 18, 'sex': 'M'}

    # 默认参数得放在参数组前面
    def test(name,age=18,**kwargs):
        print(name)
        print(age)
        print(kwargs)

    test('alex',sex='M',hobby='tesla',age=3)
    test('alex',3,sex='M',hobby='tesla')
    test('alex') # 后面的**kwargs不赋值输出为空字典
    def test(name,age=18,*args,**kwargs):
        print(name)
        print(age)
        print(args)
        print(kwargs)
    test('alex',age=34,sex='M',hobby='tesla') # alex 34 () {'sex': 'M', 'hobby': 'tesla'}

10.高阶函数
~~~~~~~~~~~

.. code:: python

    # 高阶函数 变量可以指向函数，函数的参数能接受变量，那么一个函数就可以接受另一个函数作为参数，这个函数就叫做高阶函数
    def f(x):
        return x
    def add(x,y,f):
        return f(x)+f(y)
    res=add(1,2,f)
    print(res)  # 3
