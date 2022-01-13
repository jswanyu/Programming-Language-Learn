# Python的条件控制

## 一、基本语法：
[![condition-control.png](https://z3.ax1x.com/2021/09/12/49RfTf.png)](https://imgtu.com/i/49RfTf)

if语句执行有个特点，它是从上往下判断，如果在某个判断上是True，把**该判断对应的语句执行后，就忽略掉剩下的elif和else** 。因此若有的if条件写的不严谨导致，同时符合多个条件，只会执行第一个符合条件的语句块

python需要严格**遵循缩进规则**


```python
age = 20
if age >= 6:
    print('teenager')
elif age >= 18:       
    print('adult')
else:
    print('kid')
```

> teenager


当使用and和or进行混合条件判断时，条件判断是从左到右的并且在and和or两侧的条件不一定都执行


```python
a,b,c,d = 5,7,8,4
if a<b or c>d:     # c>d并不会判断，因为左边的a<b已经是True了
    print('ok')
```

> ok


链式比较也是可以的


```python
4>3>2>1
```

> True



## 二、三元表达式
python中三元表达式允许讲一个if-else代码块联合起来，在一行代码或一个语句中生成数据  

其格式为：`执行表达式1 if 判断表达式 else 执行表达式2 ` ，意思为，如果if判断表达式为True，则执行表达式1，否则执行表达式2  

**三元表达式中的else语句不能去掉！**

如果条件以及真假表达式非常复杂，不建议使用三元表达式，会牺牲可读性


```python
a = 10
value = 1 if a>5 else 0   # 
value
```

> 1




```python
a = 10
value = 1 if a>5
value
```

>  File "<ipython-input-3-d5a66a229922>", line 2
>  value = 1 if a>5
>                  ^
>  SyntaxError: invalid syntax



## 三、if的嵌套
if-elif-else可以嵌套在if结构中，同样使用缩进


```python
print('数字猜谜游戏')
real_number = 7
guess = 0
while guess!=real_number:
    guess = int(input('输入你猜的数字'))
    if guess==real_number:
        print('你猜对了')
    elif guess<real_number:
        print('你猜小了')
    elif guess>real_number:
        print('你猜大了')
```

>数字猜谜游戏
>输入你猜的数字3
>你猜小了
>输入你猜的数字8
>你猜大了
>输入你猜的数字7
>你猜对了


### 四、其他注意点
if必须严格遵循语法的形式，**不能加大括号什么的，容易与集合混淆**


```python
name ="pag"
if name == "pag":{
  print(name == "pag")  # 这里的是构建了一个集合
}
```

> True



```python
a = {print(name == "pag")}
print(type(a))
```

>True
><class 'set'>



```python
if name == "pag":{
  print(name == "pag")
  print("line 2 of if condition")  # 在{}中间多加一条语句，会报错，因为这是集合
}
```

>  File "<ipython-input-5-16af6f816c76>", line 3
>  print("line 2 of if condition")  # 在{}中间多加一条语句，会报错，因为这是集合
>      ^
>  SyntaxError: invalid syntax




```python
if name == "pag":{
  print(name == "pag"),
  print("line 2 of if condition")  # 再加一个逗号就能正常运行，因为集合中间可以加逗号
}
```

>True
>line 2 of if condition

