# 错误与异常
## 一、错误
不符合python语言标准的错误：SyntaxError


```python
while True print('hello')
```

>   File "<ipython-input-1-d3123ece03ef>", line 1
>     while True print('hello')
>                ^
> SyntaxError: invalid syntax



## 二、异常
### 1.异常的定义
即便 Python 程序的语法是正确的，在运行它的时候，也有可能发生错误。运行期检测到的错误被称为异常


```python
1/0 # 0 不能作为除数，触发异常
```

> ZeroDivisionError: division by zero



```python
a*3 # a 未定义，触发异常
```

> NameError: name 'a' is not defined



```python
2+'2' # 类型不同，触发异常
```

> TypeError: unsupported operand type(s) for +: 'int' and 'str'



### 2.异常处理

### 2.1 try…except语句
try 语句按照如下方式工作：  
* 首先，执行 try 子句（在关键字 try 和关键字 except 之间的语句）。
* 如果没有异常发生，忽略 except 子句，try 子句执行后结束。
* 如果在执行 try 子句的过程中发生了异常，那么 try 子句余下的部分将被忽略。如果异常的类型和 except 之后的名称相符，那么对应的 except 子句将被执行。
* 如果一个异常没有与任何的 except 匹配，那么这个异常将会传递给上层的 try 中。
* 一个 try 语句可能包含多个except子句，分别来处理不同的特定的异常。**最多只有一个分支会被执行**。


```python
while True:
    a = int(input('请输入一个数字：'))
    if a==1:
        break
    print('你输入的数字为：%d' % a)  # 程序可以执行，但碰到int函数转化不了的就会报错
```

>请输入一个数字：4
>你输入的数字为：4
>请输入一个数字：a

> ValueError: invalid literal for int() with base 10: 'a'



```python
while True:
    try:
        a = int(input('请输入一个数字：'))
        if a==1:
            break
        print('你输入的数字为：%d' % a)
    except ValueError:
        print('请输入数字！')
```

>请输入一个数字：4
>你输入的数字为：4
>请输入一个数字：a
>请输入数字！
>请输入一个数字：1


也可以忽略异常的名称，只写`except:/except Exception:`，用于出现其它的未知异常，`Exception`是python的异常库，如下一段程序最后一个except


```python
import math

while True:
    try:
        a=int(input('请输入一个数字：'))
        if a==7:
            break
        result=1/math.log(a)
        print('运算之后结果为：%f' % result)
    except ValueError:         # 数值异常
        print ('ValueError: 输入需要为大于0的数字！')
    except ZeroDivisionError:  # 数值为0带来的异常
        print ('log(value) must != 0')
    except:                    # 其它异常 
        print ('unknow error')
```

>请输入一个数字：4
>运算之后结果为：0.721348
>请输入一个数字：a
>ValueError: 输入需要为大于0的数字！
>请输入一个数字：1
>log(value) must != 0
>请输入一个数字：7


一个except子句可以同时处理多个异常，这些异常将被**放在一个括号里成为一个元组**，括号必不可少


```python
import math

while True:
    try:
        a=int(input('请输入一个数字：'))
        if a==7:
            break
        result=1/math.log(a)
        print('运算之后结果为：%f' % result)
    except (ValueError,ZeroDivisionError):  # 数值异常
        print ('ValueError: 输入需要为大于0的数字且不等于1！')
```

>请输入一个数字：a
>ValueError: 输入需要为大于0的数字且不等于1！
>请输入一个数字：1
>ValueError: 输入需要为大于0的数字且不等于1！
>请输入一个数字：4
>运算之后结果为：0.721348
>请输入一个数字：7


异常处理并不仅仅处理那些直接发生在 try 子句中的异常，而且还能处理**子句中调用的函数（甚至间接调用的函数）里抛出的异常**


```python
def f():
    1/0
try:
    f()
except ZeroDivisionError:
    print('0不能作为分母')
```

> 0不能作为分母



### 2.2 try-except-else语句

try…except 语句还有一个可选的 else 子句，如果使用这个子句，那么必须放在**所有的 except 子句**之后  

else 子句将在 **try 子句没有发生任何异常**的时候执行  

使用 else 子句比把所有的语句都放在 try 子句里面要好，这样可以避免一些意想不到，而 except 又无法捕获的异常


