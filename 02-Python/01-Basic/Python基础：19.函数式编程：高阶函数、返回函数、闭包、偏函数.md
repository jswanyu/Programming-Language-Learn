# 一、高阶函数
**一个函数可以接收另一个函数作为参数，这种函数就称之为高阶函数**

首先在python中，变量可以指向函数，该变量可以来调用这个函数


```python
f = abs
f(-10)
```

> 10



所以abs这三个字母其实也只是一个变量而已，但其指向了绝对值函数，如果把abs重新赋值为另一个数字，就不能调用abs函数了。我们知道函数的参数可以接收变量，那么变量又可以指向函数，所以一个函数就可以接收另一个函数作为参数，这种函数就称之为``高阶函数``


```python
def add_abs(x,y,abs):    # 简单的高阶函数的例子
    return abs(x) + abs(y)
add_abs(-1,1,abs)
```

> 2



### python中有很多非常有用的高级函数
* map/reduce
* filter
* sorted  

放到python内置函数一节中去讲

# 二、返回函数
高阶函数可以把函数作为结果值返回  

**返回一个函数时，该函数未执行。**可以用于不需要立刻求解函数的场景


```python
def calc_sum(*args):  
    ax = 0
    for n in args:
        ax = ax + n
    return ax          
f = calc_sum(1,5,7,9)   # 调用这个函数，显然是立刻求和
print(f)
```

> 22



```python
def lazy_sum(*args): # 实现一个可变参数的求和
    def sum():       # 不需要立刻求和，而是在后面的代码中，根据需要再计算。可以不返回求和的结果，而是返回求和的函数
        ax = 0
        for i in args:
            ax += i
        return ax
    return sum       # 返回求和函数
f = lazy_sum(1,5,7,9) # 需要将其赋给一个变量,此时f这个变量就成了求和函数。此时，相关参数和变量都已经保存好了
print(f)              # 打印函数信息
print(f())            # 调用求和函数
```

><function lazy_sum.<locals>.sum at 0x000001F19D1E7620>
>22


**上一段代码需要注意的是，将返回函数付给一个变量时，该变量加括号才算调用了函数**


```python
f1 = lazy_sum(1, 3, 5, 7, 9)  
f2 = lazy_sum(1, 3, 5, 7, 9)
f1==f2  # 每次调用lazy_sum都会返回一个新的函数，即使传入相同的参数
```

> False



* 函数lazy_sum中又定义了函数sum，并且，内部函数sum可以引用外部函数lazy_sum的参数和局部变量
* 注意到上面第三段程序中，第一次引用lazy_sum返回了sum后，lazy_sum的参数和内部局部变量还可以被新函数引用，所以其又引用第二次，赋给f2
* 当lazy_sum返回函数sum时，相关参数和变量都保存在返回的函数中，这种方法称为``闭包``

## 闭包


```python
def count():
    fs = []
    for i in range(1, 4):
        def f():                # i在增加时，f并没有调用，即并没有进行1*1 2*2 3*3的计算
             return i*i
        fs.append(f)            # 向列表fs中添加三个函数，但并没有调用
    return fs

f1, f2, f3 = count()
print(f1())                     # 直到遇到f1() f2() f3() 才调用i*i，但此时i已经变成了3，所以结果都是9
print(f2())
print(f3())
```

>9
>9
>9



```python
def count():
    fs = []
    for i in range(1, 4):
        def f():                
             return i*i
        fs.append(f())          # 向列表fs中添加三个函数并调用
    return fs

f1, f2, f3 = count()
print(f1)                    # 直到遇到f1() f2() f3() 才调用i*i，但此时i已经变成了3，所以结果都是9
print(f2)
print(f3)
```

>1
>4
>9


**返回函数不要引用任何循环变量，或者后续会发生变化的变量,容易出错**

## 偏函数
* 不同于数学的偏函数
* 是调用functools.partial，作用是把一个函数的某些参数设置默认值，返回一个新的函数，调用这个新函数会更简单
* 设置默认值之后，想要用其他的值也是可以的


```python
int('1000000',base = 2)  # 比如想将字符串以二进制转换为整数，每次都要写base=2会比较麻烦
```

> 64




```python
import functools
int2 = functools.partial(int, base=2)  # 将int函数的base固定为2，建立新函数int2
int2('1000000')
```

> 64




```python
int2('1000000',base=10)  # 不用默认值2 ，改用10
```

> 1000000

