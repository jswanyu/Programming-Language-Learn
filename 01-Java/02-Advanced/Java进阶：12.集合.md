目前的笔记都未使用泛型

# 一、Java集合概述

## 1. 集合与数组

**集合、数组**都是对多个数据进行存储操作的结构，简称Java**容器**。此时的存储，主要是指**内存**层面的存储，**不涉及持久化**的存储（.txt,.jpg,.avi,数据库中）

数组的特点：

* 一旦初始化以后，长度就确定
* 数组一旦定义好，它的数据类型也就确定

数组的缺点：

* 一旦初始化以后，其**长度不可修改**。
* 数组中提供的**方法有限**，对于添加、删除、插入数据等操作，非常不便，同时**效率不高**。
* 获取数组中实际元素的个数的需求，数组没有现成的属性或方法可用
* 数组存储数据的特点：有序、可重复。对于无序、不可重复的需求，不能满足。

集合能够解决上述问题。



## 2. 集合框架

Java集合的框架为：

* Collection接口：单列集合，用来存储一个一个的对象
  * List接口：存储有序的、可重复的数据 --> 动态数组
    * *ArrayList、LinkedList、Vector*
  * Set接口：存储无序的、不可重复的数据 --> 数学中的“集合”
    * *HashSet、LinkedHashSet、TreeSet*
* Map接口：双列集合，用来存储一对一对(key - value)数据
  * *HashMap、LinkedHashMap、TreeMap、Hashtable、Properties*



# 二、Collection接口

## 1. Collection接口方法

Collection 接口是List、Set 和Queue 接口的父接口，该接口里定义的方法既可用于操作Set 集合，也可用于操作List 和Queue 集合。

JDK不提供此接口的任何直接实现，而是提供更具体的子接口(如：Set和List)实现。

在Java 5 之前，Java 集合会丢失容器中所有对象的数据类型，把所有对象都当成Object 类型处理；从JDK 5.0 增加了泛型以后，Java 集合可以记住容器中对象的数据类型

Collection 接口有如下常用方法：

* 添加

  * add(Object obj)

  * addAll(Collection coll)

* 获取有效元素的个数

  * int size()

* 清空集合

  * void clear()

* 是否是空集合

  * boolean isEmpty()

* 是否包含某个元素

  - boolean contains(Object obj)：是通过元素的equals方法来判断是否是同一个对象
  - boolean containsAll(Collection c)：也是调用元素的equals方法来比较的。拿两个集合的元素挨个比较。

* 删除
  * boolean remove(Object obj) ：通过元素的equals方法判断是否是要删除的那个元素。只会**删除找到的第一个元素**，返回剩下的元素
  * boolean removeAll(Collection coll)：返回当前集合的差集

* 取两个集合的交集
  * boolean retainAll(Collection c)：返回交集的结果，不影响c

* 集合是否相等
  * boolean equals(Object obj)
* 转成对象数组
  * Object[] toArray()
* 获取集合对象的哈希值
  * hashCode()
* 遍历
  * iterator()：返回迭代器对象，用于集合遍历



注意点：

向Collection接口的实现类的对象中添加数据obj时，要求obj所在类要重写equals()，否则很多方法无法实现





## 2. Iterator迭代器接口

Iterator对象称为迭代器(设计模式的一种)，主要用于遍历Collection **集合**中的元素。迭代器模式的定义为：提供一种方法访问一个容器(container)对象中各个元素，而又不需暴露该对象的内部细节。迭代器模式，就是为**容器**而生。

Collection接口继承了java.lang.Iterable接口，该接口有一个iterator()方法，**所有实现了Collection接口的集合类都有一个iterator()方法**，用以返回一个实现了Iterator接口的对象。

**Iterator 仅用于遍历集合**，Iterator本身并不提供承装对象的能力。如果需要创建Iterator 对象，则必须有一个被迭代的集合。**集合对象每次调用iterator()方法都得到一个全新的迭代器对象**，默认指针都在集合的**第一个元素之前**。



###  2.1 使用Iterator遍历Collection

集合元素的遍历操作，使用迭代器Iterator接口，使用内部的方法：

* hasNext()：判断集合是否还有下一个元素
* next()：**指针刚开始是在第一个元素之前，调用next()后指针先下移，将下移以后的位置上的元素返回**

遍历集合时需要先用hasNext()判断集合是否有下一个元素，否则没有元素的话直接next()会报异常

```java
@Test
public void test1(){
    Collection coll = new ArrayList();
    coll.add(123);
    coll.add(456);
    coll.add(new Person("Jerry",20));
    coll.add(new String("Tom"));
    coll.add(false);

    // 一定要先构造迭代器
    Iterator iterator = coll.iterator();
    while (iterator.hasNext()){
        System.out.println(iterator.next());
    }
}
```



遍历集合典型错误:

```java
@Test
public void test2(){
    Collection coll = new ArrayList();
    coll.add(123);
    coll.add(456);
    coll.add(new Person("Jerry",20));
    coll.add(new String("Tom"));
    coll.add(false);

    // 错误方式一：
    // 想遍历一个元素时，执行了两次next()，跳着输出元素，同时会报NoSuchElementException异常
    Iterator iterator = coll.iterator();
    while(iterator.next() != null){
        System.out.println(iterator.next());
    }

    // 错误方式二：
    // 集合对象每次调用iterator()方法都得到一个全新的迭代器对象，死循环输出第一个元素
    while(coll.iterator().hasNext()){
        System.out.println(coll.iterator().next());
    }
}
```



### 2.2 Iterator迭代器remove()的使用

迭代器的remove()方法不同于集合的remove()方法，同时注意如果还未调用next()或在上一次调用 next 方法之后已经调用了 remove 方法，再调用remove都会报异常：IllegalStateException

