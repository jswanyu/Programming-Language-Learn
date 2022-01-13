# 一、字符串

字符串是以单引号`''`或双引号`""`括起来的任意文本。`''`或`""`本身只是一种表示方式，不是字符串的一部分。字符串`'abc'`只有a，b，c这3个字符

Python**没有单独的字符类型**，一个字符就是长度为 1 的字符串

如果`'`本身也是一个字符，那就可以用`""`括起来。比如`"I'm OK"`包含的字符是I，'，m，空格，O，K这6个字符

在Python3中，所有的字符串都是Unicode字符串


```python
print("I'm ok")
```

> I'm ok



## 1.转义字符

如果字符串内部既包含`'`又包含`"`怎么办？可以用转义字符`\`来标识。`\'`表示单引号 `\"`表示双引号

下表展示了python的转义字符（ESC, escape character）

[![esc.png](https://z3.ax1x.com/2021/09/12/49RIfg.png)](https://imgtu.com/i/49RIfg)


```python
print('I\'m \"ok\" ')
```

> I'm "ok" 



使用r/R表示原始字符串，不会发生转义。r是指raw，原生的


```python
 print(r"this is a line with \n")
```

> this is a line with \n



如果字符串内部有很多换行，用\n写在一行里不好阅读，为了简化，Python允许用`''' '''`的格式表示多行内容

并且，`''' '''`还是多行注释，python解释器会自动识别


```python
c = '''
hello world
hello wayu
hello python
'''
print(c)
```

>hello world
>hello wayu
>hello python


​    

## 2.字符串运算符
python内置了很多字符串的运算符

[![string-operator.png](https://z3.ax1x.com/2021/09/12/49WehD.png)](https://imgtu.com/i/49WehD)




```python
print('hello'+'world')
print('hello','world')  # ,逗号可以代替空格使其连接
print('hello '*3)       
str7='hello python world'
print('hello' in str7)
```

>helloworld
>hello world
>hello hello hello 
>True



注意**字符串不能减除**


```python
str1='hello'
str2='h'
str3=str1-str2
print(str3)
```

> TypeError: unsupported operand type(s) for -: 'str' and 'str'



## 3.字符串占位(字符串格式化)

### 3.1 第一种占位操作  `'%d' % ()`
第一种占位操作是用的最早的，用`%`的格式先在字符串中占位，后面添加小括号指向变量（如果只有一个占位，括号可以省略），中间用`%`连接

浮点型的小数位和C语言中类似，格式为%a.bf ，其中a表示显示的最小总宽度，b表示保留几位小数

如果不太确定应该用什么，%s永远起作用，它会把任何数据类型转换为字符串

下表为常用的字符串格式化操作符：

[![string-format.png](https://z3.ax1x.com/2021/09/12/49WA76.png)](https://imgtu.com/i/49WA76)

补充：%i 十进制整数


```python
str='hello wanyu'
a=1
b=2.0
print('%s %d %.2f' % (str,1,b))
```

> hello wanyu 1 2.00

### 3.2 第二种占位操作  `str.format()`
在写字符串时，可以先用`{}`占位，再用`format()`方法传入值,括号中的参数需要用逗号隔开。默认传入顺序是从前向后，注意format是字符串的方法，所以应该结构是`'   '.format()`。这种方式不用再去判断使用传入字符串类型是%s，还是 %d

大多数的 Python 代码仍然使用` % `操作符。但是这种旧式的格式化最终会被移除, 应该更多的使用 `str.format()`


```python
print('hello {} {} !'.format('python',123))
str = 'hello {} !'
print(str.format('world'))
```

>hello python 123 !
>hello world !



**有关占位顺序**

如果想改变传入顺序，可以在`{ }`用从0开始的数字代表先后顺序

也可以先在`{ }`中放入变量，再在format()方法中直接指定变量值


```python
print('{2} {1} {0}'.format('hello','python','!'))
print('{str0} {str1} {str2}'.format(str0='hello',str1='python',str2='!'))
```

>! python hello
>hello python !



**有关占位长度和小数有效数字**

在`{ }`中放入`a:b.cf` 表示这个位置的可选项多了新的要求，即占b个位(b省略则是全部输出)，保留c位小数，其中f还可以替换为d等转义字符中的字母，a仍然可以放表示顺序的数字，或者关键字


```python
import math
print('常量 PI 的值近似为 {1:.3f}，常量 e 的值近似为 {0:10.2f}'.format(math.e,math.pi))
```

> 常量 PI 的值近似为 3.142，常量 e 的值近似为       2.72



**字典传入字符串**

如果有一个很长的格式化字符串, 而你不想将它们分开, 那么在格式化时最好通过变量名来访问，而非使用位置。最简单的就是传入一个字典, 然后使用方括号` [] `来访问键值，在`{}`中依然可以放入数字与后面format中的字典**顺序**形成对应关系


```python
dict1 = {'Google': 1, 'Runoob': 2, 'Taobao': 3}
dict2 = {'Google': 4, 'Runoob': 5, 'Taobao': 6}
print('Runoob: {0[Runoob]}; Google: {0[Google]}; Taobao: {1[Taobao]}'.format(dict1,dict2))   # 这里的0/1表示第0/1个传入的参数
```

> Runoob: 2; Google: 1; Taobao: 6



也可以通过在format中的变量前使用` ** `来实现相同的功能，用到再深入了解


```python
dict1 = {'Google': 1, 'Runoob': 2, 'Taobao': 3}
dict2 = {'Google': 4, 'Runoob': 5, 'Taobao': 6}
print('Runoob: {Runoob}; Google: {Google}; Taobao: {Taobao}'.format(**dict1))
```

> Runoob: 2; Google: 1; Taobao: 3



`{ }`里可以放入下面的符号用于在格式化某个值之前对其进行转化，用到再深入了解  

`!a `(使用 ascii())  
`!s `(使用 str())  
`!r `(使用 repr())  



### 3.3 第三种占位操作`f ' {} '`

f-string 是 python3.6 之后版本添加的。它以 f 开头，后面跟着字符串，字符串中的表达式用大括号 {} 包起来，它会将变量或表达式计算后的值替换进去。

也可以理解为这种方法就是把第二种的.format() 替换为了 f

这种方式也不用再去判断使用 %s，还是 %d


```python
name = 'Runoob'
print(f'Hello {name}') # 替换变量
print(f'{1+2}')        # 替换计算后的值
```

> Hello Runoob
>
> 3



## 4.字符串内建函数&方法

要区分函数和方法的使用

### 4.1 常用函数
字符串的函数不是太多，主要以方法为主。常用的函数有以下几个：
* len(str)返回字符串长度
* max(str)返回字符串 str 中最大的字母
* min(str)返回字符串 str 中最小的字母


```python
str1='hello'
print(len(str1))
print(max(str1))
print(min(str1))
```

>5
>o
>e



### 4.2 字符串大小写

* 方法upper()将字符串全部转化为大写
* 方法lower()将字符串全部转化为小写
* 方法capitalize()将字符串首字母转化为大写
* 方法title()返回"标题化"的字符串,就是说所有单词都是以大写开始，其余字母均为小写
* 方法swapcase()将字符串中大写转换为小写，小写转换为大写


```python
str5='hello python'
str6='java'
print(str5.upper())
print(str5.lower())
print(str5.capitalize())
print(str5.title())
print(str6.swapcase())
```

>HELLO PYTHON
>hello python
>Hello python
>Hello Python
>JAVA



### 4.3 字符串的切分

**字符串是不可变对象！** 对于不变对象来说，调用对象自身的任意方法，也不会改变该对象自身的内容。这些方法会创建新的对象并返回，保证了不可变对象本身永远是不可变

* 字符串也可以用切片操作符操作，操作结果仍是字符串
* 方法split()可以将字符串进行切分成一个**列表**，注意返回值是列表，切分完之后需要赋给一个新的变量，否则原变量是不变的，也体现了字符串是不可变对象


```python
str1='hello word'
print(str1[4:7])
str2='1 2 3 4 5'
str3=str2.split()
print(str3)
print(str2)
print(type(str3))
```

>o w
>['1', '2', '3', '4', '5']
>1 2 3 4 5
><class 'list'>


* 方法split()默认值是以空格为分界，可传入其他分界值。若是以字符串中没有的字符为分界值，则返回一个只有一个字符串元素的**列表**


```python
str3='1 2,3 4,5 6'
print(str3.split(','))
print(str2.split('?'))
print(type(str2.split('?')))
print(str2.split('?')[0])
```

>['1 2', '3 4', '5 6']
>['1 2 3 4 5']
><class 'list'>
>1 2 3 4 5



### 4.4 字符串的合并

方法join()可以将字符进行合并。语法`  'sep'.jion(seq)`。

* 其中sep为分隔符，可以为空。
* seq是要连接的元素序列、字符串、元组、字典
* 返回值：返回一个以分隔符sep连接各个元素后生成的字符串


```python
str2='1 2 3 4 5'
str3=str2.split()
str4=','.join(str3)
str5='?'.join(str3)   #以?为分隔符
str6=str2.join(str3)  #以1 2 3 4 5为分隔符
print(str4)
print(str5)
print(str6)
```

>1,2,3,4,5
>1?2?3?4?5
>11 2 3 4 521 2 3 4 531 2 3 4 541 2 3 4 55



### 4.5 字符串的替换

* 方法`replace(a,b)`可以指定替换字符串，后面的内容替换前面的


```python
str5='hello python'
str5.replace('python' , 'world')
```

> 'hello world'



### 4.6 字符串去掉空格
* 方法strip([chars])去掉两边字符串头尾指定的字符（默认为空格或换行符）或字符序列
* 方法lstrip([chars])去掉左边...
* 方法rstrip([chars])去掉右边...


```python
str6 = '   hello python？   '
str6.strip()
```

> 'hello python？'




```python
str6 = '   hello python   '
str6.lstrip()
```

> 'hello python   '




```python
str6 = '   hello python   '
str6.rstrip()
```

> '   hello python'



### 4.7 字符串填充
* zfill (width) 返回长度为 width 的字符串，原字符串右对齐，前面填充0
* rjust(width,[, fillchar])返回一个原字符串右对齐，并使用fillchar(默认空格）填充至长度 width 的新字符串
* ljust(width,[, fillchar])返回一个原字符串左对齐，并使用fillchar(默认空格）填充至长度 width 的新字符串


```python
str1 = "hello python ...wow!!!"
print (str1.zfill(50))
print (str1.rjust(50))  # 默认是空格
print (str1.ljust(50, '*'))
```

>0000000000000000000000000000hello python ...wow!!!
>                      hello python ...wow!!!
>hello python ...wow!!!****************************

