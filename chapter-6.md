# 第六章 接口、lambda类与内部类

## 6.1 接口

### 6.1.1 接口概念

+ 在Java中，接口不是类，而是对类的一组描述，这些类要遵从接口描述的统一格式进行定义
+ 接口的结构与抽象类非常相似，接口本身也具有数据成员和抽象方法，但它与抽象类有下列不同
    1. 接口的数据成员都是静态且必须初始化
    2. 接口中的方法必须全部声明为`abstract`
+ 接口中的所有方法自动地属于`public`，因此不必提供关键字`public`
+ 接口不能含有实例域，在Java SE 8之前也不能在接口中实现方法
+ 为了让类实现一个接口，需要
    1. 使用关键字`implement`将类声明为实现给定的接口
    2. 对接口中的所有方法进行定义
    3. 在实现接口时，必须把所有方法都设置成`public`
    4. 实现抽象方法时必须使用完全相同的方法头

#### 案例分析

`Arrays`类中的`sort`方法承诺可以对对象数组进行排序，但要求满足下列前提： 对象所属的类必须实现了`Comparable`接口

下面是Java SE 5。0中已经改进为泛型类型的`Comparable`接口代码

```
public interface Comparable<T>
{
    int compareTo(T other);
}

int compareTo(Employee other) // 不带类型参数的话就默认调用Object类型参数

```

说明，任何实现`Comparable`接口的类都需要包含`compareTo`方法并且这个方法的参数必须是一个`Object`对象，最终返回一个整形数值

这里我们在`Employee`类中提供一个`compareTo`方法：

```
class Employee implements Comparable<Employee>
{
    public int compareTo(Employee otherObject)
    {
        return Double.compare(salary, other.salary);
    }
}
```

问题：为什么不直接在`Employee`类中提供`compareTo`方法而必须要实现`Comparable`接口？

解答：Java是一种强类型(strongly typed)语言，在调用方法的时候编译器会检查方法是否存在。在`sort`方法中可能存在下列语句：

```
if (a[i].compareTo(a[j]) > 0)
{
    //rearrange a[i] and a[j]
}
```

为此，编译器必须确认a[i]一定有`CompareTo`方法，如果a是一个`Comparable`对象的数组，就可以确保有`compareTo`方法。如果a[i]不属于实现了`Comparable`接口的类，那么虚拟机将抛出异常

### 6.1.2 接口的特性

1. 接口不是类，不能用`new`运算符来实例化一个接口
2. 尽管不能构造接口的对象，却能声明接口的变量
3. 接口变量必须引用实现了接口的对象
4. 可以使用`instanceof`检查一个对象是否实现了某个特定的接口
5. 接口中的域将被自动设置为`public static final`。出于习惯或提高清晰度也可以加上，但不建议这么做

C++注释：

1. C++允许一个类有多个超类，这种特性称为多重继承(multiple inheritance)，而Java不支持多重继承的主要原因是多重继承会让语言本身变得非常复杂，效率也会降低
2. C++的多重继承伴随而来许多复杂特性，因此C++程序员也很少使用多重继承

### 6.1.3 静态方法

在Java SE 8中，允许在接口中增加静态方法，虽然这样有违于将接口作为规范的初衷。到目前为止，通常的做法都是将静态方法放在伴随类中。在标准库中可以看到成对出现的接口和实用工具类，如`Collection/Collections`或`Path/Paths`

```
// 在Java SE 8中为Path接口增加以下方法

public interface Path
{
    public static Path get(String first, String... more)
    return FileSystem.getDefault().getPath(first, more);
}
```

这样就不需要Paths类了。在实现自己的接口是，不在需要为实用工具方法另外提供一个伴随类

### 6.1.4 默认方法

可以为接口方法提供一个默认实现，必须用`default`修饰符标记这样一个方法。在一些情况下默认方法很有用，如希望在发生鼠标点击事件时获得通知，就需要实现一个包含5个方法的接口：

```
public interface MouseListener
{
    default void mouseClicked(MouseEvent event) {};
    default void mousePressed(MouseEvent event) {};
    default void mouseReleased(MouseEvent event) {};
    default void mouseEntered(MouseEvent event) {};
    default void mouseExited(MouseEvent event) {};
}

\* 大多数情况下，只需要关注1或2个事件类型。
 * 可以把所有方法都声明为默认方法，默认方法什么都不做
 * 这样就不再需要相应的伴随类来实现相应方法 
 *\

class test implements MouseListener
{
    void mouseCliked(MouseEvent event)
    {
        System.out.println();
    }
}
```

默认方法的一个重要用法是"接口演化"(interface evolution)。

