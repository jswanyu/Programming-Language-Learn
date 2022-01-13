# 一、命名空间
命名空间提供了在项目中避免名字冲突的一种方法。各个命名空间是独立的，没有任何关系的，所以**一个命名空间中不能有重名**，但不同的命名空间是可以重名而没有任何影响。  

一般有三种命名空间：  
* 内置名称（built-in names）， Python 语言内置的名称，比如函数名 abs、char 和异常名称 BaseException、Exception 等等。

* 全局名称（global names），模块中定义的名称，记录了模块的变量，包括函数、类、其它导入的模块、模块级的变量和常量。

* 局部名称（local names），函数中定义的名称，记录了函数的变量，包括函数的参数和局部定义的变量。（类中定义的也是）

  [![namespace.png](https://z3.ax1x.com/2021/09/12/49RxtU.png)](https://imgtu.com/i/49RxtU)

 Python 的命名空间查找顺序为：局部的命名空间去 -> 全局命名空间 -> 内置命名空间

# 二、作用域
在一个 python 程序中，直接访问一个变量，会从内到外依次访问所有的作用域直到找到，否则会报未定义的错误  

变量的作用域决定了在哪一部分程序可以访问哪个特定的变量名称。  

Python的作用域一共有4种：  
* L（Local）：最内层，包含局部变量，比如一个函数/方法内部。  
* E（Enclosing）：包含了非局部(non-local)也非全局(non-global)的变量。比如两个嵌套函数，一个函数（或类） A 里面又包含了一个函数 B ，那么对于 B 中的名称来说 A 中的作用域就为 nonlocal。
* G（Global）：当前脚本的最外层，比如当前模块的全局变量。
* B（Built-in）： 包含了内建的变量/关键字等。，最后被搜索

在局部找不到，便会去局部外的局部找（例如闭包），再找不到就会去全局找，再者去内置中找

[![action-scope.png](https://z3.ax1x.com/2021/09/12/49RW0P.png)](https://imgtu.com/i/49RW0P)


```python
g_count = 0  # 全局作用域
def outer():
    o_count = 1  # 闭包函数外的函数中
    def inner():
        i_count = 2  # 局部作用域
```

内置作用域是通过一个名为` builtin `的标准模块来实现的，但是这个变量名自身并没有放入内置作用域内，所以必须导入这个文件才能够使用它。在Python3.0中，可以使用以下的代码来查看到底预定义了哪些变量
```python
import builtins
dir(builtins)
```

Python 中只有**模块（module），类（class）以及函数（def、lambda）**才会引入新的作用域，其它的代码块（如 if/elif/else/、try/except、for/while等）是不会引入新的作用域的，也就是说这些语句内定义的变量，外部也可以访问


```python
if True:
    msg = 'I am from Runoob'
def test():
    msg_inner = 'I am from Runoob'
print(msg) # 外部能访问
print(msg_inner) # 外部不能访问
```

> I am from Runoob

> NameError: name 'msg_inner' is not defined




## 1.全局变量和局部变量

定义在函数内部的变量拥有一个局部作用域，定义在函数外的拥有全局作用域。  

局部变量只能在其被声明的函数内部访问，而全局变量可以在整个程序范围内访问。  


```python
total = 0 # 这是一个全局变量

def sum( arg1, arg2 ):
    #返回2个参数的和
    total = arg1 + arg2 # total在这里是局部变量.
    print ("函数内是局部变量 : ", total)
    return total
 
#调用sum函数
sum( 10, 20 )
print ("函数外是全局变量 : ", total)
```

>函数内是局部变量 :  30
>函数外是全局变量 :  0




## 2.global 和 nonlocal 关键字 
global可声明该变量为全局变量 


```python
total = 0 # 这是一个全局变量

def sum( arg1, arg2 ):
    global total  # 声明全局变量
    total = arg1 + arg2 
    print ("全局变量函数内部运算后为 : ", total)
    return total
 
#调用sum函数
sum( 10, 20 )
print ("函数外全局变量也随之改变 : ", total)
```

>全局变量函数内部运算后为 :  30
>函数外全局变量也随之改变 :  30


nonlocal声明该变量非局部变量（局部作用域），而是外层非全局作用域


```python
num = 1     
def outer():
    num = 10
    def inner(): 
        num = 100      
        print(num)      # 打印100
    inner()
    print(num)          # 外层非全局的变量不会被修改
outer()
print(num) # 全局变量不会被修改
```

>100
>10
>1



```python
num = 1     
def outer():
    num = 10
    def inner():
        nonlocal num   # nonlocal关键字，声明这个num是外层非全局的变量，从这里看就是指这个num是上面的10，现在要把你赋值为100
        num = 100       
        print(num)      
    inner()
    print(num)          # 外层非全局的num会被修改
outer()
print(num) # 全局变量不会被修改
```

>100
>100
>1