```python
import math

while True:
    try:
        a=int(input('请输入一个数字：'))
        if a==7:    # 辅助停止程序
            break
        result=1/math.log(a)
    except (ValueError,ZeroDivisionError):  # 数值异常
        print ('ValueError: 输入需要为大于0的数字且不等于1！')
    else:
        print('运算之后结果为：%f' % result) # 没有异常再打印最终结果
```

>请输入一个数字：1
>ValueError: 输入需要为大于0的数字且不等于1！
>请输入一个数字：4
>运算之后结果为：0.721348
>请输入一个数字：7


### 2.3 try-finally 语句
`try-finally` 语句无论是否发生异常都将执行最后的代码  

完整结构可以为：try-except-else-finally 

[![exception.png](https://z3.ax1x.com/2021/09/12/49RTpQ.png)](https://imgtu.com/i/49RTpQ)


```python
import math

while True:
    try:
        a=int(input('请输入一个数字：'))
        if a==7:
            break
        result=1/math.log(a)
    except (ValueError,ZeroDivisionError):  # 数值异常
        print ('ValueError: 输入需要为大于0的数字且不等于1！')
    else:
        print('运算之后结果为：%f' % result) # 没有异常再打印最终结果
    finally:
        print('不管怎么样，这句话都执行')
```

>请输入一个数字：4
>运算之后结果为：0.721348
>不管怎么样，这句话都执行
>请输入一个数字：1
>ValueError: 输入需要为大于0的数字且不等于1！
>不管怎么样，这句话都执行
>请输入一个数字：7
>不管怎么样，这句话都执行



如果一个异常在 try 子句里（或者在 except 和 else 子句里）被抛出，而又没有任何的 except 把它截住，那么这个异常会**在 finally 子句执行后被抛出**

下面的例子中故意不截住ZeroDivisionError，当输入为1时，是先执行finally语句，再抛出错误，有这样一个顺序 


```python
import math

while True:
    try:
        a=int(input('请输入一个数字：'))
        if a==7:
            break
        result=1/math.log(a)
    except ValueError:      # 数值异常
        print ('ValueError: 输入需要为大于0的数字且不等于1！')
    else:
        print('运算之后结果为：%f' % result) # 没有异常再打印最终结果
    finally:
        print('不管怎么样，这句话都执行')
```

>请输入一个数字：1
>不管怎么样，这句话都执行

> ZeroDivisionError: float division by zero



### 2.4 raise抛出异常

raise是python处理异常的内置语句，用于把指定的异常抛出，所谓抛出即让解释器提示错误  
raise语法格式：`raise [Exception [, args [, traceback]]]`  
raise 唯一的一个参数指定了要被抛出的异常。它必须是一个异常的实例或者是异常的类（也就是 Exception 的子类，Exception是python定义好的类）  


```python
x = 10
if x > 5:
    raise Exception('x 不能大于 5')
```

> Exception: x 不能大于 5



如果只想知道这是否抛出了一个异常，并不想去处理它，那么一个简单的 raise 语句就可以再次把它抛出


```python
import math

while True:
    try:
        a=int(input('请输入一个数字：'))
        if a==7:
            break
        result=1/math.log(a)
        print('运算之后结果为：%f' % result)
    except ValueError:  # 数值异常
        print ('ValueError: 输入需要为大于0的数字！')
    except:             # 其它异常 
        raise           # 让解释器把其它错误抛出
```

> 请输入一个数字：1

> ZeroDivisionError: float division by zero



### 2.5 带有值的异常

异常是一种定义的类，在类中往往会定义一些属性的私有值。抛出异常有时候会需要其在返回时带上的值。  
此时我们一般使用`except ... as ...`的结构，用来拿出值


```python
def temp_convert(var):
    try:
        return int(var)
    except ValueError as Argument:
        print ("这个参数没有包含数字：", Argument)

# 调用函数
temp_convert("xyz")
```

> 这个参数没有包含数字： invalid literal for int() with base 10: 'xyz'



再例如生成器的StopIteration，生成器的return返回值会在StopIteration的value中


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



## 预定义的清理行为

一些对象定义了标准的清理行为，无论系统是否成功的使用了它，一旦不需要它了，那么这个标准的清理行为就会执行  
如关键词 with 语句就可以保证诸如文件之类的对象在使用完之后一定会正确的执行他的清理方法  


```python
with open('hello5.txt','r') as txt1:        
    print (txt1.read())
```

>0
>1
>2
>3
>4


​    
