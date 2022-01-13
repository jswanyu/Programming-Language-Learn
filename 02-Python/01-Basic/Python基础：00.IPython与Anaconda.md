# 一、IPython

IPython是一个python的交互式shell，支持变量自动补全，自动缩进，支持bash shell命令，内置了许多很有用的功能和函数,是利用Python进行科学计算和交互可视化的一个最佳的平台。



## 1.运行IPython

可以在命令提示符中输入`ipython`直接进入ipython

[![run-ipython.png](https://z3.ax1x.com/2021/09/12/49WFn1.png)](https://imgtu.com/i/49WFn1)



## 2.各种命令和工具


### 2.1 Tab补全
ipython 支持使用 `<tab>` 键自动补全命令。包括搜索命名空间、补全方法、属性、模块、函数、参数的名称

IPython默认隐藏私有方法和属性，必须要先输入下划线才能看到

[![tab-ipython-1.png](https://z3.ax1x.com/2021/09/12/49WZtO.png)](https://imgtu.com/i/49WZtO)



[![tab-ipython-2.png](https://z3.ax1x.com/2021/09/12/49Wn9e.png)](https://imgtu.com/i/49Wn9e)



### 2.2 自省

* python使用`?`显示关于对象的概要信息和函数的帮助文档

  [![introspection-1.png](https://z3.ax1x.com/2021/09/12/49RLmq.png)](https://imgtu.com/i/49RLmq)

* python使用`??`显示函数的源代码

  [![introspection-2.png](https://z3.ax1x.com/2021/09/12/49RO00.png)](https://imgtu.com/i/49RO00)

* ？还可以作用于：把一些字符和通配符(* )结合在一起，会显示所有匹配通配符表达式的命名，暂时了解即可

  [![introspection-3.png](https://z3.ax1x.com/2021/09/12/49RX7V.png)](https://imgtu.com/i/49RX7V)



### 2.3 使用 `_` 使用上个cell的输出结果：


```python
a = 12
a
```

> 12




```python
_ + 13
```

> 25



### 2.4 可以使用 `!` 来执行一些系统命令。

后续再补充


```python
!ping baidu.com
```

> 正在 Ping baidu.com [220.181.38.251] 具有 32 字节的数据:
> 来自 220.181.38.251 的回复: 字节=32 时间=6ms TTL=43
> 来自 220.181.38.251 的回复: 字节=32 时间=6ms TTL=43
> 来自 220.181.38.251 的回复: 字节=32 时间=6ms TTL=43
> 来自 220.181.38.251 的回复: 字节=32 时间=6ms TTL=43
>
> 220.181.38.251 的 Ping 统计信息:
>     数据包: 已发送 = 4，已接收 = 4，丢失 = 0 (0% 丢失)，
> 往返行程的估计时间(以毫秒为单位):
>     最短 = 6ms，最长 = 6ms，平均 = 6ms




### 2.5 当输入出现错误时，**ipython**会指出出错的位置和原因：


```python
1 + "hello"
```

>
> TypeError                                 Traceback (most recent call last)
>
> <ipython-input-4-92709354cfb6> in <module>
> ----> 1 1 + "hello"
>
> TypeError: unsupported operand type(s) for +: 'int' and 'str'




### 2.6 ipython magic命令

ipython解释器提供了很多以百分号`%`开头的`magic`命令，这些命令很像linux系统下的命令行命令（事实上有些是一样的）。

可以用命令`%lsmagic`查看所有的`magic`命令：

其中，

`Available line magic` 以一个百分号开头，作用与一行；

`Available cell magic` 以两个百分号开头，作用于整个cell。

最后一行，

`Automagic is ON, % prefix IS NOT needed for line magics.`说明在此时即使不加上`%`也可以使用作用于一行的命令。  

`Automagic`是一种特性，是指这些作用于一行的magic命令不加%也可以使用，只要没有变量被定义为与magic命令相同的名字。  

通过`%automagic`进行启用/禁用，**作用于整个cell的命令还是需要加上%%**


```python
%lsmagic
```

> Available line magics:
> %alias  %alias_magic  %autoawait  %autocall  %automagic  %autosave  %bookmark  %cd  %clear  %cls  %colors  %conda  %config  %connect_info  %copy  %ddir  %debug  %dhist  %dirs  %doctest_mode  %echo  %ed  %edit  %env  %gui  %hist  %history  %killbgscripts  %ldir  %less  %load  %load_ext  %loadpy  %logoff  %logon  %logstart  %logstate  %logstop  %ls  %lsmagic  %macro  %magic  %matplotlib  %mkdir  %more  %notebook  %page  %pastebin  %pdb  %pdef  %pdoc  %pfile  %pinfo  %pinfo2  %pip  %popd  %pprint  %precision  %prun  %psearch  %psource  %pushd  %pwd  %pycat  %pylab  %qtconsole  %quickref  %recall  %rehashx  %reload_ext  %ren  %rep  %rerun  %reset  %reset_selective  %rmdir  %run  %save  %sc  %set_env  %store  %sx  %system  %tb  %time  %timeit  %unalias  %unload_ext  %who  %who_ls  %whos  %xdel  %xmode
>
> Available cell magics:
> %%!  %%HTML  %%SVG  %%bash  %%capture  %%cmd  %%debug  %%file  %%html  %%javascript  %%js  %%latex  %%markdown  %%perl  %%prun  %%pypy  %%python  %%python2  %%python3  %%ruby  %%script  %%sh  %%svg  %%sx  %%system  %%time  %%timeit  %%writefile
>
> Automagic is ON, % prefix IS NOT needed for line magics.





#### 常用到的magic命令：
* 使用 `%pwd` 查看当前工作文件夹：`print working directory`


```python
%pwd
```

> 'D:\\codework\\python\\Jupyter Notebook\\Python'



* 一些magic命令也像python函数一样，输出可以赋给一个变量


```python
dir1 = %pwd
dir1
```

> 'D:\\codework\\python\\Jupyter Notebook\\Python'



* 使用 `%mkdir` 产生新文件夹：`make directory`


```python
%mkdir demo_test 
```

[![%mkdir demo test.png](https://z3.ax1x.com/2021/09/12/49R6ld.png)](https://imgtu.com/i/49R6ld)



* 使用 `%cd` 改变目录：`change directory`


```python
%cd demo_test/
```

> D:\codework\python\Jupyter Notebook\Python\demo_test



* 使用 `%%writefile` 将cell中的内容写入文件：
  [![%%writefile helloworld.png](https://z3.ax1x.com/2021/09/12/49Rc6A.png)](https://imgtu.com/i/49Rc6A)


```python
%%writefile hello_world.py
print('hello world !')
```

> Writing hello_world.py



* 使用 `%run` 命令来运行脚本文件：


```python
%run hello_world.py
```

> hello world !



* 使用 `%load` 命令导入脚本文件，导入之后，程序语句会自动导入到cell中，%load语句会自动注释


```python
%load hello_world.py
```


```python
# %load hello_world.py
print('hello world !')
```



* 使用 `%ls` (list files）查看当前工作文件夹的文件：


```python
%ls
```

>  驱动器 D 中的卷是 软件
>  卷的序列号是 3689-CB9D
>
>  D:\codework\python\Jupyter Notebook\Python\demo_test 的目录
>
> 2021/08/30  15:25    <DIR>          .
> 2021/08/30  15:25    <DIR>          ..
> 2021/08/30  15:25                24 hello_world.py
>                1 个文件             24 字节
>                2 个目录 369,339,363,328 可用字节



* python中删除某个文件：导入os模块，调用os.remove()函数  
  **注意函数的参数是字符串**


```python
import os
os.remove('hello_world.py')
```

查看当前文件夹，`hello_world.py` 已被删除：


```python
%ls
```

>  驱动器 D 中的卷是 软件
>  卷的序列号是 3689-CB9D
>
>  D:\codework\python\Jupyter Notebook\Python\demo_test 的目录
>
> 2021/08/30  15:28    <DIR>          .
> 2021/08/30  15:28    <DIR>          ..
>                0 个文件              0 字节
>                2 个目录 369,339,363,328 可用字节



* `%cd ..`返回上一层文件夹：


```python
%cd ..
```

> D:\codework\python\Jupyter Notebook\Python



* 使用 `%rmdir` 删除文件夹：`remove directory`


```python
%rmdir demo_test
```



* 使用 `%hist` 查看历史命令：


```python
%hist
```

> a = 12
> a
> _ + 13
> !ping baidu.com
> 1 + "hello"
> %lsmagic
> %pwd
> dir1 = %pwd
> dir1
> %mkdir demo_test
> %cd demo_test/
> %%writefile hello_world.py
> print('hello world !')
> %run hello_world.py
> %load hello_world.py
> %ls
> import os
> os.remove('hello_world.py')
> %ls
> %cd ..
> %rmdir demo_test
> %hist



* 使用 `%whos` 查看当前的变量空间：


```python
%whos
```

> Variable   Type      Data/Info
> a          int       12
> dir1       str       D:\codework\python\Jupyter Notebook\Python
> os         module    <module 'os' from 'D:\\Anaconda3\\lib\\os.py'>



* 使用 `%reset` 重置当前变量空间：


```python
%reset
```

> Once deleted, variables cannot be recovered. Proceed (y/[n])? y



* 再查看当前变量空间：


```python
%whos
```

> Interactive namespace is empty.



* `%timeit` 可以计算一段语句的执行时间


```python
import numpy as np
a = np.random.randn(100,100)
%timeit np.dot(a,a)
```

> 24.7 µs ± 1 µs per loop (mean ± std. dev. of 7 runs, 10000 loops each)





# 二、Anaconda

* Anaconda指的是一个开源的Python发行版本，其包含了conda、Python等180多个科学包及其依赖项，为了方便起见，我们一般选择直接安装Anaconda，而不是去安装python解释器，外网访问速度较慢时，可以使用国内镜像：https://mirror.tuna.tsinghua.edu.cn/anaconda/archive/   或者  https://repo.anaconda.com/archive/

* Anaconda还有非常好用的一点就是可以**搭建不同的python虚拟环境**，以应对第三方库的不同版本问题


* Jupyter notebook，安装anaconda时会附带Jupyter notebook，这是一种交互式的文档类型，可以用于代码、文本、数据可视化以及其他输出。内核使用ipython系统进行内部活动。也可以部署在服务器端，远程访问，非常好用。



第一次安装好 Anaconda 以后，可以在命令行输入以下命令使 Anaconda 保持最新：

    conda update conda
    conda update anaconda

可以使用它来安装，更新，卸载第三方的 **python** 工具包



更新最好还是使用conda，pip更新会导致环境问题（源自《利用python进行数据分析》1.4.4）

    conda install <some package>
    conda update <some package>
    conda remove <some package>



在安装或更新时可以指定安装的版本号，例如需要使用 `numpy 1.8.1`：

    conda install numpy=1.8.1
    conda update numpy=1.8.1



可以使用 `conda info` 查看conda的信息



可以用`conda list`查看库列表






# 三、python之禅


```python
import this
```

> The Zen of Python, by Tim Peters
>
> Beautiful is better than ugly.
> Explicit is better than implicit.
> Simple is better than complex.
> Complex is better than complicated.
> Flat is better than nested.
> Sparse is better than dense.
> Readability counts.
> Special cases aren't special enough to break the rules.
> Although practicality beats purity.
> Errors should never pass silently.
> Unless explicitly silenced.
> In the face of ambiguity, refuse the temptation to guess.
> There should be one-- and preferably only one --obvious way to do it.
> Although that way may not be obvious at first unless you're Dutch.
> Now is better than never.
> Although never is often better than *right* now.
> If the implementation is hard to explain, it's a bad idea.
> If the implementation is easy to explain, it may be a good idea.
> Namespaces are one honking great idea -- let's do more of those!



