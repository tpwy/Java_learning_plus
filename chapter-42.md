# 第四章 对象和类(Core Java)

由于《Java程序设计基础 第五版》和《Java核心技术 卷I》在这一部分内容相差较大，这里以课本为基础篇，《Core Java》作为补充扩展。

## 4.1 面向对象程序设计概述

### 4.1.1 类

+ 封装(encapsulation,也称数据隐藏)。从形式上看，封装是将数据和行为组合在一个包内，并对对象的使用者隐藏了数据的实现方式
+ 实现封装的关键在于绝对不能让类中的方法直接地访问其他类的实例域
+ 在Java中，所有的类都源于`Object`类，用于自定义Java类的时候可以通过扩展一个类来建立另外一个新的类(继承)

### 4.1.2 对象

对象的三个主要特性：
+ 对象的行为(behavior)——可以对对象施加哪些操作和方法？
+ 对象的状态(state)——当施加那些方法时，对象如何响应？
+ 对象表示(indentity)——如何辨别具有相同行为和状态的不同对象？

详细说明：
+ 同一个类的所有对象实例，由于支持相同的行为而具有家族式的相似性。对象的行为是用可调用的方法来定义的。
+ 对象的状态指每个对象保存着的描述当前特性的信息。对象的状态不会是自发改变的，必须通过调用方法来实现。
+ 如果不经过方法调用就可以改变对象状态，只能说明封装性遭到了破坏。
+ 每个对象都有一个唯一的身份(identity)来描述。作为一个类的实例，每个对象的标识永远是不同的，状态也常常存在差异。

### 4.1.3 类之间的关系

最常见的关系有：
+ 依赖("uses-a")——dependence，如果一个类的方法操纵另一个类的对象，我们就说一个类依赖于另一个类。我们应该尽可以地将相互依赖的类减至最少，让类之间的耦合度最小
+ 具有（"has-a")——aggregation，意味着类A的对象包含类B的对象
+ 继承("is-a")——inheritance，下一章详解

扩展：UML(Unified Modeling Language, 统一建模语言)用于绘制类图，描述类之间的关系

## 4.2 使用预定义类

### 4.2.1 对象与对象变量

以`Date`类作为例子

+ 在Java语言中，使用构造器(construtor)构造新实例。构造器是一种特殊的方法，用来构造并初始化对象。
+ 构造器的名字应该与类名相同，要想构造一个`Date`对象，需要在构造器前面加上`new`操作符。

```
new Date(); //构造了一个新对象并将它初始化为当天的日期和时间
System.out.println(new Date()); //将这个对象传递给一个方法
String s = new Date().toString(); // 将String()方法应用在Date对象上
Date birthday = new  Date(); // 引用新构造的对象变量birthday

Date deadline; //这里只声明了对象变量deadline，但该变量不是一个对象，实际上也没有引用对象，不能直接将任何Date方法应用在这个变量上

// 初始化变量deadline有两个选择

deadline = new Date(); // 用新构造的对象初始化
deadline = birthday; // 让这个变量引用同一个对象
```

+ 一个对象变量并没有实际包含一个对象，而仅仅引用了一个对象
+ 在Java中，任何对象变量的值都是对存储在另外一个地方的一个对象的引用，`new`操作符的返回值也是一个引用
+ 可以显式地将对象设置为`null`，表明这个变量目前没有引用对象
+ 局部变量不会自动初始化为`null`，必须通过调用`new`或将它们设置成`null`进行初始化

与C++的区别：

1. Java的对象变量类似于C++的对象指针
2. 所有的Java对象都存储在堆中，当一个对象包含另一个对象变量时，这个变量仍然是包含着指向另一个堆对象的指针
3. C++可以通过拷贝型构造器和复制操作符来实现对象的自动拷贝。在Java中必须使用`clone`方法获得对象的完整拷贝

### 4.2.2 Java类库中的`LocalDate`类

+ `Date`类用来表示特定的时间点，而`LocalDate`类用来表示大家熟悉的日历表示法
+ 将时间和日历分开是一种很好的面向对象设计。通常最好使用不同的类来表示不同的概念
+ 不要使用构造器来构造`LocalDate`类的对象，应当使用静态工厂方法(factory method)代表你调用构造器

