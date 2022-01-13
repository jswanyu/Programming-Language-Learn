# 一、枚举类的理解

类的对象只有有限个，确定的。我们称此类为枚举类。当需要定义一**组常量**时，强烈建议使用枚举类

若枚举只有一个对象, 则可以作为一种**单例模式**的实现方式。



# 二、枚举类的实现

枚举类的实现

- JDK1.5之前需要自定义枚举类
- JDK 1.5 新增的enum 关键字用于定义枚举类



## 1. 自定义枚举类

现在用的较少，使用说明：

- 枚举类对象的属性不应允许被改动, 所以应该使用`private final`修饰
- 枚举类的使用`private final` 修饰的属性应该在构造器中为其赋值
- 若枚举类显式的定义了带参数的构造器, 则在列出枚举值时也必须对应的传入参数
- 在创建枚举类的实例对象时，应该使用`类.对象`的结构。`类名 实例名 = 类.枚举对象常量`

```java
class Season{
    //1.声明Season对象的属性:因为是常量所以用private final修饰
    private final String seasonName;
    private final String seasonDesc;

    //2.私有化类的构造器,并给对象属性赋值
    private Season(String seasonName, String seasonDesc){
        this.seasonName = seasonName;
        this.seasonDesc = seasonDesc;
    }

    //3.提供当前枚举类的多个对象，常量定义为static final
    public static final Season SPRING = new Season("春天","春暖花开");
    public static final Season SUMMER = new Season("夏天","烈日炎炎");
    public static final Season AUTUMN = new Season("秋天","金秋送爽");
    public static final Season WINTER = new Season("冬天","白雪皑皑");

    //4.其他诉求1：获取枚举类对象的属性
    public String getSeasonName() {
        return seasonName;
    }

    public String getSeasonDesc() {
        return seasonDesc;
    }

    //5.其他诉求2：提供toString()
    @Override
    public String toString() {
        return "Season{" +
                "seasonName='" + seasonName + '\'' +
                ", seasonDesc='" + seasonDesc + '\'' +
                '}';
    }
}

public class SeasonTest {
    public static void main(String[] args) {
        Season autumn = Season.AUTUMN;
        System.out.println(autumn); //Season{seasonName='秋天', seasonDesc='金秋送爽'}
    }
}
```



## 2.  使用enum关键字定义

使用说明：

- 必须在枚举类的第一行声明枚举类对象，比如下面的Season1在第一行就列出枚举类的对象
- 枚举类的所有实例必须在枚举类中显式列出(","隔开，末尾对象";"结束)。列出的实例系统会自动添加public static final 修饰，不用手动加上
- 枚举类的构造器同样只能使用`private final`权限修饰符
- 在创建枚举类的实例对象时，应该使用`类.对象`的结构。`类名 实例名 = 类.枚举对象常量`
- 使用enum定义的枚举类默认继承了`java.lang.Enum`类，因此不能再继承其他类（自定义枚举类是继承的`java.lang.Object`）

- [ ] **JDK 1.5 中可以在switch 表达式中使用Enum定义的枚举类的对象作为表达式, case 子句可以直接使用枚举值的名字, 无需添加枚举类作为限定。**

```java
//使用enum定义枚举类
enum Season1{
    //1.提供当前枚举类的对象，多个对象之间用","隔开，末尾对象";"结束
    SPRING("春天","春暖花开"),
    SUMMER("夏天","烈日炎炎"),
    AUTUMN("秋天","金秋送爽"),
    WINTER("冬天","白雪皑皑");

    //2.声明Season对象的属性:private final修饰
    private final String seasonName;
    private final String seasonDesc;

    //3.私有化类的构造器,并给对象属性赋值
    private Season1(String seasonName, String seasonDesc){
        this.seasonName = seasonName;
        this.seasonDesc = seasonDesc;
    }


    //4.其他诉求1：获取枚举类对象的属性
    public String getSeasonName() {
        return seasonName;
    }

    public String getSeasonDesc() {
        return seasonDesc;
    }

//    //5.其他诉求2：提供toString()
//    @Override
//    public String toString() {
//        return "Season{" +
//                "seasonName='" + seasonName + '\'' +
//                ", seasonDesc='" + seasonDesc + '\'' +
//                '}';
//    }
}


public class SeasonTestEnum {
    public static void main(String[] args) {
        Season1 spring = Season1.SPRING;
        //如果未重写toString，得到的是当前枚举常量的名称
        System.out.println(spring.toString());
        //查看Season1的父类
        System.out.println(Season1.class.getSuperclass());
    }
}
```



