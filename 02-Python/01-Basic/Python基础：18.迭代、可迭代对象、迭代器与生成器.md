# 一、迭代
通过for循环来遍历list、tuple或char，称为迭代


```python
list1 = [1,2,3,4,5,6,7,8,9]
tuple1 = (1,2,3,4,5,6)
char1 = 'python'
for i in list1[0:2]:
    print(i)
for i in tuple1[0:2]:
    print(i)
for i in char1[0:2]:
    print(i)
```

>1
>2
>1
>2
>p
>y


dict默认是以key进行迭代。如果要迭代value，可以用`for value in dict.values()`。同时迭代key和value，可以用`for k, v in d.items()`


```python
dict1 = {1:'python',
        2:'java',
        3:'c++'}
for key in dict1:   # 不一定非要用key这个变量，一种规范
    print(key)
for value in dict1.values():  # 注意values和（）易写漏
    print(value)
for k,v in dict1.items():
    print(k,v)
```

>1
>2
>3
>python
>java
>c++
>1 python
>2 java
>3 c++


# 二、可迭代对象(Iterable)
能用for循环进行迭代的对象就是可迭代对象  
判断一个对象是可迭代对象的方法：通过`collections.abc`模块的`Iterable`类型判断  
可迭代的对象:`str,list,tuple,dict,set,文件对象`


```python
from collections.abc import Iterable
print(isinstance('abc', Iterable)) # str是否可迭代
print(isinstance(123, Iterable))   # 整数是否可迭代
```

>True
>False


如果要给list加下标,可以用Python内置的enumerate函数可以把一个list变成索引-元素对，这样就可以在for循环中同时迭代索引和元素本身


```python
for i, value in enumerate(['A', 'B', 'C']):
     print(i, value)
```

>0 A
>1 B
>2 C


可迭代对象有内置`iter()`函数，可以将可迭代对象转为迭代器

# 三、迭代器(Iterator)
迭代器指的是迭代取值的工具，每次调用迭代器会返回自身的下一个元素，它表示的是一个数据流，可以看做是一个有序序列，但不能提前知道序列长度，只有在需要返回下一个数据时它才会计算。所以Iterator的计算是惰性的，和列表等集合数据类型不同  

迭代器必须有两个基本的方法：iter() 和 next()  
`iter()`将可迭代对象转变为迭代器  
`next()`可以输出迭代器的下一个元素  

从类角度讲，任何实现了__iter__()和__next__()方法的对象都是迭代器  
为了防止出现无限循环的情况，`StopIteration `异常用于标识迭代的完成，没有数据时抛出StopIteration异常,这并不是错误，只是提醒迭代完成  


```python
list1 = [1,2,3]
iter1 = iter(list1)  # 将列表转为迭代器
print(next(iter1))   # 输出迭代器下一个元素
print(next(iter1))
print(next(iter1))   
print(next(iter1))
```

>1
>2
>3



    StopIteration                             Traceback (most recent call last)
    
    <ipython-input-16-8b77bc1ee74a> in <module>()
          4 print(next(iter1))
          5 print(next(iter1))
    ----> 6 print(next(iter1))


    StopIteration: 



```python
list1 = [1,2,3]
iter1 = iter(list1)  # 将列表转为迭代器
for i in iter1:      # 也常用for循环遍历迭代器内容
    print(i)
```

>1
>2
>3


迭代器一定是可迭代对象，但可迭代对象不一定是迭代器，如list、dict、str等是Iterable但不是Iterator，需要经过iter()函数转换  
常用的列表生成式就是迭代器  
可以用`collections.abc`模块的`Iterator`类型判断一个对象是否是Iterator对象


```python
from collections.abc import Iterable,Iterator
list1 = [1,2,3]
iter1 = iter(list1)  # 将列表转为迭代器
print(isinstance(list1, Iterable))
print(isinstance(list1, Iterator))  # 列表不是迭代器
print(isinstance(iter1, Iterable))
print(isinstance(iter1, Iterator))
print(isinstance((x for x in range(10)), Iterator))  # 列表生成式是迭代器
```

>True
>False
>True
>True
>True



**for循环本质**就是基于迭代器：for 循环在处理这些数据前，会调用 __iter__() 方法，将这些数据转化为一个迭代器，然后调用迭代器的 __next__() 方法，并捕获StopIteration异常，也就实现了遍历完所有数据就会结束，并不会抛出这个异常  
所以**从for循环的角度**,但凡可以被for循环取值的对象就是可迭代对象  

迭代器的优缺点：  
优点：1、提供了一种通用不依赖索引的迭代取值方式；2、同一时刻在内存中只存在一个值,更节省内存  
缺点：1、取值不如按照索引的方式灵活,不能取指定的某一个值,只能往后取,不能往前取；2、无法预测迭代器的长度

# 四、生成器(Generator)
迭代器解决了特大序列占内存的问题，但当生成序列的算法较为复杂时，需要一个能更简介code的迭代器，而不是一直定义类，这就有了生成器(个人理解)  

在 Python 中，使用了`yield`的函数被称为生成器，**生成器本质上也是迭代器**

generator和函数的执行流程不一样。函数是顺序执行，遇到return语句或者最后一行函数语句就返回。而变成generator的函数，在每次调用next()的时候执行，**遇到yield语句返回**，再次执行时**从上次返回的yield语句处继续执行**

需要给生成器设置一个条件来退出循环，不然就会无限产生内容出来

所以，生成器仅仅保存了一套生成数值的算法，并且没有让这个算法现在就开始执行，而是什么时候调它，它什么时候开始计算一个新的值，并返回


```python
def fib(max):   # 斐波那契数列
    n, a, b = 0, 0, 1
    while n < max:
        yield b           # 遇到yield语句函数返回
        a, b = b, a + b   # 再次执行的时候从此执行
        n = n + 1
        
g = fib(6)
print(type(fib))   # fib是一个特殊函数，仍然是一个函数
print(type(g))     # 将该函数赋给一个对象，这个对象成了generator
for i in g:       # 迭代得到生成器元素
    print(i)       # 拿不到函数的return返回值
```

><class 'function'>
><class 'generator'>
>1
>1
>2
>3
>5
>8


值得注意的是，要想**得到函数的return返回值**，必须捕获`StopIteration`错误，返回值包含在StopIteration的value中。而若只使用for循环或者list()函数，python并不会抛出这个异常，所以需要使用while循环加上next()函数。因此，编写和应用生成器时，看是否需要返回值，来选择用for/list()还是next()


```python
def fib(max):   # 斐波那契数列
    n, a, b = 0, 0, 1
    while n < max:
        yield b           # 遇到yield语句函数返回
        a, b = b, a + b   # 再次执行的时候从此执行
        n = n + 1
    return 'done'
g = fib(6)
while True:
    try:
        print(next(g))
    except StopIteration as e:   # 捕获StopIteration错误
        print('Generator return value:', e.value) # 返回值在异常的value中
        break  # 不加break，便会一直打印返回值
```

>1
>1
>2
>3
>5
>8
>Generator return value: done


另外，只要把一个列表生成式的`[]`改成`()`，也能创建一个generator


```python
g = (x * x for x in range(10))
type(g)
```

> generator



**迭代器和生成器都常被用于使用list()函数来生成列表**  
在写接受任意序列类型(列表、元组、N维数组)，甚至是一个迭代器的函数时，可以先检查这个对象是否是列表，如果不是就将其转为列表


```python
from collections.abc import Iterable

x = (1,2,3)
if not isinstance(x,list) and isinstance(x,Iterable):
    x = list(x)
print(x)
```

> [1, 2, 3]

