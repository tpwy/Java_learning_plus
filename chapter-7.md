# 第七章 异常、断言和日志

## 7.1 异常的基本概念

异常(Exception)是指在程序运行中由代码产生的一种错误。

### 7.1.1错误和异常

当程序不能正常运行或运行结果不正确时，表明程序中有错误。

按错误的性质可以分为三种：

1. 语法错：

    由于违反程序设计语言的语法规则而产生的错误。通常在编译时能被发现，并能给出错误的位置和性质，所以又称编译错误。

2. 语义错：

    程序在语法上正确，但在语义上存在错误。语义错不能被编译系统检测出来，只有到程序运行时才能发现其错误，所以又称运行错。Java解释器在运行时一旦发现语义错，就会停止程序运行，并给出错误的位置和性质

    + 有些错误能够被程序事先处理，程序中应设法避免这类错误
    + 有些语义错误不能被程序事先处理，错误的发生不由程序本身所控制，因此必须进行异常处理

3. 逻辑错：

    如果程序编译通过也可运行，但运行结果与预期结果不符，这类错误是程序不能实现程序员的设计意图和设计功能而产生的错误，所有称为逻辑错。这类错误必须凭借程序员自身的程序设计经验找出错误的原因和位置并进行改正

按错误的严重程度不同，可以把运行错分为下面两类

1. 错误：指程序在执行过程中所遇到的硬件或操作系统的错误。这种错误对于程序而言是致命的，错误将导致程序无法运行，而且程序本身不能处理错误，只能依靠外部敢于，否则会一直处于非正常状态
2. 异常：是指在硬件和操作系统正常时，程序遇到的运行错。异常对于程序而言是非致命的，虽然异常会导致程序非正常终止，但Java语言的异常处理机制是程序自身能够捕获和处理异常，由异常处理代码调整程序运行方法，使得程序仍可继续运行。

### 7.1.2 Java语言的异常处理机制

1. 异常
    
    + 异常是指程序在运行过程中发生由于算法考虑不周或软件设计错误等导致的程序异常事件
    + Java语言提供的异常处理机制是通过面向对象的方法来处理异常的
    + 在Java语言中，所有异常都是以类的形式存在
    + 除了内置的异常类之外，Java语言也允许用户自行定义异常类

2. 抛出异常

    + 在程序运行过程，如果发生异常时间，则产生代表该异常的一个"异常对象"，并把它交给运行系统，由运行系统寻找相应的代码来处理这一异常。
    + 生成异常对象并把它提交给运行系统的过程称为抛出错误
    + 异常对象根据其类型，可能是由应用程序本身产生，也可能由Java虚拟机产生
    + 异常对象中包含了异常事件类型以及发生异常时应用程序自身的状态和调用过程等必须的信息

3. 捕获异常

    + 异常抛出后，运行系统从生成异常对象的代码开始，沿方法的调用栈逐层回溯查找，知道想到包含相应异常处理的方法，并把异常对象提交给方法为止，这个过程称为捕获异常
    + Java定义的异常类中包含了该运行错误的讯息和处理错误的方法等内容。
    + 异常本质上是一个程序在必行期间发生的事件，这个事件将中断的程序必行。

### 7.1.3 异常处理类

+ 在Java程序设计语言中，异常对象都是派生于`Throwable`类的一个实例。它是`java.lang`包中的一个类，用来表示所有的异常情况。
+ 该类派生了两个子类`java.lang.Error`和`java.lang.Exception`。
+ 其中`Error`子类由系统保留，因为该类定义哪些应用程序无法捕捉到的错误。
    + `Error`类及其子类的对象，代表了程序运行时Java系统内部的错误
    + `Error`类及其子类的对象是由Java虚拟机生成并抛出给系统的。通常Java程序不对这种错误进行直接处理，必须交由操作系统处理
+ `Exception`子类则是供应用程序使用的，它是用户程序能够捕捉到的异常情况
    + 一般所说的异常都是指`Exception`类及其子类
    + `Exception`类对象是Java程序抛出和处理的对象，它有不同的子类分别对应于各种不同类型的异常

