# 一、输出
print()为python的打印函数，括号中加上字符串，就可以向屏幕上输出指定的文字，也可以打印数字

可以接受多个字符串，用逗号`,`隔开。print()会依次打印每个字符串，**遇到逗号会输出一个空格**


```python
print('hello world')
```

> hello world



```python
print('hello', 'world')
```

> hello world



```python
print(1+2)
```

> 3



**习惯于使用 `,` 不用 `+` 。** 因为有时候传入的参数不一定是字符串类型，`+`仅适用于字符串拼接


```python
dict1={123:1,
       'secend':'hello',
       (1,2,3):{'123':1,'secend':'hello'}
      }
for keys,values in dict1.items():
    print(keys + ':' + values)
```

> TypeError                                 Traceback (most recent call last)
>
> <ipython-input-4-b9108e6d8407> in <module>
>       4       }
>       5 for keys,values in dict1.items():
> ----> 6     print(keys + ':' + values)
>
> TypeError: unsupported operand type(s) for +: 'int' and 'str'



```python
dict1={123:1,
       'secend':'hello',
       (1,2,3):{'123':1,'secend':'hello'}
      }
for keys,values in dict1.items():
    print(keys,':',values)
```

> 123 : 1
> secend : hello
> (1, 2, 3) : {'123': 1, 'secend': 'hello'}



print 默认输出是换行的，如果要**实现不换行需要在变量末尾加上 `end=" "`**

同理，希望打印出的内容间隔不是换行或者空格，还可以指定其他符号


```python
print( 'a', end=" " )
print( 'b')
print( 'c', end="@" )
print( 'd')
```

> a b
> c@d



# 二、输入

python用`input()`函数捕获用户的输入，括号内可以加上你想让用户看见的字符串。通常将用户输入的内容赋值给一个变量


```python
name = input('what is your name ?')
print('your name is:',name)
```

> what is your name ?
> your name is: 



input()返回的**数据类型是str**，所以进行其他形式的运算时，需要转换


```python
s = int(input('birth: '))
birth = int(s)
if birth < 2000:
    print('00前')
else:
    print('00后')
```

> birth: 10

> ValueError                                Traceback (most recent call last)
>
> <ipython-input-8-7197081a1cfe> in <module>
> ----> 1 s = int(input('birth: '))
>       2 birth = int(s)
>       3 if birth < 2000:
>       4     print('00前')
>       5 else:
>
> ValueError: invalid literal for int() with base 10: ''




# 三、注释
Python单行注释用#

Python多行注释用`''' '''`  或者 `""" """`，常用于在函数首部对函数进行说明


```python
print('hello world')
# 这是一个注释
```

> hello world



```python
'''
多行注释
print('hello world')
'''
print('python')
```

> python



# 四、续行符

python中用\来另起一行，\后必须要换行写内容，写了\但**不换行会报错**

在 `[ ]`, `{ }`, 或 `( )` 中的多行语句，不需要使用反斜杠(\)


```python
item_one = 1
item_two = 2
item_three = 3
total = item_one    \
        + item_two  \
        + item_three
total
```

> 6




```python
print('ab'
)
```

> ab



### 同一行显示多条语句

Python可以在同一行中使用多条语句，语句之间使用分号`;`分割


```python
import sys; x = 'runoob'; sys.stdout.write(x + '\n')
```

> runoob


python语句后直接加分号也可以，但没人用


```python
print("hello world");
```

> hello world



# 五、其它

以下代码在一些博客上经常见到

第一行注释是为了告诉Linux/OS X系统，这是一个Python可执行程序，Windows系统会忽略这个注释；

第二行注释是为了告诉Python解释器，按照UTF-8编码读取源代码，否则，你在源代码中写的中文输出可能会有乱码


```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
```