```java
//测试Iterator中的remove()方法
@Test
public void test3(){
    Collection coll = new ArrayList();
    coll.add(123);
    coll.add(456);
    coll.add(new Person("Jerry",20));
    coll.add(new String("Tom"));
    coll.add(false);

    // 删除集合中”Tom”
    // 如果还未调用next()或在上一次调用 next 方法之后已经调用了 remove 方法，
    // 再调用remove都会报IllegalStateException。

    Iterator iterator = coll.iterator();

    while(iterator.hasNext()){
        //未调用next()之前就调用remove()会报异常，指针还在第一个元素之前
        //iterator.remove();
        Object obj = iterator.next();
        if("Tom".equals(obj)){
            iterator.remove();
            //iterator.remove();//调用过一次不能再调用，元素已经被删除了，还没指向下一个元素
        }
    }

    //遍历集合
    iterator = coll.iterator();
    while(iterator.hasNext()){
        System.out.println(iterator.next());
    }
}
```



### 2.3 for each循环

Java 5.0 提供了for each循环迭代访问Collection和数组。遍历操作不需获取Collection或数组的长度，无需使用索引访问元素。遍历集合的**底层调用Iterator**完成操作。for each还可以用来遍历数组。

语法：

```java
for(类型 名称: 容器){
    ……
}
```

即将容器中每个元素取出来赋值给新定义的变量，然后操作这个变量，容器本身并未发生改变

```java
public class ForeachTest {
    //foreach遍历集合
    @Test
    public void test() {
        Collection coll = new ArrayList();
        coll.add(123);
        coll.add(456);
        coll.add(new Person("Jerry", 20));
        coll.add(new String("Tom"));
        coll.add(false);

        for (Object obj : coll) {
            System.out.println(obj);
        }
    }

    //foreach遍历数组
    @Test
    public void test2(){
        int[] arr = new int[]{1,2,3,4,5,6};
        //for(数组元素的类型 局部变量 : 数组对象)
        for(int i : arr){
            System.out.println(i);
        }
    }

    //练习题
    @Test
    public void test3(){
        String[] arr = {"SS", "KK", "RR"};

        //方式一：普通for赋值，确实改变了数组本身的值
//        for(int i = 0;i < arr.length;i++){
//            arr[i] = "HH";
//        }

        //方式二：for each循环赋值，并未改变数组值，因为它是将arr里每个值取出来赋给了s，改s并没有改变数组本身
        for(String s : arr){
            s = "HH";
        }

        for (int i = 0; i < arr.length; i++) 
            System.out.println(arr[i]); 
    }
}
```



###  2.4 补充：Enumeration

Enumeration接口是Iterator迭代器的“古老版本”，有类似的hasMoreElements()和nextElement()方法



## 3. Collection子接口之一：List接口

鉴于Java中数组用来存储数据的局限性，我们通常使用List替代数组。List集合类中元素有序、且可重复，集合中的每个元素都有其对应的顺序索引。List容器中的元素都对应一个整数型的序号记载其在容器中的位置，可以根据序号存取容器中的元素。JDK API中List接口的实现类常用的有：

* ArrayList：作为List接口的主要实现类；**线程不安全**的，**效率高**；底层使用**数组**Object[] elementData存储
* LinkedList：对于频繁的**插入、删除**操作，使用此类**效率比ArrayList高**；底层使用**双向链表**存储
* Vector：作为List接口的古老实现类；线程安全的，效率低；底层使用**数组**Object[] elementData存储

面试题：比较ArrayList、LinkedList、Vector三者的异同？        

 同：三个类都是实现了List接口，存储数据的特点相同：存储有序的、可重复的数据

不同：见上

**使用角度上，最好把ArrayList作为默认选择。当插入、删除频繁时，使用LinkedList**



### 3.1 ArrayList的源码分析

ArrayList是List 接口的主要实现类，**线程不安全**的，**效率高**。本质上，ArrayList是对象引用的一个”变长”**数组**。

**jdk 7**情况下：

```java
ArrayList list = new ArrayList(); //底层一开始就创建了长度是10的Object[]数组elementData
list.add(123);//elementData[0] = new Integer(123);
……
list.add(123);//如果此次的添加导致底层elementData数组容量不够，则扩容。
```

底层一开始创建了**长度是10**的Object[]数组，默认情况下，**扩容为原来的容量的1.5倍**，同时需要将原有数组中的数据复制到新的数组中

结论：**建议开发中使用带参的构造器**：ArrayList list = new ArrayList(int capacity)



**jdk 8**情况下：

```java
ArrayList list = new ArrayList();//底层Object[] elementData初始化为{}.并没有创建长度为10的数组
list.add(123);//第一次调用add()时，底层才创建了长度10的数组，并将数据123添加到elementData[0]
……
后续的添加和扩容操作与jdk7相同
```

和jdk 7的不同之处在于jdk 7中的ArrayList的对象的创建类似于单例的**饿汉式**，而jdk 8中的ArrayList的对象的创建类似于单例的**懒汉式**，**延迟了数组的创建，节省内存**。



### 3.2 LinkedList的源码分析

LinkedList底层使用了**双向链表**，内部没有声明数组，而是定义了Node类型的first和last，用于记录首末元素。同时，定义内部类Node，作为LinkedList中保存数据的基本结构。

```java
LinkedList list = new LinkedList(); 内部声明了Node类型的first和last属性，默认值为null
list.add(123);//将123封装到Node中，创建了Node对象。

//其中，Node定义为：体现了LinkedList的双向链表的说法
private static class Node<E> {
    E item;
    Node<E> next;
    Node<E> prev;

    Node(Node<E> prev, E element, Node<E> next) {
        this.item = element;
        this.next = next;
        this.prev = prev;
    }
}
```



### 3.3 Vector的源码分析

Vector 是一个古老的集合，JDK1.0就有了。大多数操作与ArrayList相同，区别之处在于Vector是线程安全的，效率较低。jdk7和jdk8中通过Vector()构造器创建对象时，底层都创建了长度为10的数组。在扩容方面，默认扩容为原来的数组长度的2倍。