```
Throwable(); // 构造方法
Throwable(String message); // 参数为对象的特定详细描述信息
String getMessage(); // 获得详细描述信息

public String toString(); // 返回描述当前Throwable类信息的字符串
public void printStackTrace(); 
// 完成一个输出操作，在当前的标准输出设备上输出当前异常对象的堆栈使用轨迹
```

### 7.1.4 异常类的层次结构

+ Throwable
    + Error错误类
        + VirtualMachineError虚拟机错误类
            + OutOfMemoryError内存溢出错误类
            + StackOverFlowError栈溢出错误类
        + LinkageError链接错误类
            + NoClassDefFoundError类定义未找到错误类
        + ...
        + java.awt.AWTError图形界面错误类
    + Exception异常类
        + RuntimeException运行时异常类
            + ArithmeticException算术异常类
            + NullPointerException空指针异常类
            + IndexOutOfBoundsException下标越界异常类
                + ArrayIndexOutOfBoundsException数组下标越界异常类
            + NegativeArraySizeException数组元素个数为负异常类
            + ClassCastException类型强制转换异常类
            + IllegalArgumentException无效参数异常类
                + NumberFormatException数值格式异常类
        + IllegalAccessException非法访问异常类
        + java.awtAWTException图形界面异常类
        + ClassNotFoundException类没有找到异常类
        + IOException输入输出异常类
            + EOFException文件已结束异常类
            + ...
            + FileNotFoundException文件未找到异常类

1. `RuntimeException`可以不编写异常处理的程序代码，依然可以正常编译。这类异常应该通过程序调试尽量避免而不是使用`try-catch-finally`语句来捕获它
2. 除`RuntimeException`外，其他则是非运行时异常，往往是由环境原因造成的，必须使用`try-catch-finally`语句去捕捉它并进行相应处理
3. Java语言规范将派生于`Error`类或`RuntimeException`类的所有异常称为非受查(unchecked)异常，所有其他的异常称为(checked)异常。

综上，程序对错误和异常的处理方式有三种：
1. 程序不能处理的错误
2. 程序应避免而可以不去捕获的运行时异常
3. 必须捕获的非运行时异常

## 7.2 异常的处理

### 7.2.1 声明受查异常

在自己编写方法的时候，不必将所有可能的异常都进行进行声明。遇到一下四种情况时需要抛出异常：
1. 调用一个抛出受查异常的方法。如`FileInputStream`构造器
2. 程序运行过程中发生错误，并且利用`throw`语句抛出一个受查异常
3. 程序出现错误
4. Java虚拟机和运行库出现的内部错误

+ 如果没有处理器捕获这个异常，当前执行的线程就会结束。
+ 对于那些可能被他人使用的Java方法，应该根据异常规范(exception specification)，在方法的首部声明可能抛出的异常。如果有多个受查异常类型，就必须在方法的首部列出所有的异常类，并用逗号隔开。
+ 对于Java的内部错误(即从Error继承的错误)和非受查异常，都不应该声明
+ 捕获异常会使得异常不被抛到方法之外，也不需要`throws`规范

### 7.2.2 抛出异常

根据异常类型的不同，抛出异常的方法也不同：

1. 系统自动抛出的异常

    所有系统定义的运行时异常都可以由系统抛出

2. 指定方法抛出的异常

    制定方法抛出异常需要用`throw`或`throws`来明确制定方法内抛出异常，如用户程序自定义的异常

在实际编程中，并不需要由产生异常的方法自己处理，而需要在方法之外处理。这时与异常有关的方法有两个：

1. 抛出异常的方法(即方法中的异常没有用`try-catch`语句捕获处理)

    + 在方法体内用`throw`语句抛出异常对象
    + 在方法头添加`throws`子句表示方法将抛出异常

