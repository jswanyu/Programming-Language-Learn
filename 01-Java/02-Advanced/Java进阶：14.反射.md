# 一、反射机制概述与动态语言

Reflection（反射）是被视为动态语言的关键，反射机制允许程序在执行期借助于Reflection API取得任何类的内部信息，并能直接操作任意对象的内部属性及方法。

加载完类之后，在堆内存的方法区中就产生了一个Class类型的对象（一个类只有一个Class对象），这个对象就包含了完整的类的结构信息。我们可以通过这个对象看到类的结构。这个对象就像一面镜子，透过这个镜子看到类的结构，所以，我们形象的称之为：反射。



动态语言：

是一类在运行时可以改变其结构的语言：例如新的函数、对象、甚至代码可以被引进，已有的函数可以被删除或是其他结构上的变化。通俗点说就是在运行时代码可以根据某些条件改变自身结构。主要动态语言：Object-C、C#、JavaScript、PHP、Python、Erlang。

静态语言：

与动态语言相对应的，运行时结构不可变的语言就是静态语言。如Java、C、C++。

Java不是动态语言，但Java可以称之为“准动态语言”。即Java有一定的动态性，我们可以利用反射机制、字节码操作获得类似动态语言的特性。Java的动态性让编程的时候更加灵活



反射相关的主要API

- `java.lang.Class`:代表一个类
- `java.lang.reflect.Method`:代表类的方法
- `java.lang.reflect.Field`:代表类的成员变量
- `java.lang.reflect.Constructor`:代表类的构造器



# 二、Class类

Java类的加载过程：

程序经过Javac.exe命令后，会生成一个或多个字节码文件(.class结尾)。接着我们使用java.exe命令对某个字节码文件进行解释运行。相当于将某个字节码文件加载到内存中。此过程就称为类的加载。加载到内存中的类，我们就称为运行时类。**此运行时类，就作为Class的一个实例。**



<font color=red>Class类的常用方法</font>

<font color=red>（Java万事万物皆对象，即指这个Class类，不知道对不对）</font>





## 1. 获取Class实例的3种方式

加载到内存中的运行时类，会缓存一定的时间。在此时间之内，我们可以通过不同的方式来获取此运行时类。

**第三种更常用**

```java
@Test
public void test01() throws ClassNotFoundException {
    //方式一：
    Class c1 = Person.class;
    System.out.println(c1);  //class Person

    //方式二：通过运行时类的对象,调用getClass()
    Person p1 = new Person();
    Class c2 = p1.getClass();
    System.out.println(c2);  //class Person

    //方式三：调用Class的静态方法：forName(String classPath)
    Class c3 = Class.forName("Person");
    System.out.println(c3);  //class Person

    //地址相同，加载到内存中的运行时类，会缓存一定的时间，同一个运行时类
    System.out.println(c1 == c2);  //true
    System.out.println(c1 == c3);  //true
}
```



## 2. 可以有Class对象的类型

（1）`class`：外部类，成员(成员内部类，静态内部类)，局部内部类，匿名内部类
（2）`interface`：接口
（3）`[]`：数组，只要数组的元素类型与维度一样，就是同一个Class
（4）`enum`：枚举
（5）`annotation`：注解`@interface`
（6）`primitivetype`：基本数据类型
（7）`void`



```java
@Test
public void test02(){
    Class s1 = Object.class;
    Class s2 = Comparable.class;
    Class s3 = String[].class;
    Class s4 = int[][].class;
    Class s5 = ElementType.class;
    Class s6 = Override.class;
    Class s7 = int.class;
    Class s8 = void.class;
    Class s9 = Class.class;

    int[] a = new int[10];
    int[] b = new int[100];
    Class s10 = a.getClass();
    Class s11 = b.getClass();
    // 只要数组的元素类型与维度一样，就是同一个Class
    System.out.println(s10 == s11);  //true
}
```



## 3. 类的加载与ClassLoader的理解：了解

当程序主动使用某个类时，如果该类还未被加载到内存中，则系统会通过如下三个步骤来对该类进行初始化。

* 1、加载：

将class文件字节码内容加载到内存中，并将这些静态数据转换成方法区的运行时数据结构，然后生成一个代表这个类的java.lang.Class对象，作为方法区中类数据的访问入口（即引用地址）。所有需要访问和使用类数据只能通过这个Class对象。这个加载的过程需要类加载器参与。

* 2、链接：将Java类的二进制代码合并到JVM的运行状态之中的过程。
    * 验证：确保加载的类信息符合JVM规范，例如：以cafe开头，没有安全方面的问题
    * 准备：正式为类变量（static）分配内存并设置类变量默认初始值的阶段，这些内存都将在方法区中进行分配。
    * 解析：虚拟机常量池内的符号引用（常量名）替换为直接引用（地址）的过程。

