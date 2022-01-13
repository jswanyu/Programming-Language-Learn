# Java常用类

# 一、String类

## 1 String类的概述

String：字符串，使用一对`“ ”`引起来表示。

* String声明为**final的，不可被继承**
* String实现了Serializable接口：表示字符串是支持序列化的。
            实现了Comparable接口：表示String可以比较大小
* String内部定义了final char[] value用于存储字符串数据
* String:代表不可变的字符序列。简称：不可变性。
  

## 2 理解String类的不可变性

通过字面量的方式（区别于new）给一个字符串赋值，此时的字符串值声明在方法区（字符串常量池）字符串常量池中，字符串常量池中是不会存储相同内容的字符串。所谓的不可变性用通俗的话讲就是，有操作想改变一个String变量，可以是可以，但别连带着把别人也变了。

* 当对字符串重新赋值时，需要重写指定内存区域赋值，不能改动原有的value值。

下面这句代码是通过字面量的方式给一个字符串赋值，此时的字符串值创建在字符串常量池中。然后在栈区创建一个变量，指向字符串的地址。

`String s1 = "abc";`

如果再创建一个字符串变量也赋值为`"abc"`，并不会在常量池再创建一个`"abc"`，而是将新的变量也指向以前创建好的`"abc"`，即字符串常量池中是不会存储相同内容的字符串的。

`String s2 = "abc";`

验证方法可以使用等号，查看两个变量地址是否相同

```java
@Test
public void test01(){
    String s1 = "abc";
    String s2 = "abc";
    System.out.println(s1==s2);  //true
}
```

当写下句代码对字符串重新赋值时，需要在字符串常量池中重新建立一个字符串`"hello"`，然后让s1指向它

`String s1 = "hello";`



* 当对现有的字符串进行连接操作时，也需要重新指定内存区域赋值，不能改动原有的value值。

  下面的代码也是在字符串常量池中新建一个区域赋值为“abcdef”

```java
@Test
public void test02(){
    String s1 = "abc";
    String s2 = "abc";
    s2 += "def";
    System.out.println(s2);
    System.out.println(s1); //原"abc"并没有改变
}
```



* 当调用String的replace()方法修改指定字符或字符串时，也需要重新指定内存区域赋值，不能改动原有的value值。

```java
@Test
public void test03(){
    String s1 = "abc";
    String s2 = s1.replace('a','m');
    System.out.println(s2);
    System.out.println(s1); //原"abc"并没有改变
}
```



## 3 String实例化的区别

* 实例化方法的区别。String类有下面不同的实例化方式，一般常用的只有前两种

```java
String str = "abc";

//本质上this.value = new char[0];
String  s1 = new String("abc"); 

//this.value = original.value;
String  s2 = new String(String original); 

//this.value = Arrays.copyOf(value, value.length);
String  s3 = new String(char[] a);

String  s4 = new String(char[] a,int startIndex,int count);
```

方式一：通过字面量定义的方式，将字符串常量`"abc"`存储在字符串常量池，目的是共享。

方式二：通过new + 构造器的方式，字符串非常量对象存.储在堆中，堆中的对象又有一个value的字段，其指向常量池中的字符串