```
LocalDate().now(); // 构造这个对象的时期
LocalDate().of(1999, 12, 31); //构造一个特定日期的对象
LocalDate newYearEve = LocalDate.of(1999, 12, 31); // 将构造的对象保存在一个对象变量中

int year = newYearEve.getYear(); // 1999 调用对象的方法

LocalDate aThousandDaysLater= newYearEve.plusDays(1000); //创建了一个新的对象并引用至变量aThousandDaysLater
```

### 4.2.3 更改器方法与访问器方法

+ 像`plusDays()`方法没有更改调用这个方法的对象,**只访问对象而不修改对象的方法**叫访问器方法(accessor method)
+ 像`GregorianCalendar.add`方法，调用后会改变调用这个方法的对象的状态，称为更改器方法(mutator method)


与C++的区别

1. 在C++中，带有`const`后缀的方法是访问器方法，默认为更改器方法。
2. 在Java中，访问器方法和更改器方法在语法上没有明显区别

应用`LocalDate`类实现本月日历

```
import java.time.*;
public class CalenderTest {
    public static void main(String[] arg)
    {
        LocalDate thisdate= LocalDate.now();
        int thisYear = thisdate.getYear();
        int thisMonth = thisdate.getMonthValue();
        int thisDay = thisdate.getDayOfMonth();

        LocalDate firstdate = LocalDate.now().minusDays(thisDay - 1);
        int firstWeekday = firstdate.getDayOfWeek().getValue();

        LocalDate date = firstdate;

        System.out.println(thisYear + "-" + thisMonth);
        System.out.println("Mon\tTue\tWed\tThu\tFri\tSat\tSun");
        for (int i=1; i < firstWeekday; i++)
            System.out.print("    ");
        while (date.getMonthValue() == thisMonth)
        {
            System.out.printf("%3d", date.getDayOfMonth());
            if (date.getDayOfMonth() == thisDay)
                System.out.print("*");
            else
                System.out.print(" ");
            date = date.plusDays(1);
            if (date.getDayOfWeek().getValue() == 1)
                System.out.println();
        }
        if (date.getDayOfWeek().getValue() != 1)
            System.out.println();
    }
}
```

## 4.3 用户自定义类

设计复杂的应用程序需要各种主力类(workhorse class),通常这些类没有`main`方法，却有自己的实例域和实例方法。要想创建一个完整的程序，应该将若干类组合在一起，其中只有一个类有`main()`方法

### 4.3.1 `Employee`类

在Java中，最简单的类定义形式为：

```
class className
{
    field1;
    field2;
    ...
    constructor1
    constructor2
    ...
    method1
    method2
    ...
}
```

举个非常简单的`Employee`类

```
import java.time.*;

class Employee
{
    private String name;
    private double salary;
    private LocalDate hireDay;

    public Employee(String name, double salary, int year, int month, int day)
    {
        this.name = name;
        this.salary = salary;
        hireDay = LocalDate.of(year, month, day);
    }

    public String getName()
    {
        return this.name;
    }

    public double getSalary()
    {
        return this.salary;
    }

    public LocalDate getHireDay()
    {
        return hireDay;
    }

    public void raiseSalary(double byPercent)
    {
        double raise = salary * byPercent / 100;
        salary += raise;
    }

}
```
+ 构造器和其他方法最重要的不同在于，构造器总是伴随`new`操作符被调用，而不能对一个已经存在的对象调用构造器来达到重新设置实例域的目的
+ 为了保证封装性，强烈建议实例域标记为`private`
+ 不要在构造器中定义与实例域重名的局部变量。这些变量只能在构造器内部访问，并且会屏蔽实例域

与C++的区别：

Java的构造器的工作方式和C++一样，但要记住Java对象都是在堆中构造的，构造器总是伴随`new`操作符一起使用。
```
Employee number007("james Bond", 1000000, 1970, 1, 1); // C++可以运行，Java则不行
```



### 4.3.2 多个源文件的使用

在上述例子中，一个源文件包括了两个类。许多程序员习惯于将每一个类存在一个单独的源文件。如果喜欢这样组织文件的话，可以有两种编译源程序的方法

