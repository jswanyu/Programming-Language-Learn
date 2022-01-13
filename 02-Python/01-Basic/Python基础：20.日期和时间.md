### 这个章节建议用到啥查啥

### 日期和时间
* Python 提供了一个 time 和 calendar 模块可以用于格式化日期和时间

#### time.time()
* 函数 time.time() 用于获取当前时间戳
* 时间间隔是以秒为单位的浮点小数，每个时间戳都以自从 1970 年 1 月 1 日午夜（历元）经过了多长时间来表示


```python
import time
print(time.time())
```

> 1583160214.5189838



#### time.localtime()

* 函数time.localtime()用于将返回浮点数的时间戳方式向时间元组转换
* 可以将时间戳作为参数输入，也可以不输入


```python
localtime = time.localtime(time.time())
print(localtime)
print(time.localtime())
```

>time.struct_time(tm_year=2020, tm_mon=3, tm_mday=2, tm_hour=22, tm_min=48, tm_sec=23, tm_wday=0, tm_yday=62, tm_isdst=0)
>time.struct_time(tm_year=2020, tm_mon=3, tm_mday=2, tm_hour=22, tm_min=48, tm_sec=23, tm_wday=0, tm_yday=62, tm_isdst=0)



时间元组是用一个元组装起来的9组数字处理时间

[![time-tuple.png](https://z3.ax1x.com/2021/09/12/49WKcd.png)](https://imgtu.com/i/49WKcd)

#### time.strftime()
* time.strftime()用于格式化日期

  [![time-format.png](https://z3.ax1x.com/2021/09/12/49Wu1H.png)](https://imgtu.com/i/49Wu1H)


```python
# 格式化成2020-03-02 22:47:23形式
print (time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()))
```

> 2020-03-02 22:47:23


#### calendar模块
* Calendar模块有很广泛的方法用来处理年历和月历


```python
import calendar

print(calendar.month(2020,3))
```

         March 2020
    Mo Tu We Th Fr Sa Su
                       1
     2  3  4  5  6  7  8
     9 10 11 12 13 14 15
    16 17 18 19 20 21 22
    23 24 25 26 27 28 29
    30 31


​    