* 3、初始化：
    * 执行类构造器()方法的过程。类构造器()方法是由编译期自动收集类中所有类变量的赋值动作和静态代码块中的语句合并产生的。（类构造器是构造类信息的，不是构造该类对象的构造器）。
    * 当初始化一个类的时候，如果发现其父类还没有进行初始化，则需要先触发其父类的初始化。
    * 虚拟机会保证一个类的()方法在多线程环境中被正确加锁和同步。



ClassLoader（类加载器）的理解：

* 类加载的作用：将class文件字节码内容加载到内存中，并将这些静态数据转换成方法区的运行时数据结构，然后在堆中生成一个代表这个类的java.lang.Class对象，作为方法区中类数据的访问入口。
* 类缓存：标准的JavaSE类加载器可以按要求查找类，但一旦某个类被加载到类加载器中，它将维持加载（缓存）一段时间。不过JVM垃圾回收机制可以回收这些Class对象。

类加载器作用是用来把类(class)装载进内存的。JVM 规范定义了如下类型的类的加载器。

[![Tu2Lkj.png](https://s4.ax1x.com/2021/12/20/Tu2Lkj.png)](https://imgtu.com/i/Tu2Lkj)

```java
@Test
public void test03(){
    //对于自定义类，使用系统类加载器进行加载
    ClassLoader classLoader = ClassLoaderTest.class.getClassLoader();
    System.out.println(classLoader);
    //调用系统类加载器的getParent()：获取扩展类加载器
    ClassLoader classLoader1 = classLoader.getParent();
    System.out.println(classLoader1);
    //调用扩展类加载器的getParent()：无法获取引导类加载器
    //引导类加载器主要负责加载java的核心类库，无法加载自定义类的。
    ClassLoader classLoader2 = classLoader1.getParent();
    System.out.println(classLoader2);

    ClassLoader classLoader3 = String.class.getClassLoader();
    System.out.println(classLoader3);
}
```





# 三、获取运行时类的完整结构

Class实例，即运行时类，可以通过反射获取类的各种结构（了解反射能够获取这些东西即可，不用每个都记住）

获取属性结构：

* getFields():获取当前运行时类及其父类中声明为public访问权限的属性
* getDeclaredFields():获取当前运行时类中声明的所有属性。（不包含父类中声明的属性）

获取权限修饰符  数据类型 变量名

* getModifiers()  获取权限修饰符
* getType()  获取数据类型
* getName()  获取变量名

获取方法及其结构：

* getMethods():获取当前运行时类及其所有父类中声明为public权限的方法

* getDeclaredMethods():获取当前运行时类中声明的所有方法。（不包含父类中声明的方法）

    对获取到的每个Method而言，又有方法可以获取其结构，例如注解、权限修饰符、返回值类型、方法名、形参列表、异常等

    * getAnnotations() 注解
    * getModifiers() 权限修饰符
    * getReturnType()  返回值类型
    * getName()  方法名
    * getParameterTypes()  形参列表
    * getExceptionTypes()

获取构造器结构

* getConstructors():获取当前运行时类中声明为public的构造器
* getDeclaredConstructors():获取当前运行时类中声明的所有的构造器

获取运行时类的父类及父类的泛型

* getSuperclass()  获取运行时类的父类
* getGenericSuperclass()  获取运行时类的带泛型的父类

获取运行时类的接口、所在包、注解等

* getInterfaces()  获取运行时类实现的接口
* getSuperclass().getInterfaces() 获取运行时类的父类实现的接口
* getPackage() 获取运行时类所在的包
* getAnnotations() 获取运行时类声明的注解



# 四、使用反射创建运行时类的对象

之前创造对象使用new方法，现在通过反射获取了运行时类，即Class的实例，可以使用运行时类创建对象

主要调用的方法是：`newInstance()`，调用此方法，创建对应的运行时类的对象。内部调用了运行时类的空参的构造器

因此要想此方法正常的创建运行时类的对象，要求：

* **运行时类必须提供空参的构造器**
* 空参的构造器的访问权限得够。通常，设置为**public**



使用反射创建对象

```java
@Test
public void test04() throws InstantiationException, IllegalAccessException {
//        Class clazz = Person.class;
        Class clazz = Class.forName("Person");
    	Object obj = clazz.newInstance();
        System.out.println(obj);  //Person{name='null', age=0}
}
```



顺便可以总结，之所以在javabean中要求提供一个public的空参构造器。原因：

* 便于通过反射，创建运行时类的对象
* 便于子类继承此运行时类时，默认调用super()时，保证父类有此构造器



举例体会反射动态性

```java
@Test
public void test05() {
    for (int i = 0; i < 10; i++) {
        int num = new Random().nextInt(3);
        String classPath = "";
        switch (num) {
            case 0:
                classPath = "java.util.Date";
                break;
            case 1:
                classPath = "java.lang.Object";
                break;
            case 2:
                classPath = "Person";
                break;
        }
        try {
            Class clazz = Class.forName(classPath);
            Object obj = clazz.newInstance();
            System.out.println(obj);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

运行时才知道结果，编译时不清楚，动态性的体现





# 五、使用反射指定对象结构

之前是使用new构造对象实例，以及在new时调用无参/有参构造器，或者通过set/get方法设置/获取实例的属性

那么现在通过反射创建了对象实例，如何使用反射进行同样的操作

后面需要用到的Person类

```java
/**
 * @Classname: Person
 * @author: wanyu
 * @Date: 2021/12/20 23:13
 */
public class Person {
    private String name;
    public int age;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public Person() {
    }

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    private Person(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }

    private String show(String nation){
        System.out.println("我来自" + nation);
        return nation;
    }

    private static void showDesc(){
        System.out.println("我来自中国");
    }
}
```



## 1. 调用运行时类的指定属性

一般为如下步骤：

* 创建运行时类
* 创建运行时类的对象
* 获取运行时类中指定变量名的属性
* 保证当前属性是可访问的（不是public的属性需要确保可访问）
* 获取、设置指定对象的此属性值

```java
@Test
public void test06() throws Exception {
    //创建运行时类
    Class clazz = Class.forName("Person");
    //创建运行时类的对象
    Person p1 = (Person) clazz.newInstance();
    Person p2 = (Person) clazz.newInstance();
    //获取运行时类中指定变量名的属性
    Field name = clazz.getDeclaredField("name");
    //保证当前属性是可访问的
    name.setAccessible(true);
    //获取、设置指定对象的此属性值
    name.set(p1,"Tom");
    name.set(p2,"Jack");

    System.out.println(name.get(p1));
    System.out.println(name.get(p2));
}
```



## 2. 调用运行时类中的指定方法

大概步骤同上，

但在调用时需要使用Method.invoke()方法，参数1：方法的调用者  参数2：给方法形参赋值的实参

```java
@Test
public void test06() throws Exception {
    // 创建运行时类
    Class clazz = Class.forName("Person");
    // 创建运行时类的对象
    Person p1 = (Person) clazz.newInstance();
    Person p2 = (Person) clazz.newInstance();
    // 获取指定的某个方法，getDeclaredMethod():
    // 参数1 ：指明获取的方法的名称  参数2：指明获取的方法的形参列表
    Method show = clazz.getDeclaredMethod("show", String.class);
    // 保证当前方法是可访问的
    show.setAccessible(true);
    // 调用方法的invoke():参数1：方法的调用者  参数2：给方法形参赋值的实参
    // invoke()的返回值即为对应类中调用的方法的返回值。
    Object returnValue1  = show.invoke(p1, "北京");  //我来自北京
    Object returnValue2  = show.invoke(p2, "南京");  //我来自南京

    System.out.println(returnValue1);  //北京
    System.out.println(returnValue2);  //南京
}
```

如果是静态方法，因为没有实例，所以直接给invoke函数传入运行时类即可，或者传入null

```java
@Test
public void test07() throws Exception {
    // 创建运行时类
    Class clazz = Class.forName("Person");
    System.out.println("+++++++++调用静态方法+++++++++++");
    Method showDesc = clazz.getDeclaredMethod("showDesc");
    showDesc.setAccessible(true);
    //传入运行时类即可
    Object returnValue = showDesc.invoke(Person.class);
    // Object returnValue = showDesc.invoke(null);
    // 写null也可以，事实上java已经知道了是从Person类中调用静态方法，之前不是静态方法的时候，invoke的第一个参数需要知道是给哪个实例对象，
    // 对于静态方法就不需要知道了，所以写null和Person.class都可以
    System.out.println(returnValue);//null
    // 如果调用的运行时类中的方法没有返回值，则此invoke()返回null
}
```



## 3. 调用运行时类中的指定构造器

步骤同上

但使用构造器构造实例还是使用之前用过的`newInstance()`方法

```java
@Test
public void test08() throws Exception {
    // 创建运行时类
    Class clazz = Class.forName("Person");

    //获取指定的构造器
    //getDeclaredConstructor():参数：指明构造器的参数列表
    Constructor constructor = clazz.getDeclaredConstructor(String.class,int.class);

    //保证此构造器是可访问的
    constructor.setAccessible(true);

    //调用此构造器创建运行时类的对象
    Object obj = constructor.newInstance("Tom",18);
    System.out.println(obj);
}
```



# 六、反射的应用：动态代理









































