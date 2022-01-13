# 字典

## 一、字典的创建
python通过`{}`创建字典结构，或者使用dict()函数。字典是一种键值对结构,`keys:values`,键值对中间用`,`隔开，键值对在字典里是无序的。

**键只可以为数字、字符串、元组，为不可变对象，且键唯一，而值可以为任何变量类型,且不唯一。**



**和list比较，dict有以下几个特点：**
* 查找和插入的速度极快，不会随着key的增加而变慢；而list查找和插入的时间随着元素的增加而增加；
* 需要占用大量的内存，内存浪费多；而list占用空间小，浪费内存很少。


```python
dict0={123:1,
       'secend':'hello',
       (1,2,3):{'123':1,'secend':'hello'}
      }
print(dict0)

# dict()函数在创建字典时，参数有多种形式
dict1 = dict([('a',1),('b',2),('c',3)]) #元素为元组的列表
dict2 = dict({('a',1),('b',2),('c',3)}) #元素为元组的集合
dict3 = dict((('a',1),('b',2),('c',3))) #元素为元组的元组
dict4 = dict([['a',1],['b',2],['c',3]]) #元素为列表的列表
dict5 = dict(a=1,b=2,c=3)               #直接指定具体键值,这种方法里键只能为字符串，且写的时候不能加''
print(dict1)
print(dict2)
print(dict3)
print(dict4)
print(dict5)
```

>  File "<ipython-input-4-0da489d2f8a7>", line 12
>  dict5 = dict((1,2)=1,b=2,c=3)               #直接指定具体键值
>               ^
>  SyntaxError: expression cannot contain assignment, perhaps you meant "=="?



字典里只允许存在一个key，创建时如果同一个key被赋值两次，后一个值会被记住


```python
dict = {'Name': 'Runoob', 'Age': 7, 'Name': '小菜鸟'}
 
print ("dict['Name']: ", dict['Name'])
```

> dict['Name']:  小菜鸟



### 字典的key必须是不可变对象的原因

因为dict根据key来计算value的存储位置，如果每次计算相同的key得出的结果不同，那dict内部就完全混乱了。

这个通过key计算位置的算法称为哈希算法（Hash）。要保证hash的正确性，作为key的对象就不能变。在Python中，字符串、数字、元组都是不可变的，因此，可以放心地作为key



## 二、字典的索引和操作符

字典不能像列表和元组一样通过位置来索引，只能通过keys来索引。字典是可变的，所以可以通过索引添加、修改、删除字典


```python
dict1 = dict([['a',1],['b',2],['c',3]])
dict1['d']=4    # 添加键值对
dict1['a']+=1   # 修改值
print(dict1)
```

> {'a': 2, 'b': 2, 'c': 3, 'd': 4}



删除需要用到`del`操作符

* del dict[ ]用于删除键值对
* del dict_name用于删除整个字典


```python
dict1 = dict([['a',1],['b',2],['c',3]])
del dict1['a']
print(dict1)
del dict1
print(dict1)
```

> {'b': 2, 'c': 3}
>
> NameError: name 'dict1' is not defined




操作符`in`可以判断键是否在字典中,不能直接判断值是否在其中


```python
dict1={123:1,
       'secend':'hello',
       (1,2,3):{'123':1,'secend':'hello'}
      }
123 in dict1
```

> True



## 三、字典常用函数

