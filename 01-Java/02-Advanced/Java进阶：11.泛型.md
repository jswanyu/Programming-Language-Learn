# 泛型

泛型，就是允许在定义类、接口时通过一个标识表示类中某个属性的类型或者是某个方法的返回值及参数类型。这个类型参数将在使用时（例如，继承或实现这个接口，用这个类型声明变量、创建对象时）确定（即传入实际的类型参数，也称为类型实参）。

泛型是从 JDK 1.5 引入的，在JDK 1.5之前只能把元素类型设计为Object，直接用Object会导致如下问题：

*   类型不安全问题，例如并不希望在整型数组里添加字符型数据
*   类型强制转换导致异常问题，例如需要将读取字符型数据强转成Integer，会导致异常



# 一、自定义泛型类、接口

自定义泛型类时，只需要在类名后加上`<T>`即可，代表类中可能会用到泛型，T换成其他字母也可以，常用`<T>`或者`<E>` ，`<K> <V>`通常是Map结构的键值泛型。泛型接口类似。

下面代码为自定义的泛型类，其中字段 `des` 定义为泛型，等待创建实例对象时指定具体类型

```java
public class PersonGeneric<T> {
    String name;
    int age;
    T des;

    public PersonGeneric(String name, int age, T des) {
        this.name = name;
        this.age = age;
        this.des = des;
    }

    public PersonGeneric(){
    }

    //如下的三个方法只是常规的方法，使用了泛型参数或者返回值而已，与泛型方法不同
    public void setDes(T des) {
        this.des = des;
    }

    public T getDes() {
        return des;
    }

    @Override
    public String toString() {
        return "PersonGeneric{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", des=" + des +
                '}';
    }
}
```

实例化时需要指明类的泛型，即具体是啥类型。如果不指定，则认为是Object类型，但并不推荐这样

```java
@Test
public void test2(){
    // 在PersonGeneric.java中定义了泛型类
    // 实例化时需要指明类的泛型，即具体是啥类型。如果不指定，则认为是Object类型，但并不推荐这样
    PersonGeneric personGeneric = new PersonGeneric("a", 1, "hello"); //这是没指定，认为是Object类型，并不推荐
    PersonGeneric<String> stringPersonGeneric = new PersonGeneric<>("b", 2, "world"); //这是指定了String类型
    PersonGeneric<Integer> integerPersonGeneric = new PersonGeneric<>("c", 3, 123); //这是指定了Integer类型
    System.out.println(stringPersonGeneric);

    //指定了类型后调用对应的方法则都变为对应的类型，如形参、返回值等
    stringPersonGeneric.setDes("java"); //这里只能传入String类型
    Integer a = integerPersonGeneric.getDes();
    System.out.println(a);
}
```



自定义泛型类、泛型接口的其他注意点：

*   泛型类可能有多个参数，此时应将多个参数一起放在尖括号内。比如：`<E1,E2,E3>`

*   泛型类的无参有参构造器无需再加`<>`

    ```java
    public PersonGeneric(){}
    //public PersonGeneric()<>{} 错误写法
    ```

*   泛型不同的引用不能相互赋值

    ```java
    ArrayList<String> list1 = new ArrayList<>();
    ArrayList<Integer> list2 = new ArrayList<>();
    //list1 = list2; 报错
    ```

*   如果泛型结构是一个接口或抽象类，则不可创建泛型类的对象。当然也是因为接口和抽象类本身的原因

*   jdk 1.7 有自动类型推断，泛型的简化操作：

    ```java
    //最初
    ArrayList<String> list1 = new ArrayList<String>();
    //jdk 1.7
    ArrayList<String> list1 = new ArrayList<>();
    ```

*   **泛型的指定中不能使用基本数据类型，可以使用包装类替换。**即8种：byte，short，int，long，float，double，char，boolean

*   在类/接口上声明的泛型，在本类或本接口中即代表某种类型，可以作为非静态属性的类型、非静态方法的参数类型、非静态方法的返回值类型。但在**静态方法中不能使用类的泛型**。原因：泛型是类在实例化时使用，而静态方法是早于实例化的，所以静态方法不能使用