假设我们提供了一个类

```
public class Bag implement Colletion
```

然后在Java SE 8中增加了一个`stream`方法，如果`stream`方法不是默认方法，那么`Bag`类将无法编译，因为它没有实现这个新方法。也就是说，为接口添加一个非默认方法不能保证"源代码兼容"(source compatible)

假设不重新编译这个类，而是使用原先一个包含这个类的JAR文件，那么这个类能正常加载，尽管没有新方法。但是在一个`Bag`类实例上调用`stream`方法就会报错

使用默认方法可以解决以上的两个问题

### 6.1.5 解决默认方法冲突

如果在接口中将一个方法定义为默认方法，然后又在超类或另一个接口中定义了同样方法。Java将使用一下规则来解决冲突：

1. 超类优先：如果超类提供了一个具体方法，同名并且有相同参数类型的默认方法会被忽略
2. 接口冲突：如果一个超接口提供了一个默认方法，另一个接口提供了一个同名并且有相同参数类型的方法，必须用覆盖这个方法来解决冲突

#### 案例分析

```
interface Named
{
    default String getName() {
        return getClass().getName() + "_" + hashCode();
    }
}

interface Person
{
    String name="Teacher";
    default String getName() {
        return name;
    }
}

class Teacher implements Named, Person
{
    private String name;

    public Teacher(String name)
    {
        this.name = name;
    }

    public String getName() {return Person.super.getName();} // 选择两个冲突方法中的一个
}

// 如果Named接口中没有提供默认方法，Teacher类不会继承Person接口的默认方法，需要自己重新实现
// 如果两个接口都没有提供默认方法，那么类可以实现这个方法，也可以作为抽象类不实现。这与Java SE 8之前情况一样

```

警告： 由于"类优先"的规则，我们不能让一个默认方法重新定义`Object`类中的某个方法

## 6.2 lambda 表达式

+ lambda表达式是一个可传递的代码块，可以在以后执行一次或多次
+ 到目前为止，在Java中传递代码段并不容易。Java是一种面向对象语言，所以必须先构造一个对象，这个对象的类需要有一个方法能包含所需的代码

### 6.2.1 lambda 表达式的语法

一种lambda表达式形式：参数，箭头(->)以及一个表达式

```
Comparator<String> comp = (first, second)
        -> first.length() - second.length(); 
// 可以推导出一个lambda表达式的参数类型就可以忽略其类型
// 无需制定lambda表达式的返回类型，lambda表达式的返回类型总是由上下文推导出来的

(String first, String second) ->
{
    if (first.length() < second.length()) return -1;
    else if (first.length() > second.length()) return 1;
    else return 0;
}

() -> {for (int i = 100; i >= 0; i--) System.out.println(i);}; // 没有参数时仍要提供空括号

ActionListener listener = event ->
    System.out.println("The time is " + new Date()); // 只有一个参数的时候，可以推导出参数类型，甚至可以省略小括号
```

### 6.2.2 函数式接口

在Java中有许多封装代码块的接口，如`ActionListener`或`Comparator`，lambda表达式与这些接口是兼容的。对于只有一个抽象方法的接口，需要这种接口的对象时，就可以提供一个lambda表达式，这种接口称为函数式接口(functional interface)

lambda表达式可以转换为接口。下面来看一个例子

```
Timer t = new Timer(1000, event ->
{
    System.out.println("At the tone, the time is " + new Date());
    Toolkit.getDefaultToolkit().beep();
};
```

在Java中，lambda表达式只能转换为函数式接口

### 6.2.3 方法引用(method reference)

有时候可能已经存在现有的方法来完成你想要传递到其他代码的某个动作

```
Timer t = new Timer(1000, event -> System.out.println(event));
// 等价于
Timer t = new Timer(1000, System::println)

// 不考虑字母大小写而对字符串排序
Arrays.sort(string, String::compareToIgnoreCase)

// 我们用::操作符分割方法名与对象或类名，主要有三种情况

Object::instanceMethod
Class::staticMethod
Class::instanceMethod
```

如果有多个同名的重载方法，编译器会尝试从上下文找出你指的那一个方法。类似于lambda表达式，方法引用不能独立存在，总是会转换成函数式接口的实例

### 6.2.4 构造器引用

+ 构造器引用和方法引用类似，只不过方法名为new。
+ 选择哪个构造器取决于上下文

(涉及到泛型程序设计的问题，暂且跳过)

### 6.2.5 变量作用域

lambda表达式有三个部分：
1. 一个代码块
2. 参数
3. 自由变量的值，这里指非参数并且不在代码中定义的变量

