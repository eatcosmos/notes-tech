[AI训练营Python\-阿里云天池](https://tianchi.aliyun.com/specials/promotion/aicamppython)

## 注释

这有啥好讲的，不过我喜欢python # 注释，双斜杠有点不对称总感觉，对了python的''' '''和""" """注释也很对称好看。

## 运算符

这应该也没什么好讲的

**算术运算符**

print(3 // 4) # 0 因为3//4=0.7然后向下取整，比0.7小的整数就是0了

print(3 % 4) # 3 因为3里面有0个4，3-0*4=3，就是取余数

**比较运算符**

可以用 >=的形式

**逻辑运算符**

print((3 > 2) and (3 < 5))

**位运算符**

bin(表示转化为二进制输出0b110格式)

~按位取反 ^按位异或 比如4(0b100) ^5(0b101)得到0b001，就是相同取0，不同取1

print(用逗号分隔需要打印的)

**三元运算符**

主要目的是为了更易阅读，也符合日常的表达

x, y = 4, 5

small = x if x < y else y

print(small) # 4

**其他运算符** 这个还是比较有用的，其他语言里好像没有

存在 in 对应一个列表集合之类，是is对应一个变量，字符要加上单引号，字符串要加上双引号

### is is not比较的是两个变量的内存地址

好比身份验证必须是本人，变量比较的话就是比较存放位置，相当于本体了

== !=比较的变量存放的值

## 3. 变量和赋值

- Python变量名是大小写敏感的 foo != Foo
- 在使用之前比如print()，需要对其先赋值

## 4.数据类型与转换

Python 里面万物皆对象（object），整型也不例外，只要是对象，就有相应的属性 （attributes） 和方法（methods）。

b = dir(int) # 获取属性的方法，其实就是属性的列表变量

比如int有内置的方法 bit_length是计算二进制形式位数的

import decimal

from decimal import Decimal

a = decimal.getcontext() # 获取参数

Decimal对象的默认精度是28位

布尔型

True False

1       0

## 5.pring()函数

print(*objects, sep=' ', end='\n', file=sys.stdout, flush=False)

print(item, 'another string', sep='&')

# 位运算

## 1. 原码、反码和补码

按位取反

~ 1 = 0

~ 0 = 1

