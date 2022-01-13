# 一、数字
## 1.数字类型

* 整数：Python3 里只有一种整数类型 int，表示为长整型
* 浮点数：浮点数也就是小数，之所以称为浮点数，是因为按照科学记数法表示时，一个浮点数的小数点位置是可变的。如 1.23、3E-2
* 布尔型：True、False
* 复数


```python
a=2
b=3.0
c=True
print(type(a)) # type()函数打印类型
print(type(b))
print(type(c))
```

> <class 'int'>
> <class 'float'>
> <class 'bool'>



**复数** 
复数由实数部分和虚数部分构成，可以用a + bj,或者complex(a,b)表示， **复数的实部a和虚部b都是浮点型。** python 不支持复数转换为整数或浮点数

复数常见的用法有：
* a为复数，a.real为实数部分，a.imag虚数部分的实数
* complex(x) 将x转换到一个复数，实数部分为 x，虚数部分为 0
* complex(x, y) 将 x 和 y 转换到一个复数，实数部分为 x，虚数部分为 y，其中x 和 y 是数字表达式


```python
d=3+4j
e=complex(3,4)
print(type(d))
print(type(e))
print(d.real)
print(d.imag)
```

> <class 'complex'>
> <class 'complex'>
> 3.0
> 4.0



## 2.常见数值运算函数

常见数值运算函数见下表。其中`abs(x)`、`round(x)`是python内置函数，其他的函数需要调用math模块