关于代码块以及自由变量有一个术语：闭包(closure)。在Java中，lambda表达式就是闭包

+ lambda表达式可以捕获外围作用域中变量的值。但要确保所捕获的值是明确定义的，这里有一个重要的限制，就是只能引用值不改变的变量。
+ 如果在lambda表达式中改变变量，并发执行多个动作是不安全的。
+ 如果引用的变量可能在外部改变，那么这样的引用也是不合法的
+ lambda表达式中捕获的变量必须是**实际上的最终变量**(effectively final)，实际上的最终变量是值，这个变量初始化之后就不会再为它赋新值。
+ lambda表达式的外部和内部有相同的作用域时，同样使用命名冲突和遮蔽的相关规则，因此lambda表达式中声明与一个局部变量同名的参数或局部变量是不合法的。
+ 在方法中不能有两个同名的局部变量，同样在lambda表达式中也不能有同名的局部变量
+ 在lambda表达式中使用`this`关键字，是指创建这个lambda表达式的方法的this参数，如下例

```
public class Appication
{
    public void init()
    {
        ActionListener listener = event ->
        {
            System.out.println(this.toString());
            ...
        }
        ...
    }
}
```

在lambda表达式中，`this`的使用没有任何特殊之处，无论是在lambda表达式中，还是在`init()`方法的其他位置，含义都没有改变，始终调用`Application`对象的`toString`方法

### 6.2.6 处理lambda 表达式

使用lambda表达式的重点是延迟执行(deferred execution)，希望延迟执行代码有很多原因：
+ 在一个单独的线程中运行代码；
+ 多次运行代码；
+ 在算法的恰当位置运行代码；
+ 发生某种情况时运行代码；
+ 只在必要的时候才运行代码；

+ 大多数标准函数式接口都提供了非抽象方法来生成或合并函数。例如`Predicate.isEqual(a)`等同于`a::equals`,不过a为`null`时也能工作
+ 已经提供了默认的方法`and、or和negate`来合并谓词，如`Predicate.isEqual(a)or(Predicate.isEquals(b))`等同于`x -> a.euqals(x) || b.equals(x)`
+ 如果设计你自己的接口，其中只有一个抽象方法，可以用`@FunctionInterface`注解来标记这个接口，这样做有两个优点：
    1. 如果你无意增加了另一个非抽象方法，编译器会产生一个错误讯息
    2. 另外javadoc页里会指出你的接口是一个函数式接口
+ 并不是必须使用注解。根据定义，任何只有一个抽象方法的接口都是函数式接口

## 6.3 内部类

内部类(inner class)是定义在另一个类中类。

使用内部类主要原因有以下三点：
1. 内部类方法可以访问该类定义所在的作用域中的数据，包括私有的数据
2. 内部类可以对同一个包的其他类隐藏起来
3. 当想要定义一个回调函数且不想编写大量代码时，使用匿名(anonymous)内部类比便捷

C++注释：

+ C++有嵌套类。一个嵌套的类包含在外围类的作用域内。
+ 嵌套类有两个好处：命名控制和访问控制
    + 命名控制避免了与其他同名的类发生冲突，但这个问题在Java中并不重要，因为Java已经提供了有效的命名控制
    + 访问控制：嵌套类只会对其他代码都是不可见的，即使设计成公有的，它的数据域也只能被被嵌套类所控制而不会暴露给其他代码。在Java中只有内部类能够实现这样的控制
+ Java内部类有一个隐式引用，可以引用实例化后该内部对象的外围类对象。通过这个指针可以访问外围类对象的全部装的
+ Java中`static`内部类没有这种附加指针，这样的内部类与C++的嵌套类很相似

### 6.3.1 使用内部类访问对象状态

+ 内部类既可以访问自身的数据域，还可以访问创建它的外围类对象的数据域。
+ 内部类的对象总有一个隐式引用指向了创建它的外部类对象，这个引用称为`outer`
+ 注意`outer`并不是关键字，这里仅仅用来说明内部类的机制

```
package innerClass;

import java.awt.*;
import java.util.*;
import java.awt.event.*;
import javax.swing.*;
import javax.swing.Timer;

public class innerClassTest {
    public static void main(String[] args)
    {
        TalkingClock clock = new TalkingClock(1000, true);
        clock.start();

        JOptionPane.showConfirmDialog(null, "Quit program?");
        System.exit(0);
    }
}

class TalkingClock
{
    private int interval;
    private boolean beep;

    public TalkingClock(int interval, boolean beep)
    {
        this.interval = interval;
        this.beep = beep;
    }

    public void start()
    {
        ActionListener listener = new TimePrinter();
        Timer t = new Timer(interval, listener);
        t.start();
    }

    class TimePrinter implements ActionListener // 内部类
    {
        public void actionPerformed(ActionEvent event)
        {
            System.out.println("At the tone, the time is " + new Date());
            if (beep) Toolkit.getDefaultToolkit().beep();
        }
    }
}
```
### 6.3.2 内部类的特殊语法规则

