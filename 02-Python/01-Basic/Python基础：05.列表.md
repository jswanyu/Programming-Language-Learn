# 列表
***
## 一、列表的创建
* 通过[]创建一个列表，列表里可以放任意类型的变量，以`,`隔开，不限制长度
* 使用list()函数创建列表，**参数是列表的形式，且只有一个列表**，一般会赋给一个变量


```python
list1=[1,2.5,'hello',True]
list2=list([[1,2],[3,4]])
print(list1)
print(list2)
```

>[1, 2.5, 'hello', True]
>[[1, 2], [3, 4]]



```python
list([1,2],[3,4])  # 传入两个列表会报错
```

> TypeError: list expected at most 1 argument, got 2



## 二、列表的操作符

[![list-operator.png](https://z3.ax1x.com/2021/09/12/49RvkT.png)](https://imgtu.com/i/49RvkT)


```python
list1=[1,2.5,'hello',True]
print(len(list1))
list2=[123,456]
print(list1 + list2)
print(list2*2)
print(123 in list2)

for i in list1:
    print(i)
```

>4
>[1, 2.5, 'hello', True, 123, 456]
>[123, 456, 123, 456]
>True
>1
>2.5
>hello
>True



```python
# 还支持+=
list3 = [1,2,3]
list3+=[4,5,6]   
list3
```

> [1, 2, 3, 4, 5, 6]



***
## 三、列表的索引

长度为n的列表可以通过使用操作符`[ ]`从0到n-1的正数来**正向**索引，也可以用-1到-n的负数来**反向**索引。

这种用一个具体位置数字访问列表元素的方法叫做索引，也叫列表的访问，一般只针对一个元素，同时索引出来的只是元素，不是一个单元素列表（除非元素本身就是列表）。


```python
list1 = [1, 2.5, 'hello', True]
print(list1[0])
print(list1[2])
print(list1[-1])
print(list1[-3])
```

>1
>hello
>True
>2.5


索引可以用来赋值改变列表内容


```python
list1[0]=3   # 修改列表内容
list1
```

> [3, 2.5, 'hello', True]





## 四、列表的切片

切片是通过使用操作符`[ ]`索引多个元素，切片出来的是一个列表。列表、元组、字符串都可以通过切片来索引内容

切片格式为：`item[x:y:z]`表示取出对象`[x,y)`的范围，注意是**左闭右开**。每隔z个索引一个内容。

x省略表示从头索引，y省略表示索引到尾，z省略表示无需间隔。x,y都省略表示整个对象，可以用来将列表复制给另一个对象，即`[:]`


```python
list1 = [1,2,3,4,5,6,7,8,9]
print(list1[0:2])
print(list1[:4])
print(list1[0:7:2])
list2 = list1[:]
print(list2)
```

>[1, 2]
>[1, 2, 3, 4]
>[1, 3, 5, 7]
>[1, 2, 3, 4, 5, 6, 7, 8, 9]


切片同样可以通过赋值来修改列表


```python
list1 = [1, 2.5, 'hello', True]
list1[2:]=[9,8,7,6,5,4,3,2,1]
print(list1)
```

> [1, 2.5, 9, 8, 7, 6, 5, 4, 3, 2, 1]


也支持用负数进行切片，最后一个元素索引是-1。**切记**倒数切片的时候，依然是**从前往后切**。只是位置用负数表示而已


```python
list1 = [1,2,3,4,5,6,7,8,9]
print(list1[-4:-1]) # 顺序是678。而不是876
```

> [6, 7, 8]



## 五、列表的嵌套

列表中也可以放列表，访问多层列表，用多层的`[]`来访问


```python
list4=[1,2,[3,4]]
print(list4[2])
print(list4[2][1])
```

>[3, 4]
>4



## 六、列表的常用函数

常用的全局函数较少，主要是方法
* len()
* max()
* min()
* sorted()  # 放在下面讲


```python
list4 = [1,2,3,4,5,6]
print(len(list4))  # len()列表元素个数
print(max(list4))  # max()返回列表元素最大值
print(min(list4))  # min()返回列表元素最小值
```

>6
>6
>1



## 七、列表的常用方法

### 1.列表添加元素的方法
* 方法`append(obj)`用于添加列表元素，默认在列表最后添加obj，括号里的东西就是添加的内容，以括号里的格式为准，括号里是列表就往列表里添加列表


```python
list2=[123,456]
list2.append([123,456])
print(list2)
list2.append(123)
print(list2)
```

>[123, 456, [123, 456]]
>[123, 456, [123, 456], 123]


* 方法`insert(x,obj)`用于在指定位置x添加元素obj


```python
list2.insert(1,789)
list2
```

> [123, 789, 456, [123, 456], 123]



* 方法`extend(obj)`用于在列表末尾添加指定列表中的内容，**注意这里的obj一定得是列表，并且添加的是内容，不是像append一样括号里是啥就加啥**


```python
list2=[123,456]
list3=[123,456]
list2.append([7,8,9])  # 添加整个列表
list3.extend([7,8,9])  # 添加789这三个数
print(list2)
print(list3)
```

>[123, 456, [7, 8, 9]]
>[123, 456, 7, 8, 9]



```python
list2=[123,456]
list2.extend(1)  # 添加的不是列表会报错
```

> TypeError: 'int' object is not iterable



### 2.列表删除元素的方法

* `del 列表名[索引] `格式可以删除列表元素。可以用单个索引删除单个元素，也可以用切片删除多个元素


```python
list3=[1,2,3,4,5,6,7,8,9]
del list3[0:2]
list3
```

> [3, 4, 5, 6, 7, 8, 9]



* 方法`remove()`可以删除指定内容的元素，括号里为内容，若有多个相同内容，则先删除第一个


```python
list3=[1,8,3,4,8]
list3.remove(8)
list3
```

> [1, 3, 4, 8]



* 方法`pop(index=-1)`可以弹出索引元素,所谓弹出是指将弹出的元素赋给某个变量。括号里的参数为索引位置，默认弹出最后一个。


```python
list3=[1,2,3,4,5,6,7,8,9]
abc = list3.pop()  # 默认弹出最后一个
print(list3)
print(abc)
list3.pop(0)
print(list3)
```

>[1, 2, 3, 4, 5, 6, 7, 8]
>9
>[2, 3, 4, 5, 6, 7, 8]



### 3. 列表的排序方法，内置方法list.sort()

方法sort()用于数字列表排序，排序之后，**原列表被改变**。如果排序时不要求原列表保持不变，这种效率更高一点，因为不用拷贝对象


```python
list6=[1,5,3,9,10,2,7,1]
list6.sort()
list6
```

> [1, 1, 2, 3, 5, 7, 9, 10]



方法sort()还可以指定二级排序key，key指定的函数将作用于list的每一个元素上，并根据key函数返回的结果进行排序

方法sort()还有参数reverse，其默认值为False，若指定为True，则翻转排序。（也有单独的方法list.reverse()用于列表翻转，倒序排列）


```python
list6=[-1,5,-3,9,10,2,7,1]
list6.sort(key = lambda x:x*(-1),reverse = True)  # 按照元素乘以-1后从小到大排序，再翻转排序，这里是匿名函数
list6
```

> [-3, -1, 1, 2, 5, 7, 9, 10]




```python
list7=[1,5,3,9,10,2,7,1]
list7.reverse()
list7
```

> [1, 7, 2, 10, 9, 3, 5, 1]



### 列表排序还可以使用python的内置全局函数sorted()

二者的区别是，使用函数sorted()进行排序，**原列表不会被改变**，它是在原列表的拷贝对象上进行排序。函数sort()也有参数reverse和key，内置函数中细讲。

**务必注意sorted()是全局函数，不是list的方法**


```python
list7=[1,5,3,9,10,2,7,1]
list8=sorted(list7,reverse=True)
print(list7)
print(list8)
```

>[1, 5, 3, 9, 10, 2, 7, 1]
>[10, 9, 7, 5, 3, 2, 1, 1]



```python
list7=[1,5,3,9,10,2,7,1]
list8=sorted(list7,reverse=True)
print(list7)
print(list8)
```

>[1, 5, 3, 9, 10, 2, 7, 1]
>[10, 9, 7, 5, 3, 2, 1, 1]



### 4.列表的复制方法

* 方法list.copy()用于复制列表，返回复制后的新列表
* 也可以使用上文提到的切片赋值方法来复制

如果想开辟新的内存空间给复制后的变量**不能简单的使用赋值语句**，赋值语句是将两个变量指向同一个对象


```python
list6 = [1,2,3]
list7 = list6     #这样简单的赋值，不管6和7谁变了，两个都会变
print(id(list6))
print(id(list7))
list8 = list6.copy() # 这两种才是真正的复制方法
list9 = list6[:]
print(id(list8))
print(id(list9))
```

>2187198271168
>2187198271168
>2187198271360
>2187198270912



### 5.列表的其他方法

* 方法list.count()用于计数,参数为待计数的内容,返回计数次数
* 方法list.index()用于定位索引,括号里是内容，返回结果是内容第一次出现在列表中的位置标号。列表中没有的内容会报错
* 方法list.reverse()用于列表翻转，倒序排列
* 方法list.clear()用于清空列表


```python
list5=[1,1,2,5,1,2,3,3]
print(list5.count(2))
print(list5.index(3))
print(list5.reverse())
list5.clear()
print(list5)
```

>2
>6
>None
>[]