1. 使用通配符调用编译器：`javac Employee*.java`
2. 直接编译带有`main()`的源文件：`javac EmployeeTest.java``

对于第二种方法，并没有显式的编译`Employee.java`，但是当java编译器发现`EmployeeTest.java`使用了`Employee`类的时候会查找名为`Employee.class`的文件。如果没有找到，则会自动搜索`Employee.java`然后对它进行编译。更重要的是，如果`Employee.java`版本比已有的`Employee.class`文件版本新的话，Java编译器将自动地重新编译这个文件

注释：
+ 如果熟悉UNIX的"make"工具，可以认为Java编译器内置了"make"功能。


### 4.3.3 隐式参数与显式参数

+ 隐式(implicit)参数：出现在方法名前的`Employee`类对象
+ 显式(explicit)参数：位于方法名后面括号中的数值，明显地列在方法声明中
+ 我们用`this`关键字表示隐式参数，这样可以把实例域和局部变量区别开

与C++的区别：

C++定义方法的格式
```
//在C++中，通常在类的外面定义方法：
void Employee::raiseSalary(double byPercent) // C++
{
    ...
}

//如果在类的内部定义方法，这个方法自动地变成内联(inline)方法
class Employee
{
    ...
    int getName() {return name;} // inline in C++
}
```

而在Java中，所有的方法必须定义在类的内部，但并不表示它们是内联方法。是否将某个方法设置为内联方法是Java虚拟机的任务

### 4.3.4 封装的优点

+ 例子中的`getName()`等方法都是典型的访问器方法，由于它们只返回实例域值，也称为域访问器
+ 如果实例域是`public`，那么破坏该域值的地方可能出没在任何地方，而当限制只能用域更改器修改，只需要调试这个方法即可

为了获得或设置实例域的值，需要提供一下三项内容：
1. 一个私有的数据域
2. 一个公有的域访问器方法
3. 一个共有的域更改器方法

注意：
+ 不要编写可以返回引用可变对象的访问器方法，这样违反了设计原则。在例子中`Date`对象是可变的，可以通过另一个变量来引用并修改这个对象的状态，从而破坏了封装性
+ 如果需要返回一个可变对象的引用，首先对它进行克隆(clone)

### 4.3.5 基于类的访问权限

+ 方法可以访问所调用对象的私有数据，奇怪的是一个方法可以访问所属类的所有对象的私有数据。
+ C++也有同样的原则，方法可以访问所属类的私有特性，而不仅限于访问隐式参数的私有特性

### 4.3.6 私有方法

+ 私有方法常用于不应该称为公有接口的一部分的辅助方法
+ 由于私有方法不会被外部的其他类操作调用，可以轻易删去，而公有访问则需要注意其他代码对它的依赖性

### 4.3.7 `final`实例域

+ 可以将实例域定义为`final`，构建对象时必须初始化这样的域。
+ 必须确保在每一个构造器执行之后，这个域的值被设置，并且在后面的操作中怒能再对它进行修改
+ `final`修饰符大多应用于基本(primitive)类型域或不可变`immutable`类的域
+ 用在可变的类时候，只能表示引用变量不会再指示其他对象，但对象本身可以更改

## 4.4 静态域和静态方法中

### 4.4.1 静态域

如果将域设置为`static`，每个类中只有一个这样的域。而每一个对象对于所有的实例却都有自己的一份拷贝。

```
// 为员工设置id
class Employee
{
    private static int nextId = 1;
    private int id;
    ...
    public void setId()
    {
        id = nextId;
        nextId++;
    }
}
```

### 4.4.2 静态常量

静态变量使用得比较少，但静态常量使用则比较多

```
public class Math
{
    ...
    public static final double PI = 3.14125; //可以通过Math.PI获得这个变量
    ...
}
```

### 4.4.3 静态方法

+ 静态方法是一种不能向对象实施操作的方法。
+ 在运算过程中，不使用任何对象，即没有隐式的参数
+ 静态方法是没有`this`参数的方法
+ 建议使用类名来调用静态方法，不推荐使用对象来调用

C++注释：

1. Java中的静态域和静态方法在功能上和C++相同，但语法书写上有所不同
2. `static`关键字可解释为：属于类且不属于类对象的变量和函数

### 4.4.4 工厂方法

静态方法中还有另一种常见用途。即使用静态工厂方法(factory method)来构造对象。

不利用构造器完成的原因：
1. 无法命名构造器，构造器的名字必须与类名相同
2. 当使用构造器的尚在，无法改变所构造的对象类型

```
import java.text.NumberFormat;
public class NumberForamtTest {
    public static void main(String[] args)
    {
        NumberFormat currencyFormatter = NumberFormat.getCurrencyInstance();
        NumberFormat percentFormatter = NumberFormat.getPercentInstance();
        double x = 0.1;

        System.out.println(currencyFormatter.format(x)); // print $0.10
        System.out.println(percentFormatter.format(x)); // print 10%
    }
}
```

### 4.4.5 `main`方法

`main`方式是一个静态方法，不对任何对象进行操作。事实上当启动程序时还没有任何一个对象，静态的`main`方法将执行并创建程序所需要的对象

提示：每一个类都可以有一个`main`方法，这是常用于类进行单元测试的技巧。当该类是更大型应用程序中的一部分的时候，`main`方法将不会被执行。

## 4.5 方法参数

+ 按值调用(call by value)表示方法接收的是调用者提供的数值
+ 按引用调用(call by reference)表示方法接收的是调用者提供的变量地址
+ Java语言总是采用按值调用。也就是说，方法得到的是所有参数值的一个拷贝，方法不能修改传递给它的任何参数变量的内容

```
public static void tripleValue(double x) // doesn't work
{
    x = 3 * x;
}