[![dict-function.png](https://z3.ax1x.com/2021/09/12/49R5tS.png)](https://imgtu.com/i/49R5tS)



## 四、字典常用方法

### 1. 获取字典内容的方法
* `dict.keys()`,`dict.values()`,`dict.items()`分别以**迭代器**形式返回字典中的所有键、值、键值对。可以用list()函数将迭代器转化为列表，见后面迭代器内容。通常会搭配fo循环使用

**方法使用时记得加括号！！！容易漏**


```python
dict1={123:1,
       'secend':'hello',
       (1,2,3):{'123':1,'secend':'hello'}
      }
print(dict1.keys())
print(dict1.values())
print(dict1.items())
print(list(dict1.keys()))
for x,y in dict1.items():
    print(x,y)
```

>dict_keys([123, 'secend', (1, 2, 3)])
>dict_values([1, 'hello', {'123': 1, 'secend': 'hello'}])
>dict_items([(123, 1), ('secend', 'hello'), ((1, 2, 3), {'123': 1, 'secend': 'hello'})])
>[123, 'secend', (1, 2, 3)]
>123 1
>secend hello
>(1, 2, 3) {'123': 1, 'secend': 'hello'}




* 方法`dict.get(key, default=None) `可以返回指定键的值，如果键不在字典中返回默认值None。默认值并不影响原字典中的键值对
    * key -- 字典中要查找的键  
    * default -- 如果指定键的值不存在时，返回该默认值


```python
dict1={123:1,
       'secend':'hello',
       (1,2,3):{'123':1,'secend':'hello'}
      }
dict1.get(123)
print(dict1.get(123))
print(dict1.get(789)) # 返回默认的None
print(dict1.get(789,'python')) # 返回指定的python
print(dict1)  # 并没有改变原字典
```

>1
>None
>python
>{123: 1, 'secend': 'hello', (1, 2, 3): {'123': 1, 'secend': 'hello'}}


* 方法`dict.setdefault(key, default=None) `也会返回对应的值。如果 key 不在字典中，则插入 key 及设置的默认值，并返回默认值 ，默认值为 None


```python
dict1={123:1,
       'secend':'hello',
       (1,2,3):{'123':1,'secend':'hello'}
      }
abc = dict1.setdefault(789,'python')
print(abc)
print(dict1)
```

>python
>{123: 1, 'secend': 'hello', (1, 2, 3): {'123': 1, 'secend': 'hello'}, 789: 'python'}



### 2.删除字典元素的方法

* `dict.pop(key[,default])`用于删除字典中的键值对，key值必须给出，**返回被删除的value**。如果没有 key，返回 default 值


```python
dict1={'first':123,
       'secend':456,
       'third':789
      }
a = dict1.pop('first')
print(a)
print(dict1)
```

>123
>{'secend': 456, 'third': 789}


* `popitem()`方法删除字典中的最后一对**键和值**，并以元组形式返回。如果字典已经为空，就报出KeyError异常


```python
dict1={'first':123,
       'secend':456,
       'third':789
      }
a = dict1.popitem() # 以元组形式返回键和值，而pop只返回值
print(type(a))
print(a)
print(dict1)
```

><class 'tuple'>
>('third', 789)
>{'first': 123, 'secend': 456}


* `dict.clear()`删除字典内所有元素


```python
dict2={'first':123,
       'secend':456,
       'third':789
      }
dict2.clear()
print(dict2)
```

> {}


### 3.更新字典的方法

* `dict.update(dict1)` 将dict1更新到指定字典dict里。所谓更新，即没有就添加，有相同的就修改，以dict1中的为准


```python
dict3={'hello':123,'python':456}
dict4={'world':789,'python':100}
dict3.update(dict4)
print(dict3)
```

> {'hello': 123, 'python': 100, 'world': 789}


### 4.复制字典的方法
* `dict.copy()`函数返回一个字典的**浅复制**，浅复制就是拷贝一个新的对象给另一个变量，改变新对象并不会影响原对象


```python
dict1 = {'first':123,
       'secend':456,
       'third':789
      }
dict2 = dict1.copy()
print (dict2)
dict2['first'] = 'abc'
print (dict1)
print (dict2)  # 改变dict2但未改变dict1
```

>{'first': 123, 'secend': 456, 'third': 789}
>{'first': 123, 'secend': 456, 'third': 789}
>{'first': 'abc', 'secend': 456, 'third': 789}


### 5.创建新字典的方法
* `dict.fromkeys(seq[, value])`函数用于创建一个新字典，以**序列 seq 中元素**做字典的**键**，**value**为字典**所有键的初始值**,value只能输入一个值


```python
list1 = ['first','secend','third']
dict2 = dict1.fromkeys(list1,'python')
dict2
```

> {'first': 'python', 'secend': 'python', 'third': 'python'}