2. 处理异常的方法

    由一个方法抛出异常后，该方法内又没有处理异常的语句，则系统就会将异常向上传递，一层一层向上传递知道上层的方法中有处理异常的语句，如果一直没有，最终将追溯到`main()`方法，这时JVM将进行处理

对于程序中无法处理必须交给系统处理的异常，由于系统直接调用主方法`main()`,可以在主方法头使用`throws`子句声明抛出异常交由系统处理

### 7.2.3 自定义异常类

创建自定义类要求：
1. 声明一个新的异常类，用户自定义的类推荐是`Exception`类的直接或间接子类
2. 为用户自定义的异常类定义属性和方法，或覆盖父类的属性和方法。习惯上在自定义异常类中加入两个构造方法，一个是默认的构造方法，一个是带有详细异常信息的构造器(`toString`方法将会打印出这些详细信息)


### 7.2.4 捕获异常

如果某个异常没有在任何地方进行捕获，那程序就会终止执行，并在控制台上打印除异常信息，其中包括异常的类型和堆栈的内容

要想捕获一个异常，必须设置`try/catch`语句块

```
public static void main(String[] args)
{
    int num;
    try
    {
        check(args[0]);
        num = Integer.parseInt(args[0]);
        if (num > 60)
            System.out.println("成绩为： " + num + " 及格");
        else
            System.out.println("成绩为： " + num + " 不及格");
    }
    catch (NullPointerException e)
    {
        System.out.println("空指针异常： " + e.toString());
    }
    catch (NumberFormatException e)
    {
        System.out.println("输入的参数不是数值类型");
    }
    catch (Exception e)
    {
        System.out.println("命令行中没有参数");
    }
}

static void check(String str1) throws NullPointerException
{
    if (str1.length() > 2)
    {
        str1 = null;
        System.out.println(str1.length());
    }

    char ch;
    for(int i = 0; i < str1.length(); i++)
    {
    ch = str1.charAt(i);
        if (!Character.isDigit(ch))
        {
            throw new NumberFormatException();
        }
    }
}
```

注意，捕获多个异常的时候，异常变量隐含为`final`变量，所以不能像下例一样在子句体中为e赋予不同的值

```
catch (FileNotFoundException | UnknownException e) {...}
```

#### 7.2.4.1 再次抛出异常与异常链

在catch子句中还可以抛出一个以后，这样做的目的是改变异常的类型。推荐在再次抛出异常时将原始异常设置为新异常的"原因"

```
try
{
    access the database
}
catch (SQLException e)
{
    Throwable se = new ServletException("database error");
    se.initCase(e)
    throw se;
}

// 当捕获到异常时，可以用下列语句重新得到原始异常

Throwable e = se.getCause();

// 如果你想只记录一个异常，再将它重新抛出，而不做任何改变的话

try
{
    access the database
}
catch (Exception e)
{
    logger.log(level, message, e);
    throw e;
}
```

#### 7.2.4.2 finally子句

+ try语句可以只有finally子句而没有catch子句
+ 强烈建议解耦合`try/catch`和`try/finally`语句块，这样可以提高代码的清晰度

```
InputStream in = ...;
try
{
    try
    {
        code that might throw exceptions
    }
    finally
    {
        in.close();
    }
}
catch (IOException e)
{
    show error message;
}
```

+ 当finally子句包含return语句时，将会覆盖try语句中的返回值
+ 如果finally子句也抛出异常，那么原始的异常将会丢失，转而抛出finally子句造成的异常

#### 7.2.4.3 带资源的try语句

+ 带资源的try语句(try-with-resource)类似于Python的with语句，能够自动调用`xx.close()`关闭资源
+ 使用带资源的try语句可以有效解决finally子句带来的异常丢失的问题

```
try (Scanner in = new Scanner(new FileInputStream("/usr/share/dict/words"), "UTF-8"))
{
    while (in.hasNext())
        System.out.println(in.next());
}

// 还可以指定多个资源

try (Scanner in = new Scanner(new FileInputStream("/usr/share/dict/words"), "UTF-8");
    PrintWriter out = new PrintWriter("out.txt"))
{
    while (in.hasNext())
        out.println(in.next().toUpperCase());
}
``` 