double percent = 10;
tripleValue(percent);

//具体的执行步骤：
//x被初始化为percent值的一个拷贝
//x处理为30，但percent仍然是10
//方法结束后参数变量x被销毁
```

方法参数共有两个类型：
1. 基本数据类型——一个方法不可能就该一个基本数据类型的参数
2. 对象引用——有效，因为这里的拷贝同样是被一个对象的引用

### 误解
+ 许多程序设计语言提供了两种参数传递的方式：值调用和引用调用
+ 认为Java程序设计语言对对象采用的是引用调用。这种理解是不对的

(don't bb, show me the code)

```
// 编写一个交换两个雇员对象的方法：
public static void swap(String[] args) //doesn't work
{
    Employee temp = x;
    x = y;
    y = temp;
}

Employee a = new Employee("Alice", ...);
Employee b = new Employee("Bob", ...);
swap(a, b);
// 如果Java对对象采用按引用调用，那么这个方法应该能够实现交换数据的效果
// 实际上方法并没有改变存储在变量a和b中的对象的引用
```

### 总结

Java中方法参数的使用情况
1. 一个方法不能修改一个基本数据类型的参数
2. 一个方法可以改变一个对象参数的状态
3. 一个方法不能让对象变量引用一个新的参数

C++注释：

C++有值调用和引用调用。引用变量标有&符号

## 4.6 对象构造

### 4.6.1 重载

+ 如果多个方法有相同的名字、不同的参数，便产生重载
+ Java允许重载任何方法，而不只是构造器方法
+ 要完整的描述一个方法，需要指出文件名以及参数类型。即方法的签名(signature)

### 4.6.2 默认域初始化

建议明确的对域进行初始化而不是用默认域初始化

### 4.6.3 无参数构造器

如果在编写一个类时没有编写构造器，那么系统就会提供一个无参数构造器，将所有的实例域设置为默认值

### 4.6.4 显式域初始化

+ 推荐的设计习惯：不管怎样调用构造器，每个实例域都能返回一个有意义的初值
+ 在执行构造器之前先执行赋值操作，初始值不一定是常量值

C++注释：

在C++中，不能直接初始化类的实例域。所有的域必须在构造器中设置。

### 4.6.5 参数名

+ 在编写很小的构造器时，常常在参数命名上出现错误。
+ 通常，参数用单个字符命名。但这样有一个缺陷：只有阅读代码才了解参数的含义
+ 另一种做法：在参数前面加上一个前缀"a"
+ 推荐做法，直接命名，在使用`this`指示隐式参数

C++注释：

在C++中，长使用下划线或某个固定字母作为前缀，Java程序员通常不这样做

### 4.6.6 调用另一个构造器

```
public Employee(double s)
{
    //calls Employee(String, double)
    this("Employee #" + nextId, s); // 这样编写可以节省代码
    nextId++;
}
```

C++注释：

在Java中，`this`的引用等价于C++的`this`指针。但是C++中一个构造器不能调用另一个构造器，必须抽取去公共初始化代码编写成一个独立的方法

### 4.6.7 初始化块

不常用也不推荐使用(其实就是懒得看)

### 4.6.8 对象析构与`finalize`方法

+ 由于Java有自动的垃圾回收器，不需要人工回收内存，所有Java不支持析构器
+ `finalize`方法将在垃圾回收器清除对象之前调用(也不推荐使用)

## 4.7 包

+ Java允许使用包(package)将类组织起来
+ 标准的Java类库分布在多个包中，标准的Java包具有一个层次结构。
+ 使用包的主要原因是为了确保类名的唯一性

### 4.7.1 类的导入

1. 可以在每个类名之前添加完整的包名(不推荐)
2. 使用`import`语句导入特定的类或整个包。
3. `import`语句应位于源文件的顶部(但位于`package`语句的后面)

注意，当发生命名冲突的时候，就不得不注意包的名字

```
import java.util.*;
import java.sql.*;