使用外围类引用的正规语法：`OuterClass.this`

```
public void actionPerformed(ActionEvent event)
{
    System.out.println("At the tone, the time is " + new Date());
    if (TalkingClock.this.beep) Toolkit.getDefaultToolkit().beep();
}
```

反过来，可以采用下列语法更加明确地编写内部对象的构造器

```
outerObject.new InnerClass(construction parameters)

// 例子

ActionListener listener = this.new TimePrinter();

//通常this限定词是多余的，不过可以通过显式地命名将外围类引用设置为其他的对象

TalkingClock jabberer = new TalkingClock(1000, true);
TalkingClock.TimePrinter listener = jabberer.new TimePrinter();

// 注意，在外围类的作用域之外，可以这样引用内部类： OuterClass.InnerClass
```

+ 内部类中声明的所有静态域都必须是`final`，因为我们希望一个静态域只有一个实例，不过对于每个外部对象，会分别有一个单独的内部类实例。如果这个域不是`final`的话就不唯一了
+ 内部类不能有`static`方法。

### 6.3.3 局部内部类

如果一个类只在相应方法中创建这个类型的对象时使用一次，可以在这个方法中定义局部类

```
public void start()
{
    class TimePrinter implements ActionListener
    {
        public void actionPerformed(ActionEvent event)
        {
            System.out.println("At the tone, the time is" + new Date());
            if (TalkingClock.this.beep) Toolkit.getDefaultToolkit().beep();
        }
    }

    ActionListener listener = new TimePrinter();
    Timer t = new Timer(interval, listener);
    t.start();
}
```

+ 局部类不能用`public`或`private`访问说明符来进行声明，它的作用域被限定在这个局部类的块中
+ 局部类的优势在于对外部世界完全地隐藏起来，即使`TalkingClock`类的其他代码也无法访问它。除了`start`方法没有任何方法能够访问它

### 6.3.4 由外部方法访问变量

+ 与其他内部类相比，局部类还有一个优点。它们不仅能够访问包含它们的外部类，还能访问局部变量，不过哪些局部变量必须事实上为`final`
+ 当`final`限制造成不方便的时候，我们可以把局部变量设置成一个长度为1的数组，可以相当于调用了一个可变对象

### 6.3.5 匿名内部类

如果只创建这个类的一个对象，就不必命名了。这样的类被称为匿名内部类(anonymous inner class)

```
public void start()
{
    ActionListener listener = new ActionListener() {
        public void actionPerformed(ActionEvent event) {
            System.out.println("At the tone， the time is " + new Date());
            Toolkit.getDefaultToolkit().beep();
        }
    };
    Timer t = new Timer(interval, listener);
    t.start();
}

// 通常的语法格式为：

new SuperType(construction parameters)
{
    inner.class methods and data
}

// 当然现在使用内部类的方法不如lambda表达式来的简洁

public void start()
{
    Timer t = new Timer(interval, event -> 
    {
        System.out.println("At the tone, the time is " + new Date);
        if (beep) Toolkit.getDefaultToolkit().deep();
    });
    t.start();
}
```

### 6.3.6 静态内部类

有时候使用内部类只是为了把一个类隐藏在另外一个类的内部，并不需要内部类引用外围类对象。为此可以将内部类声明为`static`，以便取消产生的引用

```
public class StaticInnerClassTest {
    public static void main(String[] args)
    {
        double[] d = new double[20];
        for (int i = 0; i < d.length; i++)
        {
            d[i] = 100 * Math.random();
        }

        ArrayAlg.Pair p = ArrayAlg.minmax(d);
        System.out.println("min = " + p.getFirst());
        System.out.println("max = " + p.getSecond());

    }
}

class ArrayAlg
{
    public static class Pair
    {
        private double first;
        private double second;

        public Pair(double first, double second)
        {
            this.first = first;
            this.second = second;
        }

        public double getFirst() {
            return first;
        }

        public double getSecond() {
            return second;
        }

    }

    public static Pair minmax(double[] value)
    {
        double min = Double.POSITIVE_INFINITY;
        double max = Double.NEGATIVE_INFINITY;

        for (double e: value)
        {
            if (min > e) min = e;
            if (max < e) max = e;
        }

        return new Pair(min, max);

    }
}
```