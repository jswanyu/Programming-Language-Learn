# 类：面向对象编程
[![oop.png](https://z3.ax1x.com/2021/09/12/49Wpp4.png)](https://imgtu.com/i/49Wpp4)

[![oop1.png](https://z3.ax1x.com/2021/09/12/49WPXR.png)](https://imgtu.com/i/49WPXR)

## 一、类的创建与使用
通过`class name:`来创建类，`obj = classname()`来实例化类，`obj.name`来调用属性  

类定义的各个方法与普通的函数只有一个特别的区别：它们必须有一个额外的第一个参数名称,，按照惯例它的名称是 self ，换成其他的也可以

**self经常忘写**

**self 代表的是类的实例，代表当前对象的地址，理解为C++的this指针**


```python
class people:
    name = 'python'
    def myname(self):
        print('my name is',self.name)
    def prt(self):
        print(self)
    def prt1(other):
        print(other)
a = people()
print(a.name)
b = people()
a.myname()
a.prt()
a.prt1()
b.prt()   # 能够看出a/b的地址是不同的
```

>python
>my name is python
><__main__.people object at 0x00000211984A00B8>
><__main__.people object at 0x00000211984A00B8>
><__main__.people object at 0x00000211984A0390>


类有一个名为 `__init__()` 的特殊方法（构造方法），该方法在类实例化时会自动调用，理解为C++的构造函数

`__init()__`方法可以有参数，类在实例化时，直接使用`obj = class(arg1,arg2,arg3...)`赋予实例化对象这些参数


```python
class myclass():
    def __init__(self,name,height,weight):
        self.n = name
        self.h = height
        self.w = weight
    def speak(self):
        print('my name is %s, my height is %d cm, my weight is %.1fkg' % (self.n,self.h,self.w))
a = myclass('wanyu',175,70)       
a.speak()
```

> my name is wanyu, my height is 175 cm, my weight is 70.0kg


## 二、继承
语法：`class DerivedClassName(modname.BaseClassName):`  

继承本文件中的类，则模块名可省略，子类继承父类，父类也叫基类

有关父类中`__init__`的继承与否，又会分为三种形式：

### 1.子类需要自动调用父类的方法：
子类不重写`__init__()`方法，实例化子类后，会自动调用父类的`__init__()`的方法  


```python
class people():
    name = ''    #规范
    height = 0
    weight = 0
    def __init__(self,n,h,w):
        self.name = n  # 对比上一段函数，详细的命名放在前面在调用时候更清晰的知道意思,上一段代码写的时候没考虑
        self.height = h
        self.weight = w
    def speak(self):
        print('my name is %s, my height is %d cm, my weight is %.1fkg' % (self.name,self.height,self.weight)) 
    
class student(people):
        pass  #调用父类的构造函数
a = student('wanyu',175,70)
a.speak()
```

> my name is wanyu, my height is 175 cm, my weight is 70.0kg


### 2.子类不需要自动调用父类的方法：
子类重写`__init__()`方法，实例化子类后，将不会自动调用父类的__init__()的方法


```python
class people():
    name = ''    #规范
    height = 0
    weight = 0
    def __init__(self,n,h,w):
        self.name = n  
        self.height = h
        self.weight = w
    def speak(self):
        print('my name is %s, my height is %d cm, my weight is %.1fkg' % (self.name,self.height,self.weight)) 
    
class student(people):
    grade = ''
    def __init__(self,g):
        self.grade = g

    def speak_grade(self):           # 定义新函数
        print('i am in %d grade' % self.grade)
        
a = student(9)
a.speak_grade()
```

> i am in 9 grade


### 3.子类重写`__init__()`方法又需要调用父类的方法：

* 使用`super`关键词：在子类定义`__init__`中加入： `super(student,self).__init__(arg1,arg2,...)`  
* 使用另一种方法：在子类定义`__init__`中加入：`父类名称.__init__(self,arg1,arg2,...)` 

**继承只能继承所有的父类init参数，不能继承部分**


```python
class people():
    name = ''    #规范
    height = 0
    weight = 0
    def __init__(self,n,h,w):
        self.name = n  
        self.height = h
        self.weight = w
    def speak(self):
        print('my name is %s, my height is %d cm, my weight is %.1fkg' % (self.name,self.height,self.weight)) 

class student(people):
    grade = ''
    
    def __init__(self,n,h,w,g):
        # super(student,self).__init__(n,h,w)
        people.__init__(self,n,h,w)  # 调用父类的init函数  这里必须传入people类的所有init参数
        self.grade = g
    def speak_grade(self):           # 定义新函数
        print('i am in %d grade' % self.grade)

a = student('wanyu',175,70,9)
a.speak()
a.speak_grade()
```

>my name is wanyu, my height is 175 cm, my weight is 70.0kg
>i am in 9 grade


如果只继承部分init参数，会报错


```python
class people():
    name = ''    #规范
    height = 0
    weight = 0
    def __init__(self,n,h,w):
        self.name = n  
        self.height = h
        self.weight = w
    def speak(self):
        print('my name is %s, my height is %d cm, my weight is %.1fkg' % (self.name,self.height,self.weight)) 

class student(people):
    grade = ''
    
    def __init__(self,n,h,g):
        # super(student,self).__init__(n,h,w)
        people.__init__(self,n,h)  # 这里只传入people类的所部分init参数，少传入参数w，报错
        self.grade = g
    def speak_grade(self):           # 定义新函数
        print('i am in %d grade' % self.grade)

a = student('wanyu',175,9)
```

> TypeError: __init__() missing 1 required positional argument: 'w'



### 4.方法重写

如果从父类继承的方法不能满足子类的需求，可以对其进行改写，这个过程叫方法的覆盖（override），也称为方法的重写/重载


```python
class people():
    name = ''    #规范
    height = 0
    weight = 0
    def __init__(self,n,h,w):
        self.name = n
        self.height = h
        self.weight = w
    def speak(self):
        print('my name is %s, my height is %d cm, my weight is %.1fkg' % (self.name,self.height,self.weight)) 

class student(people):
    grade = ''
    def __init__(self,n,h,w,g):
        people.__init__(self,n,h,w)  # 调用父类的init函数
        self.grade = g
    def speak(self):  # 方法重写
        print('my name is %s, my height is %d cm, my weight is %.1fkg. And i am in %d grade' % (self.name,self.height,self.weight,self.grade))

a = student('wanyu',175,70,9)
a.speak()
```

> my name is wanyu, my height is 175 cm, my weight is 70.0kg. And i am in 9 grade



### 5.多继承

子类可以继承多个父类，语法为`class DerivedClassName(Base1, Base2, Base3):`  

需要**注意圆括号中父类的顺序**，若是父类中有相同的方法名，而在子类使用时未指定，python从左至右搜索 即方法在子类中未找到时，从左到右查找父类中是否包含方法


```python
class people:
    #定义基本属性
    name = ''
    age = 0
    #定义私有属性,私有属性在类外部无法直接进行访问
    __weight = 0
    #定义构造方法
    def __init__(self,n,a,w):
        self.name = n
        self.age = a
        self.__weight = w
    def speak(self):
        print("%s 说: 我 %d 岁。" %(self.name,self.age))
               
class speaker():  # 另一个类，多重继承之前的准备
    topic = ''
    name = ''
    def __init__(self,n,t):
        self.name = n
        self.topic = t
    def speak(self):
        print("我叫 %s，我是一个演说家，我演讲的主题是 %s"%(self.name,self.topic))
        
class sample(speaker,people): #多重继承
    a =''
    def __init__(self,n,a,w,t):
        people.__init__(self,n,a,w)
        speaker.__init__(self,n,t)
 
test = sample("Tim",25,80,"Python")
test.speak()   #方法名同，默认调用的是在括号中排前地父类的方法
```

> 我叫 Tim，我是一个演说家，我演讲的主题是 Python



## 三、私有属性与私有方法

`__private_attrs`：私有变量，两个下划线开头，声明该属性为私有，不能在类的外部被使用或直接访问  

`__private_method`：两个下划线开头，声明该方法为私有方法，只能在类的内部调用 ，不能在类的外部调用

可以在间接使用，比如类的公用方法调用了私有属性，那么在外部调用公共方法时，就能调用私有属性


```python
class JustCounter:
    __secretCount = 0  # 私有变量，两个下划线开头，声明该属性为私有，不能在类的外部被使用或直接访问
    publicCount = 0    # 公开变量
 
    def count(self):
        self.__secretCount += 1  # 在类内部的方法中使用时 self.__private
        self.publicCount += 1
        print (self.__secretCount)

counter = JustCounter()
counter.count()  # print (self.__secretCount)执行，打印1
counter.count()  # print (self.__secretCount)执行，打印2
try:
    print (counter.publicCount)    # publicCount=2 ,打印2 
    print (counter.__secretCount)  # 报错，实例不能访问私有变量
except AttributeError:
    print('实例不能访问私有变量')
```

>1
>2
>2
>实例不能访问私有变量



```python
class Site:
    def __init__(self, name, url):
        self.name = name       # public
        self.__url = url       # private
 
    def who(self):
        print('name  : ', self.name)
        print('url : ', self.__url)
 
    def __foo(self):          # 私有方法，两个下划线开头，声明该方法为私有方法，只能在类的内部调用 ，不能在类的外部调用
        print('这是私有方法')
 
    def foo(self):            # 公共方法
        print('这是公共方法')
        self.__foo()          # 内部调用私有方法 

x = Site('菜鸟教程', 'www.runoob.com')
try:
    x.who()        # 正常输出，因为这是公有方法，虽然其中调用了私有属性，但这是在类的内部调用的私有属性
    x.foo()        # 正常输出，因为其在类的内部调用的私有方法
    x.__foo()      # 报错
except AttributeError:
    print('AttributeError：类的外部不能调用类的私有方法')
```

>name  :  菜鸟教程
>url :  www.runoob.com
>这是公共方法
>这是私有方法
>AttributeError：类的外部不能调用类的私有方法



## 四、类的专有方法

类一般有如下专有方法：

[![class own method.png](https://z3.ax1x.com/2021/09/12/49RgOI.png)](https://imgtu.com/i/49RgOI)



所有专有方法中，`__init__()`要求无返回值，或者返回 None。而其他方法，如`__str__()`、`__add__()`等，一般都是要返回值的。见运算符重载部分  


```python
class myclass():
    '这是帮助信息'
    def __init__(self):
        self.name = 'wanyu'

print(myclass.__doc__)
```

> 这是帮助信息



## 五、运算符重载

运算符重载是指通过使用类的专有方法对类的运算符进行重新的定义，这样在实例化时就会调用新的运算


```python
class people:
    def __init__(self,name,age):
        self.name=name
        self.age=age
        
class god:
    def __init__(self,name,age):
        self.name=name
        self.age=age
        
    def __str__(self):   # 重载类的专有方法__str__()
        return '这个人的名字是%s,已经有%d岁了！'%(self.name,self.age)     # 原本此类实例化为a时，a只是一个people对象，现在可以打印字符了

a=people('孙悟空',999)
b=god('孙悟空',999)
print(a)  # 不重载
print(b)  # 重载
```

><__main__.people object at 0x000002119852BDD8>
>这个人的名字是孙悟空,已经有999岁了！



类的专有方法中，也是存在默认优先级的，多个方法都有返回值，但**一般优先取` __str__() `的返回值**


```python
class Vector:
    def __init__(self, a, b):
        self.a = a
        self.b = b
    def __repr__(self):
        return 'Vector (%d, %d)' % (self.b, self.a)
    def __str__(self):
        return 'Vector (%d, %d)' % (self.a, self.b)

v1 = Vector(2,10)
print (v1)  # 结果是 Vector(2,10)，说明是按照__str__的return语句
print (v1.__repr__())
```

>Vector (2, 10)
>Vector (10, 2)



更详细的说明举例：
当解释器碰到 a+b 时，会做以下事情：   
从 a 类中找` __add__ `若返回值不是 NotImplemented, 则调用` a.__add__(b)`。  
那么在下面的例子中，解释器碰到v1+v2，先去v1类中查找` __add__ `，找到了之后，按照它的语句去执行。+这个运算执行完了，还得去优先级最高的`__str__`中找最终的返回形式


```python
class Vector:
    def __init__(self, a, b):
        self.a = a
        self.b = b
 
    def __str__(self):  # 重新定义str属性
        return '想加这两个数：%d, %d' % (self.a, self.b)   
   
    def __add__(self,other): # 重新定义加运算
        return Vector(self.a + other.a, self.b + other.b)

v1 = Vector(2,10)
v2 = Vector(5,-2)
print(v1)
print(v2)
print (v1 + v2)
```

>想加这两个数：2, 10
>想加这两个数：5, -2
>想加这两个数：7, 8



前面讲的是运算符重载的正向方法，还有反向方法。上面的例子中，若 a 类中没有` __add__ `方法，则检查 b 有没有` __radd__ `，这个就是反向方法。如果有，则调用 `b.__radd__(a)`，若没有，则返回 NotImplemented。

每个正规的运算符都有正向方法重载，反向方法重载。有一些有就地方法（即不返回新的对象，而是修改原本对象）  

下面的例子中，重新定义radd，让字符实现"新的加法"


```python
class Vector1:
    def __init__(self, a, b):
        self.a = a
        self.b = b 

class Vector2:
    def __init__(self, a, b):
        self.a = a
        self.b = b
    
    def __radd__(self,other): # 重新定义加运算
        return '想加这两组字符：（%s+%s）（%s+%s）' % (self.a,other.a,self.b,other.b)

v1 = Vector1('a','b')   # Vector1未定义add
v2 = Vector2('x','y')   # Vector1定义radd
print (v1 + v2)
```

> 想加这两组字符：（x+a）（y+b）

