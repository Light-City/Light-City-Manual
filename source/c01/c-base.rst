复试C语言
=========

.. figure:: http://p20tr36iw.bkt.clouddn.com/c.jpg
   :alt: 

All Code
--------

.. code:: c

    1.代码规律
    /*
    i++与++i
    */

    #include<stdio.h>
    int main()
    {
        int i = 0;
        int a;

    //  i++;
    //  printf("%d\n",i);  //i=1---i=i;i=i+1;

    //  ++i;
    //  printf("%d\n", i);  //i=1---i=i+1;i=i;

    //  a = i++;
    //  printf("%d\n", a);   //a=0 ---a=i;i=i+1;

    //  a = ++i;
    //  printf("%d\n", a);  //a=1 ---i=i+1;a=i;

    //  a=i+i++;
    //  printf("%d\n", a);  //a=0 ---a=i+i;i=i+1;

    //  a = i + ++i;
    //  printf("%d\n", a);  //i=i+i->i=1->a=i+i;->a=2

    //  a = i + ++i + ++i;
    //  printf("%d\n", a);//i=i+1->i=1->i=i+1->i=2->a=i+i+i;a->6

    //  a = i + ++i + i++;
    //  printf("%d\n", a);//i++->最后进行加1,++i，此时加1，i=1->a=i+i+i;a=3;->i++中i操作，i=i+1

    //  i = i + ++i + i++;
    //  printf("%d\n",i);//i=4

    //  a = i + (i++) + (i++);
    //  printf("%d\n", a); //a=i+i+i;->i=i+1->i=1;->i=i+i->i=2;

    //  i = i + (i++) + (i++);
    //  printf("%d\n", i); //i=2

        return 0;

    }

    2.i++ 与 ++i 的主要区别有两个：
    >i++ 返回原来的值，++i 返回加1后的值。

    >i++ 不能作为左值，而++i 可以。

    *左值与右值的根本区别在于是否允许取地址&运算符获得对应的内存地址。*

    >左值是对应内存中有确定存储地址的对象的表达式的值，而右值是所有不是左值的表达式的值。
    int i=0;
    int *p1=&(++i); //True
    int *p2 = &(i++); //False

    ++i=1;  //True
    i++=5;  //False