由于Vector总是比ArrayList慢，所以尽量避免使用。多线程方面可以依然使用ArrayList，有方法将其变为线程安全的



### 3.4 List接口中的常用方法测试

List除了从Collection集合继承的方法外，List 集合里添加了一些根据**索引**来操作集合元素的方法，因为Collection接口还要考虑到set，不能设置有索引的方法

* void add(intindex, Object ele)：在index位置插入ele元素
* boolean addAll(int index, Collection eles)：从index位置开始将eles中的所有元素添加进来
* Object get(int index)：获取指定index位置的元素
* int indexOf(Object obj):返回obj在集合中首次出现的位置，如果不存在，返回-1
* int lastIndexOf(Object obj):返回obj在当前集合中末次出现的位置，如果不存在，返回-1
* Object remove(int index):移除指定index位置的元素，并返回此元素
* Object set(int index, Object ele):设置指定index位置的元素为ele
* List subList(int fromIndex, int toIndex):返回从fromIndex到toIndex位置的**左闭右开**区间的子集合
  

总结：常用方法

 * 增：add(Object obj)
 * 删：remove(int index) / remove(Object obj)   务必注意如果remove里只有数字参数，其是删除索引，如果是想删除元素2，要用包装类，见面试题2
 * 改：set(int index, Object ele)
 * 查：get(int index)
 * 插：add(int index, Object ele)
 * 长度：size()
 * 遍历：
    * ① Iterator迭代器方式
    * ② 增强for循环
    * ③ 普通的循环



### 3.5 List面试题

1. 请问ArrayList/LinkedList/Vector的异同？ArrayList底层是什么？扩容机制？Vector和ArrayList的最大区别?

ArrayList和LinkedList的异同二者都线程不安全，相对线程安全的Vector，执行效率高。此外，ArrayList是实现了基于动态数组的数据结构，LinkedList基于链表的数据结构。 对于随机访问get和set，ArrayList觉得优于LinkedList，因为LinkedList要移动指针。对于新增和删除操作add(特指插入)和remove，LinkedList比较占优势，因为ArrayList要移动数据。

 ArrayList和Vector的区别Vector和ArrayList几乎是完全相同的，唯一的区别在于Vector是同步类(synchronized)，属于强同步类。因此开销就比ArrayList要大，访问要慢。正常情况下，大多数的Java程序员使用ArrayList而不是Vector，因为同步完全可以由程序员自己来控制。Vector每次扩容请求其大小的2倍空间，而ArrayList是1.5倍。Vector还有一个子类Stack。


2. 下面代码的输出为？  

    考察：区分List中remove(int index)和remove(Object obj)

```java
@Test
public void testListRemove() {
    List list = new ArrayList();
    list.add(1);
    list.add(2);
    list.add(3);
    updateList(list);
    System.out.println(list);
}

private void updateList(List list) {
    //list.remove(2);//删除索引2
    list.remove(new Integer(2)); //删除元素2
}
```



## 4. Collection子接口之二：Set接口

Set存储无序的、不可重复的数据，Set接口是Collection的子接口，set接口**没有提供额外的方法**。Set 集合**不允许包含相同**的元素，如果试把两个相同的元素加入同一个Set 集合中，则添加操作失败。**Set 判断两个对象是否相同不是使用`==`运算符，而是根据`equals()`方法**。其实现类常用的有：

* HashSet：作为Set接口的主要实现类；线程不安全的；可以存储null值
    * LinkedHashSet：作为HashSet的子类，遍历其内部数据时，可以按照添加的顺序遍历，**对于频繁的遍历操作，LinkedHashSet效率高于HashSet.**
* TreeSet：可以按照添加对象的指定属性，进行排序。



### 4.1 HashSet

HashSet 是 Set 接口的典型实现，大多数时候使用 Set 集合时都使用这个实现类。底层也是**数组（更准确的说是数组+链表）**，初始容量为**16**，当如果使用率超过**0.75**，（16\*0.75=12）就会扩大容量为原来的2倍（16扩容为32，依次为64,128…等）。HashSet 按 Hash 算法来存储集合中的元素，因此具有很好的存取、查找、删除性能。HashSet是**无序**的，集合**元素可以是null**，同时HashSet**不是线程安全**的。

```java
HashSet<Object> hashSet = new HashSet<>();
```

#### 4.1.1 HashSet添加元素的底层过程：

我们向HashSet中添加元素a，首先调用元素a所在类的hashCode()方法，计算元素a的**哈希值**，此哈希值接着通过**某种算法**计算出在HashSet底层数组中的**存放位置**（即索引位置），判断数组此位置上是否已经有元素：

*   如果此位置上没有其他元素，则元素a添加成功。 **--->情况1**