//在程序使用Date类的时候就会出现编译错误

//解决方法

import java.util.Date; //添加一个特定的import语句

如果两个Date类都需要使用，那么就加上前缀包名
```

C++注释：

+ C++程序员经常把`import`和`#include`弄混，其实这两者并没有共同之处。在C++中必须使用`#include`将外部特性的声明加载进来
+ Java通过显式地给出包名可以不使用`import`语句，而C++无法避免使用`#include`指令
+ `import`的唯一好处是便捷，在C++中与包机制类似的是命名空间。`import`语句类似于C++中的`namespace`和`using`指令

### 4.7.2 静态导入

`import`语句不仅可以导入类，还增加导入静态方法和静态域的功能

### 4.7.3 将类放入包中

要想把一个类放在包里，就必须把包的名字放在源文件的开头，包中定义类的代码之前。

eg

```
package com.horstmann.corejava;

public class Employee
{
    ...
}
```

+ 如果在源文件中放置`package`语句，这个源文件的类就被放在一个默认包(default package)中，默认包是一个没有名字的包。
+ 将包中的文件放在与完整的包名想匹配的子目录中，编译器也将类文件放在相同的目录结构中
+ 编译器在编译源文件的时候不检查目录结构，如果一个源文件没有按`package`语句说明的放置，并且不依赖于其他包，就不会发生编译错误。但最终程序无法运行，因为包与目录不匹配，虚拟机找不到类

## 4.8 类路径

+ 前面提到，类存储在文件系统的子目录中，类的路径必须与包名匹配
+ 另外，类文件也可以存储在JAR(Java归档)文件中。这样既可以节省又可以改善性能
+ JAR文件使用ZIP格式组织文件和子目录

为了使类能够被多个程序共享，需要做到：
1. 把类放在一个目录下
2. 将JAR文件放在一个目录下
3. 设置类路径(class path)
    + UNIX环境下类路径中的不同项目采用冒号(:)分隔
    + Windows环境下则以分号(;)分隔

### 4.8.1 设置类路径

最好采用`-classpath`(或-cp)选项制定类路径

```
java -classpath /home/usr/classdir:.:/home/usr/archives/archive.jar MyProg

java -classpath c:\classdir;.;c:\archives\archive.jar MyProg
```

也可以通过设置`CLASSPATH`环境变量完成这个操作

```
export CLASSPATH=/home/usr/classdir:.:/home/usr/archives/archive.jar

set CLASSPATH=c:\classdir;.;c:\archives\archive.jar
```

## 4.9 文档注释

仔细翻课本，再另外梳理

## 4.10 类设计技巧

1. 一定要保证数据私有
2. 一定要对数据初始化
3. 不要在类中使用过多的基本类型
4. 不是所有的域都需要独立的域访问器和域更改器
5. 将职责过多的类进行分解
6. 类名和方法名要能够体现它们的职责
7. 优先使用不可变的类