# 一、数据类型

Python3 中有六个标准的数据类型：
* Number（数字）
* String（字符串）
* List（列表）
* Tuple（元组）
* Set（集合）
* Dictionary（字典）



## 1.按照数据类型是否可变分为：

**不可变数据（3 个）**：Number（数字）、String（字符串）、Tuple（元组）

**可变数据（3 个）**：List（列表）、Dictionary（字典）、Set（集合）

为什么要设计str、None这样的不变对象呢？因为不变对象一旦创建，对象内部的数据就不能修改，这样就减少了由于修改数据导致的错误。此外，由于对象不变，多任务环境下同时读取对象不需要加锁，同时读一点问题都没有。我们在编写程序时，如果可以设计一个不变对象，那就尽量设计成不变对象



## 2.按照数据类型是否属于sequence（序列）

**序列** ：string、list 和 tuple String（字符串）、List（列表）、Tuple（元组）

**非序列** ： Number（数字）、Dictionary（字典）、Set（集合）



## 3. 其他数据类型

### 3.1 空值
空值是Python里一个特殊的数据类型，用None表示。**None不能理解为0**，因为0是有意义的，而None是一个特殊的空值，**None也是不可变的**



### 3.2 bool布尔型

Ture:非0值

False: `' '`、`" "`、`''' '''`、`""" """`、`0`、`( )`、`[ ]`、`{ }`、`None`、`0.0`、`0L`、`0.0+0.0j`

[![True-and-False.png](https://z3.ax1x.com/2021/09/12/49WlnI.png)](https://imgtu.com/i/49WlnI)





## 二、变量

Python 中的变量不需要声明。每个变量在使用前都必须赋值，变量赋值以后该变量才会被创建。


### 1.理解动态语言
在 Python 中，变量就是变量，它没有类型，我们所说的"类型"是变量所指的内存中对象的类型。可以把任意数据类型赋值给变量，同一个变量可以反复赋值，而且可以是**不同类型**的变量。这种变量本身类型不固定的语言称之为**动态语言**，与之对应的是**静态语言**。静态语言在定义变量时必须指定变量类型，如果赋值的时候类型不匹配，就会报错



### 2.变量命名 

变量名必须是**大小写英文、数字和_的组合，且不能用数字开头**。需要注意：**变量命名不要和python保留字相同**

遇到多个单词组合命名常使用两种命名方式：(一个文件中尽量只使用一种命名方式)
* 驼峰式：即前一个单词小写，后一个单词首字母大写。如：stuName
* 下划线式：单词之间用下划线连接。如：stu_name


```python
a = 123
a = 'abc'
a
```

> 'abc'




```python
import keyword # python保留字
keyword.kwlist
```

> ['False',
>  'None',
>  'True',
>  'and',
>  'as',
>  'assert',
>  'async',
>  'await',
>  'break',
>  'class',
>  'continue',
>  'def',
>  'del',
>  'elif',
>  'else',
>  'except',
>  'finally',
>  'for',
>  'from',
>  'global',
>  'if',
>  'import',
>  'in',
>  'is',
>  'lambda',
>  'nonlocal',
>  'not',
>  'or',
>  'pass',
>  'raise',
>  'return',
>  'try',
>  'while',
>  'with',
>  'yield']



### 3.多个变量赋值

Python允许同时为多个变量赋相同的值。

也允许同时为多个变量赋不同的值，其本质为：元组打包和解包。
* temp = 2,3  元组打包
* x,y = temp  序列解包


```python
a = b = c = 1
```


```python
x,y = 2,3
print(x,y)
```

> 2 3




### 4.常量

所谓常量就是不能变的变量，比如常用的数学常数π就是一个常量。在Python中，通常用全部大写的变量名表示常量。但事实上PI仍然是一个变量，Python根本没有任何机制保证PI不会被改变，所以，用全部大写的变量名表示常量只是一个习惯上的用法，如果你一定要改变变量PI的值，也没人能拦住你


```python
import math
PI = math.pi
print(PI)
```

> 3.141592653589793



### 5.变量类型转换

有时候，我们需要对数据内置的类型进行转换，数据类型的转换，只需要将数据类型作为函数名即可。

以下几个内置的函数可以执行数据类型之间的转换。这些函数返回一个新的对象，表示转换的值。

[![datatype-conversion.png](https://z3.ax1x.com/2021/09/12/49R4k8.png)](https://imgtu.com/i/49R4k8)


```python
a=1
b=1.0
c='123'
f=True
g=0.0
print(int(b))
print(float(a))
print(float(c))
print(str(a))
print(int(f))
print(bool(a))
print(bool(g))
```

> 1
> 1.0
> 123.0
> 1
> 1
> True
> False



需要注意的是**不能将非数字的字符型转为整型、浮点型**

```python
d='abc@'
print(int(d)) #报错
```

字符串内容为浮点型要转换为整型，需要先用浮点型转换，再转换为整型
```python
a = '1.1'
int(a)   #报错
```



# 三、对象

python中，对象才有类型，变量是没有类型的。变量仅仅是对象的引用(指针)


可以把任意数据类型赋值给变量，同一个变量可以反复赋值，而且可以是不同类型的变量。下面这段代码`[1,2,3]`是list类型，`'python'`是string类型，变量a没有类型  
```python
a=[1,2,3]
a='python'
```

理解变量和对象的关系在计算机内存中非常重要。

1.当我们写：a = 'ABC'时，Python解释器干了两件事情：  

 * 1、在内存中创建了一个'ABC'的字符串；
 * 2、在内存中创建了一个名为a的变量，并把它指向'ABC'


2.下面这段代码，对应着变量和对象的关系
```python
a = 'ABC'
b = a
a = 'XYZ'
print(b)
```


[![variables-objects.png](https://z3.ax1x.com/2021/09/12/49W3HP.png)](https://imgtu.com/i/49W3HP)


3.相对应的，如果只是改变a本来指向的对象，b也还是指向它。下面的代码输出`[1, 2, 3, 4]`
```python
a = [1,2,3]
b = a
a.append(4)  # a变量还是指向a
print(b)
```

4.不可变和可变类型对象的理解
* 不可变类型：变量赋值 a=5 后再赋值 a=10，这里实际是新生成一个 int 值对象 10，再让 a 指向它，而 5 被丢弃，不是改变a的值，相当于新生成了a
* 可变类型：变量赋值 la=[1,2,3,4] 后再赋值 la[2]=5 则是将 list la 的第三个元素值更改，本身la没有动，只是其内部的一部分值被修改了

