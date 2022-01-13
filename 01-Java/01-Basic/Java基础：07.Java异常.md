# 异常

# 一、异常概述及分类

开发过程中系统运行时会遇到一些问题，很多问题不是靠代码能够避免的，比如：客户输入数据的格式，读取文件是否存在，网络是否始终保持通畅等等。在Java语言中，将程序执行中发生的不正常情况称为**异常**(开发过程中的语法错误和逻辑错误不是异常)。**为了防止异常出现，我们应该提前进行异常的处理，而不是在终端直接呈现为报错，当异常出现后，程序员应该去修改代码减少异常的发生，处理异常只是一道保险而已。**

```java
/*
 * java异常类体系结构如下
 * 
 * java.lang.Throwable
 * 		|----java.lang.Error:一般不编写针对性的代码进行处理
 * 		|----java.lang.Exception:可以进行异常处理
 * 			|----编译时异常(checked)列举部分：
 * 				|----IOEXception
 * 					|----FileNotFoundException
 * 				|----ClassNotFoundException
 * 			|----运行时异常(unchecked)列举部分：
 * 				|----NullPointerException
 * 				|----ArrayIndexOutOfBoundsException
 * 				|----ClassCaseException
 * 				|----NumberFormatException
 * 				|----InputMismatchException
 * 				|----ArithmaticException
 * 
 * 面试题:常见的异常有哪些？举例说明
 */
```

Java程序在执行过程中所发生的异常事件**广义**上可分为两类：

* **错误（Error）**： Java虚拟机无法解决的严重问题，一般不编写针对性的代码进行处理，而是修改代码。如：JVM系统内部错误、资源耗尽等严重情况

```java
public class ErrorTest {
	public static void main(String[] args) {
		//1.栈溢出:java.lang.StackOverflowError
		main(args);
		//2.堆溢出:java.lang.OutOfMemoryError
		Integer[] arr = new Integer[1024*1024*1024];
	}
}
```

* **异常（Exception）**：**狭义**上的异常是指其它因编程错误或偶然的外在因素导致的一般性问题，可以使用针对性的代码进行处理。如：空指针访问、试图读取不存在的文件、网络连接中断、数组角标越界等。异常又可以分为：

  * **编译时异常**：是指编译器要求必须处置的异常，编译不会通过。即程序在运行时由于外界因素造成的一般性异常。编译器要求Java程序必须捕获或声明所有编译时的异常，对于这类异常，如果程序不处理，程序无法正常运行（编译不通过）。

    ```java
    // ******************以下是编译时异常***************************
    // 代码编译时会提示：Unhandled exception: java.io.FileNotFoundException、Unhandled exception: java.io.IOException
    @Test
    public void test01() {
        File file = new File("hello.txt");
        FileInputStream fis = new FileInputStream(file);
    
        int data = fis.read();
        while(data != -1){
            System.out.print((char)data);
            data = fis.read();
        }
    
        fis.close();
    }
    ```

  * **运行时异常**：是指编译器不要求强制处置的异常，即使写了，编译也会通过。一般是指编程时的逻辑错误，是程序员应该积极避免其出现的异常。`java.lang.RuntimeException`类及它的子类都是运行时异常。对于这类异常，可以不作处理，因为这类异常很普遍，若全处理可能会对程序的可读性和运行效率产生影响。

    ```java
    // ******************以下是运行时异常***************************
    // 为了显示异常，以下代码编写时直接触发异常条件，编译不通过。真实环境中往往是可以编译通过，但因为运行时某些值动态改变而发生异常，导致程序终止
    
    // NullPointerException，空指针异常
    // java.lang.NullPointerException: Cannot invoke "String.charAt(int)" because "str" is null
    @Test
    public void test02(){
        String str = null;
        System.out.println(str.charAt(0));
    }
    
    // ArrayIndexOutOfBoundsException，数组越界异常
    // java.lang.ArrayIndexOutOfBoundsException: Index 10 out of bounds for length 10
    @Test
    public void test03(){
        int[] arr = new int[10];
        System.out.println(arr[10]);
    }
    
    // ClassCaseException，类型转换异常
    // java.lang.ClassCastException: class java.util.Date cannot be cast to class java.lang.String
    @Test
    public void test04(){
        Object obj = new Date();
        String str = (String) obj;
    }
    
    // NumberFormatException，数字格式化异常
    // java.lang.NumberFormatException: For input string: "abc"
    @Test
    public void test05(){
        String str = "abc";
        int num = Integer.parseInt(str);
    }
    
    // InputMismatchException，输入类型异常
    // 输入的如果不是整型会报错
    @Test
    public void test06(){
        Scanner scanner = new Scanner(System.in);
        int score = scanner.nextInt();
        System.out.printf("你输入的是:",score);
        scanner.close();
    }
    
    // ArithmeticException
    // java.lang.ArithmeticException: / by zero
    @Test
    public void test07() {
        int a = 10;
        int b = 0;
        System.out.println(a / b);
    }
    ```

    