### 7.2.5 分析堆栈轨迹元素

堆栈轨迹(stack trace)是一个方法调用过程的列表，它包含了程序执行过程中方法调用的特定位置

```

// 可以调用Throwable类的printStackTrace方法访问的堆栈轨迹的文本描述信息

Throwable t = new Throwable();
StringWriter out = new StringWriter();
t.printStackTrace(new PrintWriter(out));
String description = out.toString();

// 更灵活的方法是使用getStackTrace方法，它会得到一个StackTraceElement对象的一个数组

Throwable t = new Throwable();
StackTraceElement[] frames = t.getStackTrace();
for (StackTraceElement frame:frames)
    analyze frame

// 静态的Thread.getAllStackTrace方法可以获得所有线程的堆栈轨迹

Map<Thread, StackTraceElement[]> map = Thread.getAllStackTraces();
for (Thread t : map.keySet())
{
    StackTraceElement[] frames = map.get(t);
    analyze frames
}
```

## 7.3 使用异常处理机制的技巧

1. 异常处理不能替代简单的测试(捕获异常所花费的时间大大超过前者)
2. 不要过分细化异常
3. 利用异常层次结构(选择更加适合的类或子类，或创建自己的异常类)
4. 不要压制异常
5. 在检测错误时，"苛刻"要比放任更好
6. 不要羞于传递异常

## 7.3 断言

### 7.3.1 断言的概念

假设确信某个属性符合要求，并且代码的执行依赖于这个属性

```
double y = Math.sqrt(x);

// 为了避免x<0,可以通过抛出异常进行检查

if (x < 0) throw new IllegalArgumentException("x < 0")

但这样的代码会一直保留在程序中，即使测试完也不会自动删除。这样大量的检查会导致程序运行变慢
```

Java引入了关键字`assert`，这个关键字有两种格式

1. assert 条件——如果结果为false，抛出AssertionError异常
2. assert 条件：表达式——表达式传入AssertionError构造器，转换成一个消息字符串

这里表达式部分的唯一目的是产生一个消息字符串，AssertionError对象并不存储表达式的值。

上例可以简化为 

```
assert x >= 0;
assert x >= 0 : x;
```

C++注释：C语言中的`assert`会将断言中的条件转换成一个字符串，断言失败时将条件打印出来。而Java只打印表达式传递给AssertionError对象的内容。

### 7.3.2 启用和禁用断言

在默认情况下，断言被禁用。启用命令

```
java -enableassertions MyApp
java -ea MyApp

// 在整个类或整个包中使用断言

java -ea:MyClass -ea:com.mycompany.mylib...MyApp
// 开启了MyClass类，和com.mycompany.maylib包和它的子包中所有类的断言

// 禁用某个特定类和包的断言
// 用 -disableassertions或-da选项

java -ea:.... -da:MyClass MyApp

```

+ 启用和禁用断言不需重新编译程序，启用和禁用断言是类加载器(class loader)的功能。
+ 直接由虚拟机加载的类，可以使用这些开关选择性的启动或禁用断言
+ 对于-ca和-da不能应用到的没有类加载器的"系统类"，对于这些系统，我们采用-enablesystemassertions/-ead开关启用断言

### 7.3.3 使用断言

使用情况：
1. 断言失败是致命的不可恢复的错误
2. 断言检查只用在开发和测试阶段

assert常用于自我检查。断言是一种测试和调试阶段所使用的战术性工具；而日志记录是一种在程序的整个生命周期都可以使用的策略型工具

## 7.5 记录日志

+ Java程序员都很熟悉在有问题的代码中插入一些`System.out.println`方法调用来帮助观察程序运行的操作过程
+ 解决问题后就要把语句删掉，一旦出现问题又要重新插入。这样的调试方法显得麻烦