*   异常类不能声明为泛型

    ```java
    // public class MyException<T> extends Exception{} 报错
    ```

*   在创建泛型数组时，写法需要注意，不能new泛型数组的对象，而是要先new Object的数组，再强转成泛型的

    ```java
    public class PersonGeneric<T> {
        //T[] arr = new T[10]; 这种写法编译不能通过
        T[] arr = (T[]) new Object[10]; //正确的写法
    }
    ```

*   泛型如果不指定，将被擦除，泛型对应的类型均按照Object处理，但不等价于Object。经验：泛型要么都使用，要么都不用






# 二、自定义泛型方法

方法，也可以被泛型化，不管此时定义在其中的类是不是泛型类，是泛型类的话也与泛型方法的泛型参数没有关系。

泛型方法的语法：

```java
//[访问权限] <泛型> 返回类型  方法名([泛型标识参数名称]) 抛出的异常
public class GenericMethods<T> {
    public <E> List<E> copyFromArrayToList(E[] arr){
        ArrayList<E> list = new ArrayList<>();
        for(E e : arr){
            list.add(e);
        }
        return list;
    }
}
```

类GenericMethods的泛型是T，而下面自定义的泛型方法泛型是E，也就是说泛型类的泛型和泛型方法的泛型没关系

```java
//GenericMethods种测试
public static void main(String[] args) {
    GenericMethods<String> stringGenericMethods = new GenericMethods<>();//构建String类型的GenericMethods对象
    Integer[] arr = {1, 2, 3, 4};
    List<Integer> integers = stringGenericMethods.copyFromArrayToList(arr);  //传入的是整型数组，返回的也是整型数组
    System.out.println(integers);
}
```

值得一提的是，**泛型方法可以定义成静态方法**，因为泛型参数是在调用方法时确定的，并非在实例化类时确定。

```java
public class GenericMethods<T> {
    public static <E> List<E> copyFromArrayToList(E[] arr){
        ArrayList<E> list = new ArrayList<>();
        for(E e : arr){
            list.add(e);
        }
        return list;
    }

    public static void main(String[] args) {
        Integer[] arr = {1, 2, 3, 4};
        List<Integer> integers = GenericMethods.copyFromArrayToList(arr);
        System.out.println(integers);
    }
}
```



感觉泛型方法写的不是很清楚，后续用得多可以参考书籍写详细点，从最简单的泛型方法写，没搜到太好的博客，可以参考《java编程思想》和《java核心技术：卷一》



# 三、泛型在继承上的体现

在类的继承上，父类有泛型，子类可以不保留父类泛型，也可以保留，同时子类也可以有自己独有的泛型，了解即可

*   子类不保留父类泛型

    *   全部擦除

        ```java
        class Father<T1, T2> {}
        class Son1 extends Father {}      // 等价于class Son extends Father<Object,Object>{}
        class Son1<A,B> extends Father {} // 子类有自己独有泛型
        ```

    *   指定具体类型

        ```java
        class Father<T1, T2> {}
        class Son2 extends Father<Integer, String> {}  //指定Integer和String，其他类型不能用，有时会用
        class Son2 extends<A,B> Father<Integer, String> {}
        ```

*   子类保留父类泛型

    *   全部保留

        ```java
        class Father<T1, T2> {}
        class Son3<T1, T2> extends Father<T1, T2> {}          //全部保留父类泛型，用的较多
        class Son3<T1, T2, A, B> extends Father<T1, T2> {}    //全部保留父类泛型的同时也有子类独有泛型
        ```

    *   部分保留

    *   ```java
        class Father<T1, T2> {}
        class Son4<T2> extends Father<Integer, T2>
        class Son4<T2,A,B> extends Father<Integer, T2> 
        ```

还有值得注意的地方是，类A是类B的父类，但是`G<A>` 和`G<B>`二者不具备子父类关系，二者是并列关系。但`A<G>` 是 `B<G>` 的父类

