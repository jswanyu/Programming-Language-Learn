# 模块
模块是一个包含所有你定义的函数和变量的文件，其后缀名是.py。模块可以被别的程序引入，以使用该模块中的函数等功能。

模块名要遵循Python变量命名规范，不要使用中文、特殊字符

先查看系统是否已存在该模块，检查方法是在Python交互环境执行import abc，若成功则说明系统存在此模块

## 一、import语句
`import moudle_name`  ：导入指定的py文件，此时这个py文件被认为是一个模块，当前脚本文件可以调用模块中定义好的参数和函数   
`import module_name as newname`  ：此语法可以用来简化模块名称，方便随时调用   
`from modname import name`  : 从模块中导入一个指定的部分到当前命名空间中，而不是导入整个模块  
`from modname import * `  : 此语法把一个模块的所有内容全都导入到当前的命名空间，但是其会被认为是一种“拙劣实践”   

一个模块只会被导入一次，不管执行了多少次import。这样可以防止导入模块被一遍又一遍地执行，在notebook中体现为第一次导入时会执行一下文件中的语句，后面再导入则不会  

在较大的模块中有很多参数与函数，为了不与当前文件中变量名混淆，最好在调用时使用modname.name的结构  


```python
import numpy as np
list1 = np.arange(10)
print(list1)
```

> [0 1 2 3 4 5 6 7 8 9]


## 二、搜索路径
当解释器遇到 import 语句，如果模块在当前的搜索路径就会被导入。 搜索路径是一个解释器会先进行搜索的所有目录的列表，是由一系列目录名组成的，Python解释器就依次从这些目录中去寻找所引入的模块  

搜索路径是在Python编译或安装的时候确定的，安装新的库应该也会修改。搜索路径被存储在sys模块中的path变量  

sys.path 输出是一个列表，其中第一项是空串''，代表当前目录，即我们执行python解释器的目录，对于脚本的话就是运行的脚本所在的目录


```python
import sys
sys.path
```

>['',
> 'D:\\software\\Anaconda3\\python36.zip',
> 'D:\\software\\Anaconda3\\DLLs',
> 'D:\\software\\Anaconda3\\lib',
> 'D:\\software\\Anaconda3',
> 'D:\\software\\Anaconda3\\lib\\site-packages',
> 'D:\\software\\Anaconda3\\lib\\site-packages\\win32',
> 'D:\\software\\Anaconda3\\lib\\site-packages\\win32\\lib',
> 'D:\\software\\Anaconda3\\lib\\site-packages\\Pythonwin',
> 'D:\\software\\Anaconda3\\lib\\site-packages\\IPython\\extensions',
> 'C:\\Users\\wanyu\\.ipython']



## 三、``'__main__'``

当**.py**文件被直接运行时，`if __name__ ==’__main__'`之下的代码块将被运行；当.py文件以模块形式被导入时，`if __name__ == '__main__'`之下的代码块不被运行


```python
if __name__ == '__main__':
    pass # 所需要执行的语句
```

## 四、作用域
在模块中定义的函数与变量一般分为外部不需要的和需要的，外部需要的定义为public，正常定义即可，外部不需要的定义成private，即在变量名前加`_`或`__`

## 五、包
包是一种管理 Python 模块命名空间的形式，采用"点模块名称"使用模块的时候，不用担心不同模块之间的全局变量相互影响。采用点模块名称这种形式也不用担心不同库之间的模块重名的情况

目录只有包含一个叫做 ``__init__.py`` 的文件才会被认作是一个包

``__init__.py``可以是空文件，也可以有Python代码，因为其本身就是一个模块

一般推荐使用`from Package import specific_submodule`语句来导入包中的模块

## 六、模块通用函数
### dir()函数
内置的函数 dir() 可以找到模块内定义的所有名称。以一个字符串列表的形式返回  

如果没有给定参数，那么 dir() 函数会罗列出当前定义的所有名称