[![numeric-function.png](https://z3.ax1x.com/2021/09/12/49RzhF.png)](https://imgtu.com/i/49RzhF)


```python
print(abs(-1.2))       
print(round(3.5))
print(round(4.567,2))  # round可以指定具体四舍五入到第几位小数点
```

> 1.2
> 4
> 4.57



```python
import math
print(math.ceil(4.5))   # ceil天花板
print(math.exp(1))
print(math.fabs(-10))   # 方便统一记忆的话，不如记math.fabs
print(math.floor(4.5))  # floor地板
print(math.log(math.e))  
print(math.log(100,10))
print(math.log10(100))
print(math.modf(4.5))
print(math.pow(3,2))
print(math.sqrt(9))
print(min(3,4,5))
print(min([3,4,5]))     # min和max参数可以是列表或具体的数
```

> 5
> 2.718281828459045
> 10.0
> 4
> 1.0
> 2.0
> 2.0
> (0.5, 4.0)
> 9.0
> 3.0
> 3
> 3



**其中有关round()函数的注意点：**

* round函数在取整浮点数时，遵循的原则是：`4舍6入5看齐,奇进偶不进`
* 待进位是5的时候，还需要看待进位的前一位，前一位是奇数才进一，偶数不进。如果还有小数点第二位或更多位，则不管奇偶，大于0.5都是进位的
* 但到了保留小数点后多位时，又会出问题，这和python自身的二进制浮点数精度有关，所以精度要求高的时候不要用round函数，用decimal模块，用到时候再学


```python
print(round(3.4))  #小于等于4舍
print(round(3.6))  #大于等于6入
print(round(3.5))  #奇进
print(round(4.5))  #偶不进
print(round(4.51)) #有多余位，则进
print(round(4.565,2))  #奇进
print(round(4.675,2))  #偶不进
```

>3
>4
>4
>4
>5
>4.57
>4.67



## 3.数学常数

math模块中有e和pi这两个数学常数


```python
print(math.e)
print(math.pi)
```

    2.718281828459045
    3.141592653589793



## 4.三角函数

pi涉及到三角函数的函数调用，用到查看即可，使用频率较低

[![trigonometric-function.png](https://z3.ax1x.com/2021/09/12/49WMjA.png)](https://imgtu.com/i/49WMjA)


```python
import math
print(math.cos(math.pi/3))
```

> 0.5000000000000001



## 5.科学计数法

科学计数法：在e的前后加上主数字和10的次幂。注意**此处的e与自然底数无关**，不要混淆


```python
print(1.52e4)
print(1.52e-2)
print(15.23e2)
```

> 15200.0
> 0.0152
> 1523.0



# 二、运算符

python有多种运算符：算术运算符、比较运算符、赋值运算符、位运算符、逻辑运算符、成员运算符、身份运算符等

## 1.算术运算符
`/`除    `//`取整    `%`取余  这三个切忌弄混

`**`幂  

它们的运算顺序与数学相同，括号，幂运算，乘除，加减


```python
a=32
b=10
print(a+b)
print(a-b)
print(a*b)
print(a/b)
print(a%b)
print(a//b)

a=2
b=3
print(a**b)
```

>42
>22
>320
>3.2
>2
>3
>8



补充：函数`divmod()`以元组形式返回除法的商和余数


```python
divmod(7,2)
```

> (3, 1)



`//`取整结果**不一定是整型**，这与分母分子的数据类型有关，不同类型的数混合运算时会将整数转换为浮点数

`//`取整是向下取接近商的整数，**负数运算时容易搞错**


```python
print(7.0//2)
print(7//2)
print(-9//4)
```

> 3.0
> 3
> -3



整数和浮点数在计算机内部存储的方式是不同的，**整数运算永远是精确的（包括除法）**，而浮点数运算则可能会有四舍五入的误差

之所以整数的除法运算永远是精确的，因为其包括两种，一种是`/`，一种是`//`，**`/`除法计算结果是浮点数，即使是两个整数恰好整除，结果也是浮点数**，还有一种除法是`//`，两个整数的取整自然是整数，因此这两种除法都是精确的





## 2.比较运算符

==等于   !=不等于      >大于     <小于      >=大于等于      <=小于等于


```python
a=32
b=10

if a==b:
    print('yes')
else:
    print('no')
```

> no



## 3.赋值运算符

赋值运算符：`=`

增量运算符：如`+=` 加法赋值运算符 `b+=a `等效于 `b=a+b`，其他的还有`-=` `*=` `/=` `**=`


```python
a=31
b=20
b+=a
b
```

> 51



**Python中没有`++`和`--`的操作**！

python中的数字类型是不可变数据，也就是数字类型数据在内存中不会发生改变，当变量值发生改变时，会新申请一块内存赋值为新值，然后将变量指向新的内存地址
`a = 10; a += 1`
两次id(a)是不同的

`+=`是改变变量，相当于重新生成一个变量，把操作后的结果赋予这个新生成的变量
`++`是改变了对象本身，而不是变量本身，即改变数据地址所指向的内存中的内容，python限制了这样的做法

## 4.位运算符
按位运算：与& 或| 非~ 异或^ 左移<< 右移>>


```python
a=60   # 0011 1100
b=13   # 0000 1101
c=a & b  
c      # 0000 1100
```

> 12



### 进制
上面涉及到了位运算符的知识点，**补充下python的进制问题**
* Python默认变量为十进制，不要对变量进行二进制赋值
* 2 进制是以 `0b` 开头；8 进制是以` 0o `开头的；16 进制是以 `0x` 开头的，给变量赋值为这些进制前，需要加上这些开头的符号
* 但给变量赋值为这几种进制，python默认输出仍然是十进制。因此需要以其他进制输出，还要用**bin()，oct()，hex()**这三个函数，对应二、八、十六进制


```python
a=0b111100
print(a)
print(bin(a))
```

>60
>0b111100



## 5.逻辑运算符

逻辑的与或非：and/or/not。**注意逻辑运算符跟位运算符与或非是不同的**
* and ：从左到右计算表达式，若存在假，返回第一个假值，若所有值均为真，则返回最后一个值
* or  ：从左到有计算表达式，若存在真，返回第一个真值，若所有值均为假，则返回最后一个值
* not ：如果表达式为 True，返回 False 。如果表达式为 False，它返回 True
* 优先级：not > and > or


```python
a=60
b=13
c=666
d=0
e=0.0
f=None
print(a and b and c) # 都为真值，返回最后一个值
print(a and d and b) # d为假值，返回d
```

>666
>0



```python
print(f or e or d) # 都为假值，返回最后一个值
print(f or a or d) # a为真值，返回a
```

>0
>60



```python
print(not a)
print(not d)
```

>False
>True



## 6.成员运算符

in，表示在指定的序列中

not in ,表示不在指定的序列中


```python
a=1
b=2
c=[1,3,1,8,9,10]

a in c
```

> True




```python
b in c
```

> False



## 7.身份运算符
is  判断两个标识符是否引用自一个对象

is not 判断两个标识符是否不是引用自一个对象

**is和==是有区别的，is用于判断两个变量是否为同一个对象，==用于判断引用变量的值是否相等**


```python
a = 20
b = 20
print(id(a))
print(id(b))
if(a is b):
    print('a和b有相同的标识')
```

>140703924103568
>140703924103568
>a和b有相同的标识



补充一个额外知识点：

在交互式环境中，编译器会有一个**小整数池**的概念，会把[-5，256]间的**整数**预先创建好。在这个区间内的整数对象分配了相同的内存地址，超过这个范围的时候，地址就会不同

还会有部分仅包含数字、字母和下划线的字面常量字符串也会被驻留，即分配同一内存空间，用到再说


```python
x1 = 3
y1 = 3
print(x1 is y1) # 整数[-5，256]
print(id(x1))
print(id(y1))

x2 = 257
y2 = 257
print(x2 is y2) # 超过区间不符合
print(id(x2))
print(id(y2))

x3 = 3.5
y3 = 3.5
print(x3 is y3)  # 小数不符合
print(id(x3))
print(id(y3))

x4 = 3.5
y4 = x4
print(x4 is y4)  # 将y指向与x相同的地方，所以x与y相同
```

>True
>140703924103024
>140703924103024
>False
>1813601033264
>1813601033392
>False
>1813601031472
>1813601032944
>True



## 8.运算优先级

[![operator-precedence.png](https://z3.ax1x.com/2021/09/12/49W91J.png)](https://imgtu.com/i/49W91J)
