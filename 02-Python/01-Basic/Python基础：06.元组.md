# 元组
## 一、元组的定义
元组（tuple），用小括号`()`定义，元素用逗号隔开。tuple一旦初始化就不能修改，属于**不可变数据**，如果可能，能用tuple代替list就尽量用tuple。

可以把字符串看作一种特殊的元组


```python
a = (1,2,3,) # 元组中大于一个元素时，逗号加不加都可以
type(a)
```

> tuple



定义tuple时，tuple的元素必须被确定下来，哪怕他是空的。

注意如果要定义一个**只有1个元素的tuple，必须加一个逗号**,因为括号()既可以表示tuple，又可以表示数学公式中的小括号，这就产生了歧义，因此，Python规定，这种情况下，按小括号进行计算，所以结果是一个数字


```python
b = ()
c = (1)
d = (1,)
print(type(b))
print(type(c))
print(type(d))
```

><class 'tuple'>
><class 'int'>
><class 'tuple'>



## 二、元组不可变的理解

tuple所谓的“不可变”是说，tuple的每个元素，中间的指向关系永远不变。**即指向'a'，就不能改成指向'b'，指向一个list，就不能改成指向其他对象，但指向的这个list本身是可变的！

虽然tuple不可改变，但它可以包含可变的对象，比如list列表。所以要创建一个内容也不变的tuple，就必须保证tuple的每一个元素本身也不能变。

下面这段代码，表面上看，tuple的元素确实变了，但其实变的不是tuple的元素，而是list的元素。tuple一开始指向的list并没有改成别的list。


```python
t = ('a', 'b', ['A', 'B'])
t[2][0] = 'X'
t[2][1] = 'Y'
t
```

> ('a', 'b', ['X', 'Y'])



## 三、元组的索引与切片
和列表相同，也可以通过下标来访问，截取，组合。虽然可以用切片索引元素，但**不能修改**元素


```python
a = (1,2,3,4,5,6,7,8,9)
print(a[2])
print(a[0:2])
print(a[-2])
print(a[2:])
```

>3
>(1, 2)
>8
>(3, 4, 5, 6, 7, 8, 9)



## 四、元组的运算符

[![tuple-operator.png](https://z3.ax1x.com/2021/09/12/49W1Bt.png)](https://imgtu.com/i/49W1Bt)

补充：
* 可以通过del语句来删除整个元组


```python
a = (1,2,3)
del a
a
```

> NameError: name 'a' is not defined



## 五、元组的常用函数

* tuple.count()计量某个数值在元组中出现的次数
* max()返回元素最大值
* min()返回元素最小值


```python
a = (1,2,3,1)
print(a.count(1))
print(max(a))
print(min(a))
```

>2
>3
>1



## 六、拆包

拆包就是python把元组（或者其他序列数据类型）的元素拆分开来，逐个赋给新的变量    

使用这个功能可以给多个变量赋值或交换变量名，比其他语言要简洁


```python
a,b,c = (4,5,6)
print(a,b,c)
a,b,(c,d) = (4,5,(6,7))
print(a,b,c,d)
a,b = b,a
print(a,b)
```

>4 5 6
>4 5 6 7
>5 4



拆包也可以遍历元组或列表组成的序列


```python
list1 = [(1,2,3),(4,5,6),(7,8,9)]
for a,b,c in list1:
    print(a,b,c)
```

>1 2 3
>4 5 6
>7 8 9

