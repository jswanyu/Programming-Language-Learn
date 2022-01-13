# python常用内置函数
下图是python的内置函数：

[![python-functions.png](https://z3.ax1x.com/2021/09/12/49WCc9.png)](https://imgtu.com/i/49WCc9)

在此只列举常用的，不常见的可以参考菜鸟教程

***
## all()函数
all() 函数用于判断给定的可迭代参数 iterable 中的所有元素是否都为 TRUE，如果是返回 True，否则返回 False

**空元组、空列表返回值为True，这里要特别注意**

any() 函数用于判断给定的可迭代参数 iterable 是否全部为 False，则返回 False，如果有一个为 True，则返回 True。


```python
print(all(['a', 'b', 'c', 'd']))
print(all(['a', 'b', '', 'd']))
print(all(('a', 'b', '', 'd')))
print(all((0, 1, 2, 3)))
print(all([]))
print(all(()))
```

    True
    False
    False
    False
    True
    True


***
## enumerate() 函数
`enumerate(sequence, [start=0])`,其中sequence为序列、迭代器或其他支持迭代对象，start是下标起始位置，默认为0  
返回值：返回 enumerate(枚举) 对象，即同时列出数据和数据下标。这个返回值是一个**迭代器**，可以用list()转为列表  

enumerate() 函数一般用在 for 循环当中，将一个可遍历的数据对象(如列表、元组或字符串)组合为一个索引序列，同时列出数据和数据下标


```python
from collections.abc import Iterator
seasons = ['Spring', 'Summer', 'Fall', 'Winter']
print(enumerate(seasons))
print(isinstance(enumerate(seasons), Iterator))  # 判断是否为迭代器
print(list(enumerate(seasons)))                  # list()将迭代器转为列表
for i, element in enumerate(seasons,start=1):    # 这是常用的用法，下标自己定义
    print(i, element)
```

    <enumerate object at 0x00000225102F22D0>
    True
    [(0, 'Spring'), (1, 'Summer'), (2, 'Fall'), (3, 'Winter')]
    
    1 Spring
    2 Summer
    3 Fall
    4 Winter


***
## help()函数
help() 函数可以打印输出一个函数的说明文档


```python
help(print)
```

    Help on built-in function print in module builtins:
    
    print(...)
        print(value, ..., sep=' ', end='\n', file=sys.stdout, flush=False)
        
        Prints the values to a stream, or to sys.stdout by default.
        Optional keyword arguments:
        file:  a file-like object (stream); defaults to the current sys.stdout.
        sep:   string inserted between values, default a space.
        end:   string appended after the last value, default a newline.
        flush: whether to forcibly flush the stream.


​    

***
## isinstance()函数：  
`isinstance(object, classinfo)` 其中object -- 实例对象，classinfo -- 可以是直接或间接类名、基本类型或者由它们组成的元组  
返回值：如果对象的类型与参数二的类型（classinfo）相同则返回 True，否则返回 False  

**isinstance() 与 type() 区别：**  
type() 不会认为子类是一种父类类型，不考虑继承关系。type用于求一个未知数据类型对象。  
isinstance() 会认为子类是一种父类类型，考虑继承关系。isinstance用于判断一个对象是否是已知数据类型。  

**python基本数据类型：**  
int，float，bool，complex，**str**(字符串)，list，**dict**(字典)，set，tuple


```python
a = 2
print(isinstance (a,int))
print(isinstance (a,(str,int,list))) # 是元组中的一个也返回 True
```

    True
    True



```python
class A:
    pass
 
class B(A): # A是B的父类
    pass
  
print(isinstance(A(), A))    # 实例A是否是A类的类型
print(type(A()))             # 实例A的类型是什么
print(isinstance(B(), A))    # 实例B是否是A类的类型
print(type(B()) == A)        # type不考虑继承
```

    True
    <class '__main__.A'>
    True
    False


***
## reversed()函数
`reversed(seq)`,seq -- 要转换的序列，可以是 tuple, string, list 或 range。返回一个反转的**迭代器**  

**如果没有实例化(即未使用list函数或for循环)，它并不会产生一个倒序的序列**


```python
# 字符串
seqString = 'Runoob'
print(list(reversed(seqString)))
 
# 元组
seqTuple = ('R', 'u', 'n', 'o', 'o', 'b')
print(list(reversed(seqTuple)))
 
# range
seqRange = range(5, 9)
print(list(reversed(seqRange)))
 
# 列表
seqList = [1, 2, 4, 3, 5]
print(list(reversed(seqList)))
```

    ['b', 'o', 'o', 'n', 'u', 'R']
    ['b', 'o', 'o', 'n', 'u', 'R']
    [8, 7, 6, 5]
    [5, 3, 4, 2, 1]


***
## sorted()函数
`sorted(iterable, key=None, reverse=False)`，其中iterable--**可迭代对象**，reverse--排序规则，reverse = True 降序，reverse = False 升序（默认）  

**key指定的函数将作用于list的每一个元素上，并根据key函数返回的结果进行排序**。返回值为重新排序的列表

* sort 与 sorted 区别：sort 是应用在 list 上的方法，sorted 可以对所有可迭代的对象进行排序操作。list 的 sort 方法返回的是对已经存在的列表进行操作，无返回值，而内建函数 sorted 方法返回的是一个新的 list，而不是在原来的基础上进行的操作。


```python
print(sorted([36, 5, -12, 9, -21]))  # 默认按照数值大小升序排列
print(sorted([36, 5, -12, 9, -21], key=abs))  # 按照指定的abs值进行升序排列
print(sorted([36, 5, -12, 9, -21], key=abs,reverse = True)) # 按照指定的abs值进行降序排列
print(sorted([36, 5, -12, 9, -21], key=lambda x:x*(-1)))  # 按照指定的函数进行升序排列
```

    [-21, -12, 5, 9, 36]
    [5, 9, -12, -21, 36]
    [36, -21, -12, 9, 5]
    [36, 9, 5, -12, -21]



```python
print(sorted(['bob', 'about', 'Zoo', 'Credit'])) # 字符串默认按照ASCLL大大小升序排列，'Z'<'a'
print(sorted(['bob', 'about', 'Zoo', 'Credit'],key=str.lower))  # 按照指定的函数，即字符串小写字母ASCLL升序排列
print(sorted(['bob', 'about', 'Zoo', 'Credit'],key=len))  # 按照字符串长度升序排列
```

    ['Credit', 'Zoo', 'about', 'bob']
    ['about', 'bob', 'Credit', 'Zoo']
    ['bob', 'Zoo', 'about', 'Credit']


字典排序一般指定按照哪个键来排序，也会涉及到多列排序


```python
array = [{"age":20,"name":"a"},{"age":25,"name":"b"},{"age":10,"name":"c"}]
print(sorted(array,key=lambda x:x["age"]))   # 按照字典中的'age'升序排列
```

    [{'age': 10, 'name': 'c'}, {'age': 20, 'name': 'a'}, {'age': 25, 'name': 'b'}]



```python
d1 = [{'name':'alice', 'score':38}, {'name':'bob', 'score':18}, {'name':'darl', 'score':28}, {'name':'christ', 'score':28}]
l = sorted(d1, key=lambda x:(-x['score'], x['name']))  # 先按照成绩降序排序，相同成绩的按照名字升序排序：
print(l)
```

    [{'name': 'alice', 'score': 38}, {'name': 'christ', 'score': 28}, {'name': 'darl', 'score': 28}, {'name': 'bob', 'score': 18}]



## reduce函数

* reduce()函数接收两个参数，一个是函数，一个是Iterable，reduce将传入的函数依次对序列的元素做**累积计算**
* 注意体会map和reduce的区别


```python
from functools import reduce
def add(x, y):
     return x + y  
reduce(add, [1, 3, 5, 7, 9])  # 1+3+5+7+9
```




    25



***
## map()函数
* map()函数根据提供的函数对指定序列做映射，其接收两个参数，一个是函数，一个是Iterable，map将传入的函数依次作用到序列的**每个元素**，并把结果作为新的Iterator返回
* map函数能够清楚的看出程序需要对可迭代对象做出怎样的函数操作


```python
from collections.abc import Iterable,Iterator
def f(x):
    return x*x
a = map(f,[1, 2, 3, 4, 5, 6, 7, 8, 9])  # 第一个参数是函数，另一个是可迭代的列表，返回的是一个迭代器
print(isinstance(a,Iterator))
list(a)   # map返回的是一个惰性序列，要用list将其都计算出来
```

    True





    [1, 4, 9, 16, 25, 36, 49, 64, 81]




```python
list(map(str, [1, 2, 3, 4, 5, 6, 7, 8, 9]))   # 将数字转为字符
```




    ['1', '2', '3', '4', '5', '6', '7', '8', '9']



***
## filter()函数
* Python内建的filter()函数用于过滤序列
* filter()函数接收两个参数，一个是函数，一个是Iterable，filter()把传入的函数依次作用于每个元素，然后根据返回值是True还是False决定保留还是丢弃该元素
* filter()函数返回的是一个Iterator，也就是一个惰性序列，所以要强迫filter()完成计算结果，需要用list()函数获得所有结果并返回list


```python
def is_odd(x):  # 保留奇数
    return x%2 == 1
list(filter(is_odd,[1, 2, 4, 5, 6, 9, 10, 15]))
```




    [1, 5, 9, 15]



***
## zip()函数
`zip([iterable, ...])`,其中iterabl为一个或多个可迭代对象。返回一个迭代器  

zip() 函数用于将可迭代的对象作为参数，将对象中对应的元素打包成一个个元组，然后返回由这些元组组成的对象，这样做的好处是节约了不少的内存  

如果各个可迭代对象的元素个数不一致，则返回列表长度与最短的对象相同  

利用` * `号操作符，可以将元组解压为列表，注意符号是放置在括号内的  


```python
from collections.abc import Iterable,Iterator

a = [1,2,3]
b = [4,5,6]
c = [7,8]
zipped = zip(a,b)
print(zipped)
print(isinstance(zipped, Iterator)) 
print(list(zipped))     
print(list(zip(a,c)))  # 元素个数与最短的列表一致
```

    <zip object at 0x00000146B7F8CD88>
    True
    [(1, 4), (2, 5), (3, 6)]
    [(1, 7), (2, 8)]


**zip()函数常用的场景是同时遍历多个序列**，有时也可以和enumerate()函数一起使用


```python
a = [1,2,3]
b = [4,5,6]
for x,y in zip(a,b):  # 同时遍历a,b序列
    print(x,y)
```

    1 4
    2 5
    3 6



```python
list1 = ['Spring', 'Summer', 'Fall', 'Winter']
list2 = ['bob', 'about', 'Zoo', 'Credit']
for i,(x,y) in enumerate(zip(list1,list2)):   # 和enumerate()函数一起使用
    print('%d:%s,%s' % (i,x,y))
```

    0:Spring,bob
    1:Summer,about
    2:Fall,Zoo
    3:Winter,Credit



```python
a,b = zip(*[(1, 4), (2, 5), (3, 6)]) # 与 zip 相反，zip(*) 可理解为解压，返回二维矩阵式
a,b  # 有一种将行列颠倒的感觉
```




    ((1, 2, 3), (4, 5, 6))



通过zip函数配对两个序列生成字典


```python
key_list = [1,2,3,4,5]
value_list = ['bob', 'about', 'Zoo', 'Credit','Alex']
dict1 = dict(zip(key_list,value_list))
dict1
```




    {1: 'bob', 2: 'about', 3: 'Zoo', 4: 'Credit', 5: 'Alex'}



***
## str()函数与repr()函数  
str() 函数将对象转化为适于人阅读的形式  
repr() 函数将对象转化为供解释器读取的形式  
二者均是将任意的值转化为字符串，但函数str( )将其转化成为适于人阅读的前端样式文本，而repr(object)就是原本未处理的用于编译器阅读的后台底层代码


```python
a = 'hello, world!\n'
print(str(a))
print(repr(a))
```

    hello, world!
    
    'hello, world!\n'


***
## range()函数
range() 函数返回的是一个可迭代对象（类型是对象），而不是列表类型，转为列表需要用list函数  

语法：`range(stop)`或`range(start, stop[, step])`
* start: 计数从 start 开始。默认是从 0 开始
* stop: 计数到 stop 结束，但**不包括 stop**
* step：步长，默认为1  

**加了step，就得有start，否则不生成数据**


```python
list(range(5))
```




    [0, 1, 2, 3, 4]




```python
list(range(0, 30, 5))
```




    [0, 5, 10, 15, 20, 25]




```python
list(range(10, 2))  # 得不到想要的列表
```




    []



***
## slice函数
slice() 函数实现切片对象，主要用在切片操作函数里的参数传递  

    class slice(stop)
    class slice(start, stop[, step])

* start -- 起始位置
* stop -- 结束位置
* step -- 间距  

返回一个切片对象，返回序列中对应位置的元素


```python
myslice1 = slice(5)    # 设置截取5个元素的切片
print(myslice1)
arr = list(range(10))
print(arr)
print(arr[myslice1])
```

    slice(None, 5, None)
    [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
    [0, 1, 2, 3, 4]



```python
myslice2 = slice(1,20,2)    # 设置截取5个元素的切片
print(myslice2)
arr = list(range(20,40))
print(arr[myslice2])
```

    slice(1, 20, 2)
    [21, 23, 25, 27, 29, 31, 33, 35, 37, 39]