# 二、异常处理机制

在编写程序时，经常要在可能出现错误的地方加上检测的代码，如进行x/y运算时，要检测分母为0，数据为空，输入的不是数据而是字符等。过多的if-else分支会导致程序的代码加长、臃肿，可读性差。因此采用异常处理机制。Java采用的异常处理机制**，是将异常处理的程序代码集中在一起，与正常的程序代码分开**，使得程序简洁、优雅，并易于维护。

## 1. try-catch-finally

### 1.1 语法

```java
/*
 * try{
 * 		//可能出现异常的代码
 * }catch(异常类型1 变量名1){
 * 		//处理异常的方式1
 * }catch(异常类型2 变量名2){
 * 		//处理异常的方式2
 * }catch(异常类型3 变量名3){
 * 		//处理异常的方式3
 * }
 * ...
 * finally{
 * 		//一定会执行的代码
 * }
 */
```

try语句块选定捕获异常的范围，将可能出现异常的代码放在try语句块中，一旦出现异常，就会在异常代码处生成一个对应异常类的对象并将此对象抛出，一旦抛出对象以后，其后的代码就不再执行。

在catch语句块中是对**括号中的异常对象**进行处理的代码。一旦处理完成，就跳出当前的 try-catch-finally结构，继续执行其后的代码。

finally语句为异常处理提供一个统一的出口，其中的语句无论什么情况都会被执行，finally语句是可选的。

基本使用：

```java
@Test
public void test08(){

    String str = "abc";
    try{
        int num = Integer.parseInt(str);
        System.out.println("hello-----1");
    }catch(NumberFormatException e){
        System.out.println("出现数值转换异常");
    }
    finally{
        System.out.println("总会被执行");
    }
    System.out.println("hello----2");
}
```

> 出现数值转换异常
> 总会被执行
> hello----2

### 1.2 注意事项

* try语句后面必须加上catch语句或者finally语句，不可以单独存在。try-catch和try-catch-finally结构更常用。

* 在try结构中声明的变量，在出了try结构以后，就不能再被调用

* 每个try语句块可以伴随一个或多个catch语句，用于处理可能产生的不同类型的异常对象，捕获到哪个异常就去执行对应处理异常的代码

* catch中的异常类型如果没有子父类关系，则谁声明在上，谁声明在下无所谓。 

  catch中的异常类型如果满足子父类关系，则要求**子类一定声明在父类的上面**。否则会报错，因为父类在子类上面的话，都将进入父类的处理语句中

  ```java
  @Test
  public void test09(){
  
      String str = "abc";
      try{
          int num = Integer.parseInt(str);
          System.out.println("hello-----1");
      }catch(NumberFormatException e){
          System.out.println("出现数值转换异常");
      }catch(NullPointerException e){
          System.out.println("出现空指针异常"); //NullPointerException和NumberFormatException无继承关系，顺序无所谓
      }catch(Exception e){ //Exception是父类，只能出现在下面，否则会报错
          System.out.println("出现异常");
      }
      finally{
          //System.out.println(num); //报错
          System.out.println("总会被执行");
      }
      //System.out.println(num); //报错
      System.out.println("hello----2");
  }
  ```

  > 出现数值转换异常
  > 总会被执行
  > hello----2

* 经常需要捕获异常的有关信息，去访问异常对象的成员变量或调用它的方法，常用的方法为：
  
  * getMessage() 获取异常信息，返回字符串
  
    ```java
    @Test
    public void test10(){
        String str = "abc";
        try{
            int num = Integer.parseInt(str);
        }catch(NumberFormatException e){
            System.out.println(e.getMessage());
        }
    }
    ```
  
    > For input string: "abc"
  
  * printStackTrace() 获取打印异常类名和异常信息，以及异常出现在程序中的位置。无返回值，更常用
  
    ```java
    @Test
    public void test11(){
        String str = "abc";
        try{
            int num = Integer.parseInt(str);
        }catch(NumberFormatException e){
            e.printStackTrace();
        }
    }
    ```
  
    > java.lang.NumberFormatException: For input string: "abc"
    > 	at java.base/java.lang.NumberFormatException.forInputString(NumberFormatException.java:67)
    > 	at java.base/java.lang.Integer.parseInt(Integer.java:668)
    > 	at java.base/java.lang.Integer.parseInt(Integer.java:786)
    >
    > ​	.......
  
  
  
