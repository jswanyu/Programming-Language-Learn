# 一、列表生成式
Python内置的非常简单却强大的可以用来创建list的生成式，即在可迭代对象中将满足条件的元素生成为一个列表，条件可以省略

其结构为：`[x表达式 for x in 可迭代对象 if 判断语句] `  


与下面的for循环是等价的 
```python
result = []
for val in collection:
    if condition:
        result.append(expr)
```


```python
[x * x for x in range(1,11)]
```

> [1, 4, 9, 16, 25, 36, 49, 64, 81, 100]



### 1.表达式还可以调用方法


```python
L = ['Hello', 'World', 'IBM', 'Apple']  
[x.lower() for x in L if len(x)>3]  # 字符串变成小写,且筛选长度大于3的字符串
```

> ['hello', 'world', 'apple']




```python
# 练习
L = ['Hello', 'World', 18, 'Apple', None]  # 把列表中字符串提取出来生成新列表，并小写
[x.lower() for x in L if isinstance(x, str) == 1]  # isinstance函数可以判断一个变量是不是对应的类型
```

>['hello', 'world', 'apple']



### 2.使用三元表达式
x表达式可以使用三元表达式进一步确定x的值，但一定要注意**需要明确计算出x才行**

**务必注意：**
如果涉及到if-else，要注意for语句前后位置的区别，for语句前面是一个表达式，它必须根据元素计算出一个结果，后面是一个筛选条件。因此需要将三元表达式放在for的前面，for后面只能有if，不能有else，否则不能达到筛选的效果  


```python
# 现在要实现一个列表生成式，将1-10偶数不动，奇数赋0
[x for x in range(1, 11) if x % 2 == 0 else 0]  # 筛选条件中不能放else，否则不能达到筛选的效果
```

> SyntaxError: invalid syntax




```python
[x if x % 2 == 0 for x in range(1, 11)]  # 前面的这个表达式必须要能根据x算出结果，但此时少else，表达模糊，计算不出结果，并不是正确的三元表达式
```

> SyntaxError: invalid syntax




```python
[x if x % 2 == 0 else 0 for x in range(1, 11)]  # x if x % 2 == 0 else 0 能够根据x算出精确的结果
```

> [0, 2, 0, 4, 0, 6, 0, 8, 0, 10]




```python
[x for x in range(1, 11) if x % 2 == 0 ] # 如果只是想筛选2的整数倍，把if条件放在后面
```

> [2, 4, 6, 8, 10]



### 3.列表生成式的for循环可以使用两个甚至多个变量


```python
d = {'x': 'A', 'y': 'B', 'z': 'C' }
[a + '=' + b for a,b in d.items()]
```

> ['x=A', 'y=B', 'z=C']



### 4.嵌套列表生成式
当在**表达式中出现两个对象**，或者是**嵌套的列表**等情况就可以使用两层循环,**嵌套列表生成式执行的顺序一定是和写成for循环的顺序相同**，一般也就是先执行第一个for循环。三层及以上的用的不多，可读性较差


```python
[m * n for m in range(1,5) for n in range(2,4)]
```

> [2, 3, 4, 6, 6, 9, 8, 12]




```python
[m + n for m in 'ABC' for n in 'XY']  # 先A 再XY  先B 再XY 先C 再XY
```

> ['AX', 'AY', 'BX', 'BY', 'CX', 'CY']




```python
list1 = [(1,2,3),(4,5,6),(7,8,9)]  # 想将次嵌套式的列表扁平化为一个一维的列表
# 如果采用普通for循环
list_demo = []
for x in list1:
    for y in x:
        list_demo.append(y)
list_demo
```

> [1, 2, 3, 4, 5, 6, 7, 8, 9]




```python
list1 = [(1,2,3),(4,5,6),(7,8,9)]  # 想将次嵌套式的列表扁平化为一个一维的列表
# 如果采用嵌套列表生成式,更简洁
list_demo = [y for x in list1 for y in x]  # 谨记嵌套列表生成式执行的顺序一定是和写成for循环的顺序相同
list_demo
```

> [1, 2, 3, 4, 5, 6, 7, 8, 9]



还有一个情况是**列表生成式中的列表生成式**，他与嵌套列表生成式是不同的，它是**先执行后面的for语句，即把后面的for循环放在最外层循环**  
他是在列表中生成列表，注意区分


```python
list1 = [(1,2,3),(4,5,6),(7,8,9)]
list_demo1 = [[y for y in x ]for x in list1]
print(list_demo1)
```

> [[1, 2, 3], [4, 5, 6], [7, 8, 9]]



```python
matrix = [           
    [1, 2, 3, 4],
    [5, 6, 7, 8],
    [9, 10, 11, 12]
]
[[row[i] for row in matrix] for i in range(4)]   # 将3X4的矩阵列表转换为4X3列表
```

> [[1, 5, 9], [2, 6, 10], [3, 7, 11], [4, 8, 12]]




```python
list(zip(*matrix)) # 也可以通过zip函数解包
```

> [(1, 5, 9), (2, 6, 10), (3, 7, 11), (4, 8, 12)]




```python
transposed = []
for i in range(4):
    transposed.append([row[i] for row in matrix])
transposed
```

> [[1, 5, 9], [2, 6, 10], [3, 7, 11], [4, 8, 12]]





# 二、集合生成式

把列表生成式的`[]`换为{}就成为集合生成式


```python
strings = ['a','as','bat','car','dove','python']
unique_strings = {len(x) for x in strings}  # 将列表中字符串的长度转为集合
unique_strings
```

> {1, 2, 3, 4, 6}




```python
# 也可以用map函数到达同样的效果
set(map(len,strings))
```

> {1, 2, 3, 4, 6}





# 三、字典生成式

除了把列表生成式的`[]`换为{}，在最开始的表达式中需要写成键值对的形式，这样就成为了字典生成式


```python
strings = ['a','as','bat','car','dove','python']
dict_strings = {key:value for key,value in enumerate(strings)}  # 使用enumerate函数创造索引值
dict_strings
```

> {0: 'a', 1: 'as', 2: 'bat', 3: 'car', 4: 'dove', 5: 'python'}





**使用小括号包裹生成式会变成生成器对象，而不是元组**