记录日志API则是为解决这个问题而设计的，拥有以下优点：
1. 可以容易地取消全部日志记录，或仅仅取消某个级别的日志，并且打开和关闭这样的操作也很容易
2. 可以简单地进制日志记录的输出，因此将日志代码留在程序中的开销很小
3. 日志记录可以被定向到不同的处理器(控制台、存储为文件)
4. 日志记录器和处理器都可以对记录进行过滤。过滤器可以根据过滤实现器制定的标准丢弃无用的记录项
5. 日志记录可以用不同的方法格式化(纯文本、XML)
6. 应用程序可以有多个日志记录器，它们使用类似包名的这种具有层次结构的名字
7. 在默认情况下日志系统的配置由配置文件控制，配置可以替换

### 7.5.1 基本日志

生成简单的日志记录可以使用全局日志记录器(global logger)并调用其`info`方法

```
Logger.getGlobal().info("File->Open menu item selected");

// 默认情况下将显示
Jul 16, 2017 9:25:31 PM logger.loggerTest main
信息: File->Open menu item selected

//在适当地方调用以下语句可以取消所有日志
Logger.getGlobal().setLevel(Level.OFF);
```

### 7.5.2 高级日志

企业级(industrial-strength)日志：在一个专业的应用程序中，不要将所有的日志都记录到一个全局日志记录器中，而是自定义日志记录器

```
// 可以调用getLogger方法创建或获取记录器
private static final Logger myLogger = Logger.getLogger("com.mycompany.myapp")
```

注意未被任何变量引用的日志记录器可能会被垃圾回收，因此我们使用一个静态变量在存储日志记录器的引用

日志记录器名也具有层次结构，父与子之间将共享一些属性。

通常有以下7个日志记录器级别：

+ SEVERE
+ WARNING
+ INFO
+ CONFIG
+ FINE
+ FINER
+ FINEST

默认情况下只记录前三个级别。也可以设置其他的级别，如

```
logger.setLevel(Level.FINE);
```

则现在FINE和更高级别的记录都可以记录下来了。另外`Level.ALL`方法还可以开启所有级别的记录，`Level.OFF`关闭所有级别的记录

```
// 对所有级别有下面几种记录方法
logger.warning(message);
logger.fine(message);

// 使用log方法指定级别
logger.log(Level.FINE, message);
```

默认的日志配置记录了`INFO`或更高级别的所有记录，所以使用`CONFIG、FINE、FINER或FINEST`级别来记录有助于判断但对于程序员又没有太大意义的调试信息。如果将记录级别设置为`INFO`或更低，则需要修改日志处理器的配置，默认的日志配置器不会处理低于`INFO`级别的信息

默认的日志记录器将显示包含日志调用的类名和方法名，可以通过`logp`方法获得调用类和方法的确切位置。方法的签名为

```
void logp(Level l, String className, String methodName, String message)

// 还有一些用来跟踪执行流的方法

void entering(String className, String methodName)
void entering(String className, String methodName, Object param)
void entering(String className, String methodName, Object[] params)

void exiting(String className, String methodName)
void exiting(String className, String methodName, Object result)

// 举个例子
int read(String file, String pattern)
{
    logger.entering("com.mycompany.mylib.Reader", "read",
        new Object[] {file, pattern});
    ...
    logger.exiting("com.mycompany.mylib.Reader", "read", count);
    return count;
}
// 这些调用将生成FINER级别和以字符串ENTRY和RETURN开始的日志记录
```

记录日志常用于记录那些不可预料的异常，可以使用下面两个方法提供日志记录中包含的异常描述内容

```
void throwing(String className, String methodName, Throwable t)
void log(level l, String mwssage, Throwable t)

// 典型用法是

if(...)
{
    IOException exception = new IOException("...");
    logger.throwing("com.mycompany.mylib.Reader", "read", exception);
    throw exception;
}

// 还有
try
{
    ...
}
catch (IOException e)
{
    Logger.getLogger("com.mycompany.myapp").log(level l.WARNING, "Reading image", e);
}
```

