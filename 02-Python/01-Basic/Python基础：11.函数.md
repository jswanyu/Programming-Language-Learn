# 函数
## 一、函数的定义
函数代码块以 `def` 关键词开头，后接函数标识符名称和圆括号 ()。任何传入参数和自变量必须放在圆括号中间，圆括号之间可以用于定义参数。函数内容以冒号起始，并且缩进。return [表达式] 结束函数，选择性地返回一个值给调用方。不带表达式的return相当于返回 None

函数的命名需要规范，且不能和python内置函数重名，具体的可以去查看内置函数部分内容。


```python
def add(a,b):
    return a+b
add(2,3)
```

> 5



函数的第一行语句可以选择性地使用文档字符串—用于存放函数说明。可以通过`函数名.__doc__`的方法或者`help()`函数来查看


```python
def add(a,b):
    '''返回两个参数的和'''
    return a+b
print(add.__doc__)
print(help(add))
```

>返回两个参数的和
>Help on function add in module __main__:
>
>add(a, b)
>
>返回两个参数的和
>
>None



## 二、函数参数

在学习具体的函数参数前，需要学习参数的传递，实参形参等简单概念略过，主要是讨论不可变类型和可变类型传参的区别

### 1.参数传递

* 不可变类型：类似 c++ 的值传递，如 整数、字符串、元组。如fun（a），传递的只是a的值，没有影响a对象本身。比如在 fun（a）内部修改 a 的值，只是修改另一个复制的对象，不会影响 a 本身。
* 可变类型：类似 c++ 的引用传递，如 列表，字典。如 fun（la），则是将 la 真正的传过去，修改后fun外部的la也会受影响


```python
def change(a):
    # 传递不可变类型参数
    a=1
    
    
def changelist(la):
    # 传递可变类型参数
    la.append([1,2,3])
    
b=2
change(b) # 试图改变不可变类型的对象
print(b)

l1=[7,8,9]
changelist(l1)  # 改变可变类型的对象
print(l1)
```

>2
>[7, 8, 9, [1, 2, 3]]



### 2.必需参数(位置参数)

必需参数(位置参数)须以正确的顺序传入函数。调用时的数量必须和声明时的一样，不然会出现语法错误


```python
def add(a,b):
    return a+b
add(2,3)
```

> 5



### 3.关键字参数
使用关键字参数允许函数调用时参数的顺序与声明时不一致，因为 Python 解释器能够用参数名匹配参数值


```python
def subtract(a,b):
    return a-b
subtract(b=2,a=3)
```

> 1



###  4.默认参数
定义函数时可以给出参数的默认值。调用函数时，如果没有传递参数，则会使用默认参数。

需要注意的是：**默认参数要放在位置参数后面**，否则Python的解释器会报错。定义时尽量把变化大的参数放前面，变化小的参数作为默认参数放后面。

有多个默认参数时，调用的时候，可以按顺序提供默认参数或者用关键字参数


```python
def subtract(a,b=6):
    return a-b

def add1(a,b=1,c=2):
    return a+b+c

print(subtract(8))
print(add1(3,2,1)) #按位置改变默认参数
```

>2
>6



**默认参数必须指向不变对象！**

Python函数在定义的时候，默认参数的值就被计算出来了，因为默认参数也是一个变量，每次调用该函数，如果改变了默认参数的内容，则下次调用时，默认参数的内容就变了。因此否则没有意义


```python
def add_end(L=[]):  # 默认参数是个空列表
    L.append('END')
    return L
print(add_end())
print(add_end())   # 默认参数每次都发生变化，没有意义
```

>['END']
>['END', 'END']



```python
def add_end1(L=None):
    if L is None:
        L = []
    L.append('END')
    return L
print(add_end1())
print(add_end1())
print(add_end1([1,2,3]))
```

>['END']
>['END']
>[1, 2, 3, 'END']



### 5.不定长参数

有时可能需要一个函数能处理比当初声明时更多的参数。这些参数叫做不定长参数。

#### 5.1 参数带`*`
加了星号` * `的参数会以元组(tuple)的形式导入，存放所有未命名的变量参数。


```python
def add(a,*b):
    for i in b:  # 注意b此时是元组，元组与数值不能直接相加，不能返回a+b
        a+=i    
    return a
print(add(2,3,4))
print(add(2))    # *对应处不传参数也可以
```

>9
>2



**对已有的list或者tuple调用一个不定长参数的函数，总不能把列表中的值一个个输进去。此时可以在list或tuple前面加一个`*`号，把list或tuple的元素变成不定长参数传进去，这种方法很常用！！**


```python
def add(a,*b):
    for i in b:
        a+=i    
    return a
x = [1,2,3]
add(3,*x)
```

> 9



#### 5.2 参数带`**`
参数带两个星号 ** 会以字典的形式导入，可以扩展函数的参数和功能，用户的输入自带参数和值


```python
def person(name, age, **kw):
    print('name:', name, 'age:', age, 'other:', kw)
person('Bob', 35, city='Beijing')
```

> name: Bob age: 35 other: {'city': 'Beijing'}



**也可以先组装出一个dict，然后调用的时候，在dict加上两个*，把该dict参数传进去**


```python
extra = {'city': 'Beijing', 'job': 'Engineer'}
person('Jack', 24, **extra)
# **extra表示把extra这个dict的所有key-value用关键字参数传入到函数的**kw参数，kw将获得一个dict
# 注意kw获得的dict是extra的一份拷贝，对kw的改动不会影响到函数外的extra
```