*   如果此位置上有其他元素b(或以链表形式存在的多个元素），则比较元素a与元素b的hash值（因为位置是将哈希值通过某种算法算出来的，所以有可能哈希值不同，但算出来的位置可能相同）：
    *   如果hash值不相同，则元素a添加成功。**--->情况2**
    *   如果hash值相同，进而需要调用元素a所在类的**equals()**方法：
        *   equals()返回 true，元素a添加失败
        *   equals()返回 false，则元素a添加成功。**--->情况3**

对于添加成功的情况2和情况3而言：元素a 与已经存在指定索引位置上数据以**双向链表**的方式存储。

*   jdk 7 : 元素a放到数组中，指向原来的元素。

*   jdk 8 : 原来的元素在数组中，指向元素a         

助记：七上（新元素都放到原数组中，在旧元素的上面，与旧元素构成双向链表）八下（新元素都放到旧元素的下面，与旧元素构成双向链表）

总结：**判断位置-->判断哈希值-->判断equals()返回值**，hashCode()和equals()结合的判定过程保证了Set对象不可重复特性的同时，插入效率又高

*练习：4.1.4 HashSet面试题练习2*



#### 4.1.2 HashSet的无序性和不可重复性：

了解HashSet元素的添加过程，能够更好的理解HashSet的无序性和不可重复性。

*   无序性：**不等于随机性**。存储的数据在底层数组中并非按照数组索引的顺序添加，而是根据数据的哈希值决定，因此多次打印HashSet，结果的顺序是相同的
*   不可重复性：用上述添加元素的过程来保证集合里，相同的元素只能添加一个。HashSet 集合判断两个元素相等的标准：两个对象通过hashCode() 方法比较相等，并且两个对象的equals()方法返回值也相等。


因此对于存放在Set容器中的对象，对应的类一定要重写`equals()`和`hashCode(Object obj)`方法，以实现对象相等规则。即：“相等的对象必须具有相等的哈希值”

```java
@Test
public void test1(){
    HashSet<Object> set = new HashSet<>();
    set.add(123);
    set.add(456);
    set.add("fgd");
    set.add("book");
    set.add(new Person("Tom",12));
    set.add(new Person("Tom",12));
    Iterator<Object> iterator = set.iterator();
    while (iterator.hasNext()){
        System.out.println(iterator.next());
    }
}
```

对于上面的代码，第二个Person对象不会添加到集合中，同时代码执行多次，集合存放元素的顺序是相同的。

#### 4.1.3 关于hashCode()和equals()的重写

重写`equals()`方法的时候一般都需要同时复写`hashCode()`方法。通常参与计算`hashCode()`的对象的属性也应该参与到`equals()`中进行判定。只有这样才能尽可能保证“相等的对象必须具有相等的哈希值”。比如Person类中的name和age都应该参与到`hashCode()`的计算中

hashCode()：

-   在程序运行时，同一个对象多次调用`hashCode()`方法应该返回相同的值。
-   当两个对象的`equals()`方法比较返回`true`时，这两个对象的`hashCode()`方法的返回值也应相等。
-   对象中用作`equals()` 方法比较的`Field`(对象的属性)，都应该用来计算`hashCode`值。

equals()：

*   参与计算`hashCode()`的对象的属性也应该参与到`equals()`中进行判定



一般建议使用**IDEA自动重写**hashCode()和equals()。例如Person类中重写equals()和hashCode()：

```java
@Override
public boolean equals(Object o) {
    if (this == o) return true;
    if (o == null || getClass() != o.getClass()) return false;
    Person person = (Person) o;
    return age == person.age && Objects.equals(name, person.name);
}

@Override
public int hashCode() {
    return Objects.hash(name, age);
}
```

在调用工具自动重写hashCode()和equals()时，其中的hashCode() 往底层查看，会有31这个数值。

```java
//Objects.hash(name, age);
public static int hash(Object... values) {
    return Arrays.hashCode(values);
}

//Arrays.hashCode(values);
public static int hashCode(Object a[]) {
    if (a == null)
        return 0;

    int result = 1;

    for (Object element : a)
        result = 31 * result + (element == null ? 0 : element.hashCode());

    return result;
}
```

系数选择31的原因：

*   选择系数的时候要选择尽量大的系数。因为如果计算出来的hash地址越大，所谓的“冲突”就越少，查找起来效率也会提高。（减少冲突）
*   31只占用5 bits,相乘造成数据溢出的概率较小。
*   31可以由i*31== (i<<5)-1来表示，现在很多虚拟机里面对于2的指数幂都有做相关优化。（提高算法效率）
*   31是一个素数，素数作用就是：如果我用一个数字来乘以这个素数，那么最终出来的结果只能被素数本身和被乘数还有1来整除！(减少冲突)       （2 4 8 16 32 64，这些数本身不考虑，因为都是非素数，其中2、4、8、16周围的素数偏小，64周围的63、65均为非素数，因此选择32周围的素数31）



#### 4.1.4 HashSet面试题

练习1：在List内去除重复数字值，要求尽量简单

解决方法，构建HashSet，将List元素添加进HashSet，再转为List

```java
@Test
public void test3(){
    List list = new ArrayList();
    list.add(1);
    list.add(2);
    list.add(2);
    list.add(4);
    list.add(4);
    List list2 = duplicateList(list);
    for (Object integer : list2) {
        System.out.println(integer);
    }
}

public static List duplicateList(List list) {
    HashSet set = new HashSet();
    set.addAll(list);
    return new ArrayList(set);
}
```

>   1
>   2
>   4



练习2：下列代码的输出结果

ps： User和Person类似，区别是User的name和age可以类外访问

```java
@Test
public void test4(){
    HashSet set = new HashSet();
    User u1 = new User("AA",1);
    User u2 = new User("BB",2);
    set.add(u1);
    set.add(u2);
    System.out.println(set); //[User{name='AA', age=1}, User{name='BB', age=2}]

    u1.name = "CC";
    set.remove(u1);
    System.out.println(set); //[User{name='CC', age=1}, User{name='BB', age=2}]

    set.add(new User("CC",1));
    System.out.println(set); //[User{name='CC', age=1}, User{name='BB', age=2}, User{name='CC', age=1}]
    
    set.add(new User("AA",1));
    System.out.println(set);
    //[User{name='CC', age=1}, User{name='BB', age=2}, User{name='AA', age=1}, User{name='CC', age=1}]
}
```

出现12行处的结果原因是，这个集合最开始放位置的时候，是根据 aa,1  bb,2  这两个组合的hash值去算位置的，现在第10行将aa换成cc，11行去删除name为cc的u1对象，想删除就得先找到这个对象，然而此时计算hash值是根据 cc,1 这样的组合去算的，所以算出这个位置会发现这个位置上没有对象，所以删除未果。但u1确实已经由aa 改为了cc，所以还是会打印出来。

出现15行处的结果原因和刚才删除一样，现在是根据cc,1这样的组合计算hash值，另外两个元素还是放在一开始由 aa,1  bb,2  这两个组合计算的hash值所在的位置，因此这个 cc,1 的组合可以添加成功

出现19行的结果则是因为，现在重新计算aa,1组合的hash值，结果发现hash值和最开始的aa,1 一样，那么其位置自然也一样，但是HashSet添加元素的第三个步骤还要判定equals方法，此时第一个元素是cc, 新元素是aa，是不同的，所以也可以添加成功，而且是在相同位置处以链表形式添加



### 4.2 LinkedHashSet

`LinkedHashSet`是`HashSet`的子类，`LinkedHashSet`根据元素的`hashCode`值来决定元素的存储位置，但它同时使用**双向链表**维护元素的次序，这使得元素**看起来**是以**插入顺序保存**的（即插入一个元素后，会有指针指向下一个待插入的元素）。LinkedHashSet同样不允许集合元素重复。

**`LinkedHashSet`插入性能略低于`HashSet`**，但对于**频繁的遍历操作**（迭代访问Set 里的全部元素）有很好的性能。

```java
@Test
public void test2(){
    LinkedHashSet<Object> set = new LinkedHashSet<>();
    set.add(456);
    set.add(123);
    set.add("AA");
    set.add("CC");
    set.add(new Person("Tom",12));
    set.add(new Person("Tom",12));

    Iterator iterator = set.iterator();
    while(iterator.hasNext()){
        System.out.println(iterator.next());
    }
}
```

>   456
>   123
>   AA
>   CC
>   Person{name='Tom', age=12}



### 4.3 TreeSet

TreeSet是SortedSet接口的实现类，TreeSet可以确保集合元素处于排序状态。TreeSet底层使用**红黑树**结构存储数据.

它的特点是：有序，集合中只能有**同一种数据类型**，查询速度比List快

TreeSet两种排序方法：**自然排序和定制排序**。默认情况下，TreeSet采用自然排序。

如果试图把一个对象添加到TreeSet时，则该对象的类必须**实现 Comparable 接口**。实现Comparable的类必须实现 compareTo(Object obj) 方法，这样两个对象才能通过 compareTo(Object obj) 方法的返回值来比较大小。也正因如此，只有相同类的两个实例才会比较大小，所以向 TreeSet 中添加的应该是**同一个类的对象**。对于 TreeSet 集合而言，**它判断两个对象是否相等的唯一标准是：两个对象通过compareTo(Object obj)方法比较返回值**。当需要把一个对象放入TreeSet中，重写该对象对应的equals()方法时，应保证该方法与compareTo(Object obj) 方法有一致的结果：如果两个对象通过equals()方法比较返回 true，则通过compareTo(Object obj)方法比较应该返回 0。否则，让人难以理解。

#### 自然排序

TreeSet会调用集合元素的 compareTo(Object obj) 方法来比较元素之间的大小关系，然后将集合元素按**升序**(默认情况)排列。

```java
@Test
public void test1() {
    TreeSet set = new TreeSet();
    set.add(34);
    set.add(-34);
    set.add(43);
    set.add(11);
    set.add(8);

    Iterator iterator = set.iterator();
    while(iterator.hasNext()){
        System.out.println(iterator.next());
    }
}
```

>   -34
>   8
>   11
>   34
>   43

```java
@Test
public void test2() {
    TreeSet set = new TreeSet();
    set.add(new Person("Tom",12));
    set.add(new Person("Jerry",32));
    set.add(new Person("Jim",2));
    set.add(new Person("Mike",65));
    set.add(new Person("Jack",33));
    set.add(new Person("Jack",56));


    Iterator iterator = set.iterator();
    while(iterator.hasNext()){
        System.out.println(iterator.next());
    }
}


//Person类
public class Person implements Comparable{
	//……
    
    //按照姓名从大到小排列,年龄从小到大排列
    @Override
    public int compareTo(Object o) {
        if (o instanceof Person){
            Person person = (Person) o;
            int compare = this.name.compareTo(person.name);//按照姓名从大到小排列
            if (compare != 0){
                return compare;
            }else{
                return Integer.compare(this.age,person.age);//年龄从小到大排列
            }
        }
        else{
            throw new RuntimeException("输入的类型不匹配");
        }
    }
}
```

>   Person{name='Jack', age=33}
>   Person{name='Jack', age=56}
>   Person{name='Jerry', age=32}
>   Person{name='Jim', age=2}
>   Person{name='Mike', age=65}
>   Person{name='Tom', age=12}



#### 定制排序

TreeSet的自然排序要求元素所属的类实现Comparable接口，如果元素所属的类没有实现Comparable接口，或不希望按照升序(默认情况)的方式排列元素或希望按照其它属性大小进行排序，则考虑使用定制排序。定制排序，通过Comparator接口来实现。需要重写 compare(T o1,T o2) 方法。利用int compare(T o1,T o2)方法，比较o1和o2的大小：如果方法返回**正整数，则表示o1大于o2**；如果**返回0，表示相等**；返回**负整数，表示o1小于o2。**

要实现定制排序，需要将实现Comparator接口的实例作为形参传递给TreeSet的构造器。此时，仍然只能向TreeSet中添加类型相同的对象，否则发生ClassCastException异常。使用定制排序判断两个元素相等的标准是：通过Comparator比较两个元素返回了0。

```java
@Test
public void test3(){
    Comparator com = new Comparator() {
        //按照年龄从小到大排列
        @Override
        public int compare(Object o1, Object o2) {
            if(o1 instanceof Person && o2 instanceof Person){
                Person u1 = (Person)o1;
                Person u2 = (Person)o2;
                return Integer.compare(u1.getAge(),u2.getAge());
            }else{
                throw new RuntimeException("输入的数据类型不匹配");
            }
        }
    };

    TreeSet set = new TreeSet(com);
    set.add(new Person("Tom",12));
    set.add(new Person("Jerry",32));
    set.add(new Person("Jim",2));
    set.add(new Person("Mike",65));
    set.add(new Person("Mary",33));
    set.add(new Person("Jack",33));
    set.add(new Person("Jack",56));


    Iterator iterator = set.iterator();
    while(iterator.hasNext()){
        System.out.println(iterator.next());
    }
}
```

>   Person{name='Jim', age=2}
>   Person{name='Tom', age=12}
>   Person{name='Jerry', age=32}
>   Person{name='Mary', age=33}
>   Person{name='Jack', age=56}
>   Person{name='Mike', age=65}



# 三、Map接口

Map：双列数据，存储key-value对的数据。其组织框架为

*   *HashMap：作为Map的主要实现类；线程不安全的，效率高；可以存储null的key和value*
    *   *LinkedHashMap：保证在遍历map元素时，可以按照添加的顺序实现遍历。因为使用了链表，对于频繁的遍历操作，效率高于HashMap*
*   *TreeMap：将key-value对进行排序，实现排序遍历。此时考虑key的自然排序或定制排序，底层使用红黑树*
*   *Hashtable：作为古老的实现类；线程安全的，效率低；不能存储null的key和value*
    *   *Properties：常用来处理配置文件。key和value都是String类型*



Map中存储的key-value的特点：

*   Map与Collection并列存在。用于保存具有映射关系的数据:key-value
*   Map中的key和value都可以是任何引用类型的数据
*   Map中的key：无序的、不可重复的，使用**Set存储所有的key**，Map对象所对应的类须重写hashCode()和equals()方法。常用String类作为Map的“键”
*   Map中的value：无序的、可重复的，使用**Collection存储所有的value**，value所在的类要重写equals()
*   Map中的entry：无序的、不可重复的，使用Set存储所有的entry
*   一个键值对key-value构成了一个**Entry对象**。
*   key和value之间存在单向一对一关系，即通过指定的key总能找到唯一的、确定的value
    

## 1. HashMap

### 1.1  HashMap及底层实现原理

HashMap是Map接口使用频率最高的实现类，允许使用null键和null值。与HashSet一样，不保证映射的顺序（即无序性，不同于随机性）。

HashMap中key-value的特点如上文所述，HashMap判断两个key相等的标准是：两个key通过equals()方法返回true，hashCode值也相等。HashMap判断两个value相等的标准是：两个value 通过equals()方法返回true。



**HashMap底层实现原理：**

#### 1.1.1 JDK7及以前

HashMap的内部存储结构其实是**数组+链表**的结合。当实例化一个HashMap时，系统会创建一个长度为Capacity的Entry数组，这个长度在哈希表中被称为容量(Capacity)，在这个数组中可以存放元素的位置我们称之为“桶”(bucket)，每个bucket都有自己的索引，系统可以根据索引快速的查找bucket中的元素。每个bucket中存储一个元素，即一个Entry对象，但每一个Entry对象可以带一个引用变量，用于指向下一个元素，因此，在一个桶中，就有可能生成一个Entry链，而且新添加的元素作为链表的head（旧元素放在链表后面）。

[![I7AbEn.png](https://z3.ax1x.com/2021/11/18/I7AbEn.png)](https://imgtu.com/i/I7AbEn)

```java
HashMap map = new HashMap():
```

JDK7中，HashMap实例化以后，底层立即创建了长度是16的一维数组Entry[ ] table。

假设HashMap中已经存在元素，其添加新元素(key1, value1)的过程与HashSet有些类似：

```java
map.put(key1,value1)；
```

首先，调用key1所在类的hashCode()计算key1哈希值，此哈希值经过某种算法计算以后，得到在Entry数组中的存放位置bucket。

*   如果此位置上的数据为空，此时的(key1, value1)添加成功。 **----> 情况1**

 *    如果此位置上的数据不为空（意味着此位置上存在一个或多个数据（以链表形式存在）），比较key1和已经存在的一个或多个数据的哈希值：
      *    如果key1的哈希值与已经存在的数据的哈希值都不相同，此时(key1, value1)添加成功。**----> 情况2**
      *    如果key1的哈希值和已经存在的某一个数据(key2, value2)的哈希值相同，继续比较：调用key1所在类的equals(key2)方法，比较：
            *                如果equals()返回false：此时(key1, value1)添加成功。**----> 情况3**
            *                如果equals()返回true：使用value1**替换**value2。 **----> 情况4**

关于情况2和情况3：此时(key1, value1)和原来的数据以**链表**的方式存储。

#### 1.1.2 JDK8及以后

HashMap是数组+链表+红黑树实现

[![I7AqNq.png](https://z3.ax1x.com/2021/11/18/I7AqNq.png)](https://imgtu.com/i/I7AqNq)

相比于JDK7，JDK8的添加元素过程类似，但不同之处有很多：

*   实例化一个HashMap对象时，JDK8并没有立刻创建长度为16的数组，而是等待首次调用put()方法时，底层才创建，类似于单例模式的**懒汉式**，**延迟了数组的创建，节省内存**。
*   JDK 8底层的数组是：Node[]，而非Entry[]
*   形成链表时，JDK8将新的元素作为tail，即放在旧元素的下面（助记：七上八下）
*   当数组的某一个索引位置上的元素以**链表形式存在的数据个数 > 8** 且当前**数组的长度 > 64**时，此时此索引位置上的所数据改为使用**红黑树**存储。因此，对于JDK8来说，新加的键值对可能是一个node节点作为链表的tail，也有可能是树的叶子节点



### 1.2 HashMap的扩容

扩容：

当HashMap中的元素越来越多的时候，hash冲突的几率也就越来越高，因为数组的长度是固定的。所以为了提高查询的效率，就要对HashMap的数组进行扩容，在HashMap数组扩容之后，最消耗性能的点就出现了：原数组中的数据必须重新计算其在新数组中的位置，并放进去，这就是resize。

扩容时机：

当HashMap中的元素个数超过数组大小（注意是数组总长度length，不是数组中个数size）loadFactor时，就 会 进 行 数 组 扩 容，loadFactor的默认值（DEFAULT_LOAD_FACTOR）为0.75，这是一个经过统计学分析出来的折中取值。默认情况下，数组大小（DEFAULT_INITIAL_CAPACITY）为16，那么当HashMap中元素个数超过160.75=12（这个值就是代码中的threshold值，也叫做临界值）的时候，就把数组的大小扩展为2*16=32，即扩大一倍，然后重新计算每个元素在数组中的位置，而这是一个非常消耗性能的操作，所以如果我们已经预知HashMap中元素的个数，那么预设元素的个数能够有效的提高HashMap的性能。

JDK8扩容的变化：

JDK8中，当HashMap中的其中一个链的对象个数如果达到了**8个**，此时如果capacity没有达到**64**，那么HashMap会先扩容解决，如果已经达到了64，那么这个链会变成**红黑树**，结点类型由Node变成TreeNode类型。当然，如果当映射关系被移除后，下次resize方法时判断树的结点个数**低于6**（8*0.75=6）个，也会把**红黑树再转为链表**。

### 1.3 HashMap源码中的重要常量

```java
/*
 *      DEFAULT_INITIAL_CAPACITY : HashMap的默认容量，16
 *      DEFAULT_LOAD_FACTOR：HashMap的默认加载因子：0.75
 *      threshold：扩容的临界值，=容量*填充因子：16 * 0.75 => 12
 *      TREEIFY_THRESHOLD：Bucket中链表长度大于该默认值，转化为红黑树:8
 *      MIN_TREEIFY_CAPACITY：桶中的Node被树化时最小的hash表容量:64
*/
```



### 1.4 HashMap其他问题

*   **映射关系的key是否可以修改？answer：不要修改**

因为映射关系存储到HashMap中会存储key的hash值，这样就不用在每次查找时重新计算每一个Entry或Node（TreeNode）的hash值了，因此如果已经put到Map中的映射关系，再修改key的属性，而这个属性又参与hashCode值的计算，那么会导致匹配不上。

*   HashMap的底层实现原理？
*   HashMap 和 Hashtable的异同？
*   CurrentHashMap 与 Hashtable的异同？（暂时不讲）



## 2. LinkedHashMap

LinkedHashMap是HashMap的子类，在HashMap存储结构的基础上，使用了一对双向链表来记录添加元素的顺序。与LinkedHashSet类似，LinkedHashMap可以维护Map 的迭代顺序：迭代顺序与Key-Value 对的插入顺序一致



## 3. Map的常用方法

添加、删除、修改操作：
 *      Object put(Object key,Object value)：将指定key-value添加到(或修改)当前map对象中
 *      void putAll(Map m):将m中的所有key-value对存放到当前map中
 *      Object remove(Object key)：移除指定key的key-value对，并返回value
 *      void clear()：清空当前map中的所有数据

元素查询的操作：

 *      Object get(Object key)：获取指定key对应的value
 *      boolean containsKey(Object key)：是否包含指定的key
 *      boolean containsValue(Object value)：是否包含指定的value
 *      int size()：返回map中key-value对的个数
 *      boolean isEmpty()：判断当前map是否为空
 *      boolean equals(Object obj)：判断当前map和参数对象obj是否相等

元视图操作的方法：

 *      Set keySet()：返回所有key构成的Set集合
 *      Collection values()：返回所有value构成的Collection集合
 *      Set entrySet()：返回所有key-value对构成的Set集合



总 结：常用方法：

*   添加：put(Object key,Object value)

*   删除：remove(Object key)
*   修改：put(Object key,Object value)
*   查询：get(Object key)
*   长度：size()
*   遍历：keySet() / values() / entrySet()
    



其中遍历操作值得关注一下：

```java
@Test
public void test3(){
    Map map = new HashMap();
    map.put("AA",123);
    map.put(45,1234);
    map.put("BB",56);

    //遍历所有的key集：keySet()
    Set set = map.keySet();
    Iterator iterator = set.iterator();
    while (iterator.hasNext()){
        System.out.println(iterator.next());
    }
    System.out.println();

    //遍历所有的values集：values()
    Collection values = map.values();
    for (Object obj:values) {
        System.out.println(obj);
    }
    System.out.println();

    // 遍历所有的key-values，遍历的目的不是打印（HashMap可以直接打印），而是能取到每个元素
    // 方式一
    Set entrySet = map.entrySet();
    Iterator iterator1 = entrySet.iterator();
    while (iterator1.hasNext()){
        Object obj = iterator1.next();
        //entrySet集合中的元素都是entry,可以强转成entry，否则没有对应的方法获得key,value
        Map.Entry entry = (Map.Entry)obj;
        System.out.println(entry.getKey() + "---->" + entry.getValue());
    }

    //方式二：
    Set keySet = map.keySet();
    Iterator iterator2 = keySet.iterator();
    while(iterator2.hasNext()){
        Object key = iterator2.next();
        Object value = map.get(key);
        System.out.println(key + "---->" + value);
    }
}
```



## 4. TreeMap

TreeMap存储Key-Value 对时，需要根据key-value对进行排序。TreeMap可以保证所有的Key-Value 对处于有序状态。TreeSet底层使用**红黑树**结构存储数据
TreeMap的Key的排序：
自然排序：TreeMap的所有的Key 必须实现**Comparable接口**，而且**所有的Key应该是同一个类的对象**，否则将会抛出ClasssCastException
定制排序：创建TreeMap时，传入一个**Comparator** ，该对象负责对TreeMap中的所有key 进行排序。此时不需要Map 的Key实现Comparable 接口

TreeMap判断两个key相等的标准：两个key通过compareTo()方法或者compare()方法返回 0

```java
public class TreeMapTest {
    //自然排序
    @Test
    public void test(){
        TreeMap map = new TreeMap();
        User u1 = new User("Tom",23);
        User u2 = new User("Jack",32);
        User u3 = new User("Jack",20);
        User u4 = new User("Rose",18);

        map.put(u1,98);
        map.put(u2,89);
        map.put(u3,76);
        map.put(u4,100);

        Set set = map.entrySet();
        Iterator iterator = set.iterator();
        while (iterator.hasNext()){
            Object obj = iterator.next();
            Map.Entry entry = (Map.Entry) obj;
            System.out.println(entry.getKey() + "--->" + entry.getValue());
        }
    }

    //定制排序
    @Test
    public void test2(){
        TreeMap map = new TreeMap(new Comparator() {
            @Override
            public int compare(Object o1, Object o2) {
                if (o1 instanceof User && o2 instanceof User){
                    User u1 = (User) o1;
                    User u2 = (User) o2;
                    return Integer.compare(u1.getAge(),u2.getAge());
                }else{
                    throw new RuntimeException("输入的类型不匹配！");
                }
            }
        });

        User u1 = new User("Tom",23);
        User u2 = new User("Jerry",32);
        User u3 = new User("Jack",20);
        User u4 = new User("Rose",18);

        map.put(u1,98);
        map.put(u2,89);
        map.put(u3,76);
        map.put(u4,100);

        Set entrySet = map.entrySet();
        Iterator iterator1 = entrySet.iterator();
        while (iterator1.hasNext()) {
            Object obj = iterator1.next();
            Map.Entry entry = (Map.Entry) obj;
            System.out.println(entry.getKey() + "---->" + entry.getValue());
        }
    }
}
```

User部分代码：

```java
public class User implements Comparable{
    private String name;
    private int age;

    //……
    
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        User user = (User) o;
        return age == user.age && Objects.equals(name, user.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name, age);
    }

    //按照姓名从大到小排列,年龄从小到大排列
    @Override
    public int compareTo(Object o) {
        if (o instanceof User){
            User user = (User) o;
            int compare = -this.name.compareTo(user.name);
            if (compare!=0){
                return compare;
            }else{
                return Integer.compare(this.age,user.age);
            }
        }else{
            throw new RuntimeException("输入的类型不匹配");
        }
    }
}
```



## 5. Hashtable

Hashtable是个古老的Map 实现类，JDK1.0就提供了。不同于HashMap，Hashtable是**线程安全**的（尽管现在线程安全也还是使用HashMap）。Hashtable实现原理和HashMap相同，功能相同。底层都使用哈希表结构，查询速度快，很多情况下可以互用。与HashMap不同，Hashtable**不允许使用null 作为key和value**。与HashMap一样，Hashtable也不能保证其中Key-Value 对的顺序（**无序性**），Hashtable判断两个key相等、两个value相等的标准，与HashMap一致。



## 6. Properties

Properties 类是Hashtable的子类，该对象用于**处理属性文件**。由于属性文件里的**key、value都是字符串**类型，所以Properties 里的key和value都是字符串类型。存取数据时，建议使用setProperty(String key,Stringvalue)方法和getProperty(String key)方法

用到再说





# 四、Collections工具类

如同操作数组的工具类Arrays一样，操作集合也有自己的工具类：Collections，它提供操作Set、List和Map 等集合的方法

Collections中提供了一系列静态的方法对集合元素进行**排序、查询和修改**等操作，还提供了**对集合对象设置不可变**、对集合对象实现**同步控制**等方法

面试题：Collection 和 Collections的区别？
 *       Collection是集合类的上级接口，继承于他的接口主要有Set 和List.
 *       Collections是针对集合类的一个帮助类，他提供一系列静态方法实现对各种集合的搜索、排序、线程安全化等操作。



列举一些方法：

排序操作：（均为static方法）

*   reverse(List)：反转List 中元素的顺序
*   shuffle(List)：对List集合元素进行随机排序
*   sort(List)：根据元素的自然顺序对指定List 集合元素按升序排序
*   sort(List，Comparator)：根据指定的Comparator 产生的顺序对List 集合元素进行排序
*   swap(List，int，int)：将指定list 集合中的i处元素和j 处元素进行交换

查询操作：

*   Object max(Collection)：根据元素的自然顺序，返回给定集合中的最大元素

*   Object max(Collection，Comparator)：根据 Comparator 指定的顺序，返回给定集合中的最大元素

*   Object min(Collection)

*   Object min(Collection，Comparator)

*   int frequency(Collection，Object)：返回指定集合中指定元素的出现次数

*    void copy(List dest,List src)：将src中的内容复制到dest中，**使用此方法时需要注意dest的size要和src一样**

    ```java
    @Test
    public void test2(){
        List list = new ArrayList();
        list.add(123);
        list.add(43);
        list.add(765);
        list.add(-97);
        list.add(0);
    
        //错误演示，报异常：IndexOutOfBoundsException("Source does not fit in dest")
    //        List dest = new ArrayList();
    //        Collections.copy(dest,list);
    
        //正确使用：
        //先用 Arrays.asList将dest的size撑起来,使用Object[] 数组即可，传入list的长度
        List dest = Arrays.asList(new Object[list.size()]);
        Collections.copy(dest,list);
        System.out.println(dest);
    }
    ```

*   boolean replaceAll(List list，Object oldVal，Object newVal)：使用新值替换 List 对象的所有旧值

线程同步：

*   Collections 类中提供了多个 synchronizedXxx() 方法，该方法可使将指定集合包装成线程同步的集合，从而可以解决多线程并发访问集合时的线程安全问题。以List为例

    ```java
    List list = new ArrayList();
    //返回的list1即为线程安全的List
    List list1 = Collections.synchronizedList(list);
    ```

    