[![String实例化.png](https://z3.ax1x.com/2021/10/22/5cmVoT.png)](https://imgtu.com/i/5cmVoT)

```java
@Test
public void test04(){
    //通过字面量定义的方式：此时的s1和s2的数据abc声明在方法区中的字符串常量池中。
    String s1 = "abc";
    String s2 = "abc";

    //通过new + 构造器的方式:此时的s3和s4保存的地址值，是数据在堆空间中开辟空间以后对应的地址值。
    String s3 = new String("abc");
    String s4 = new String("abc");

    //如果是调用equals方法自然是都相等
    System.out.println(s1 == s2);//true
    System.out.println(s1 == s3);//false
    System.out.println(s3 == s4);//false，这是两个对象，自然也不相等，但它们内部的value地址上是相同的
}
```



面试题：String s = new String("abc"); 方式创建对象，在内存中创建了几个对象？ 

回答：两个，一个是堆空间中new结构，另一个是char[]对应的常量池中的数据："abc"



## 4 String不同拼接操作的对比

常量与常量的拼接结果在常量池。且常量池中不会存在相同内容的常量。     

final定义的变量是常量，所以` final String s1 = "java";  s2 = s1 + "se"; `   s2也是定义在字符串常量池中的

只要其中有一个是变量，结果就在堆中   

如果拼接的结果调用intern()方法，返回值一定是取自字符串常量池中的字符串

String字符串拼接时候的使用陷阱：不能多次执行**变量+字符串**的操作，否则会在堆中创造大量字符串副本，降低效率





## 5 String的常用方法

* int length()：返回字符串的长度：return value.length
* char charAt(int index)：返回某索引处的字符return value[index]
* boolean isEmpty()：判断是否是空字符串：return value.length==0
* String toLowerCase()：使用默认语言环境，将String中的所有字符转换为小写
* String toUpperCase()：使用默认语言环境，将String中的所有字符转换为大写
* String trim()：返回字符串的副本，忽略前导空白和尾部空白
* boolean equals(Object obj)：比较字符串的内容是否相同
* boolean equals IgnoreCase(String anotherString)：与equals方法类似，忽略大小写
* String concat(String str)：将指定字符串连接到此字符串的结尾。等价于用“+”
* int compareTo(String anotherString)：比较两个字符串的大小
* String substring(int beginIndex)：返回一个新的字符串，它是此字符串的从beginIndex开始截取到最后的一个子字符串。
* String substring(int beginIndex,int endIndex)：返回一个新字符串，它是此字符串从beginIndex开始截取到endIndex(不包含)的一个子字符串。
  
* boolean endsWith(String suffix)：测试此字符串是否以指定的后缀结束

* boolean startsWith(String prefix)：测试此字符串是否以指定的前缀开始

* boolean startsWith(String prefix, int toffset)：测试此字符串从指定索引开始的子字符串是否以指定前缀开始

* boolean contains(CharSequence s)：当且仅当此字符串包含指定的 char 值序列时，返回 true

* int indexOf(String str)：返回指定子字符串在此字符串中第一次出现处的索引

* int indexOf(String str, int fromIndex)：返回指定子字符串在此字符串中第一次出现处的索引，从指定的索引开始

* int lastIndexOf(String str)：返回指定子字符串在此字符串中最右边出现处的索引

* int lastIndexOf(String str, int fromIndex)：返回指定子字符串在此字符串中最后一次出现处的索引，从指定的索引开始反向搜索

  注：indexOf和lastIndexOf方法如果未找到都是返回-1



替换：

* String replace(char oldChar, char newChar)：返回一个新的字符串，它是通过用 newChar 替换此字符串中出现的所有 oldChar 得到的。
* String replace(CharSequence target, CharSequence replacement)：使用指定的字面值替换序列 替换 此字符串所有匹配字面值目标序列的子字符串。



正则表达式相关

* String replaceAll(String regex, String replacement)：使用给定的 replacement 替换此字符串所有匹配给定的正则表达式的子字符串。
* String replaceFirst(String regex, String replacement)：使用给定的 replacement 替换此字符串匹配给定的正则表达式的第一个子字符串。

* boolean matches(String regex)：告知此字符串是否匹配给定的正则表达式。
* String[] split(String regex)：根据给定正则表达式的匹配拆分此字符串。

* String[] split(String regex, int limit)：根据匹配给定的正则表达式来拆分此字符串，最多不超过limit个，如果超过了，剩下的全部都放到最后一个元素中。
  



## 6 String类型的转换

### 6.1 String与基本数据类型包装类的转换

String --> 基本数据类型、包装类：调用包装类的静态方法：**parseXxx(str)** 

基本数据类型、包装类 --> String：调用String重载的：**valueOf(xxx)**



### 6.2 String 与 char[]之间的转换

String --> char[ ]:调用String的 **toCharArray()** 方法   

char[ ] --> String:调用**String的构造器**



### 6.3 String与byte[]之间的转换

编码：String --> byte[]:调用String的**getBytes()**      字符串 -->字节  (看得懂 --->看不懂的二进制数据)
解码：byte[] --> String:调用**String的构造器**             字节 --> 字符串 （看不懂的二进制数据 ---> 看得懂）

说明：解码时，要求解码使用的字符集必须与编码时使用的**字符集一致**，否则会出现乱码。



# 二、StringBuffer、StringBuilder

## 1. String、StringBuffer、StringBuilder对比

三者的异同？

 * String：不可变的字符序列；
 * StringBuffer：可变的字符序列；**线程安全的，效率低**；
 * StringBuilder：可变的字符序列；jdk5.0新增的，**线程不安全的，效率高；**
 * 底层都使用char[]存储，String定义的是final char[ ]，后两个没有final

因此不涉及到多线程可以用StringBuilder

## 2. StringBuffer的分析

```java
String str = new String();//char[] value = new char[0];
String str1 = new String("abc");//char[] value = new char[]{'a','b','c'};

StringBuffer sb1 = new StringBuffer();//char[] value = new char[16];底层创建了一个容量是16的数组。
System.out.println(sb1.length());//为0，length指的是里面的内容，16是容量
sb1.append('a');//value[0] = 'a';
sb1.append('b');//value[1] = 'b';
StringBuffer sb2 = new StringBuffer("abc");//char[] value = new char["abc".length() + 16]; 容量是19
```

问题1  System.out.println(sb2.length());//3

问题2  扩容问题:如果要添加的数据底层数组盛不下了，那就需要扩容底层的数组.

​			默认情况下，扩容为原来容量的2倍+2，同时将原有数组中的元素复制到新的数组中。

**StringBuilder同上类似**



意义：开发中建议大家使用：StringBuffer(int capacity) 或 StringBuilder(int capacity)，即初始化时指定容量。



## 3. StringBuffer的方法

 * StringBuffer append(xxx)：提供了很多的append()方法，用于进行字符串拼接
 * StringBuffer delete(int start,int end)：删除指定位置的内容
 * StringBuffer replace(int start, int end, String str)：把[start,end)位置替换为str
 * StringBuffer insert(int offset, xxx)：在指定位置插入xxx
 * StringBuffer reverse() ：把当前字符序列逆转
 * public int indexOf(String str)
 * public String substring(int start,int end):返回一个从start开始到end索引结束的左闭右开区间的子字符串
 * public int length()
 * public char charAt(int n )
 * public void setCharAt(int n ,char ch)

总结：

增：append(xxx)

删：delete(int start,int end)

改：setCharAt(int n ,char ch) / replace(int start, int end, String str)

查：charAt(int n )

插：insert(int offset, xxx)

长度：length();

遍历：for() + charAt() / toString()



**StringBuilder同上类似**



## 4. String、StringBuffer、StringBuilder转换

String --> StringBuffer/StringBuilder ： 调用 StringBuffer/StringBuilder 构造器

StringBuffer/StringBuilder  --> String :   

* 调用String构造器
* 调用 StringBuffer/StringBuilder的 toString()方法



## 5. String、StringBuffer、StringBuilder效率对比    

从高到低排列：StringBuilder > StringBuffer > String



# 三、日期时间常用类

## JDK8之前：

### 1. System.currentTimeMillis()

时间类：

System类提供的`public static long currentTimeMillis()`用来返回当前时间与1970年1月1日0时0分0秒之间以毫秒为单位的时间差，称为时间戳。

```java
//测试 System.currentTimeMillis()
@Test
public void currentTimeMillis_test(){
    long time = System.currentTimeMillis();
    System.out.println(time);
}
```



### 2. java.util.Date()

日期时间类：

java.util.Date类

* 无参构造器：Date()：创建一个对应**当前时间**的Date对象

* 有参构造器：Date(arg)：创建自己**指定毫秒数**的Date对象

### 3. java.sql.Date()

java.sql.Date类

对应着数据库中的日期类，是util.Date类的子类

```java
//测试java.util.Date、java.sql.Date
@Test
public void Date_test(){
    /*************************** java.util.Date **********************************/
    // 构造器一：Date()：创建一个对应当前时间的Date对象
    Date date1 = new Date();
    // 两个方法的使用:
    // toString():显示当前的年、月、日、时、分、秒，在实际调用时，直接打印即可
    // getTime():获取当前Date对象对应的毫秒数。（时间戳）
    System.out.println(date1);
    System.out.println(date1.getTime());    //1589026216998

    //构造器二：创建自己指定毫秒数的Date对象
    Date date2 = new Date(1589026216998L);
    System.out.println(date2.toString());

    /*************************** java.sql.Date **********************************/
    //创建java.sql.Date对象
    java.sql.Date date3 = new java.sql.Date(35235325345L);
    System.out.println(date3);  //1971-02-13


    /*************************** 二者转换 **********************************/
    //如何将java.util.Date对象转换为java.sql.Date对象
    //情况一：多态强转
    Date data4 = new java.sql.Date(35235325345L);
    java.sql.Date data5 = (java.sql.Date)data4;
    //情况二：使用java.util.Date的getTime()方法
    Date date6 = new Date();
    java.sql.Date date7 = new java.sql.Date(date6.getTime());
}
```





### 4. java.text.SimpleDateFormat

`Date`类的API不易于国际化，大部分被废弃了，`java.text.SimpleDateFormat`类是一个不与语言环境有关的方式来格式化和解析日期的具体类。

它允许进行

- 格式化：日期 --> 文本
- 解析：文本 --> 日期

```java
//测试java.text.SimpleDateFormat
@Test
public void SimpleDateFormat_test() throws ParseException {

    /******************** 默认构造器，用的比较少 *****************************/
    SimpleDateFormat sdf = new SimpleDateFormat();
    Date date = new Date();
    System.out.println(date);

    //格式化：日期 --> 字符串
    String format = sdf.format(date);
    System.out.println(format);

    //解析：字符串 --> 日期, 格式化的逆过程
    //解析:要求字符串必须是符合SimpleDateFormat识别的格式
    String str = "2021/11/2 下午4:14";
    Date date1 = sdf.parse(str);
    System.out.println(date1);
    System.out.println();

    /******************** 按照指定的方式格式化和解析：调用带参的构造器 *****************************/
    SimpleDateFormat sdf1 = new SimpleDateFormat("yyyy-MM-dd hh:mm:ss"); //一般都会按照这种形式来格式化
    Date date2 = new Date();

    //格式化：日期 --> 字符串
    String format1 = sdf1.format(date2);
    System.out.println(format1);

    //解析：字符串 --> 日期, 格式化的逆过程
    //解析:要求字符串必须是符合SimpleDateFormat识别的格式(通过构造器参数体现),例如上面的"yyyy-MM-dd hh:mm:ss"，不符合这个格式就会抛出异常
    Date date3 = sdf1.parse(format1);
    System.out.println(date3);

    Date date4 = sdf1.parse("2020,05,30 01:01:01"); //抛出异常
    System.out.println(date4);
}
```

### 5. java.util.Calendar

`Calendar`是一个抽象基类，主用用于完成日期字段之间相互操作的功能

一个Calendar的实例是系统时间的抽象表示，获取`Calendar`实例的方法有

- 使用`Calendar.getInstance()`方法，**常用**

  ```java
  Calendar calendar = Calendar.getInstance();
  ```

- 调用它的子类`GregorianCalendar`的构造器

常用方法：

* `get(intfield)`方法来取得想要的时间信息。比如`YEAR、MONTH、DAY_OF_WEEK、HOUR_OF_DAY 、MINUTE、SECOND`
* `public void set(intfield,intvalue)` 设置时间信息
* `public void add(intfield,intamount)`  对时间信息加减
* `public final Date getTime()`  日历类---> Date
* `public final void setTime(Date date)`  Date ---> 日历类

```java
//测试java.util.Calendar
@Test
public void Calendar_test(){
    Calendar calendar = Calendar.getInstance();

    //常用方法
    //get()
    int days = calendar.get(Calendar.DAY_OF_MONTH);
    System.out.println(days);
    System.out.println(calendar.get(Calendar.DAY_OF_YEAR));

    //set()
    //calendar可变性
    calendar.set(Calendar.DAY_OF_MONTH,22);
    days = calendar.get(Calendar.DAY_OF_MONTH);
    System.out.println(days);   //22

    //add()
    calendar.add(Calendar.DAY_OF_MONTH,-3);
    days = calendar.get(Calendar.DAY_OF_MONTH);
    System.out.println(days);   //22-3 --》19

    //getTime():日历类---> Date
    Date date = calendar.getTime();
    System.out.println(date);   //Tue May 19 17:12:06 CST 2020

    //setTime():Date ---> 日历类
    Date date1 = new Date();
    calendar.setTime(date1);
    days = calendar.get(Calendar.DAY_OF_MONTH);
    System.out.println(days);   //10
}
```

注意:

- 获取月份时：一月是0，二月是1，以此类推，12月是11
- 获取星期时：周日是1，周二是2，。。。。周六是7



## JDK8

JDK8新增的时间包：

java.time–值对象的基础包，含了所有关于本地日期（LocalDate）、本地时间（LocalTime）、本地日期时间（LocalDateTime）、时区（ZonedDateTime）和持续时间（Duration）的类

java.time.chrono–提供对不同的日历系统的访问

java.time.format–格式化和解析时间和日期

java.time.temporal–包括底层框架和扩展特性

java.time.zone–包含时区支持的类

说明：**大多数开发者只会用到基础包和format包**，也可能会用到temporal包。因此，尽管有68个新的公开类型，大多数开发者，大概将只会用到其中的三分之一。

### 1. LocalDate、LocalTime、LocalDateTime

LocalDate、LocalTime、LocalDateTime是java.time几个常用的类，它们的实例是**不可变的对象**，

- `LocalDate`代表IOS格式（yyyy-MM-dd）的日期,可以存储生日、纪念日等日期。
- `LocalTime`表示一个时间，而不是日期。
- `LocalDateTime`是用来表示日期和时间的，**这是最常用的类之一**

常用方法：

[![java.time.LocalDateTime.png](https://z3.ax1x.com/2021/11/02/IkKnRf.png)](https://imgtu.com/i/IkKnRf)

```java
//LocalDate、LocalTime、LocalDateTime的使用
@Test
public void LocalDateTime_test(){
    //now()：静态方法根据当前时间创建对象指定时区的对象
    LocalDate localDate = LocalDate.now();
    LocalTime localTime = LocalTime.now();
    LocalDateTime localDateTime = LocalDateTime.now();

    System.out.println(localDate);
    System.out.println(localTime);
    System.out.println(localDateTime);
    System.out.println();

    //of()：设置指定的年、月、日、时、分、秒。没有偏移量
    LocalDateTime localDateTime1 = LocalDateTime.of(2020,10,6,13,23,43);
    System.out.println(localDateTime1);
    System.out.println();

    //getXxx()：获取相关的属性
    System.out.println(localDateTime1.getDayOfMonth());
    System.out.println(localDateTime.getDayOfWeek());
    System.out.println(localDateTime.getMonth());
    System.out.println(localDateTime.getMonthValue());
    System.out.println(localDateTime.getMinute());
    System.out.println();

    //withXxx():设置相关的时间属性
    //体现不可变性
    localDateTime1.withDayOfMonth(22);
    System.out.println(localDateTime1.getDayOfMonth()); //仍然是6
    LocalDateTime localDateTime2 = localDateTime1.withDayOfMonth(22);
    System.out.println(localDateTime2.getDayOfMonth()); //赋给新对象之后变为22
    LocalDateTime localDateTime3 = localDateTime1.withHour(4);
    System.out.println(localDateTime3.getHour());
    System.out.println();

    //plusXxx()、minusXxx()：加减相关的时间属性
    //体现不可变性
    LocalDateTime localDateTime4 = LocalDateTime.of(2020,10,6,13,23,43);
    LocalDateTime localDateTime5 = localDateTime4.plusHours(5);
    System.out.println(localDateTime5.getHour()); //13+5=18
    LocalDateTime localDateTime6 = localDateTime4.minusMonths(3);
    System.out.println(localDateTime6.getMonthValue()); //10-3=7
}
```



### 2. Instant

Instant：时间戳，表示自1970年1月1日0时0分0秒（UTC）开始的秒数，因为`java.time`包是基于纳秒计算的，所以`Instant`的精度可以达到纳秒级。



[![java.time.Instant.png](https://z3.ax1x.com/2021/11/02/Ik8HYj.png)](https://imgtu.com/i/Ik8HYj)

```java
@Test
public void Instant_test(){
    //创建对象方式一：now()，获取中时区对应的标准时间
    Instant instant = Instant.now();
    System.out.println(instant);

    //创建对象方式二：ofEpochMilli():通过给定的毫秒数，获取Instant实例，类似于Date(long millis)
    Instant instant1 = Instant.ofEpochMilli(1550475314878L);
    System.out.println(instant1);

    //atOffset()添加时间的偏移量
    OffsetDateTime offsetDateTime = instant.atOffset(ZoneOffset.ofHours(8));//东八区
    System.out.println(offsetDateTime);

    //toEpochMilli():获取自1970年1月1日0时0分0秒（UTC）开始的毫秒数  ---> Date类的getTime()
    long milli = instant.toEpochMilli();
    System.out.println(milli);  //1589104867591
}
```



### 3. DateTimeFormatter

格式化时间日期类，类似于上文的`java.text.SimpleDateFormat`

它允许进行

- 格式化：日期 --> 文本
- 解析：文本 --> 日期

创建实例有三种方式，见下文代码，开发中主要使用第一种自定义方式，其它方法还有：

[![Ikad4U.png](https://z3.ax1x.com/2021/11/02/Ikad4U.png)](https://imgtu.com/i/Ikad4U)

```java
//测试DateTimeFormatter类
@Test
public void DateTimeFormatter_test(){
    //重点：方式一：自定义的格式。如：ofPattern(“yyyy-MM-dd hh:mm:ss”)
    DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd hh:mm:ss");
    //格式化
    LocalDateTime localDateTime = LocalDateTime.now();
    System.out.println(localDateTime);
    String str = formatter.format(localDateTime);
    System.out.println(str);
    //解析
    TemporalAccessor parse = formatter.parse("2020-05-10 06:26:40");
    System.out.println(parse);
    System.out.println();


    //方式二：预定义的标准格式。如：ISO_LOCAL_DATE_TIME;ISO_LOCAL_DATE;ISO_LOCAL_TIME
    DateTimeFormatter formatter1 = DateTimeFormatter.ISO_LOCAL_DATE_TIME;
    //格式化:日期-->字符串
    String str1 = formatter1.format(localDateTime);
    System.out.println(str1);
    //解析：字符串 -->日期
    TemporalAccessor parse1 = formatter1.parse("2020-05-10T18:26:40.234");
    System.out.println(parse1);
    System.out.println();

    //方式三：
    //本地化相关的格式。如：DateTimeFormatter.ofLocalizedDateTime()
    //FormatStyle.LONG / FormatStyle.MEDIUM / FormatStyle.SHORT :适用于LocalDateTime
    DateTimeFormatter formatter2 = DateTimeFormatter.ofLocalizedDateTime(FormatStyle.LONG);
}
```



### 4. 其它

用到再看

* ZoneId：该类中包含了所有的时区信息，一个时区的ID，如Europe/Paris
* ZonedDateTime：一个在ISO-8601日历系统时区的日期时间，如2007-12-03T10:15:30+01:00Europe/Paris。
  其中每个时区都对应着ID，地区ID都为“{区域}/{城市}”的格式，例如：Asia/Shanghai等
* Clock：使用时区提供对当前即时、日期和时间的访问的时钟。
* 持续时间：Duration，用于计算两个“时间”间隔
* 日期间隔：Period，用于计算两个“日期”间隔
* TemporalAdjuster : 时间校正器。有时我们可能需要获取例如：将日期调整到“下一个工作日”等操作。
* TemporalAdjusters : 该类通过静态方法(firstDayOfXxx()/lastDayOfXxx()/nextXxx())提供了大量的常用TemporalAdjuster 的实现。
  



# 四、Java比较器

Java中的对象，正常情况下，只能进行比较：`==`或` !=` 。不能使用 `>`或`<`的，但是在开发场景中，我们需要对多个对象进行排序，言外之意，就需要比较对象的大小。 如何实现？使用两个接口中的任何一个：Comparable或 Comparator

Java实现对象排序的方式有两种：

* 自然排序：java.lang.Comparable
* 定制排序：java.util.Comparator



## 1. Comparable

像String、包装类等实现了Comparable接口，重写了compareTo(obj)方法，给出了比较两个对象大小的方式，一般都是从小到大的排列

**因此想要对自定义对象进行排序，需要继承Comparable接口，然后重写compareTo(obj)方法，在compareTo(obj)方法中指明如何排序 **

一般排序的规则是：

* 如果当前对象this大于形参对象obj，则返回正整数，
* 如果当前对象this小于形参对象obj，则返回负整数，
* 如果当前对象this等于形参对象obj，则返回零。

```java
//测试Comparable类
@Test
public void Comparable_test(){
    Goods[] arr = new Goods[5];
    arr[0] = new Goods("lenovoMouse",34);
    arr[1] = new Goods("dellMouse",43);
    arr[2] = new Goods("xiaomiMouse",12);
    arr[3] = new Goods("huaweiMouse",65);
    arr[4] = new Goods("microsoftMouse",43);
    Arrays.sort(arr);
    System.out.println(Arrays.toString(arr));
}

//Goods类中重写compareTo()部分的代码
//指明商品比较大小的方式:按照价格从低到高排序,再按照产品名称从高到低排序
@Override
public int compareTo(Object o) {
    if(o instanceof Goods){
        Goods goods = (Goods)o;
        //方式一：
        if(this.price > goods.price){
            return 1;
        }else if(this.price < goods.price){
            return -1;
        }else{
//                return 0;
            return -this.name.compareTo(goods.name);
        }
        //方式二：Double.compare方法可以实现同样的效果，产品名称需要再写判断
        //return Double.compare(this.price,goods.price);
    }
//        return 0;
    throw new RuntimeException("传入的数据类型不一致！");
}
```



Comparable 的典型实现：

*   BigDecimal、BigInteger以及所有的数值型对应的包装类：按它们对应的数值大小进行比较
*   Character：按字符的unicode值来进行比较
*   Boolean：true 对应的包装类实例大于false 对应的包装类实例
*   String：按字符串中字符的unicode 值进行比较
*   Date、Time：后边的时间、日期比前面的时间、日期大



## 2. Comparator

当元素的类型没有实现java.lang.Comparable接口而又不方便修改代码，或者实现了java.lang.Comparable接口的排序规则不适合当前的操作，那么可以考虑使用 Comparator的对象来排序，

需要**重写`compare(Object o1,Object o2)`方法**，比较o1和o2的大小，如果方法返回正整数，则表示o1大于o2；如果返回0，表示相等；返回负整数，表示o1小于o2。**注意compare方法是比较两个形参，和Comparable的compareTo方法是不同的**

**Comparable接口与Comparator的使用的对比：**

* Comparable接口的方式一旦一定，保证Comparable接口实现类的对象在任何位置都可以比较大小。  

* Comparator接口属于**临时性**的比较，临时构造了一个比较器

```java
//测试Comparator类
@Test
public void Comparator_test(){
    String[] arr = new String[]{"AA","CC","KK","MM","GG","JJ","DD"};
    //Arrays.sort(arr); //默认是按照字符串从小到大排序
    //临时改为按照字符串从大到小的顺序排列
    Arrays.sort(arr, new Comparator<String>() {
        @Override
        public int compare(String o1, String o2) {
            return -o1.compareTo(o2);
        }
    });
    System.out.println(Arrays.toString(arr));
}
```



# 五、System类

`System`类代表系统，系统级的很多属性和控制方法都放置在该类的内部。该类位于`java.lang`包

由于该类的构造器是`private`的，所以无法创建该类的对象，也就是无法实例化该类。其内部的成员变量和成员方法都是`static`的，所以也可以很方便的进行调用

成员变量：

* `System`类内部包含`in、out`和`err`三个成员变量，分别代表标准输入流(键盘输入)，标准输出流(显示器)和标准错误输出流(显示器)

成员方法：

* native long currentTimeMillis()：该方法的作用是返回当前的计算机时间，时间的表达格式为当前计算机时间和GMT时间(格林威治时间)1970年1月1号0时0分0秒所差的毫秒数。

* void exit(int status)：该方法的作用是退出程序。其中status的值为0代表正常退出，非零代表异常退出。使用该方法可以在图形界面编程中实现程序的退出功能等。

* void gc()：该方法的作用是请求系统进行垃圾回收。至于系统是否立刻回收，则取决于系统中垃圾回收算法的实现以及系统执行时的情况。

* String getProperty(String key)：该方法的作用是获得系统中属性名为key的属性对应的值。系统中常见的属性名以及属性的作用如下表所示：

  [![IZQuaF.png](https://z3.ax1x.com/2021/11/04/IZQuaF.png)](https://imgtu.com/i/IZQuaF)



# 六、Math类

`java.lang.Math`提供了一系列静态方法用于科学计算。其方法的参数和返回值类型一般为`double`型。

abs 绝对值
acos,asin,atan,cos,sin,tan 三角函数
sqrt 平方根
pow(double a,doble b) a的b次幂
log 自然对数
exp e为底指数
max(double a,double b)
min(double a,double b)
random() 返回0.0到1.0的随机数
long round(double a) double型数据a转换为long型（四舍五入）
toDegrees(double angrad) 弧度—>角度
toRadians(double angdeg) 角度—>弧度

# 七、大数类

## 1. BigInteger

`Integer`类作为`int`的包装类，能存储的最大整型值为`2^31 -1`，`Long`类也是有限的，最大为`2^63 -1`。如果要表示再大的整数，不管是基本数据类型还是他们的包装类都无能为力，更不用说进行运算了。

`java.math`包的`BigInteger`可以表示不可变的任意精度的整数。`BigInteger`提供所有Java 的基本整数操作符的对应物，并提供java.lang.Math 的所有相关方法。另外，BigInteger还提供以下运算：模算术、GCD 计算、质数测试、素数生成、位操作以及一些其他操作。

构造器：

- `BigInteger(String val)`：根据字符串构建`BigInteger`对象

常用方法：

[![IZllTS.png](https://z3.ax1x.com/2021/11/04/IZllTS.png)](https://imgtu.com/i/IZllTS)



## 2. BigDecimal

一般的Float类和Double类可以用来做科学计算或工程计算，但**在商业计算中，要求数字精度比较高，故用到`java.math.BigDecimal`类**。BigDecimal类支持**不可变的、任意精度的有符号十进制定点数**

构造器:

- `public BigDecimal(double val)`
- `public BigDecimal(String val)`

常用方法：

* public BigDecimal add(BigDecimal augend)
* public BigDecimal subtract(BigDecimal subtrahend)
* public BigDecimal multiply(BigDecimal multiplicand)
* public BigDecimal divide(BigDecimal divisor, int scale, int roundingMode)

```java
@Test
public void BigDecimal_test() {
    BigDecimal bd = new BigDecimal("12435.351");
    BigDecimal bd2 = new BigDecimal("11");
    //System.out.println(bd.divide(bd2)); //抛出异常，没有指定结果保留方式
    System.out.println(bd.divide(bd2, BigDecimal.ROUND_HALF_UP)); //指定保留方式，向上舍入
    System.out.println(bd.divide(bd2, 25, BigDecimal.ROUND_HALF_UP)); //指定保留位数
}
```