> name: Jack age: 24 other: {'city': 'Beijing', 'job': 'Engineer'}



### `*`单独出现

声明函数时，参数中星号` * `可以单独出现，但` * `后的参数在调用时必须用关键字传入。这种结构用于，限制调用者可以传入的参数名，同时可以提供默认值。如果缺少`*`，Python解释器将无法识别位置参数和命名关键字参数


```python
def person(name, age, *, city, job): # 限制调用者可以传入的参数名只能为city和job   话说这个时候直接的位置传参不也可以吗，留下疑问
    print(name, age, city, job)
def person1(name, age, *, city='Beijing', job):  # 提供默认值
    print(name, age, city, job)    
person('Jack', 24, city='nanjing', job='Engineer')
person1('Bob', 35,  job='Engineer')
```

>Jack 24 nanjing Engineer
>Bob 35 Beijing Engineer


* 如果函数定义中已经有了一个可变参数，后面跟着的命名关键字参数就不再需要一个特殊分隔符*了


```python
def person(name, age, *args, city, job):
    print(name, age, args, city, job)
```



## 三、函数返回值

`return [表达式] `语句用于退出函数，选择性地向调用方返回一个表达式。不带参数值的return语句返回None  

**函数运行时碰到第一个return语句就立即返回，结束函数，通常用于选择性的执行某条return语句**


```python
def ex1(a,w=4):
    if len(a) < w:
        return a[:]
    return tuple(a)
a = [1,2,3]
b = [1,2,3,4,5,6]
print(ex1(a))
print(ex1(b))
```

>[1, 2, 3]
>(1, 2, 3, 4, 5, 6)



函数可以有多个返回值，中间用逗号隔开，其实是返回一个元组。

调用函数时可以将返回的值分别赋给多个变量或直接返回给一个变量(这一个变量会接收为元组形式)


```python
def add(a,b):
    return a+b,a-b
x,y=add(2,3)
z = add(2,3)
print(x,y)
print(z)
```

>5 -1
>(5, -1)


也可以以列表形式返回多个值，用的较少


```python
def add(a,b):
    return [a+b,a-b]
z=add(2,3)
x,y = z
print(z,x,y)
```

> [5, -1] 5 -1



## 四、函数的递归

一个函数在内部调用自身本身，这个函数就是递归函数


```python
def fact(n):
    if n==1:
        return 1
    return n * fact(n - 1)
fact(5)   # fact(3000)会栈溢出
```

> 120



使用递归函数需要注意防止栈溢出。在计算机中，函数调用是通过栈（stack）这种数据结构实现的，**每当进入一个函数调用，栈就会加一层栈帧，每当函数返回，栈就会减一层栈帧**。由于栈的大小不是无限的，所以，递归调用的次数过多，会导致栈溢出

解决递归调用栈溢出的方法是通过尾递归优化，尾递归是指，**在函数返回的时候，调用自身本身，并且，return语句不能包含其它表达式，只调用自身**。这样，编译器或者解释器就可以把尾递归做优化，使递归本身无论调用多少次，都只占用一个栈帧，不会出现栈溢出的情况

遗憾的是，大多数编程语言没有针对尾递归做优化，**Python解释器也没有做优化**，所以，即使把上面的fact(n)函数改成尾递归方式，也会导致栈溢出。


```python
def fact(n): # 尾递归优化
    return fact_iter(n, 1)

def fact_iter(num, product):
    if num == 1:
        return product
    return fact_iter(num - 1, num * product)
fact(5)   # fact(3000)会栈溢出
```

> 120




```python
def fact(n):
    if n==1:
        return 1
    elif n==0:
        return 1        
    else:
        s = n * (n-1)
        a = s * fact(n-2)
    return a
fact(5)   # fact(6000)会栈溢出，按照尾递归描述貌似也不属于尾递归，但是貌似能堆的栈更多
```

> 120



## 五、匿名函数
有些简单的函数不需要显式地定义，可以使用匿名函数，匿名函数不再使用 def 语句定义函数，不需要写出函数名（自身并没有一个显式的`__name__`属性），因此不必担心函数名冲突

匿名函数语法为` lambda arg1,arg2,...,argn:一个表达式`
* arg是函数的参数，可以有多个。可以设置默认值参数，调用时可以使用关键字参数
* 匿名函数只能有一个表达式，所以仅仅能在lambda表达式中封装有限的逻辑进去。匿名函数不用写return，返回值就是该表达式的结果


lambda 函数拥有自己的命名空间，且不能访问自己参数列表之外或全局命名空间里的参数

匿名函数也是一个函数对象，也可以把匿名函数赋值给一个变量，再利用变量来调用该函数

匿名函数优点是调用小函数时不占用栈内存从而增加运行效率


```python
sum = lambda arg1, arg2: arg1 + arg2   #  把匿名函数赋值给一个变量
sum(10,20)
```

> 30




```python
g= lambda x,y : x**2+y**2  
g(y=3,x=2)   # 关键字参数
```

> 13




```python
list(map(lambda x:x*x,[1,2,3,4,5,6,7,8,9]))  # 内置函数map的第一个参数是函数，现在可以直接用匿名函数
```

> [1, 4, 9, 16, 25, 36, 49, 64, 81]




```python
list(filter(lambda x:x%2==1,range(1,20)))    # 同理
```

> [1, 3, 5, 7, 9, 11, 13, 15, 17, 19]