# 三、枚举类的常用方法



目前常用的如下：

* toString()：返回当前枚举类对象常量的**名称**，可以重写提升可读性
* values()：返回所有的枚举类对象构成的**数组**
* valueOf(String objName)：返回枚举类中对象名是objName的**对象**。和直接用点式结构创建实例对象是一样的，如果没有objName的枚举类对象，则抛异常：IllegalArgumentException

```java
@Test
public void enumMethodsTest(){
    //toString()：返回当前枚举类对象常量的名称
    System.out.println("************ toStirng() **************");
    Season1 winter = Season1.WINTER;
    System.out.println(winter.toString());

    //values()：返回所有的枚举类对象构成的数组
    System.out.println("************ values() **************");
    Season1[] season1s = Season1.values();
    for (int i = 0; i < season1s.length; i++) {
        System.out.println(season1s[i]);
    }

    //valueOf(String objName):返回枚举类中对象名是objName的对象。如果没有objName的枚举类对象，则抛异常：IllegalArgumentException
    System.out.println("************ valueOf() **************");
    Season1 spring = Season1.valueOf("SPRING");
    System.out.println(spring);
}
```





# 四、使用enum关键字定义的枚举类实现接口

枚举类实现接口时，有两种情况：

* 实现接口，在enum类中实现抽象方法 ，这种很简单，直接重写接口中的方法即可

```java
//接口
interface Info{
    void show();
}

//使用enum定义枚举类
enum Season1 implements Info{
    //1.提供当前枚举类的对象，多个对象之间用","隔开，末尾对象";"结束
    SPRING("春天","春暖花开"),
    SUMMER("夏天","烈日炎炎"),
    AUTUMN("秋天","金秋送爽"),
    WINTER("冬天","白雪皑皑");

    //2.声明Season对象的属性:private final修饰
    private final String seasonName;
    private final String seasonDesc;

    //3.私有化类的构造器,并给对象属性赋值
    private Season1(String seasonName, String seasonDesc){
        this.seasonName = seasonName;
        this.seasonDesc = seasonDesc;
    }

    @Override
    public void show() {
        System.out.println("这是一个季节。");
    }
}
```

* 让枚举类的对象分别实现接口中的抽象方法，即针对每种枚举对象，有自己的接口方法。在每个枚举对象后加上`{}`，在其中重写方法

```java
enum Season1 implements Info{
    //1.提供当前枚举类的对象，多个对象之间用","隔开，末尾对象";"结束
    SPRING("春天","春暖花开"){
        //重写show()
        @Override
        public void show() {
            System.out.println("一元复始、万物复苏");
        }
    },
    SUMMER("夏天","夏日炎炎"){
        @Override
        public void show() {
            System.out.println("蝉声阵阵、烈日当空");
        }
    },
    AUTUMN("秋天","秋高气爽"){
        @Override
        public void show() {
            System.out.println("天高气清、金桂飘香");
        }
    },
    WINTER("冬天","冰天雪地"){
        @Override
        public void show() {
            System.out.println("寒冬腊月、滴水成冰");
        }
    };

    //2.声明Season对象的属性:private final修饰
    private final String seasonName;
    private final String seasonDesc;

    //3.私有化类的构造器,并给对象属性赋值
    private Season1(String seasonName, String seasonDesc){
        this.seasonName = seasonName;
        this.seasonDesc = seasonDesc;
    }

    @Override
    public void show() {
        System.out.println("这是一个季节。");
    }
}


@Test
public void enumInterfaceTest(){
    Season1 spring = Season1.SPRING;
    spring.show(); //一元复始、万物复苏
}
```