* try-catch处理异常后，是会继续执行其后代码的，为什么还需要finally呢，因为涉及到catch代码中可能也会有异常，或者try-catch语句中有返回值的情况，这些情况想要执行一段在后面必须执行的代码，就得靠finally结构

  * catch语句中存在异常的情况

  ```java
  @Test
  public void test12() {
      try {
          int a = 10;
          int b = 0;
          System.out.println(a / b);
      } catch (ArithmeticException e) {
          int[] arr = new int[10];
          System.out.println(arr[10]); //数组越界异常
      } catch (Exception e) {
          //catch的异常并不会进入到这里进行处理，事实上这段代码中没有语句对其进行处理。catch的异常还是会被编译器报出。
          System.out.println("123");
      } finally {
          System.out.println("总会被执行");
      }
  }
  ```

  > 总会被执行
  >
  > 
  >
  > java.lang.ArrayIndexOutOfBoundsException: Index 10 out of bounds for length 10
  >
  > at Exception_laarn.ExceptionTest.test12(ExceptionTest.java:88)
  >
  > .......

  * try-catch-finally语句中存在返回值的情况

  ```java
  @Test
  public void test13() {
      int num = method();
      System.out.println(num);
  }
  
  public int method() {
      try {
          int[] arr = new int[10];
          System.out.println(arr[10]);
          return 1;
      } catch (ArrayIndexOutOfBoundsException e) {
          return 2;
      } finally {
          System.out.println("总会被执行");
          //由于finally中的语句总会被执行，如果没有finally语句则返回2，现在是返回3
          return 3;
      }
  }
  ```

  > 我一定会被执行
  > 3

* 像数据库连接、输入输出流、网络编程Socket等资源，JVM是不能自动的回收的，我们需要自己手动的进行资源的释放。此时的资源释放，就一般声明在finally中

* try-catch-finally结构可以嵌套

* 如果是捕捉IO输入输出流中的异常，一定要在try{…}catch{…}后，**加finally{…}把输入输出流关闭**





## 2. throws

### 2.1 throws基础用法

直接抛出异常是Java中处理异常的第二种方式。如果一个方法中的语句执行时**可能**生成某种异常，但是并不能确定如何处理这种异常，则此方法应显示地声明抛出异常，表明该方法将不对这些异常进行处理，而由该方法的调用者负责处理

在方法声明中后加上**throws**语句可以声明抛出异常的**列表**，用逗号隔开，throws后面的异常类型可以是方法中**可能**产生的异常类型，也可以是它的父类。一旦当方法体执行时，出现异常，仍会在异常代码处生成一个异常类的对象，此对象符合throws后的异常类型时，就会被抛出，异常代码后续的代码，不会再执行。

**务必注意：throws表示出现异常的一种可能性，并不一定会发生这些异常**

```java
//把异常抛出，但不解决
@Test
public void test14() throws ArithmeticException,RuntimeException{
    int a = 10;
    int b = 0;
    System.out.println(a / b);
}
```

> java.lang.ArithmeticException: / by zero
>
> ......

注意事项：

* try-catch-finally的结构是真正的将异常给处理掉了。throws的方式只是将异常抛给了方法的调用者。并没有真正将异常处理掉。

* 抛出的异常要是方法中会产生的异常类型，也可以是它的父类，如果异常类型不对应是没有意义的，编译器还是会抛出正确的异常类型

  ```java
  //抛出不对应的异常类型没有意义
  @Test
  public void test15() throws ArrayIndexOutOfBoundsException{
          int a = 10;
          int b = 0;
          System.out.println(a / b);
      }
  ```

  > java.lang.ArithmeticException: / by zero
  >
  > ......

  

### 2.2 重写方法声明抛出异常的原则

方法重写时如果也要抛出异常，**子类**重写的方法抛出的异常类型要求**不能大于父类**被重写的方法抛出的异常类型

