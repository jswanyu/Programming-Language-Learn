# 集合
集合（set）和dict类似，也是一组key的集合 **（不可变对象！）**，但不存储value。由于key不能重复，所以，在set中，没有重复的key

## 一、集合的创建
可以使用大括号 `{ }` 或者 `set() `函数创建集合。创建一个**空集合**必须用 set() 而不是 { }，因为 { } 是用来创建一个空字典


```python
set1={1,2,3,4,5,1,2,6,8,3,4} # 重复元素在set中自动被过滤
set2 = {(1,2),(3,4)}
set3=set('python')
print(set1)
print(set2)
print(set3)
```

>{1, 2, 3, 4, 5, 6, 8}
>{(1, 2), (3, 4)}
>{'n', 'p', 'y', 'o', 'h', 't'}


set()函数创建字符串集合的注意点：
* 用set()创建字符串集合时，如果小括号内只有一个字符串，会拆分为单个字母
* 用set()创建多个字符串，需要将多个字符串用[]{}()括起来


```python
s2 = {'hello','python'}   # 用{}创建的时候很正常
s3 = set('python')     
s4 = set(('python','hello'))#
print(s2)
print(s3)
print(s4)
```

>{'python'}
>{'hello', 'python'}
>{'n', 'p', 'y', 'o', 'h', 't'}
>{'hello', 'python'}



python中还有一种集合是不可变集合：不能再添加或删除任何元素的集合 (用到再详细展开) 

通过内置函数`frozenset()`创建，参数为iterable，返回frozenset对象，如果不提供任何参数，默认会生成空集合

需要使用不可变集合的原因：

在集合的关系中，有集合的中的元素是另一个集合的情况，但是普通集合（set）本身是可变的，那么它的实例就不能放在另一个集合中（set中的元素必须是不可变类型）。所以，frozenset提供了不可变的集合的功能，当集合不可变时，它就满足了作为集合中的元素的要求，就可以放在另一个集合中了。


```python
a = frozenset(range(10))
s1 = {a}
print(type(a))
print(type(s1))
```

><class 'frozenset'>
><class 'set'>



## 二、集合的操作符

### 2.1 运算操作符
* 交集a&b 
* 并集a|b ,
* 差集：集合a中包含而集合b中不包含的元素a-b ，b-a相反
* 两个集合中不重复的元素集合： a^b
* 集合的相等判定：当且仅当两个集合的内容一摸一样时，两个集合才相等


```python
a=set('abcdefg')
b=set('aceghjk')
print(a|b)
print(a&b)
print(a-b)
print(a^b)
print({1,2,3} == {3,2,1})
```

>{'a', 'd', 'c', 'b', 'g', 'j', 'h', 'e', 'f', 'k'}
>{'a', 'c', 'g', 'e'}
>{'d', 'f', 'b'}
>{'d', 'j', 'b', 'h', 'f', 'k'}
>True


集合的部分方法能够实现上述相同功能：  
* union() 方法返回两个集合的并集，重复的元素只会出现一次。set.union(set1, set2...)
* intersection() 方法用于返回两个或更多集合的交集。set.intersection(set1, set2 ...)
* difference() 方法用于返回集合的差集。set.difference(set)
* symmetric_difference() 方法返回两个集合中不重复的元素集合



### 2.2 其他操作符

in用于判断元素是否在集合中


```python
s1={1,2,3,4,5,1,2,6,8,3,4}
3 in s1
```

> True





## 三、集合的常用函数

* len(s)计算集合元素个数


```python
s1={1,2,3,4,5,1,2,6,8,3,4}
len(s1)
```

> 7





## 四、集合的常用方法

### 1.添加元素
* s.add( x )，将元素 x 添加到集合 s 中，如果元素已存在，则不进行任何操作
* s.update( x ),可以将新的元素或集合更新到当前集合中。参数可以是列表、元组等


```python
s1={1,2,3,4,5,6}
s1.add('12')
s1.update([8,9])
s1
```

> {1, '12', 2, 3, 4, 5, 6, 8, 9}




```python
# s.update( "字符串" ) 与 s.update( {"字符串"} ) 含义不同:，与最开始使用set()函数建立字符串集合时类似
thisset = set(("Google", "Runoob", "Taobao"))
print(thisset)
thisset.update({"Facebook"})
print(thisset)
thisset.update("Yahoo")
print(thisset)
```

>{'Google', 'Taobao', 'Runoob'}
>{'Google', 'Taobao', 'Facebook', 'Runoob'}
>{'Taobao', 'Runoob', 'o', 'Y', 'Facebook', 'a', 'h', 'Google'}



### 2.删除元素

* s.remove( x ),将元素 x 从集合 s 中移除，如果元素不存在，则会发生错误
* s.discard( x ),也是移除集合中的元素，且如果元素不存在，不会发生错误
* s.pop(),随机返回集合中的一个元素。**pop()没有参数，随机删除一个**
* s.clear()清空集合


```python
s1={1,2,3,4,5,6}
s1.remove(2)
s1.discard(3)
s1.discard(7)
print(s1)
```

> {1, 4, 5, 6}



```python
s1={1,2,3,4,5,6}
s2=set('python')
a = s1.pop()
b = s2.pop()
print(s1)
print(s2)
print(a,b)
```

>{2, 3, 4, 5, 6}
>{'p', 'y', 'o', 'h', 't'}
>1 n



```python
s1.clear()
s1
```

> set()



### 3.其他方法
[![set-method1.png](https://z3.ax1x.com/2021/09/12/49WVAK.png)](https://imgtu.com/i/49WVAK)

[![set-method2.png](https://z3.ax1x.com/2021/09/12/49Wk0x.png)](https://imgtu.com/i/49Wk0x)