调用`throwing`可以记录一条FINER级别的记录和一条以THROW开始的信息

### 7.5.3 修改日志管理器配置


默认情况下配置文件位于：`jar/lib/logging.properties`。要想使用另一个配置文件，需要将`java.util.logging.config.file`特性设置为配置文件的存储位置，并用以下命令启动应用程序

```
java -Djva.util.logging.config.file=configFile MainClass // 在VM启动过程中初始化，即在main执行之前完成

System.setProperty("java.util.logging.config.file", file)
// 在main中调用可以调用LogManager.readConfiguration()来重新初始化日志管理器
```

要想修改默认的日志记录级别，就需要编辑配置文件，并修改一下命令行

```
.level=INFO
// 添加一下内容来制定自己的日志记录级别

com.mycompany.myapp.level=FINE

// 也就是在日志记录器后面添加后缀.level

// 日志记录器并不将信息发送到控制台上，这时处理器的任务
// 要想在控制台上看到FINE级别的消息

java.util.logging.ConsoleHandler.level=FINE
```

###  7.5.4 本地化

+ 本地化的应用程序包含资源包(resource bundle)中的本地特定信息，资源包由各个地区的映射集合组成。
+ 一个程序可以包含多个资源包，一个用于菜单；其他用于日志消息。
+ 每个资源包都有一个名字，要想将映射添加到一个资源包中，需要为每个地区创建一个文件
+ 可以将这些文件与应用程序的类文件放在一起，以便`ResourceBundle`类自动地对它们进行定位
+ 这些文件都是纯文本文件


```
// 在请求日志记录器时，可以指定一个资源包
Logger logger = Logger.getLogger(loggerName, "com.mycompany.logmessage");

// 为日志消息指定资源包的关键字
Logger.info("readingFile");
```

通常需要在本地化的消息中增加一些参数，因此消息应该包含占位符{0}、{1}等。例如在日志消息中包含文件名，就应该用下列方式包括占位符

```
Reading file {0}.
Achtung! Datei {0} wird eingelesen.
```

然后通过调用下面一个方法向占位符传递具体的值

```
logger.log(level.INFO, "readingFIle", fileName);
logger.log(level.INFO, "renamingFile", new Object[] {oldName, newName});
```

### 7.5.5 处理器

+ 默认情况下日志记录器将记录发送到ConsoleHandler中，并由它输出到System.out流中。 
+ 日志记录器还会将记录发送到父处理器中，而最终的处理器(命名为“”)有一个ConsoleHandle
+ 处理器也有日志记录级别

（这个坑太深，挖不动先跳了）


## 7.6 调试技巧

首先，一个方便且功能强大的调试器很重要。调试器是Eclipse、NetBeans(这里推荐IDEA)这类专业集成开发环境的一部分

在启动调试器之间的一些建议(这些方法真是嗯。。。。。)

1. 打印或记录任意变量的值
2. 在每一个类中放置单独的main方法进行单元测试
3. 使用JUnit单元测试框架组织测试用例套件
4. 使用日志代理(logging proxy（跳过)
5. 使用Throwable类提供的printStackTrace方法获得堆栈情况
6. 利用`printStackTrace(PrintWriter s)`将堆栈轨迹发送到文件中或者捕获到一个字符串里
7. 在bash或Windows shell中捕获System.err——`java Myprogram 2> errors.txt`
8. 调用静态方法`Thread.setDefaultUncaughtExceptionHandler`方法改变非捕获异常的处理器
9. 用`-verbose`标志启动Java虚拟机，观察类的加载过程。
10. `-Xlint`告诉编译器对一些普遍容易出现的代码问题进行检查
11. Java虚拟机增加了对Java应用程序进行监控(monitoring)和管理(management)的支持
12. 使用jmap实用工具获得一个堆的转储，其中显示堆中的每个对象
13. 使用`-Xprof`标志运行Java虚拟机，就会运行一个基本的剖析器来跟踪代码中经常被调用的方法