# 文件操作
## 一、文件的打开

### open()函数  
open() 函数用于打开一个文件，并返回文件对象，在对文件进行处理过程都需要使用到这个函数  
`file_name = open(file, mode='r')`函数常用形式是接收两个参数：文件名(file)和模式(mode),默认是只读模式，打开后返回给file_name这个对象，用于后续的文件操作  
常见的模式如下：

[![file-mode.png](https://z3.ax1x.com/2021/09/12/49RH6s.png)](https://imgtu.com/i/49RH6s)

[![file-mode1.png](https://z3.ax1x.com/2021/09/12/49R7lj.png)](https://imgtu.com/i/49R7lj)

**使用 open() 方法一定要保证关闭文件对象，即调用 close() 方法**  



***
## 二、读文件
**读文件的方法都会随着读取进度而推进文件指针！！！**  

### 2.1 file.read(size)
从文件读取指定的字节数。**如果未给定或为负则读取所有**


```python
fo = open('./hello.txt')
word = fo.read(10)
print(word)
fo.close()
```

> hello worl



```python
fo = open('./hello.txt','r')
word = fo.read()
print(word)
fo.close()
```

>hello world!
>hello python!
>hello wanyu!


​    

### 2.2 file.readline(size) 
用于从文件读取整行，包括 "\n" 等转义字符  
如果指定了一个非负数的参数，则返回指定大小的字节数，包括 "\n" 字符。**若未给定参数则读取文件首行**


```python
fo = open('./hello.txt','r')
line = fo.readline()
line1= fo.readline(10)
print(line)
print(repr(line)) # 用repr函数看下原本字符串
print(line1)
fo.close()
```

>hello world!
>
>'hello world!\n'
>hello pyth



### 2.3 file.readlines()

该方法用于读取所有行(直到结束符 EOF)并返回**列表！！！**  
该列表一般由 Python 的` for... in ... `结构进行处理。 如果碰到结束符 EOF 则返回空字符串。


```python
fo = open('./hello.txt','r')
lines = fo.readlines()
print(lines)
for line in lines:
    line = line.strip() # 去掉空白
    print(line)
fo.close()
```

>['hello world!\n', 'hello python!\n', 'hello wanyu!']
>hello world!
>hello python!
>hello wanyu!



## 三、写文件

### 3.1 file.write()
用于向文件中写入指定**字符串**。**要注意文件打开方式**


```python
fo = open('./hello1.txt','w')  # 先写一个新文件
fo.write('hello file\n')
fo.write('hello java')
fo.close()
```


```python
fo = open('./hello1.txt','w')  # 这段运行会把原文件内容覆盖掉
fo.write('123\n')
fo.write('456')
fo.close()
```


```python
fo = open('./hello1.txt','a')  # 'a'打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾
fo.write('\nhello file\n')
fo.write('hello java')
fo.close()
```


```python
fo = open('./hello1.txt','r')
word = fo.read()
print(word)
fo.close()
```

>123
>456
>hello file
>hello java



### 3.2 file.writelines( [ str ] )

方法用于向文件中写入一序列的字符串。这一序列字符串可以是由迭代对象产生的，如一个字符串列表。换行需要制定换行符`\n`. 


```python
fo = open("hello2.txt", "w")
seq = ["菜鸟教程 1\n", "菜鸟教程 2"]
fo.writelines( seq )
fo.close()
```


```python
fo = open("hello2.txt", "r")
word = fo.read()
print(word)
fo.close()
```

>菜鸟教程 1
>菜鸟教程 2



### 3.3 文件指针

写文件时候经常会用到文件指针
#### 3.3.1 file.tell()
tell() 方法返回文件的当前位置，即文件指针当前位置


```python
fo = open('./hello2.txt','r')
word = fo.readline()
print('读取的内容是：',word)
pos = fo.tell()
print(pos)
fo.close()
```

>读取的内容是： 菜鸟教程 1
>
>12



####  3.3.2 file.seek(offset [   , whence])

移动文件指针到指定位置
* offset -- 开始的偏移量，也就是代表需要移动偏移的字节数，如果是负数表示从倒数第几位开始  
* whence：0 代表从文件开头开始算起，1 代表从当前位置开始算起，2 代表从文件末尾算起。默认值为0  


```python
fo = open('./hello2.txt', 'r')
word = fo.readline()
print('读取的内容是：',word)
pos = fo.tell()
print(pos)
fo.seek(0,0)   # 指针设置在开头
pos = fo.tell()
print(pos)
fo.close()
```

>读取的内容是： 菜鸟教程 1
>
>12
>0




## 四、关闭文件
文件未关闭容易导致很多错误


```python
txt = open('hello3.txt','w')
for i in range(5):
    txt.write(str(i)+'\n')
# txt.close()  
txt2 = open('hello3.txt','r')
print (txt2.read())  # 程序运行之后文件中并没有写入东西，因为没关闭
```


​    


```python
txt = open('hello3.txt','w')
for i in range(5):
    txt.write(str(i)+'\n')
txt.close()  # 关闭
txt2 = open('hello3.txt','r')
print (txt2.read())  
```

>0
>1
>2
>3
>4


​    

在写入的时候有可能一些中间过程出问题，导致程序无法正常运行，也就无法运行close语句，这时候可以用处理异常的方法,通过finally语句，把文件关闭


```python
txt = open('hello4.txt','w')
try:
    for i in range(10):
        a=10/(i-5)
        txt.write(str(a)+'\n')
except Exception:
    print ('error:',i)
finally:
    txt.close()
```

> error: 5


## with open() as name:
这种结构可以自动关闭文件，不用担心出问题，同时也命名了新文件。但要处理异常还是要用try结构


```python
with open('hello5.txt','w') as txt:
    for i in range(5):
        txt.write(str(i)+'\n')
with open('hello5.txt','r') as txt1:        
    print (txt1.read())
```

>0
>1
>2
>3
>4


​    

### file.closed()方法
如果文件已关闭，则为True


```python
with open('hello5.txt') as txt:
    pass
print(txt.closed)
```

> True

