# 一、File类的使用

java.io.File类：文件和文件目录路径的抽象表示形式，与平台无关。 即File类的一个对象，代表一个文件或一个文件目录(俗称：文件夹)

File 能新建、删除、重命名文件和目录，但File 不能访问文件内容本身。如果需要访问文件内容本身，则需要使用输入/输出流。

想要在Java程序中表示一个真实存在的文件或目录，那么必须有一个File对象，但是Java程序中的一个File对象，可能没有一个真实存在的文件或目录。

File对象可以作为参数传递给流的构造器



## 1. File类的实例化

三种构造器：

*      File(String filePath):以filePath为路径创建File对象，可以是绝对路径或者相对路径
*      File(String parentPath,String childPath):以parentPath为父路径，childPath为子路径创建File对象。
*      File(File parentFile,String childPath):根据一个父File对象和子文件路径创建File对象



## 2. File类的常用方法

File类中涉及到关于文件或文件目录的创建、删除、重命名、修改时间、文件大小等方法，并未涉及到写入或读取文件内容的操作。如果需要读取或写入文件内容，必须使用IO流来完成。

后续File类的对象常会作为参数传递到流的构造器中，指明读取或写入的"终点"。

 * public String getAbsolutePath()：获取绝对路径
 * public String getPath() ：获取路径
 * public String getName() ：获取名称
 * public String getParent()：获取上层文件目录路径。若无，返回null
 * public long length() ：获取文件长度（即：字节数）。不能获取目录的长度。
 * public long lastModified() ：获取最后一次的修改时间，毫秒值



如下的两个方法仅适用于文件目录：

* public String[] list() ：获取指定目录下的所有文件或者文件目录的名称数组     
* public File[] listFiles() ：获取指定目录下的所有文件或者文件目录的File数组



* public boolean renameTo(File dest)：把文件重命名为指定的文件路径

file1.renameTo(file2)为例，要想保证返回true,需要file1在硬盘中是存在的，且file2不能在硬盘中存在



* public boolean isDirectory()：判断是否是文件目录
* public boolean isFile() ：判断是否是文件
* public boolean exists() ：判断是否存在
* public boolean canRead() ：判断是否可读
* public boolean canWrite() ：判断是否可写
* public boolean isHidden() ：判断是否隐藏



* public boolean createNewFile() ：创建文件。若文件存在，则不创建，返回false
* public boolean mkdir() ：创建文件目录。如果此文件目录存在，就不创建了。如果此文件目录的上层目录不存在，也不创建。
* public boolean mkdirs() ：创建文件目录。如果此文件目录存在，就不创建了。如果上层文件目录不存在，一并创建
* public boolean delete()：删除文件或者文件夹    **删除注意事项**：Java中的删除不走回收站。要想删除文件夹成功，文件夹目录下不能有子目录或文件



# 二、IO流原理及流的分类

## 1. IO流原理

I/O是Input/Output的缩写，I/O技术是非常实用的技术，用于处理设备之间的数据传输。如读/写文件，网络通讯等。

Java程序中，对于数据的输入/输出操作以“流(stream)”的方式进行。

java.io包下提供了各种“流”类和接口，用以获取不同种类的数据，并通过标准的方法输入或输出数据。

输入input：读取外部数据（磁盘、光盘等存储设备的数据）到程序（内存）中。

输出output：将程序（内存）数据输出到磁盘、光盘等存储设备中。



## 2. IO流的分类

- 按操作**数据单位**不同分为：字节流(8 bit)（用于处理图片、视频、.doc, .ppt等），字符流(16 bit)（用于处理文本文件：.txt, .java, .c, .cpp）

​		注意：不能用字符流来处理图片等字节数据。不用字节流来处理文本等字符数据，如果是纯英文的文本可能不会出错，但有其他文字在读的时候会出现乱码

- 按数据流的**流向**不同分为：输入流，输出流
- 按流的**角色**的不同分为：节点流，处理流。节点流就是从数据到内存的传输流，**处理流是对已有流（不限于是节点流）的再包装，可以提高流的处理速度**

