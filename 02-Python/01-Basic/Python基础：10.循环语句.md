# Python的循环语句

## 一、while循环
### 1.一般格式

```python
while 判断条件(condition):
	执行语句(statements)......
```

**注意冒号与缩进**，严格遵循循环格式。Python中没有do-while语句


```python
# 计算1到10的和
a=1
b=0
while a<=10:
    b+=a
    a+=1
print(b)
```

> 55


如果while循环体中只有一条语句，可以将该语句与while写在同一行中


```python
a={'wan','yu'}
while a:print(a.pop())   #while a: 其中a也是一种条件判断，类似a中只要有元素，就符合条件语句
```

>wan
>yu



### 2.无限循环

一直满足循环条件，一直执行循环语句。在有些场景比较有用（例如：服务器上的实时请求）

可以使用 CTRL+C 来退出当前的无限循环。但是在notebook中不管用


```python
while 1:
    a=input('请输入一个字母')
    print('你输入的字母是：',a)
```



### 3.while-else语句

循环+条件


```python
count = 0
while count < 5:
   print (count, " 小于 5")
   count = count + 1
else:
   print (count, " 大于或等于 5")
```

>0  小于 5
>1  小于 5
>2  小于 5
>3  小于 5
>4  小于 5
>5  大于或等于 5



## 二、for循环

[![for-circulation.png](https://z3.ax1x.com/2021/09/12/49RbXn.png)](https://imgtu.com/i/49RbXn)

如果序列中没有元素，则执行else


```python
a=[1,2,3,4,5]
for i in a:
     print(i)
else:
   print(a)
```

>1
>2
>3
>4
>5
>[1, 2, 3, 4, 5]



for循环可以通过解包遍历两个序列数据


```python
for x, y in [(1, 1), (2, 4), (3, 9)]:
     print(x, y)
```

>1 1
>2 4
>3 9



## 三、break和continue

### 1.break
break 语句可以**跳出整个 for 和 while 的循环体**。如果你从 for 或 while 循环中终止，任何对应的循环 else 块将不执行。  

**同时注意break语句只跳出``当前这一层``的for或while，若外面还有循环结构，则继续循环**

### 2.continue
continue 语句被用来告诉 Python **跳过当前循环轮中的剩余语句，进行下一轮循环**。



这两个语句通常都必须配合if语句使用  

**不要滥用break和continue语句。break和continue会造成代码执行逻辑分叉过多，容易出错。大多数循环并不需要用到break和continue语句，都可以通过改写循环条件或者修改循环逻辑**


```python
a = [10,11,12,13,14,15]
for i in a:
    if i%2 == 0:
        print (i)
    else:
        break
print (i)
```

>10
>11



```python
a = [10,11,12,13,14,15]
for i in a:
    if i%2 == 0:
        print (i)
    else:
        continue
print (i)
```

>10
>12
>14
>15



小练习：

[![break-test.png](https://z3.ax1x.com/2021/09/12/49RRmt.png)](https://imgtu.com/i/49RRmt)





## 四、pass语句

Python中pass是空语句，是为了保持程序结构的完整性，因为有时不加语句会报错。pass 不做任何事情，一般用做占位语句  
在代码段中或定义函数的时候，如果没有内容，或者先不做任何处理，直接跳过，就可以使用pass


```python
for i in [1,2,3]:
    pass
```