.. code:: c

    >大小写转化

        #include<ctype.h>
        toupper()/tolower()
        例:str[i]=toupper(str[i]);
        str[i]=tolower(str[i]);

    >交换字符串

        1.使用传字符串地址

        strSwap(&str1,&str2);

        void strSwap(char **pa,char **pb)
        {
            char *temp;
            temp = *pa;
            *pa = *pb;
            *pb = temp;
        }

        2.使用strcpy函数

        strswap(str1,str2);

        void strswap(char *pa, char *pb)
        {
            char temp[100];
            strcpy(temp, pa);
            strcpy(pa, pb);
            strcpy(pb, temp);

        }

        3.自写字符串交换函数

        strswap(str1,str2);

        void strswap(char *pa,char *pb)
        {
            char temp[100];
            int i;
            for(i=0;pa[i]!='\0';i++)
                temp[i]=pa[i];
            temp[i]='\0';
            for(i=0;pb[i]!='\0';i++)
                pa[i]=pb[i];
            pa[i]='\0';
            for(i=0;temp[i]!='\0';i++)
                pb[i]=temp[i];
            pb[i]='\0';
        }



        4.同3自写函数

        strcp(char *p1, char *p2)
        {

            while((*p1=*p2)!='\0')
            {
                p1++;
                p2++;
            }
            p1='\0';
        }

        void strswap(char *pa,char *pb)
        {
            char temp[100];
            strcp(temp,pa);
            strcp(pa,pb);
            strcp(pb,temp);
        }

    >统计字符串中单词个数及对单词中第一个字符大写

        int upfst(char *p)
        {
            int cong = 0; //判断前一字符是否为空格标志
            int count = 0; //统计单词个数
            //for (;*p;p++)
            while(*p)
            {
                if (cong)   //前一字符不为空格
                {
                    if (*p == ' ')
                        cong = 0;
                }
                else    //前一字符是空格操作
                {
                    if (*p != ' ')
                    {
                        cong = 1;
                        *p = toupper(*p);  //#include<ctype.h>
                    //  *p = *p - 32;  //转大写
                        count++;
                    }
                }
                p++;
            }
            return count;
        }

    >6.把字符串分别转换成面值相同的整数


        long  d = 0;
        while (*s)
            if (isdigit(*s))
            {
                d = d * 10 + *s - '0';
                //或d = d * 10 + *s - 48;
                s++;
            }
    >7.要产生[m,n]范围内的随机数num，可用：

        int num=rand()%(n-m+1)+m;
    >8.链表逆置

        NODE *reverseList(NODE *h)
        {
            NODE *p, *q, *r;
            p = h;
            if (!p)
                return NULL;
            q = p->next;
            p->next = NULL;
            while (q)
            {
                r = q->next;
                q->next = p;
                p = q;
                q = r
            return p;
        }

    9.删除除了尾部之外的其余*号

        void  fun(char *a, char *p)
        {
            char *t = a;

            //先把字符串中非*字符存储，此时指针刚好指到最后一个字母的下一个字符(此时为*)
            for (; t <= p; t++)
                if (*t != '*')
                    *(a++) = *t;
            //若当前字符串未扫描完，继续将其余所有的*存储，就实现了删除尾部之外的其余*号
            for (; *t != '\0'; t++)
                *(a++) = *t;
            *a = '\0';

        }

    10.只删除前面*号

        char *p = a;
        while (*p == '*')
        p++;
        for (; *p != '\0'; p++, a++)
            *a = *p;
        *a = '\0';
        或者
        char *p = a;
        while (*p++ == '*')
            p++;
        strcpy(a,p)

.. code:: c

    11.打印操作

    for (i = 0,j=0; i < k; i++,j++)
    {
        printf("%d",a[i]);
        if (j < k - 1)
            printf("+");
    }

    12.完数，例如6=1+2+3，即所有因子之和

    int fun(int n, int a[], int *k)
    {
        int t = n;
        int i, j=0;
        for (i = 1; i < n; i++)
        {

            if (n%i == 0)
            {
                a[j++] = i;
                t -= i;
            }
        }
        *k = j;

        if (t == 0)
            return 1;
        else return 0;

    }


    13.闰年

    leap = (year % 4 == 0 && year % 100 != 0 || year % 400 == 0);

    14.统计输入的数是几位数

    int  fun(int  n)
    {
        int bits = 1;
        while (n / 10)
        {
            bits++;
            n /= 10;
        }

        return bits;
    }


    15.注意事项

    1.gets/gets_s函数只能传数组名，而不能传指针

    2.求int类型数组长度

    a[]={1,6,8,3,6,0,10};
    int len=sizeof(a)/sizeof(int);

    16.排序算法

    1.快速排序

    void sort(int *p, int low, int high)
    {
        if (low > high)
            return;

            int temp, i, j, t;
            temp = p[low];
            i = low;
            j = high;
            while (i < j)
            {
                while (i<j&&p[j]>=temp)
                    j--;
                while (i < j&&p[i] <= temp)
                    i++;
                if (i < j)
                {
                    t = p[i];
                    p[i] = p[j];
                    p[j] = t;
                }
            }
            p[low] = p[i];
            p[i] = temp;
            sort(p,low,i-1);
            sort(p,i+1,high);
    }


    2.冒泡排序
    void sort(int *p, int n)
    {
        int i, j;
        int t;
        for (i = 0; i < n - 1; i++)
        {
            for(j=0;j<n-i-1;j++)
            {
                if (p[j] > p[j + 1])
                {
                    t = p[j];
                    p[j] = p[j+1];
                    p[j + 1] = t;
                }
            }
        }
    }

    3.选择排序

    void sort(int *p, int n)
    {
        int i, j, k, temp;
        for (i = 0; i < n-1; i++)
        {
            k = i;
            for (j = i; j < n; j++)
            {
                if (p[k] > p[j])
                    k = j;
            }

            temp = p[i];
            p[i] = p[k];
            p[k] = temp;
        }
    }

    4.直接插入排序

    void sort(int *p, int n)
    {
        int i,j,k,t;
        for(i=1;i<n;i++)   //注意从第2个元素操作
        {
            if(p[i]<p[i-1])
            {
                t=p[i];
                for(j=i-1;j>=0&&p[j]>t;j--)
                {
                    p[j+1]=p[j];
                }
            }
            p[j+1]=t;
        }

    }

    换成字符操作

    void sort(char *p)
    {
        int i,j,n;
        char ch;
        n=strlen(p);
        for(i=i;i<n;i++)
        {
            ch=p[i];
            j=i-1;
            while((j>=0)&&(ch<p[j]))
            {
                p[j+1]=p[j];
                j--;
            }
            p[j+1]=ch;
        }
    }

    17.约瑟夫环

    #include<stdio.h>
    int main()
    {
        int out_person = 0;
        int n,m,k;
        printf("请输入人数:");
        scanf("%d",&n);
        printf("请输入逢几退一:");
        scanf("%d", &m);
        int i;
        int a[100];

        for (i = 0; i < n; i++)
            a[i] = i+1;
        i = 0;
        while (out_person < n - 1)
        {
            if (a[i] != 0)
            {
                k++;
            }
            if (k == m)

            {
                a[i] = 0;
                out_person++;
                k = 0;
            }
            i++;
            if (i == n)
                i = 0;
        }
        int *p = a;
        while (*p == 0)
            p++;
        printf("The last one is %d\n",*p);
        return 0;
    }

.. code:: c

    >四舍五入

    通过float类型转变为int类型即可实现
    float a;
    假设a=5.6;
    则四舍五入操作为:
    int b=(int)(a+0.5);

    >逗号表达式

    >逗号表达式的要领：

    >1.从左到右逐个计算；

    >2.逗号表达式作为一个整体，它的值为最后一个表达式的值；

    >3.逗号表达式的优先级别在所有运算符中最低。

    x=y=1;
    z=x++,y++,++y;
    printf("x=%d,y=%d,z=%d",x,y,z);
    由于赋值运算符优先级大于逗号表达式优先级，所以z=1;
    x=2;y=3
    若z=(x++,y++,++y)
    则z=3;
    x=2;y=3;
    若z=(x++,++y,y++)
    则z=2;
    x=2;y=3;

    同时也说明了一个问题对于z=(x++,y++,++y);中，y++执行完后，再去执行++y，即y++后，y变为2，++y变为3.而不是先y++，只按照赋值，后面+1未操作，这样是不对的。
    带括号记住逗号表达式中取最后一个为逗号表达式的值，不带括号，要注意赋值运算或者其他运算，自左向右进行。

    >void 类型指针

    可以指向任何类型的数据，也可以用任意数据类型的指针对void指针赋值。

    int *pint;
    void *pvoid;

    pvoid = pint;  //right;
    pint = pvoid;  //error;
    pint = (int *)pvoid;//right，强制转化可以

    >动态分配

    1.静态分配
    int a[50];
    int *p;
    p=a;
    2.动态分配
    int *p=(int *)malloc(sizeof(int));

    >质数分解

    #include<stdio.h>
    int main()
    {
        int i, j, val;
        printf("请输入一个正整数:");
        scanf("%d",&val);
        printf("将该正整数分解质因数输出为：");
        printf("%d=",val);
        for (i = 2; i <val; i++)
        {


            if (val%i == 0)
            {
                printf("%d*", i);
                val /= i;
            }

        }
        printf("%d",val);
        printf("\n");

        return 0;
    }

    >栈实现括号匹配

        #include<stdio.h>
        #include<string.h>
        #define MAX 999

        int judge(char p[])
        {
            int len = strlen(p);
            int top = -1;
            char s[MAX];
            int i;
            for (i = 0; i < len; i++)
            {
                switch(p[i])
                {
                    case '(':
                    case '[':
                    case '{':
                        s[++top] = p[i];
                        break;
                    case ')':
                        if (s[top] == '(')
                            top--;
                        else
                            return 0;
                        break;
                    case ']':
                        if (s[top] == '[')
                            top--;
                        else
                            return 0;
                        break;
                    case '}':
                        if (s[top] == '{')
                            top--;
                        else
                            return 0;
                        break;
                }

            }
            if (top == -1)
                return 1;
            else
                return 0;
    }

    >头插法

    node * createlist()
    {
        int n;
        node *head = (node *)malloc(sizeof(node));
        node *p = head;
        head->next = NULL;
        if (head == NULL)
        {
            printf("申请空间失败！\n");
            return NULL;
        }
        else
        {
            printf("请输入结点个数,n=");
            scanf("%d",&n);
            int i,num;
            for (i = 0; i < n; i++)
            {
                printf("请输入第%d个结点数值:\n",i+1);
                scanf("%d",&num);


                /*
                    头插法思路：先判断是否为head结点，若为head结点，则将新造的结点先赋值，然后链接在head后面，并将此时结点的next设为空，因为此时表示末尾结点，否则会出错！！！
                    此次结束后，将p指向q所指的结点，然后新造结点将q结点的next链接到p，在往前，最后用将p结点链接至head结点后面即可！

                */

                node *q = (node *)malloc(sizeof(node));
                if (p == head)
                {
                    head->next = q;
                    q->data = num;
                    q->next = NULL;
                }
                else
                {
                    q->data = num;
                    q->next = p;

                }
                p = q;

            }
            head->next = p;

        }

        return head;
    }


    >求文件字节数

    先fseek到末尾再用ftell得出当前位置，即为总文件字节数
    fseek(fp,0l,SEEK_END);
    int filesize=ftell(fp);

    >读取文件中的数据


    //读取文件中的数据
    #include<stdio.h>
    #include<malloc.h>
    int main()
    {
        FILE *fp;
        char *p;
        fp = fopen("d:\\file.txt","r");
        fseek(fp,0l,SEEK_END);
        long filesize = ftell(fp);
        rewind(fp);或者fseek(fp,0l,SEEk_SET);
        p = (char *)malloc(filesize+1);
        fread(p,1,filesize,fp);
        p[filesize] = 0;
        puts(p);
        free(p);
        fclose(fp);

        return 0;
    }


    >字符串比较

    int compare(char *s1, char *s2)
    {
        while (*s1&&*s2&&*s1 == *s2)
        {
            s1++;
            s2++;
        }
        if (*s1 == '\0'&&*s2 == '\0')
            return 0;
        else
            return *s1 - *s2;

    }


    >转二进制

    void stobinary(int *p)
    {

        int i,j;
        int k,result;
        int m;
        for (i=0,j=0;j<N;j++)
        {
            k = 1;
            result = 0;
            while (p[j])
            {
                m = p[j] % 2;
                result += k*m;
                k = k * 10;
                p[j] /= 2;
            }
            a[i++] = result;

        }
        int t = i;

        for (i = 0; i < t; i++)
        {
            printf("%3d\n",a[i]);
        }
    }


    >三天打鱼两天晒网


    #include<stdio.h>
    int main()
    {
        int year, month, day;
        int syear, smonth;
        int a[13] = {0,31,28,31,30,31,30,31,31,30,31,30,31};
        printf("Please input year.month.date:\n");
        scanf("%d,%d,%d",&year,&month,&day);
        int i,m;
        syear = 0;
        m = 365;
        for (i = 1990; i < year; i++)
        {
            if ((i % 4 == 0 && i % 100 != 0) || (i % 400 == 0))
                syear = m + 1;
            else
                syear = m;
        }
        smonth = 0;
        int k;
        //最后一年前几个月天数
        for (i = 1; i < month; i++)
        {
            smonth += a[i];
        }

        if ((i % 4 == 0 && i % 100 != 0) || (i % 400 == 0))
            if (month > 2)
            {
                smonth += 1;
            }

        i = syear + smonth + day;

        k = i % 5;


        if (k==1||k==2||k==3)
            printf("今天打渔!\n");
        else
            printf("今天晒网!\n");
        return 0;
    }


    >大小写转化

    if (p[i] >= 'a'&&p[i] <= 'z')
            //小写转大写
        //s[i] = p[i] - 32;
            //s[i]=p[i]-'a'+'A';
        if (p[i] >= 'A'&&p[i] <= 'Z')
            //大写转小写
            s[i] = p[i] + 32;
            //s[i] = p[i] - 'A' + 'a';


    >字符数字转化为整型数字 '1'--> 1

    #include<stdio.h>
    int main()
    {
        char ch;
        ch=getchar();
        int a = ch - '0';
        //int a = ch - 48;
        printf("%d\n",a);
        //数字转字符
        int b = 98;
        char ch1 = (char)b;
        putchar(ch1);
        return 0;
    }

    >最小公倍数与最大公约数

    void fun(int m ,int n)
    {
        int r, rx;
        rx = m*n;

        while (n)
        {
            r = m%n;
            m = n;
            n = r;
        }
        printf("最大公约数为:%d\n",m);
        printf("最小公倍数为:%d\n",rx/m);

    }

    >斐波那契数列



    //递归实现

    int fib1(int n)
    {
        if (n == 1 || n == 2)
            return 1;
        else
            return fib1(n-1)+fib1(n-2);
    }


    //非递归实现

    int fib2(int n)
    {
        int f1, f2, f3;
        f1 = 1;
        f2 = 1;
        f3 = 0;
        if (n == 1 || n == 2)
            f3=1;
        else
        {
            while (n > 2)
            {
                f3 = f1 + f2;
                f1 = f2;
                f2 = f3;
                n--;
            }

        }

        return f3;

    }

    >验证哥德巴赫猜想

    >哥德巴赫猜想：任何一个大于6的偶数均可表示为两个素数之和。输入两个整数m，n（6小于等于m，m小于等于n，n小于等100），将m，n之间的偶数表示成两个素数之和


    #include<stdio.h>
    #include<math.h>
    int main()
    {
        int isPrime(int n);
        int n, m;
        printf("请输入m和n:");
        scanf("m=%d,n=%d",&m,&n);
        int i,j,count=0;
        if (m > 6)
        {
            for (i = m; i <= n; i++)
            {
                if (i % 2 == 0)
                {
                    //注意此处为就j<=i/2
                    for (j = 2; j <= i / 2; j++)
                    {
                        if (isPrime(j) && isPrime(i - j))
                        {
                            printf("%d=%d+%d\n",i,j,i-j);
                            count ++;
                            break;
                        }
                    }
                }
            }
        }
        printf("%d到%d之间共有%d个哥德数.\n",m,n,count);


        return 0;
    }

    int isPrime(int n)
    {
        int i,flag=1;

        for (i = 2; i < sqrt(n); i++)
        {
            if (n == 2)
                return flag;
            else
            {
                if (n%i == 0)
                {
                    flag = 0;
                    break;
                }
            }
        }
    }

    >用穷举法求某数段的素数

    #include<stdio.h>
    int main()
    {
        int i, j;
        int m, n;
        int flag,count=0;
        printf("请输入上下界:\n");
        scanf("%d%d",&m,&n);

        for (i = m; i <= n; i++)
        {
            flag = 1;
            if (i == 1)
            {
                flag = 0;
                continue;
            }
            for (j = 2; j < i; j++)
            {
                if (i%j == 0)
                {
                    flag = 0;
                    break;
                }
            }
            if (flag)
            {
                printf("%d是素数!\n",i);
                count++;
            }

        }
        printf("共%d个素数!\n",count);
        return 0;
    }

    >水仙花数

    >所谓 "水仙花数 "是指一个三位数，其各位数字立方和等于该数本身

    #include<stdio.h>
    int main()
    {
        int a, b, c;
        int i,count=0;



        for (i = 100; i < 1000; i++)
        {
            a = i / 100;
            b = i % 100 / 10;
            c = i % 100 % 10;

            if (i == (a*a*a + b*b*b + c*c*c))
            {
                printf("a=%d,b=%d,c=%d\n", a, b, c);
                printf("%d是水仙花数.\n",i);
                count++;
            }


        }
        printf("共%d个水仙花数\n",count);
        return 0;
    }

    >完全平方数

    #include<stdio.h>
    #include<math.h>
    int main()
    {
        int n;
        printf("请输入一个整数:\n");
        scanf("%d",&n);


        //注意sqrt函数原型 ---- double sqrt(double x);
        if (sqrt(n) == (int)sqrt(n))
        {
            printf("%d是完全平方数.\n", n);
            printf("%d=%d*%d\n", n, (int)sqrt(n), (int)sqrt(n));
        }
        else
            printf("%d不是完全平方数.\n", n);

        return 0;
    }

    >完数

    >一个数如果恰好等于它的因子之和，这个数就称为“完数”。例如6的因子为1,2,3，6=1+2+3，因此6是“完数”。编程找出1000之内的所有完数.

    #include<stdio.h>
    int main()
    {

        int i, sum, j, count=0, k;
        int m;
        int a[100];
        for (i = 1; i <= 1000; i++)
        {
            sum = i;
            k = 0;
            for (j = 1; j < i; j++)
            {
                if (i%j == 0)
                {
                    sum -= j;
                    a[k++] = j;
                }
            }
            if (sum == 0)
            {
                printf("\n%d是完数!\n", i);
                printf("其因子是:");
                for (m = 0; m < k; m++)
                    printf("%d ",a[m]);
                count++;
            }
        }
        printf("\n共%d个完数!\n",count);

        return 0;
    }


    >求近似数（如定积分、用牛顿迭代法或二分法或弦截法求多元方程的根）

    牛顿迭代法

    #include<stdio.h>
    #include<math.h>
    double func(double x)
    {
        return x*x*x*x - 3 * x*x*x + 1.5*x*x - 4.0;
    }


    double func1(double x)
    {
        return 4 * x*x*x - 9 * x*x + 3 * x;
    }

    int Newton(double *x,double precision,int maxcyc)
    {
        double x1, x0;
        int k;
        x0 = *x;
        for (k = 0; k < maxcyc; k++)
        {
            if (func1(x0) == 0.0)
            {
                printf("迭代过程中导数为0!\n");
                return 0;
            }
            x1 = x0 - func(x0) / func1(x0);
            if (fabs(x1-x0) < precision || fabs(func(x1)) < precision)
            {
                *x = x1;
                return 1;
            }
            else
            {
                x0 = x1;
            }
        }
        printf("迭代次数超过预期!\n");
        return 0;
    }

    int main()
    {
        double x, precision;
        int maxcyc;
        printf("输入初始迭代值x0:");
        scanf("%lf",&x);
        printf("输入最大迭代次数:");
        scanf("%d", &maxcyc);
        printf("迭代要求的精度:");
        scanf("%lf", &precision);
        if (Newton(&x, precision, maxcyc) == 1)
        {
            printf("该值附近的根为:%lf\n", x);
        }
        else
        {
            printf("迭代失败!\n");
        }


        return 0;
    }

    //精简版
    #include<stdio.h>
    #include<math.h>
    double fun(double x)
    {
        return 2*x*x*x - 4*x*x + 3*x - 6;
    }
    double fun1(double x)
    {
        return 6*x*x - 8 * x + 3.0;
    }

    int main()
    {
        double x, x0, f,f1,precision;
        printf("请输入初始x0:");
        scanf("%lf",&x);
        printf("请输入精度precision:");
        scanf("%lf",&precision);
        do
        {
            x0 = x;
            f=fun(x0);
        } while (fabs(x-x0)>= precision);
        printf("%lf\n",x);
        return 0;
    }

    二分法
    1 确定区间[a,b],验证f(a)·f(b)<0
    2 求区间(a,b)的中点x
    3 判断
    (1) 若f(a)·f(c)<0,则令b=x;
    (2) 若f(x)·f(b)<0,则令a=x.
    4 判断f(x)是否达到精确度ξ:即若┃f(c)┃<ξ，则x=c就是使f(x)接近零点的近似值，否则重复2-4.
    #include<stdio.h>
    #include<math.h>
    int main()
    {
        double func(double x);
        double root(double a, double b);
        root(-10,10);

        return 0;
    }


    double func(double x)
    {
        return 2 * x*x*x - 4 * x*x + 3 * x - 6.0;
    }

    double root(double a, double b)
    {
        double x;
        double a1=a, b1=b;
        x = (a + b) / 2;
        if(func(x)==0.0)
        {
            printf("该方程在%lf到%lf区间内的根为:%lf.\n",a1,b1,x);
            return x;
        }
        else
        {
            while (fabs(a - b) > 1e-6)
            {
                if (func(a)*func(x) < 0)
                    b = x;
                else
                    a = x;
                x = (a + b) / 2;
            }
        }
        printf("该方程在%lf到%lf区间内的根为:%lf.\n", a1, b1, x);
        return x;
    }

    弦截法

    /*
        函数方程：
            y - f2 = (f2 - f1) / (x2 - x1)(x - x2);
        化简得:
            x=(f2*x1-f1*x2)/(f2-f1);
    */

    #include<stdio.h>
    #include<math.h>
    double xpoint(double x1, double x2);  //弦与x轴的交点横坐标
    double root(double x1, double x2);    //求根函数
    double f(double x); //求x点的函数值
    int main()
    {


        double x1, x2, f1, f2, x;

        do
        {
            printf("请输入x1,x2:");
            scanf("%lf,%lf",&x1,&x2);
            f1 = f(x1);
            f2 = f(x2);

        } while (f1*f2>=0); //当循环运算到f(x1)*f(x2)>=0时(0是必要条件参数)，即f(x1)、f(x2)同符号，且任一个接近于0时，意味着与x轴接近相交，此时存在一个方程实根。
        x = root(x1,x2);
        printf("方程的一个解为：%.2f\n",x);
        return 0;
    }

    double xpoint(double x1, double x2)
    {
        double x = 0;
        x = (x1*f(x2)-x2*f(x1)) / (f(x2)-f(x1));
        return x;
    }


    double root(double x1,double x2)
    {
        double x, y, y1, y2;
        y1 = f(x1);
        y2 = f(x2);
        do
        {
            x = xpoint(x1,x2);
            y = f(x);
            if (y*y1 > 0)
            {
                x1 = x;
                y1 = y;
            }
            else
            {
                x2 = x;
                y2 = y;
            }
        } while (fabs(y)>=0.00001);

        return x;
    }


    double f(double x)
    {
        double y = 0;
        y = x*x*x - 5 * x*x + 16 * x - 80;
        return y;
    }

    >求两个矩阵之和、之积

    #include<stdio.h>
    #define N 2
    int main()
    {
        void print(int(*a)[N]);
        void vx(int(*a)[N], int(*b)[N]);
        int i, j;
        int a[N][N],b[N][N];
        printf("输入两个矩阵:\n");
        printf("矩阵1:\n");
        for (i = 0; i < N; i++)
            for (j = 0; j < N; j++)
                scanf("%d",&a[i][j]);
        printf("---------\n");
        print(a);
        printf("矩阵2:\n");
        for (i = 0; i < N; i++)
            for (j = 0; j < N; j++)
                scanf("%d", &b[i][j]);
        printf("---------\n");
        print(b);


        vx(a,b);
        return 0;
    }


    void vx(int (*a)[N],int (*b)[N])
    {
        void print(int(*a)[N]);
        int i,j,k,res;
        int c[N][N],d[N][N];
        for (i = 0; i < N; i++)
            for (j = 0; j < N; j++)
            {
                res = 0;
                c[i][j] = a[i][j] + b[i][j];
                for(k=0;k<N;k++)
                    res += a[i][k] * b[k][j];
                d[i][j]=res;
            }
        printf("两矩阵相加:\n");
        print(c);
        printf("两矩阵相乘:\n");
        print(d);
    }
    void print(int (*a)[N])
    {
        int i,j;
        for (i = 0; i < N; i++)
        {
            for (j = 0; j < N; j++)
                printf("%d ",a[i][j]);
            printf("\n");
        }
    }

    >统计输入字符中的单词个数


    #include<stdio.h>
    int main()
    {
        int analysis(char *s);
        char s[100];
        int word;
        gets_s(s);
        word=analysis(s);
        printf("该字符串中共有%d个单词.",word);

        return 0;
    }


    int analysis(char *s)
    {
        char *p = s;
        int len = 0;
        while (*p++)
            len++;
        int i,flag=0,word=0;
        for (i=0;i<len;i++)
        {
            if (flag==0)
            {
                if (s[i] != ' ')
                {
                    word++;
                    flag = 1;
                }
            }
            else
            {
                if (s[i] == ' ')
                    flag = 0;
            }
        }
        return word;
    }
    >位运算

    1.与 &
    1&0=0;0&0=0;1&1=1;
    2.或 |
    1|0=1;0|0=0;1|1=1;
    3.非 ~
    ~1=-2
    ~16=-17
    00010000  16二进制数
    11101111  16二进制数取非运算
    取反+1即：
    00010000+1=00010001=-17
    4.异或
    1^0=1;0^0=0;1^1=0

    1.该函数给出一个字节中被置为1的位的个数
    #include<stdio.h>
    int main()
    {
        int sum = 0;
        //题中给出一个字节，则占8位，取值范围为0~255
        int a;
        printf("请输入一个字节的数，以十进制输入:\n");
        scanf("%d",&a);
        if (a < 0 || a>255)
        {
            printf("error\n");
            return 1;
        }

        int i, k=1;
        for (i = 1; i < 9; i++)
        {
            if (a == (a | k))
                sum++;
            k *= 2;
        }
        printf("共%d位\n",sum);
        return 0;
    }

    >复制字符串


    #include<stdio.h>
    #include<string.h>
    int main()

    {
        char *Mystrcpy(char *s1, char *s2);
        void print(char *s1);
        char a[] = "adad";
        char b[20],*s;



        /*未封装函数*/
        char *p1, *p2;
        /*p1 = a;
        p2 = b;*/
        while (*p1 != '\0')
        {
            *p2 = *p1;
            p1++;
            p2++;

        }
        int i;

        *p2 = '\0';
        for (i = 0; b[i] != '\0'; i++)
        {
            printf("%c",b[i]);
        }
        printf("\n");
        return 0;


        /*封装函数后*/
        s=Mystrcpy(b,a);
        print(s);
    }


    /*复制字符串*/
    /*方法一*/
    char *Mystrcpy(char *s1,char *s2)
    {

        char *p1, *p2;
        p1 = s1; p2 = s2;

        /*方法一*/
        /*while (*p2 != '\0')
        {
            *p1 = *p2;
            p1++;
            p2++;

        }
        *p1 = '\0';*/

        /*方法二*/
        //while (*p1++=*p2++);

        /*方法三*/
        //方法三实际上等价于方法二
        //注意此处"="只有一个，为赋值运算并非等于运算
        /*while (*p1=*p2)
        {
            p1++;
            p2++;
        }
    */


        //方法四
        /*
        '\0'字符串对应的ASCII为0,当p2所指向的字符为'\0'时，此时退出循环，未将结束字符串标记赋值给p1所指向的字符串。故在最后加上*p1='\0'/
        */
        do
        {
            *p1++ = *p2++;
        } while (*p2);
        *p1 = '\0';
        /*
        方法五
        方法四等同于方法五
        */
        while (*p2)
        {
            *p1++ = *p2++;
        }
        *p1 = '\0';

        return s1;
    }

    void print(char *s1)
    {
        int i;
        for (i = 0; s1[i] != '\0'; i++)
        {
            printf("%c", s1[i]);
        }
        printf("\n");

    }

    /*复制字符串最简单方法*/
    #include<stdio.h>
    #include<string.h>
    int main()
    {
        char s1[] = "asda";
        char s2[20];

        strcpy(s2,s1);
        printf("%s",s2);
    }


    >汉诺塔问题

    #include<stdio.h>
    int main()
    {
        void move(int n, char pre, char mid, char end);
        int n;
        printf("请输入要移动的块数：");
        scanf("%d",&n);
        move(n,'a','b','c');

    }

    void move(int n,char pre,char mid,char end)
    {
        if (n == 1)
            printf("%c->%c\n",pre,end);
        else
        {
            move(n - 1, pre, end, mid);
            move(1,pre,mid,end);
            move(n-1,mid,pre,end);
        }

    }


    >issue

    printf("x=%5f\n",x);//等价于printf("x=%.5f\n");
    int scanf //T
    float case //F

    >同构数
    >正整数n若是它平方数的尾部，则称n为同构数

    #include<stdio.h>
    int main()
    {
        int i;
        for (i = 1; i < 100; i++)
        {
            int pow = i*i;
            int a;
            if (i < 10)
            {
                a = pow % 10;
                if (i == a)
                {
                    printf("%d是%d的同构数.\n",i,pow);
                }
            }
            else if (i >= 10&i<100)
            {
                a = pow % 100;
                if (i == a)
                {
                    printf("%d是%d的同构数.\n", i, pow);
                }
            }
        }

        return 0;
    }


    #include"stdio.h"
    int isomorphism(int i)
    {
        int mod;
        if (i<10)
            mod = 10;
        else
            mod = 100;
        if (i == i*i%mod)
            return 1;
        return 0; //不是，则要明确返回0
    }
    void main()
    {
        int i;
        printf("1~100之间的同构数有：\n");
        for (i = 1; i<100; i++)
        {
            if (isomorphism(i) == 1)
                printf("%4d", i);
        }
        printf("\n");
    }

    //判断是不是同构数函数
    int fun(int n)
    {
        int a, b;
        int m = n*n;
        while (n)
        {
            a = n % 10, b = m % 10;
            if (a != b)
                break;
            n /= 10;
            m /= 10;
        }
        if (n)
            return 0;
        return 1;
    }

.. code:: c

    /*单链表*/
    #include<stdio.h>
    #include<malloc.h>
    typedef struct Node {
        int data;
        struct Node *PNext;
    }Node, *PNode;
    #define ERROR 0
    #define OK 1
    PNode create_list()
    {
        int len, i;
        printf("请输入链表的长度:len=\n");
        scanf("%d",&len);
        PNode PHead = (PNode)malloc(sizeof(Node));
        PHead->PNext = NULL;
        PNode PTail=PHead;
        for (i = 0; i < len; i++)
        {
            int val;
            printf("请输入第%d个元素的值:",i+1);
            scanf("%d",&val);
            PNode PNew = (PNode)malloc(sizeof(Node));
            PNew->data = val;
            PNew->PNext = NULL;
            PTail->PNext=PNew;
            PTail = PNew;
        }
        return PHead;

    }


    void outLink(PNode pHead)
    {
        PNode p = pHead->PNext;
        while (p)
        {
            printf("%d ",p->data);
            p = p->PNext;
        }
        putchar('\n');
    }

    PNode reverse(PNode pHead)
    {
        PNode p = pHead->PNext;

        PNode q,r;
        q = p->PNext;
        p->PNext = NULL;
        while (q)
        {
            r = p;
            p = q;
            q = p->PNext;
            p->PNext = r;
        }
        pHead->PNext = p;

        return pHead;
    }

    int list_num(PNode pHead)
    {
        int num = 0;
        PNode p = pHead->PNext;
        while (p)
        {
            num++;
            p = p->PNext;
        }
        return num;
    }


    //pos从1开始
    /*
        12345
        有6个插入位置
    */
    int insert_list(pnode phead, int val, int pos)
    {
        int i;

        pnode p = phead->pnext;
        int length = list_num(p);
        if (pos<1 || pos>length+2)
            return error;
        else
        {
            i = 0;

            //定位到pos前一位,在下标pos-1处插入结点，需要知道前一结点
            while (p&&i < pos - 2)
            {
                i++;
                p = p->pnext;
            }
            if (p||(i == pos - 2))
            {
                pnode pnew = (pnode)malloc(sizeof(pnode));
                pnew->data = val;
                if (pos == 1)
                {
                    pnew->pnext = p;
                    phead->pnext = pnew;
                }
                else if (pos == length+2)
                {
                    p->pnext = pnew;
                    pnew->pnext=null;
                }
                else
                {

                    pnew->pnext = p->pnext;
                    p->pnext = pnew;
                }
                length++;
                return ok;
            }
            else
                return error;
        }
    }

    int insert_list(PNode pHead, int val, int pos)
    {

        PNode p = pHead;
        int length = list_num(p),i;
        if (pos<0 || pos>length ||!p)
            return ERROR;
        else
        {
            i = -1;
            while (p&&i < pos - 1)
            {
                i++;
                p=p->PNext;
            }
            if (i == pos - 1 || p)
            {
                PNode PNew = (PNode)malloc(sizeof(PNode));
                PNew->data = val;
                if (i == -1)
                {
                    PNew->PNext = pHead->PNext;
                    pHead->PNext = PNew;
                }
                else if (pos == length)
                {
                    p->PNext = PNew;
                    PNew->PNext = NULL;
                }
                else
                {
                    PNew->PNext = p->PNext;
                    p->PNext = PNew;
                }
                length++;
                return OK;
            }
            else
                return ERROR;
        }
    }
    //删除,根据结点值删除
    int delData(PNode pHead, int *val)
    {
        int length = list_num(pHead);
        PNode p = pHead,q;

        if (!p)
            return ERROR;
        while (p->PNext!=NULL)
        {
            if (p->PNext->data == *val)
            {
                q = p->PNext;
                p->PNext = p->PNext->PNext;
                free(q);
            }
            else
                p = p->PNext;
        }

        return OK;
    }

    //删除，按照位置删除,且pos从1开始

    int delpos(PNode pHead, int pos)
    {
        PNode q = pHead;
        PNode p = q->PNext;
        int length = list_num(pHead);
        if (pos<1 || pos>length)
            return ERROR;
        else
        {
            int i = 1;
            while (i < pos - 1)
            {
                i++;
                q = p;
                p = p->PNext;
            }
            if (pos == 1)
                pHead->PNext = pHead->PNext->PNext;
            else if (pos == length)
                q->PNext = NULL;

            length--;

            return OK;
        }
    }

    //或者pos从0开始计算
    /*12345
    有0~length范围可插入
    */
    int main()
    {
        int num;
        PNode PHead = create_list();
        outLink(PHead);

        num = list_num(PHead);
        putchar('\n');
        printf("共有%d个结点.\n", num);
        PNode rp = reverse(PHead);
        outLink(rp);
        int val;
        printf("请输入插入结点的值：");
        scanf("%d", &val);
        int pos;
        printf("请输入插入结点的位置(从1开始)：");
        scanf("%d", &pos);
        int flag = insert_list(rp, val, pos);
        if (flag == 1)
        {
            printf("插入结点成功,共%d个结点\n", list_num(rp));
            outLink(rp);
        }
        else
            printf("插入失败.\n");

        int data;
        printf("请输入要删除的结点值:");
        scanf("%d", &data);
        int f = delData(rp, &data);
        if (f == 1)
        {
            printf("删除%d成功!\n", data);
            outLink(rp);
        }
        else
            printf("删除结点不存在!\n");
        int delposi;
        printf("请输入要删除的位置:");
        scanf("%d", &delposi);
        int f1 = delpos(rp, delposi);
        if (f1 == 1)
        {
            printf("删除第%d个位置成功!\n", delposi);
            outLink(rp);
        }
        else
            printf("删除位置不存在!\n");
    }

.. code:: c

    /*
        栈
    */

    #include<stdio.h>
    #define N 10
    typedef struct Stack
    {
        int top;
        int data[N];
    }stack;

    stack s = { -1,{0} };

    int isempty()
    {
        if (s.top == -1)
            return 1;
        else
            return 0;
    }

    void setEmpty()
    {
        s.top = -1;
    }

    int push(int data)
    {
        if (s.top + 1 <= N - 1)
        {
            s.data[++s.top] = data;
            return 1;
        }
        else
        {
            return 0;
        }

    }

    int pop()
    {
        if (isempty())
            return -1;
        else
            return s.data[s.top--];
    }

    void tenTobinary(int n)
    {
        if (n == 0)
            return;
        else
        {
            int m = n % 2;
            push(m);
            tenTobinary(n/2);
        }

    }

    int main()
    {
        int a[10];
        int i;
        for (i = 0; i < 10; i++)
        {
            a[i] = i + 1;
        }
        for (i = 0; i < 10; i++)
            push(a[i]);
        while (!isempty())
        {
            printf("%d\n",pop());
        }
        tenTobinary(100);
        while (!isempty())
        {
            printf("%d", pop());
        }
        printf("\n");
        return 0;
    }

.. code:: c

    #include<stdio.h>
    #include<malloc.h>
    #define MAXSIZE 100
    typedef char dataType;
    typedef struct bnode
    {
        dataType data;
        struct bnode *lChild, *rChild;
    }Bnode,*BTree;
    typedef struct
    {
        BTree data[MAXSIZE];
        int front, rear;
    }SeQuence,*PSeqQueue;
    BTree createTree()
    {
        BTree tree;
        dataType str;
        str = getchar();
        if (str == '#')
            tree = NULL;
        else
        {
            tree = (BTree)malloc(sizeof(Bnode));
            tree->data = str;
            tree->lChild = createTree();
            tree->rChild = createTree();
        }
        return tree;
    }


    void visit(char ch)
    {
        printf("%c \t",ch);
    }
    void preOrder(BTree tree)
    {
        BTree p = tree;
        if (tree == NULL)
            return;
        visit(p->data);
        preOrder(p->lChild);
        preOrder(p->rChild);
    }
    void inOrder(BTree tree)
    {
        BTree p = tree;
        if (tree == NULL)
            return;

        inOrder(p->lChild);
        visit(p->data);
        inOrder(p->rChild);
    }
    void postOrder(BTree tree)
    {
        BTree p = tree;
        if (tree == NULL)
            return;

        postOrder(p->lChild);
        postOrder(p->rChild);
        visit(p->data);
    }

    PSeqQueue initSeqQueue()
    {
        PSeqQueue queue;
        queue = (PSeqQueue)malloc(sizeof(SeQuence));
        if (queue)
        {
            queue->front = queue->rear = 0;
        }
        return queue;
    }

    int emptyQueue(PSeqQueue queue)
    {
        if (queue&&queue->front == queue->rear)
            return 1;
        else
            return 0;

    }
    int pushQueue(PSeqQueue queue, Bnode *node)
    {
        if ((queue->rear + 1) % MAXSIZE == queue->front)
            return -1;
        else
        {
            queue->rear = (queue->rear + 1) % MAXSIZE;
            queue->data[queue->rear] = node;
            return 1;
        }
    }

    int popQueue(PSeqQueue queue, BTree *node)
    {
        if (emptyQueue(queue))
        {
            return -1;
        }
        else
        {
            queue->front = (queue->front + 1) % MAXSIZE;
            *node = queue->data[queue->front];
            return 1;
        }
    }

    void destryQueue(PSeqQueue *queue)
    {
        if (*queue)
        {
            free(*queue);
            *queue = NULL;
        }
    }
    void levelOrder(BTree tree)
    {
        BTree p = tree;
        PSeqQueue queue = initSeqQueue();
        if (p)
        {
            pushQueue(queue,p);
            while (!emptyQueue(queue))
            {
                popQueue(queue,&p);
                visit(p->data);
                if(p->lChild)
                    pushQueue(queue, p->lChild);
                if (p->rChild)
                    pushQueue(queue, p->rChild);
            }
        }

    }
    int height(BTree tree)
    {
        int h1, h2;
        if (tree == NULL)
            return 0;
        else
        {
            h1 = height(tree->lChild);
            h2 = height(tree->rChild);
            if (h1 > h2)
                return h1 + 1;
            else
                return h2 + 1;
        }

    }

    void levelCount(BTree tree,int num[],int l)
    {
        if (tree)
        {
            num[l]++;
            levelCount(tree->lChild, num, l + 1);
            levelCount(tree->rChild, num, l + 1);
        }
    }

    int countTree(BTree tree)
    {
        int lCount, rCount;
        if (tree == NULL)
            return 0;
        lCount = countTree(tree->lChild);
        rCount = countTree(tree->rChild);
        return lCount + rCount + 1;
    }

    int main()
    {
        BTree tree = createTree();
        int i = 0;
        int countNum[10] = { 0,0,0,0,0,0,0,0,0,0 }, l = 1, treeHeight, treeCount;//记录每层的节点数,l从1开始,树的深度
        treeHeight = height(tree);
        printf("\n此二叉树的深度为: %d\n", treeHeight);

        treeCount = countTree(tree);
        printf("此二叉树的节点总数为: %d\n", treeCount);

        levelCount(tree, countNum, l);
        printf("此二叉树各层的节点数为: ");
        for (i = 1; i <= treeHeight; i++) {
            printf("第%d层数目: %d,  ", i, countNum[i]);
        }
        printf("\n\n");

        printf("先序遍历此二叉树: ");
        preOrder(tree);
        printf("\n");

        printf("中序遍历此二叉树: ");
        inOrder(tree);
        printf("\n");

        printf("后序遍历此二叉树: ");
        postOrder(tree);
        printf("\n");

        printf("层次遍历此二叉树: ");
        levelOrder(tree);
        printf("\n");

        return 0;
    }

.. code:: c


    二维数组某位置上的元素在该行上最大，该列上最小
    #include<stdio.h>
    int main()
    {
        int a[4][5];
        int i, j, k, m;
        for (i = 0; i < 4; i++)
            for (j = 0; j < 5; j++)
                scanf("%d",&a[i][j]);

        for (i = 0; i < 4; i++)
        {
            for (j = 0; j < 5; j++)
            {
                //扫描行，确定行中最大值
                for (k = 0; k < 5; k++)
                {
                    if (a[i][j] < a[i][k])
                    {
                        break;
                    }
                }
                if (k == 5)
                {
                    //扫描列，确定列中最小值
                    for (m = 0; m < 4; m++)
                    {
                        if (a[i][j] > a[m][j])
                            break;
                    }
                    if (m == 4)
                        printf("满足值:%d ",a[i][j]);
                }
            }
        }

        return 0;
    }

.. code:: c

    /*
    对这个字符串从头到尾遍历，在迭代器i遍历字符串的过程中，遇到不是空格，就是遇到单词，
    i开始计算，通过对当前最大值max的对比，最终求出一个最长的单词。同时记录这个单词，
    最后的一个字母的位置，而这个最大值max就是这个单词的长度。可以倒着输出，
    字符串中的第p-max到第p个字母就是这个最长的单词。
    */
    //输出一行字符串中的最长单词及其位置
    #include<stdio.h>
    int main()
    {
        int word_length = 0, word_max=0;
        printf("请输入一个字符串:");
        char s[20];
        gets_s(s);
        int i,p;//这个p是用来记录最长单词的位置
        for (i = 0; s[i] != '\0'; i++)
        {
            if (s[i] == ' ')//扫到空格，则结算是否为最长的单词
            {
                if (word_length > word_max)
                {
                    word_max = word_length;
                    p = i;
                }

                word_length = 0;
            }
            else//如果i扫到的不是空格，那么开始计算单词的长度
                word_length++;

        }

        if (word_length > word_max)//此乃用于最长的单词在结尾的情况
        {
            word_max = word_length;
            p = i;
        }
        printf("最长单词的位置：%d\n",p-word_max+1);
        char longest[100];
        for (p = p - word_max, i = 0; word_max > 0; word_max--,p++, i++)
            longest[i] = s[p];
        longest[i] = '\0';
        printf("The longest word is %s\n", longest);

        return 0;
    }

.. code:: c

    /*
        海滩上有一堆桃子，五只猴子来分。第一只猴子把这堆桃子凭据分为五份，多了一个，
        这只猴子把多的一个扔入海中，拿走了一份。第二只猴子把剩下的桃子又平均分成五份，又多了一个，
        它同样把多的一个扔入海中，拿走了一份，第三、第四、第五只猴子都是这样做的，问海滩上原来最少有多少个桃子？
    */
    //递归

    #include<stdio.h>
    int divided(int n, int m)
    {
        //不足5个或不能分5份多1个，分配失败
        if (n / 5 == 0 || n % 5 != 1)
            return 0;
        //分到最后一个猴子，说明能分配成功
        if (m == 1)
            return 1;
        return divided(n-n/5-1,m-1);

    }

    int main()
    {
        int n;
        int m = 5;
        for (n = 1;; n++)
        {
            if (divided(n, m))
            {
                printf("%d\n",n);
                break;
            }
        }
        return 0;
    }


    //猴子分桃问题
    #include<stdio.h>

    int main()
    {
        int i,num,k;
        for (i = 6;; i += 5) //对所有能分成5份的进行试探
        {
            num = i;
            //5个猴子做同样分挑操作
            for (k = 0; k < 5; k++)
            {
                if (num / 5 == 0 || num % 5 != 1)
                    break; /*如果剩余的桃子分不成5份或者分成5份后不会正好多余1个桃子，则说明分桃过程进行不下去了，结束分桃*/
                num = num - num / 5 - 1; /* 如果分桃过程能进行下去，拿走一份，扔掉1个，下一个猴子继续分桃*/

            }
            if (k >= 5)
            {
                break;/*如果上面的循环顺利结束，说明分桃顺利结束，找到了符合条件的最小桃子数，返回*/
            }
        }
        printf("最少桃树为：%d\n",i);
    }


    /*还有一种逆向求解的方法,探试的次数要少一点*/
    #include<stdio.h>
    int MinNum()
    {
        int i, j, num;
        for (i = 6;; i += 5)
        {
            num = i;
            for (j = 5; j > 1; j--)
            {
                if (num % 4)
                    break;
                num = num + num / 4 + 1;

            }
            if (j <= 1)
                return num;

        }
    }
    int main()
    {
        printf("最少桃子数为：%d\n", MinNum());
        return 0;
    }

.. code:: c

    >文件指针是指针类型的变量

    >文件指针是指针变量，存储的是文件缓存首地址，而不是文本在计算机磁盘中的路径信息

    >b=a>>2后，a还是a，只是b变

    >注意数组陷阱:a[5]={9,1,3,4};还有一个元素0

.. code:: c

    >sizeof与strlen
     //指针是固定的4字节
    CHAR *A = "SA";
    SIZEOF(A)=4;
    //如果数组[]没数字，SIZOEF根据后面的字符串长度来算，如果有数字，直接是数字值
    CHAR A[] = "ADSASD";
    SIZEOF(A) = 7;

.. code:: c

    >转义符
    char *str1 = "asd\0sa";
    char *str1 = "ad\0阿斯达";
    puts(str1); //ad
    //分析:\0后面是字符/汉字，而不是数字，不能表示八进制数，直接推出，输出ad
    /*char *str2 = "ad\0125456";
    puts(str2);*/  //ad换一行(\n)5456
    //分析:\0后面两位与\0表示八进制数,则八进制12对应十进制10即\n;
    char *str2 = "ad\085456";
    puts(str2); //ad
    //分析:\0后面第一位直接是8，超过8进制范围，直接结束，此时只输出ad
    //char *str3 = "ad\xgsdd";//编译器报错，无法运行，\x后面的g超出十六进制范围
    //char *str3 = "ad\x1112"; //有\x则对后面所有数字组合成十六进制进行转化，而1112转化为十进制4370，会报"对字符来说太大"错误
    char *str3 = "ad\x9\05"; //\x9对应制表符
    printf("%d", strlen(str3));//strlen,不算末尾的\0,4个字符长 a d 十六进制\x9--对应十进制9即制表符\t  八进制\05对应5对应梅花
    puts(str3);
    //\x与\0一点不同之处在于，当\x后面跟的数字不符合十六进制，则会报错，因为\x不会像\0那样，\0是字符串结束标记，
    //后面跟的数字即使不符合结束标记，它也不会报错，此时会直接作为字符串结束标记，结束字符串，而\x后面必须跟合法的十六
    //进制数，否则直接报错。
    char *str4 = "\t\017\0qw";
    printf("%s", str4);
    printf("\n%d\n", strlen(str4)); //2个分别是制表符\t与八进制\017对应的十进制15即太阳，共两个字符
    char *str5 = "\ta\017bc";
    printf("\n%d,%d\n", strlen(str5), sizeof(str5));  //5 4 5个字符分别是\t a \017 b c

.. code:: c

    >数字正反序
    //正序输出 8765

    int n,t,s=1,x=0;
    while(n)
    {
    t=n%10;
    x+=t*s;
    s*=10;
    n/=10;
    }

    //逆序输出5678

    int n,t,x=0;
    while(n)
    {
    t=n%10;
    x=x*10+t;
    n/=10;
    }

.. code:: c

    >   求最后三位
    x=x/1000;

.. code:: c

    >一维与二维数组对比
    char *s[] = {"c language","pascal","masm"};
    printf("%s\n",s[1]+2);  //输出---scal
    printf("%c\n",s[1][2]);//或printf("%c",*(s[1]+2));  //输出s

.. code:: c

    >基本数据类型分类
    整型、字符型、浮点型(实型)

.. code:: c

    >函数
    自定义函数、标准库函数

.. code:: c

    >优先级
    int a = 1,b = 2, c = 0;
    printf("%d", a || b&&c); //1,&&优先级高于||优先级

    括号成员第一

    全体单目第二

    &乘除余三   //这个"余"是指取余运算即%

    加减四

    移位五

    关系六

    等于不等排第七

    位与异或和位或三分天下八九十
    逻辑与十一

    逻辑或十二

    条件(十三、？：)高于赋值(十四=)

    逗号运算级最低!

    例：
    int i = 1 << 1 + 1;
    printf("%d",i);  //4---+-优先级排四，位移五

参考链接:http://blog.csdn.net/qq\_24754061/article/details/72780229