Java的IO流共涉及40多个类，实际上非常规则，都是从下表第一行中4个抽象基类派生的

[![TSreeI.png](https://s4.ax1x.com/2021/12/15/TSreeI.png)](https://imgtu.com/i/TSreeI)

颜色加深的部分更为常用：文件流、缓冲流、转换流、对象流

# 三、节点流（或文件流）

## 1. 读数据

读取文件【四个步骤】：

* File类的实例化
* 实例化一个读文件的流对象，将已存在的一个文件加载进流。
* 创建一个临时存放数据的数组，调用流对象的读取方法将流中的数据读入到数组中。
* 关闭资源。

读单个字符：

```java
@Test
public void FileReadTest1(){
    FileReader fr = null;
    try {
        //1.实例化File对象，指明要操作的文件
        File file = new File("hello1.txt");//相较于当前的Module
        //2.提供具体的流
        fr = new FileReader(file);
        //3.数据的读入过程
        int data;
        while ((data = fr.read()) != -1){
            System.out.print((char)data);
        }
    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        try {
            //4.流的关闭操作
            if (fr!=null)
                fr.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

read()的理解：返回读入的一个字符。如果达到文件末尾，返回-1

异常的处理：为了保证流资源一定可以执行关闭操作。需要使用try-catch-finally处理

读入的文件一定要存在，否则就会报FileNotFoundException。



读多个字符：

```java
@Test
public void FileReadTest2(){
    FileReader fr = null;
    try {
        //1.File类的实例化
        File file = new File("hello1.txt");//相较于当前的Module
        //2.FileReader流的实例化
        fr = new FileReader(file);
        //3.读入的操作
        char[] cbuf = new char[5];
        int len;
        while ((len = fr.read(cbuf))!=-1){
            for (int i = 0; i < len; i++) {
                // 这里的循环终止条件必须是小于len，而不能是cbuf.length，
                // 否则每次都是输出5个字符，实际上我们只想让他每次读多少就输出多少
                System.out.print(cbuf[i]);
            }
        }
    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        try {
            //4.资源的关闭
            if (fr!=null)
                fr.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

read(char[] cbuf)：返回每次读入cbuf数组中的字符的个数。如果达到文件末尾，返回-1

## 2. 写数据

写入文件【四个步骤】：

* File类的实例化
* 实例化一个写入的流对象，将文件加载进流。该文件不存在会自动创建
* 调用流对象的写入方法，将数据写入流
* 关闭流资源，并将流中的数据清空到文件中



输出操作，对应的File可以不存在的。并不会报异常

File对应的硬盘中的文件如果不存在，在输出的过程中，会自动创建此文件。

File对应的硬盘中的文件如果存在：

如果流使用的构造器是：FileWriter(file,false) / FileWriter(file):对原有文件的覆盖

如果流使用的构造器是：FileWriter(file,true):不会对原有文件覆盖，而是在原有文件基础上追加内容


```java
@Test
public void FileWriteTest() {
    FileWriter fw = null;
    try {
        File file = new File("hello2.txt");
        fw = new FileWriter(file);
        //fw = new FileWriter(file,true);
        fw.write("Hello IO Stream!\n");
        fw.write("Hello world!");
    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        try {
            fw.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```



读写图片等字节流时方法流程类似，流构造器选择相对应的字节流构造器即可



# 四、缓冲流

为了提高数据读写的速度，Java API提供了带缓冲功能的流类，在使用这些流类时，会创建一个内部缓冲区数组，使用8192个字节(8Kb)的缓冲区。**这也是实际开发中常用的流**

缓冲流要“套接”在相应的节点流之上，根据数据操作单位可以把缓冲流分为：

- 字节流：BufferedInputStream和BufferedOutputStream
- 字符流：BufferedReader和BufferedWriter

当使用BufferedInputStream读取字节文件时，BufferedInputStream会一次性从文件中读取8192个(8Kb)，存在缓冲区中，直到缓冲区装满了，才重新从文件中读取下一个8192个字节数组。向流中写入字节时，不会直接写到文件，先写到缓冲区中直到缓冲区写满，BufferedOutputStream才会把缓冲区中的数据一次性写到文件里。

使用方法flush()可以强制将缓冲区的内容全部写入输出流。

关闭流的顺序和打开流的顺序相反。只要关闭最外层流即可，关闭最外层流也会相应关闭内层节点流



用缓冲流实现非文本文件的读写（复制文件）：

```java
@Test
public void BufferedStreamTest(){
    BufferedInputStream bis = null;
    BufferedOutputStream bos = null;
    try {
        //1.造文件
        File srcFile = new File("plot.png");
        File destFile = new File("plot1.png");

        //2.造流
        //2.1 造节点流
        FileInputStream fis = new FileInputStream(srcFile);
        FileOutputStream fos = new FileOutputStream(destFile);
        //2.2 造缓冲流
        bis = new BufferedInputStream(fis);
        bos = new BufferedOutputStream(fos);

        //3.复制的细节：读取、写入
        byte[] buffer = new byte[1024];
        int len;
        while ((len = bis.read(buffer))!=-1){
            bos.write(buffer,0,len);
        }
    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        //4.资源关闭
        if (bis!=null) {
            try {
                bis.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        if (bos!=null) {
            try {
                bos.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```

上面的1、2可以合写为：

```java
bis = new BufferedInputStream(new FileInputStream(new File("plot.png")));
bos = new BufferedOutputStream((new FileOutputStream(new File("plot1.png"))));
```





# 五、转换流

## 1. 转换流

转换流提供了在字节流和字符流之间的转换。Java API 提供了两个转换流：

* InputStreamReader：将InputStream转换为Reader，实现**将字节的输入流按指定字符集转换为字符的输入流**。需要和InputStream“套接”。

    构造器：public InputStreamReader(InputStreamin)
                   public InputSreamReader(InputStreamin,StringcharsetName)
                   如：Reader isr= new InputStreamReader(System.in,”gbk”);

* OutputStreamWriter：将Writer转换为OutputStream，实现**将字符的输出流按指定字符集转换为字节的输出流**。需要和OutputStream“套接”。
    构造器：public OutputStreamWriter(OutputStreamout)
                   public OutputSreamWriter(OutputStreamout,StringcharsetName)

很多时候我们使用转换流来处理文件乱码问题。实现编码和解码的功能。

* 解码：字节、字节数组  --->字符数组、字符串
* 编码：字符数组、字符串 ---> 字节、字节数组



转换流实现文件的读入和写出：

```java
public class InputStreamReaderTest {
    /**
     * 此时处理异常的话，仍然应该使用try-catch-finally
     * 综合使用InputStreamReader和OutputStreamWriter
     */
    @Test
    public void test2() throws IOException {
        //1.造文件、造流
        File file1 = new File("hello.txt");
        File file2 = new File("hello_gbk.txt");

        FileInputStream fis = new FileInputStream(file1);
        FileOutputStream fos = new FileOutputStream(file2);

        InputStreamReader isr = new InputStreamReader(fis,"utf-8");
        //参数2指明了字符集，具体使用哪个字符集，取决于文件hello.txt保存时使用的字符集
        OutputStreamWriter osw = new OutputStreamWriter(fos,"gbk");

        //2.读写过程
        char[] cbuf = new char[20];
        int len;
        while((len = isr.read(cbuf)) != -1){
            osw.write(cbuf,0,len);
        }

        //3.关闭资源
        isr.close();
        osw.close();
    }
}
```



## 2. 字符编码集

0 1 -->  ASCII（只适合英文）  -->  ANSI（美国国家标准学会，通常指平台默认编码，英文操作系统默认ISO-8859-1，中文是GBK，等等）  -->  Unicode （Unicode字符集只是定义了字符的集合和唯一编号，Unicode编码，则是对UTF-8、uCS-2/ UTF-16等具体编码方案的统称而已，并不是具体的编码方案。）

其中，UTF-8：是变长的编码方式，可用1-4个字节来表示一个字符，后来是6个。中文是3个字节





# 六、对象流

ObjectInputStream和OjbectOutputSteam，用于存储和读取基本数据类型数据或对象的处理流。它的强大之处就是可以把Java中的对象写入到数据源中，也能把对象从数据源中还原回来。对象流可能用的不多，**下面的序列化和反序列化是要掌握的重点**

对象序列化机制允许把内存中的Java对象转换成平台无关的二进制流，从而允许把这种二进制流持久地保存在磁盘上，或通过网络将这种二进制流传输到另一个网络节点。当其它程序获取了这种二进制流，就可以恢复成原来的Java对象。序列化是RMI（Remote Method Invoke –远程方法调用）过程的参数和返回值都必须实现的机制，而RMI 是JavaEE的基础。因此序列化机制是JavaEE平台的基础

序列化：用`ObjectOutputStream`类保存基本类型数据或对象到磁盘中或通过网络传输出去

反序列化：用`ObjectInputStream`类读取磁盘文件中的基本类型数据或对象，将其还原为内存中的一个java对象



## 1. 基本数据类型的序列化与反序列化

默认情况下，基本数据类型均可序列化

注意写出一次，操作flush()一次

```java
//序列化
@Test
public void test(){
    ObjectOutputStream oos = null;
    try {
        //创造流
        oos = new ObjectOutputStream(new FileOutputStream("object.dat"));
        //制造对象
        oos.writeObject(new String("秦始皇陵欢迎你"));
        //刷新操作
        oos.flush();
    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        if(oos != null){
            //3.关闭流
            try {
                oos.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}

//反序列化
@Test
public void test2(){
    ObjectInputStream ois = null;
    try {
        ois = new ObjectInputStream(new FileInputStream("object.dat"));

        Object obj = ois.readObject();
        String str = (String) obj;

        System.out.println(str);
    } catch (IOException e) {
        e.printStackTrace();
    } catch (ClassNotFoundException e) {
        e.printStackTrace();
    } finally {
        if(ois != null){
            try {
                ois.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```



## 2. 自定义类的序列化与反序列化

自定义类实现序列化与反序列化需要符合如下条件：

* 需要实现接口：`Serializable`或者`Externalizable`
* 除了当前类需要实现Serializable接口之外，还必须保证其内部所有属性也必须是可序列化的。（默认情况下，基本数据类型可序列化）
* 当前类提供一个全局常量：serialVersionUID



强调：如果某个类的属性不是基本数据类型或String 类型，而是另一个引用类型，那么这个引用类型必须是可序列化的，否则拥有该类型的Field 的类也不能序列化



补充：ObjectOutputStream和ObjectInputStream**不能序列化static和transient修饰**的成员变量



其中，对于第三点serialVersionUID的理解：定义语句为 `private static final long serialVersionUID;`，serialVersionUID用来表明类的不同版本间的兼容性。简言之，其目的是以序列化对象进行版本控制，有关各版本反序列化时是否兼容。如果类没有显示定义这个静态常量，它的值是Java运行时环境根据类的内部细节自动生成的。若类的实例变量做了修改（比如该类增加了一个属性），serialVersionUID可能发生变化。故建议，显式声明。简单来说，Java的序列化机制是通过在运行时判断类的serialVersionUID来验证版本一致性的。在进行反序列化时，JVM会把传来的字节流中的serialVersionUID与本地相应实体类的serialVersionUID进行比较，如果相同就认为是一致的，可以进行反序列化，否则就会出现序列化版本不一致的异常



自定义Person类：

```java
public class Person implements Serializable {
    public static final long serialVersionUID = 475463534532L;

    private String name;
    private int age;
    private int id;

    public Person() {
    }

    public Person(String name, int age, int id) {
        this.name = name;
        this.age = age;
        this.id = id;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", id=" + id +
                '}';
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

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
}
```

测试类：

```java
//序列化
@Test
public void test3(){
    ObjectOutputStream oos = null;
    try {
        //创造流
        oos = new ObjectOutputStream(new FileOutputStream("object.dat"));
        //制造对象
        oos.writeObject(new Person("李时珍",65,0));
        //刷新操作
        oos.flush();
    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        if(oos != null){
            //3.关闭流
            try {
                oos.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}

//反序列化
@Test
public void test4(){
    ObjectInputStream ois = null;
    try {
        ois = new ObjectInputStream(new FileInputStream("object.dat"));

        Person p = (Person) ois.readObject();

        System.out.println(p);
    } catch (IOException e) {
        e.printStackTrace();
    } catch (ClassNotFoundException e) {
        e.printStackTrace();
    } finally {
        if(ois != null){
            try {
                ois.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```



# 七、标准输入、输出流

System.in：标准的输入流，默认从键盘输入。System.in的类型是InputStream

System.out：标准的输出流，默认从控制台输出。System.out的类型是PrintStream，其是OutputStream的子类FilterOutputStream的子类

所以标准的输入输出流均为**字节流**，如果要将其转为字符，需要用到转换流

重定向：通过System类的setIn，setOut方法对默认设备进行改变。比如改为输出到文件

- public static void setIn(InputStreamin)
- public static void setOut(PrintStreamout)



# 八、打印流

实现将基本数据类型的数据格式转化为字符串输出

打印流：PrintStream和PrintWriter

提供了一系列重载的print()和println()方法，用于多种数据类型的输出

* PrintStream和PrintWriter的输出不会抛出IOException异常
* PrintStream和PrintWriter有自动flush功能
* PrintStream 打印的所有字符都使用平台的默认字符编码转换为字节。在需要写入字符而不是写入字节的情况下，应该使用PrintWriter 类。
* System.out返回的是PrintStream的实例



# 九、数据流

为了方便地操作Java语言的基本数据类型和String的数据，可以使用数据流。

数据流有两个类：(用于读取和写出基本数据类型、String类的数据）

DataInputStream和DataOutputStream，分别“套接”在InputStream和OutputStream子类的流上




# 十、任意存取文件流

 `RandomAccessFile` 声明在`java.io`包下，但直接继承于`java.lang.Object`类。并且它实现了`DataInput、DataOutput`这两个接口，也就意味着这个类**既可以读也可以写**。

它的特殊之处在于：支持“任意访问” 的方式，程序可以直接跳到文件的任意地方来读、写文件，

- 支持只访问文件的部分内容
- 可以向已存在的文件后追加内容

RandomAccessFile 对象包含一个记录指针，用以标示当前读写处的位置。RandomAccessFile类对象可以自由移动记录指针：

* long getFilePointer()：获取文件记录指针的当前位置
* void seek(long pos)：将文件记录指针定位到pos位置



构造器：

* public RandomAccessFile(Filefile, Stringmode)
* public RandomAccessFile(Stringname, Stringmode)

创建RandomAccessFile类实例需要指定一个mode 参数，该参数指定RandomAccessFile的访问模式：

* r: 以只读方式打开
* rw：打开以便读取和写入
* rwd:打开以便读取和写入；同步文件内容的更新
* rws:打开以便读取和写入；同步文件内容和元数据的更新

如果模式为只读r。则不会创建文件，而是会去读取一个已经存在的文件，如果读取的文件不存在则会出现异常。

如果模式为rw读写。如果文件不存在则会去创建文件，如果存在则不会创建。



```java
public class RandomAccessFileTest {
    /**
     * 使用RandomAccessFile实现数据的插入效果
     */
    @Test
    public void test5() throws IOException {
        RandomAccessFile raf1 = new RandomAccessFile("hello.txt","rw");

        raf1.seek(3);//将指针调到角标为3的位置
        //保存指针3后面的所有数据到StringBuilder中
        StringBuilder builder = new StringBuilder((int) new File("hello.txt").length());
        byte[] buffer = new byte[20];
        int len;
        while((len = raf1.read(buffer)) != -1){
            builder.append(new String(buffer,0,len)) ;
        }
        //调回指针，写入“xyz”
        raf1.seek(3);
        raf1.write("xyz".getBytes());

        //将StringBuilder中的数据写入到文件中
        raf1.write(builder.toString().getBytes());

        raf1.close();
    }

}
```