```java
public class OverrideTest {
    public static void main(String[] args) {
        OverrideTest test = new OverrideTest();
        test.display(new SuperClass());
    }

    public void display(SuperClass s){
        try {
            s.method();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

class SuperClass{
    public void method() throws IOException{ 
    }
}
class SubClass extends SuperClass{
    public void method()throws FileNotFoundException {  //子类抛出的FileNotFoundException要求是父类抛出异常IOException的子类
    }
}
```



## 3. try-catch 和throws的选择

体会1：使用try-catch-finally处理编译时异常，是得程序在编译时就不再报错，但是运行时仍可能报错。相当于我们使用try-catch-finally将一个编译时可能出现的异常，延迟到运行时出现。

体会2：开发中，由于运行时异常比较常见，所以我们通常就不针对运行时异常编写try-catch-finally了。*而是尽量自己写代码避免这类异常的出现*



体会3：开发中如何选择使用try-catch-finally 还是使用throws？

​	3.1 如果父类中被重写的方法没有throws方式处理异常，则子类重写的方法也不能使用throws，意味着如果子类重写的方法中有异常，必须使用try-catch-finally方式处理。

​	3.2 执行的方法a中，先后又调用了另外的几个方法，这几个方法是递进关系执行的。我们建议这几个方法使用throws的方式进行处理。而执行的方法a可以考虑使用try-catch-finally方式进行处理。




## 4.手动抛出异常throw

Java异常类对象除在程序执行过程中出现异常时由系统自动生成并抛出，也可根据需要使用人工创建并抛出。

- 首先要生成异常类对象，然后通过`throw`语句实现抛出操作(提交给Java运行环境)，**注意不是throws！！！**
- 可以抛出的异常必须是Throwable或其子类的实例。

```java
public class StudentTest {
    public static void main(String[] args) {
        try {
            Student s = new Student();
            //s.regist(1001);
            s.regist(-1001);
            System.out.println(s);
        } catch (Exception e) {
            System.out.println(e.getMessage());
        }
    }
}

class Student {
    private int id;
    public void regist(int id) throws Exception {//这里是异常处理
        if (id > 0) {
            this.id = id;
        } else {
            // System.out.println("您输入的数据非法！"); //只是简单的打印信息并不能体现异常，新的对象还是会Int一个初始值正常运行

            // 手动抛出异常，需要选择合适的异常类，若选择RuntimeException可以不做处理，即void regist(int id)后无需throws异常
            // 这里括号的字符串信息，也是getMessage()中返回的值
            //throw new RuntimeException("您输入的数据非法！");

            // 若选择Exception，则必须要处理，否则编译不通过。
            // 此处是生成一个异常对象，这是自己抛出的，并且这种Exception是要处理的，处理的方式是在void regist(int id)后添加throws异常
            throw new Exception("您输入的数据非法！"); //这里是异常抛出
        }
    }
}
```



## 5. throw与throws的比较

* throws出现在方法函数头；

  而throw出现在函数体。

* **throws表示出现异常的一种可能性，并不一定会发生这些异常**；

  throw则是**就是这句代码出抛出了异常**，执行throw则一定抛出了某种异常对象。

* throws主要是声明这个方法会抛出这种类型的异常，使它的调用者知道要捕获这个异常。
  throw是具体向外抛异常的动作，所以它是抛出一个异常实例。

* 两者都是消极处理异常的方式（这里的消极并不是说这种方式不好），只是抛出或者可能抛出异常，但是不会由函数去处理异常，真正的处理异常由函数的上层调用处理。

* 如果在函数体内用throw抛出了某种异常，最好要在函数名中加throws抛异常声明（尽管不加也可以），然后交给调用它的上层函数进行处理

## 6. 用户自定义异常类

偶尔会自定义异常类，但一般地，用户自定义异常类都是RuntimeException的子类。

自定义异常类需要（编写时可以照着已有的异常类模仿）：

- 通常需要编写几个重载的构造器。
- 需要提供serialVersionUID
- 需要通过throw抛出。
- 规范命名。自定义异常最重要的是异常类的名字，当异常出现时，可以根据名字判断异常类型。

```java
public class MyException extends RuntimeException{
	static final long serialVersionUID = -7034897193246939L;
	
	public MyException(){ //重写无参构造器
	}
	
	public MyException(String msg){ //重写有参构造器
		super(msg);
	}
}
```



## 7. 总结

![exception](https://jswanyu-1309100582.cos.ap-shanghai.myqcloud.com/picgo/exception.png)