```java
@Test
public void test5(){
    Object obj = null;
    String str = null;
    obj = str;  //多态，父类引用指向子类对象

    Object[] arr1 = null;
    String[] arr2 = null;
    arr1 = arr2;  //同上

    List<Object> list1 = null;
    List<String> list2 = new ArrayList<String>();
    //虽然Object和String是子父类关系，但此时的list1和list2的类型不具有子父类关系
    //list1 = list2;  //编译不通过

    show(list1);
    //show(list2);//编译不通过，因为list1和list2的类型不具有子父类关系
    show1(list2);
}

public void show1(List<String> list){
}

public void show(List<Object> list){
}

@Test
public void test6(){
    List<String> list1 = null;
    ArrayList<String> list2 = null;
    list1 = list2;//list1和list2具有子父类关系
}
```



# 四、通配符在泛型中的使用

通配符：`？`，在泛型中表示未知的类型，比如，`List<?>, Map<?,?>`。通配的意思就是都适配，即其表示的泛型是所有具体泛型的父类，比如，`List<?>` 是List、List等各种泛型List的父类。

所以上文提到的，如果类A是类B的父类，那么`G<A>和G<B>`是没有关系的，但二者共同的父类是：G<?>

```java
@Test
public void test7(){
    List<Object> list1 = null;
    List<String> list2 = null;
    List<?> list = null; 
    //list是list1和list2的父类，所以可以赋值
    list = list1;
    list = list2;
}
```

但是通配符在泛型上有一些情况并不能使用：

*   不能用在泛型方法的声明上，返回值类型前的<>不能加？

    ```java
    public static <?> void test(ArrayList<?> list){}
    ```

*   不能用在泛型类的声明上

    ```java
    class GenericTypeClass<?>{}
    ```

*   不能用在创建对象上，右边属于创建集合对象

    ```java
    ArrayList<?> list = new ArrayList<?>()  //编译不通过
    ArrayList<?> list = new ArrayList<>()   //编译通过   
    ```

使用通配符后数据的读取和写入要求：

*   读取List<?>的对象list中的元素时，永远是安全的，因为不管list的真实类型是什么，它包含的都是Object
*   写入list中的元素时，不行。因为我们不知道c的元素类型，我们不能向其中添加对象，**唯一的例外是null，它是所有类型的成员**

```java
@Test
public void test8(){
    List<?> list = null;
    List<String> list3 = new ArrayList<>();
    list3.add("AA");
    list3.add("BB");
    list3.add("CC");
    list = list3;

    //获取(读取)：允许读取数据，读取的数据类型为Object。
    Object o = list.get(0);
    System.out.println(o);

    //添加(写入)：对于List<?>就不能向其内部添加数据。除了添加null之外。
    //因为不知道元素类型，所以无法传东西进去，但null可以
//    list.add("DD");
//    list.add('?');
    list.add(null);
}
```



有限制条件的通配符的使用：

-   <?>：允许所有泛型的引用调用

-   通配符指定上限，extends：使用时指定的类型必须是继承某个类，或者实现某个接口，即<=

    ```java
    <?extends A> 
    //(无穷小, A]，只允许泛型为A及A子类的引用调用
    ```

-   通配符指定下限，下限super：使用时指定的类型不能小于操作的类，即>=

    ```java
    <? super B> 
    //[B , 无穷大),只允许泛型为B及B父类的引用调用
    ```



```java
//Person类
public class Person {
}
//Student类
public class Student extends Person{
}

@Test
public void test9(){
    List<Student> list1 = new ArrayList<>();
    List<Person> list2 = new ArrayList<>();
    List<Object> list3 = new ArrayList<>();

    List<? extends Person> list4 = null;//list4可以作为Person及其子类的父类
    list4 = list1;
    list4 = list2;

    List<? super Person> list5 = null;//list5可以作为Person及其父类的父类
    list5 = list2;
    list5 = list3;


    //读取数据：
    Person p = list4.get(0);
    //Student s = list4.get(0); //编译不通过
    Object obj = list5.get(0);
    //Person obj = list5.get(0);//编译不通过

    //写入数据：
    //list4.add(new Student());//编译不通过
    list5.add(new Person());
    list5.add(new Student());
}
```

