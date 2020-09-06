# 翻译-Python中的可改和不可改

## ChangeLog

- 20200906 翻译和调试，记录比较混乱，主要是留个记忆。

## 背景

**原文**

[Mutability and Immutability in Python — Let’s Break It Down \| by Wendy Leung \| Data Driven Investor \| Medium](https://medium.com/datadriveninvestor/mutable-and-immutable-python-2093deeac8d9)

以原文为准，翻译可能错的离谱，其中夹杂了理解，只是用翻译集中注意力。

学python时没看到下面的话

> 比较的两个变量，指向的都是地址不可变的类型（str等），那么is，is not 和 ==，！= 是完全等价的。
>
> 对比的两个变量，指向的是地址可变的类型（list，dict，tuple等），则两者是有区别的。

## 你曾经想修改一个变量 without knowing it or wanting to ?

本文我们将覆盖id type 可改对象 不可改对象，为什么这个重要呢？Python中如何处理可改和不可改？传入函数的变量怎么样？如何判断传入对象是可改还是不可改的。本文将使用python3的例子。

## 什么是id()？

id是python的内置函数，给予了我们检查一个对象唯一标记的方法。让我们看下如何工作的。

python中一切都是对象？

a = 1

那么a是一个对象，包含了一个值是1。让我们检查下a这个对象的id是多少的。

![image-20200906131854293](https://raw.githubusercontent.com/eatcosmos/assets/master/image-20200906131854293.png?token=AD5MVOGAUNAYRCSNHCEJEZK7KRYXW)

这个id指向了一个对象的内存位置。让我们在试验一个

![image-20200906134529305](https://raw.githubusercontent.com/eatcosmos/assets/master/image-20200906134529305.png?token=AD5MVOEQCR2GIAQR7ZCN6WC7KR33M)

你能看到id(a)和id(b)是不一样的，我们可以测试一下：

![image-20200906134857362](https://raw.githubusercontent.com/eatcosmos/assets/master/image-20200906134857362.png?token=AD5MVOEGGMTFUK5ZJNMERRK7KR4IO)

让我们再给另外一个变量赋值2，然后再检查下id:

![image-20200906135015945](https://raw.githubusercontent.com/eatcosmos/assets/master/image-20200906135015945.png?token=AD5MVOGIN3PCR46KUDGODWK7KR4NK)

id(c) id(b)居然相等，为什么是那样，让我们测试下是不是相等。

![image-20200906135145126](https://raw.githubusercontent.com/eatcosmos/assets/master/image-20200906135145126.png?token=AD5MVOBEO2DOEVREBJKJLTS7KR4S4)

他们有相同的唯一标志因为c引用一个对象包含了值2，b也引用了一个相同的对象包含值2，为什么不是两个对象，分别都包含值2呢？

他们都指向了同一个包含值2的对象。

![image-20200906135703583](https://raw.githubusercontent.com/eatcosmos/assets/master/image-20200906135703583.png?token=AD5MVOGIES4BUP2RNSILIEK7KR5G2)

这个对象，也就是那个id数字，具有唯一身份。表示在内存中的地址。对象可以有多个值。让我们再用strings测试下。

![image-20200906140854550](https://raw.githubusercontent.com/eatcosmos/assets/master/image-20200906140854550.png?token=AD5MVOBOSXI7AUIVDVTGKEK7KR6TI)

这里发生了什么？在上面的第第二个例子中，我们使用了大写的S在变量b中，所以值不同，也就是在内存的位置也不同了。

让我们再看更多的例子：

![image-20200906141833126](https://raw.githubusercontent.com/eatcosmos/assets/master/image-20200906141833126.png?token=AD5MVOAJXTUX66YW5DLDEGS7KR7XO)

为什么这里都是260又不是引用的同一个对象呢？

是因为Python维护了一个整型数组对象，这里的整数范围是-5到256.当你创建了一个整数再这个范围的时候，你获得了一个引用指向已经存在了的对象。

Python通过两个宏，NSMALLNEGINTS和NSMALLPOSINGS。如果值ival满足条件在-5和256之间，包含-5和256，则函数get_small_int会被调用。

```c
#ifndef NSMALLPOSINTS
#define NSMALLPOSINTS           257
#endif
#ifndef NSMALLNEGINTS
#define NSMALLNEGINTS           5
#endif
#define CHECK_SMALL_INT(ival)
    do if (-NSMALLNEGINTS <= ival && ival < NSMALLPOSINTS) {
        return get_small_int((sdigit)ival);
    } while(0)
```

注意：你可以使用is来检查两个变量是否有相同对象id：

![image-20200906143015987](https://raw.githubusercontent.com/eatcosmos/assets/master/image-20200906143015987.png?token=AD5MVOA6KDH6A336NFBVR6K7KSBDK)

让我们介绍更多的术语。

上面这两个列表变量有不同的变量名，list1 list2，我们能说他们互为别名。变量引用对象，其实就是指向对象，如果我们把一个变量赋值给变量，其实即使让新变量应用该变量所引用的对象，所以两个边路都引用相同的对象了。这就是**别名**的意义。

让我们讨论**克隆**的概念。如果我们想修改一个列表对象并且保留原始列表对象的拷贝，我们需要制作一份拷贝，这个过程就是克隆。列表的分片就是创建一个列表。

![image-20200906143723794](https://raw.githubusercontent.com/eatcosmos/assets/master/image-20200906143723794.png?token=AD5MVOFPGVKX4OGUDDS4LE27KSB6C)

## 什么是type？

type()

Python里所有的数据都存储在对象的形式里面。一个对象有3种事情：id type value。

type函数会提供其参数对象的类型：

![image-20200906145734679](https://raw.githubusercontent.com/eatcosmos/assets/master/image-20200906145734679.png?token=AD5MVOGRGCJEBAXT4KK6WES7KSEJY)

同id()一样type()对调试很有帮助的。

## 什么是可以修改的对象？

list dict set

一个程序存储数据在变量里面，表示在内存里面某个位置存储数据。内存位置的内容，在一个程序的执行的某个时刻，叫做程序的状态。

Python的一些类型对象是可以修改的，一些事不可以修改的。首先，我们讨论可以修改的对象。一个可以修改的对象是可以修改的对象，它的状态可以在被创建后修改。

![image-20200906150616559](https://raw.githubusercontent.com/eatcosmos/assets/master/image-20200906150616559.png?token=AD5MVODMYVBA3DDGWYGBOWC7KSFKM)

如果我们想要修改我们的list里的第一个值，我们可以看到list值发生了修改，但是呢，list的内存地址不变。

修改相同内存地址的内容，这就是mutable的含义。

![image-20200906150922612](https://raw.githubusercontent.com/eatcosmos/assets/master/image-20200906150922612.png?token=AD5MVOGQSLVQ2PNWXSI6UUK7KSFWA)

让我们现在看看list的值得内存地，并且看看list第一个值修改前和修改后发生了什么：

![image-20200906152059991](https://raw.githubusercontent.com/eatcosmos/assets/master/image-20200906152059991.png?token=AD5MVOFVYP2RPKJWDSE5EJC7KSHBU)

my_list[0]的id是...当list的第一个元素是'sugar glider'的时候。当my_list[0]被改变为'rabbit'的时候，id也改了。

当我们修改一个list并且改变list里面元素的值时，list保持了相同的地址。但是，你改的那个元素会有一个不同的地址，那原来的内存地址释放了吗？

## 什么是imutable对象？

integer float string tuple bool frozenset

一个不可修改的对象是这样一种对象，它的状态在创建后就不能修改了。

在Python里面，一个字符串是不可以修改的。你不能改写一个immutable的变量。

但是你可以给变量重新赋值，原来的字符串对象依然存在，被释放了吗？

![image-20200906154924808](https://raw.githubusercontent.com/eatcosmos/assets/master/image-20200906154924808.png?token=AD5MVOCBBFYHZ2YFLKTYH727KSKME)

上面病没有修改string对象；而是创建了一个新的string对象。

为了更详细地看下，我们可以使用id函数我们之前学的。调用id()来大姨出内存地址。

![image-20200906155317156](https://raw.githubusercontent.com/eatcosmos/assets/master/image-20200906155317156.png?token=AD5MVOHMHBT2YGMWAYMW7LC7KSK2U)

因为string是不可以修改的，所以只能创建一个新的string对象，可以看出来内存地址不一样的。

让我们尝试修改字符串的一个字符：

![image-20200906155506978](https://raw.githubusercontent.com/eatcosmos/assets/master/image-20200906155506978.png?token=AD5MVOAXOLRNYOVPOZEKAPC7KSLBQ)

我们得到了一个类型错误，因为string是immutable。我们不能修改string对象本身。

让我们来谈谈tuples。

说tuples是immutable只对了一部分。tuple本身不能被修改，但是tuple其实是引用他的内部元素，如果引用的内部元素是可以修改的，那么元素可以变，不变得是地址。

如果一个tuple有一个immutable域比如string，然后tuple就是不可以更改的。

## 为什么mutable和immutable对象重要，Python如何区别对待他们？

数字，字符串，和元组是immutable。list、dictionary，和set是mutable。

immutable通常用于确保对象保持常量不可变的。可变对象的值能够在任何时间和地点被改变，只要你想。

## 传递给函数的参数对于mutable和immutable分别意味着什么呢？

Python编译器必须处理函数参数，判断是mutable还是immutable。

如果一个mutable对象被一个函数引用，那么原始变量可能被修改。如果你想要避免修改原始变量，你需要拷贝到另外一个变量里。

当一个immutable对象在函数里被引用的时候，他的值不能被修改。

让我们看一个程序：

![image-20200906202725959](https://raw.githubusercontent.com/eatcosmos/assets/master/image-20200906202725959.png?token=AD5MVOA7H4VXRCOFQROSRU27KTK6Y)

变量b引用了一个值为9的对象。当我们传递b作为一个函数参数的时候，局部变量n也引用了相同的对象。但是，整型对象是immutable，所以程序会自动创建一个新的对象用于指代值10，并且把它复制给变量n。这本变量n现在指向了一个不同的对象了，不再指向b所指向的对象了。现在，n指向了一个值为10的对象，但是b依然指向了一个值为9的对象。当我们print(b)，我们得到了9。

---

让我们来看另外一个Python脚本来看看应该输出什么呢？

![image-20200906203652279](https://raw.githubusercontent.com/eatcosmos/assets/master/image-20200906203652279.png?token=AD5MVOFV4TXZ3CMUG74PIZK7KTMCC)

想想上面的结果应该是什么？或许可以画一个图示，然后再往下阅读。

这个变量my_list指向一个list对象，这个对象包含3个整型引用。list是mutable，指的是元素可以指向其他对象。当我们把my_list作为一个函数参数传递给increment的时候，这个函数具有局部变量n也指向了my_list所指向的对象。

![Image for post](https://raw.githubusercontent.com/eatcosmos/assets/master/1*3L57bSQ8gLucd1JEK4lrPQ.png?token=AD5MVOERAZKC26KNJZG37K27KTN4Q)

因为list是mutable，所以.append()方法能够修改这个位置的list。

---

让我们再看一个例子

![image-20200906220203009](https://raw.githubusercontent.com/eatcosmos/assets/master/image-20200906220203009.png?token=AD5MVOESFM4E22QEFD4L6427KTWBS)

上面例子中，我们传入了两个参数到函数assign_value(n, v)中。函数有一个本地变量n指向了list1所指向的对象，本地变量v指向了list2所指向的对象。然后做了变量重定向的操作，从图示的箭头理解会更直观清晰。

![Image for post](https://raw.githubusercontent.com/eatcosmos/assets/master/1*Ed0_H11yrSd0Qyc6X7mQ5w.png?token=AD5MVOGKVQ7MI7WMP7XBZNS7KTW4Y)

重定向后

![Image for post](https://raw.githubusercontent.com/eatcosmos/assets/master/1*dlr6Z0toJSxlN02yytM-eQ.png?token=AD5MVODGDDQVMGGLLHZ46JK7KTW5K)

## 小结

这篇文章很长，不得不佩服作者探索的耐心，因为我跟着操作都快没耐心了，所以学技术要的其实是耐心，似乎也没其他窍门了。