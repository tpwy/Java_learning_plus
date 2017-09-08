# 第四章 对象和类(课本)

由于《Java程序设计基础 第五版》和《Java核心技术 卷I》在这一部分内容相差较大，这里以课本为基础篇，《Core Java》作为补充扩展。

## 4.1 类的基本概念

1. 类的概念是为了让程序程序设计语言能更清楚的描述日常生活中的事物
2. 类是对某一类事物的描述，是抽象的、概念上的定义
3. 对象则是实际存在的属该类事物的具体的个体，也称为实例(instance)
4. 面向对象程序设计(OOP)思想的重点是类的设计而不是对象的设计

一般来说，类是由数据成员与函数成员封装而成的，其中数据成员表示类的属性，函数成员表示类的行为
+ Java语言将类内的数据成员称为field(域)[域变量、属性、成员变量]，将封装于类内的函数称为method(方法)[成员方法]

## 4.2 定义类

用户定义一个类实际上就是定义一个新的数据类型。在使用类之前必须先定义才能利用所定义的类来声明相应的变量并创建对象。

### 4.2.1 类的一般结构

```
[类修饰符] class 类名称 //修饰符是可选项
{
    [修饰符] 数据类型 成员变量名称; //声明成员变量
    ...
    [修饰符] 返回值的数据类型 方法名(参数1, 参数2,...) //声明成员方法
    {
        语句序列
        return [表达式];
    }
    ...
}
```
####  四种类修饰符的含义

|修饰符|含义|
|-|-|
| public | 当一个类声明为公共类，它可以被任何对象访问 |
| abstract | 当一个类声明为抽象类，没有实现方法，需要子类提供方法的实现，所有不能创建该类的实例 |
| final | 当一个类声明为最终类(非继承类)，表示它不能被其他类所继承 |
| 缺省 | 表示只有在相同包中的对象才能使用这个类 |

一个类可以有多个修饰符，且无前后顺序之分，但abstract和final相互对立，不能一起应用在一个类的定义上

### 4.2.2 成员变量

#### 4.2.2.1 成员变量修饰符的含义

|修饰符|含义|
|-|-|
| public | 公共访问控制符，指定该变量为公共的，可以被任何对象的方法访问 |
| private | 私人访问控制符，指定该变量只允许自己类的方法访问，其他任何类(包括子类)中的方法均不能访问该变量
| protected | 保护访问控制符，该变量只可以在被它自己的类及其子类或同一个包中的其他类访问，在子类可以覆盖这个变量
| 缺省 | 缺省访问控制符，同一个包中的其他类可以访问此成员变量，而其他包中的类不能访问该成员变量 |
| final | 最终修饰符，指定此变量的值不能改变 |
| static | 静态修饰符，指定此变量被所有对象共享，即所有的实例都可以使用该变量 |
| transient | 过度修饰符，指定该变量是一个系统保留，暂无特别作用的临时性变量 |
| volatile | 易失修饰符，指定该变量可以同时被几个线程控制和修改 |

除了访问控制符有多个之外，其他的修饰符都只有一个。一个成员变量可以被多个修饰符同时修饰，但有些修饰符不能同时定义在一起。

#### 4.2.2.2 成员变量与局部变量的区别

类和方法中均可以定义属于自己的变量，类中定义的变量是成员变量，方法中定义的变量是局部变量

1. 语法形式上看，成员变量属于类，局部变量是在方法中定义的变量或是方法的参数。成员变量可以被public、private、static等修饰符所修饰，而局部变量则不能被访问控制修饰符及static修饰。两者都可以被final修饰
2. 从变量在内存中的存储方式上看，成员变量是对象的一部分，对象时存在与堆内存的，而局部变量是存在于栈内存的
3. 从变量在内存中的生存时间来看，成员变量随对象的创建而存在。局部变量随方法的调用而产生，随着方法调用的结束而自动消失
4. 成员变量如果没有被赋初值，则自动以类型的默认值赋值(但被final修饰但没有被static修饰的成员变量必须显式地赋值)；而局部变量则不会自动赋值，必须显式地赋值后才能被使用

### 4.2.3 成员方法

|修饰符|含义|
|-|-|
| public | 同上 |
| private | 同上 |
| protect | 同上 |
| 缺省 | 同上 |
| final | 指定该方法不能被重载 |
| static | 指定不需要实例化一个对象就可以调用的方法 |
| abstract | 指定该方法之生命方法头，而没有方法体，方法体需在子类中被实现 |
| synchronized | 同步修饰符，在多线程程序中，该修饰符用在运行前，对它所属的方法加锁以防止被其他线程访问，运行结束后解锁 |
| native | 本地修饰符，指定此方法的方法体是用其他语言(如C)在程序外部编写的 |

成员方法同样可以有多个控制修饰符，多个修饰符时候要注意是否互斥

## 4.3 对象的创建和使用

一个对象的生命周期是： 创建——使用——销毁

### 4.3.1 创建对象

两个步骤
1. 声明指向"由类所创建的对象"的引用变量
2. 利用`new`运算符创建新的对象，并指派给前面所创建的变量

```
Cylinder volu;
volu = new Cylinder();

Cylinder volu = new Cylinder(); //简写
```

成员变量的初始值

| 成员变量类型 | 初始值 |
|-|-|
| byte、short、int | 0 |
| long、float、double | 0L、0F、0D |
| char | '\u0000'(表示为空) |
| boolean | false |
| 所有引用类型 | null |

### 4.3.2 对象的使用

```
class Cylinder {

    double radius;
    double height;
    final double PI = 3.14;

    double area() {
        return PI * radius * radius;
    }

    double volume() {
        return this.area() * height; 
        //类定义内可以直接调用自己的方法,使用this关键字来强调是对象本身的成员
    }
}
public class Example4_3_2 {
    public static void main(String[] args) {
        Cylinder volu;
        volu = new Cylinder();
        volu.height = 5;
        volu.radius = 2.8;
        System.out.println("半径为 " + volu.radius);
        System.out.println("高为 " + volu.height);
        System.out.println("底面积为" + volu.area());
        System.out.println("体积为 " + volu.volume());
    }
}
```

## 4.4 参数的传递

### 4.4.1 以变量为参数调用方法

```
class Cylinder {

    double radius;
    double height;
    final double PI = 3.14;
    void setCylinder(double r, double h) {
        this.radius = r;
        this.height = h;
    }
    double area() {
        return PI * radius * radius;
    }

    double volume() {
        return this.area() * height; //类定义内可以直接调用自己的方法,使用this关键字来强调是对象本身的成员
    }
}
public class Example4_3_2 {
    public static void main(String[] args) {
        Cylinder volu;
        volu = new Cylinder();
        volu.setCylinder(2.8, 5); //接收传递的参数
        System.out.println("半径为 " + volu.radius);
        System.out.println("高为 " + volu.height);
        System.out.println("底面积为" + volu.area());
        System.out.println("体积为 " + volu.volume());
    }
}
```

注意：`setCylinder()`方法中的`r、h`都是局部变量，一旦离开该方法就失去作用

说明：当方法的形式参数和类的成员变量同名的时候，需要使用`this`关键字来表示成员变量

### 4.4.2 以数组作为参数或返回值的方法调用

传递参数

```
class LeastNumb
{
    public void least(int[] array)
    {
        int tmp = array[0];
        for (int i=1; i<array.length; i++)
        {
            if (tmp > array[i])
                tmp = array[i];
        }
        System.out.println("最小值为 " + tmp);
    }
}

public class Example4_4_2 {
    public static void  main(String[] args)
    {
        int[] a = {8, 3, 7, 88, 9, 23};
        LeastNumb minNumber = new LeastNumb();
        minNumber.least(a);
    }
}
```

返回值为数组类型的方法

```
class trans
{
    int tmp;
    int[][] transponse(int[][] array)
    {
        for (int i = 0; i < array.length; i++)
        {
            for (int j = i + 1; j < array[i].length; j++)
            {
                tmp = array[i][j];
                array[i][j] = array[j][i];
                array[j][i] = tmp;
            }
        }
        return array;
    }
}

public class Example4_4_3 {
    public static void main(String[] args)
    {
        int[][] a = {{1, 2, 3}, {4, 5, 6}, {7, 8, 9}};
        trans pose = new trans();
        int[][] b = pose.transponse(a);
        for(int i = 0; i < b.length; i++)
        {
            for (int j = 0; j < b[i].length; j++)
            {
                System.out.print(b[i][j] + "\t");
            }
        System.out.print("\n");
        }
    }
}
```

## 4.5 匿名对象

当一个对象被创建之后，在调用该对象的方法时，也可以不定义对象的引用变量，而直接调用这个对象的方法。这样的对象称为匿名对象

```
Cylinder volu = new Cylinder();
volu.setCylinder(2.5, 5);

new Cylinder().setCylinder(2.5, 5); //简写
```

使用情况:
1. 对一个对象只需要进行一次方法调用
2. 匿名对象作为实参传递给一个方法调用

## 4.6 类的私有成员和公共成员

### 4.6.1 私有成员

在类的成员声明前加上私有成员访问控制修饰符`private`，则无法从该类的外部访问到类内部的成员，而只能被该类自身访问和修改，而不能被任何其他类，包括该类的子类来获取或引用，因此达到对数据最高级别保护的目的。

前文例子中的`volu.radius = 2.8;`和`volu.height = 5;`将造成编译错误

### 4.6.2 公共成员

```
class CylinderNew
{
    private double radius;
    private double height;
    private double PI = 3.14;

    public void setCylinder(double r, double h) {
        if (r > 0 && h > 0) {
            radius = r;
            height = h;
        } else {
            System.out.println("你的数据有误");
        }
    }
    double area()
    {
        return PI * radius * radius;
    }
    double volume()
    {
        return area() * height;
    }
    }

public class Example4_6_2
{
    public static void main(String[] args)
    {
        CylinderNew volu = new CylinderNew();
        volu.setCylinder(-2.5, -5);
        System.out.println("圆柱底面积： " + volu.area());
        System.out.println("圆柱体积： " + volu.volume());
    }
}
```

通过公共方法`setCylinder()`来修改私有成员，并加入判断代码来杜绝错误数据的输入。

## 4.7 方法的重载

方法的重载(Overloading)是实现"多态"的一种方法。重载是指同一个类内具有相同名称的多个方法，这些方法如果参数个数不同，或参数个数相同但类型不同，则这些同名的方法就具有不同的功能。

注意：Java语言中不允许参数个数或参数类型完全相同，而只有返回值类型不同的重载。

## 4.8 构造方法

### 4.8.1 构造方法的定义和作用

+ 构造方法(constructor)是一种特殊的方法，它是在对象被创建时初始化对象的成员的方法
+ 构造方法的名称必须与它所在的类名完全相同
+ 构造方法没有返回值，但构造方法名前不能使用`void`修饰符，因为一个类的构造方法的返回值类型就是该类本身
+ 如果使用了`void`修饰符，就不再是构造方法而变成普通的方法了，无法自动调用
+ 构造方法会在对象产生时自动执行，不需要在程序中调用
+ 如果省略构造方法，Java编译器会自动生成一个默认的构造方法。当用户为该类自定义了构造方式后，将把系统默认提供的构造方法给覆盖(Overriding)
+ 一般情况下，类都有一个或多个重载方法，通过重载，可以让用户用不同的参数来构造对象
+ Java语言允许在类内通过关键字`this`从某一个构造方法内调用另一个构造方法

```
class Cylinder
{
    private double radius;
    private  double height;
    private double PI = 3.14;
    private String color;
    public Cylinder()
    {
        this(2.5, 5, "红色"); //实现从一个构造方法到另一个构造方法
        System.out.println("无参数被调用");
    }
    public Cylinder(double r, double h, String str)
    {
        System.out.println("有参数方法被调用");
        radius = r;
        height = h;
        color = str;
    }
    public void show()
    {
        System.out.println("圆柱底半径：" + radius);
        System.out.println("圆柱高：" + height);
        System.out.println("颜色为：" + color);
    }
    double area()
    {
        return PI * radius * radius;
    }
    double volume()
    {
        return area() * height;
    }
}

public class Example4_6_2
{
    public static void main(String[] args)
    {
        Cylinder volu = new Cylinder();
        System.out.println("圆柱底面积： " + volu.area());
        System.out.println("圆柱体积： " + volu.volume());
        volu.show();
    }
}
```

注意：`this`关键字必须写在构建方法内的第一行位置

+ 构造方法一般都是公有(public)的，因为它们在创建对象时，是在类的外部被系统自动调用的
+ 构造方法被声明为`private`时，则无法在该构造方法所在的类以外的地方被调用，只能在类的内部调用。

## 4.9 静态成员

被`static`修饰的成员称为静态成员，也称类成员；而不用`static`修饰的成员称为实例成员

### 4.9.1 实例成员

+ 当我们用`new`运算符产生两个不同的对象时候，这两个对象都各自拥有自己保存自己成员的存储空间，而不与其他对象共享。
+ 两个对象的成员变量各自独立，且存于不同的内存之中。这样的成员变量在Java中称为实例变量
+ 必须先创建对象，再利用对象来调用方法。这样的方法在Java中称为实例方法

### 4.9.2 静态变量

+ 静态变量是隶属于类的变量，而不是属于任何一个类的具体对象。
+ 静态变量是一个公共的存储单元，类的任意一个对象访问它时取得的都是同一个相同的数值
+ 静态变量在某种程度上与其他语言的全局变量相似，如果不是私有的就可以在类的外部进行访问，此时不需要创建类的实例对象，只需要类名就可以引用
+ 类中若含有静态变量，静态变量必须独立于方法之外
+ 对于静态变量的使用，推荐采用`类名.静态变量名`的形式访问

### 4.9.3 静态方法

一个方法声明为`static`有以下几重含义：
1. 非`static`的方法是属于某个对象的方法，对象的方法在内存中有专属于自己专用的代码段。而`static`的方法是属于整个类的，它在内存的代码段将被所有的对象所共用
2. `static`方法不能操纵和处理属于某个对象的成员，而只能处理属于整个类的成员，即`static`方法只能访问`static`成员变量或调用`static`成员方法。
3. 在静态方法中不能使用`this`或`super`，因为`this`代表调用该方法的对象，而静态方法不需要对象来调用，`this`也不应存在与"静态方法"内部
4. 可以直接用类名来调用静态方法

如果一个类在被Java虚拟机解释器装载运行时，由于Java程序是从`main()`方法开始运行的，因此类中必须有`main()`方法。由于Java虚拟机需要在类外调用`main()`方法，所有该方法的访问权限必然是`public`，又Java虚拟机运行时系统在开始执行一个程序前，并没有创建`main()`方法所在类的一个实例对象，因此只能通过类名来调用`main()`方法来作为程序的入口，所有方法必须是`static`

### 4.9.4 静态初始化器

静态初始化器是由关键字`static`修饰的一对大括号"{}"括起来的语句组，作用与类的构造方法相似，都用于初始化工作。

与构造方法的不同：
1. 构造方法是对每个新创建的对象初始化，静态初始化器是对类本身进行初始化
2. 构造方法用`new`运算符创建新对象时由系统自动执行，而静态初始化器一般不能由程序调用，他是在所属的类被加载入内存时由系统调用执行的
3. 用`new`运算符创建多少个对象，构造方法就调用多少次，但静态初始化器则在类被加载入内存的时候只执行一次，与创建多少个对象无关
4. 不同于构造方法，静态初始化器不是方法，没有文件名、返回值和参数

例子
```
static 
{
    num = 100;
    System.out.println("静态初始化器被调用了，num的除值为" + num);
}
```

类是在第一次被使用时候才被装载的，而不是在程序启动的时候。

### 4.10 对象的应用

1. 类类型的变量属于非基本类型的变量
2. 实际上对象是一种引用型的变量，而引用型变量实际上保存的是对象在内存中的首地址。是"指向对象的变量"

#### 4.10.1 对象的赋值与比较

**当参数是基本数据类型时，则是传值方式调用；当参数是引用变量是，则是传址方式调用**

```
class Cylinder
{
    private static final double PI = 3.14;
    private double radius;
    private int height;
    public Cylinder(double r, int h)
    {
        radius = r;
        height = h;
    }
    public void compare(Cylinder volu)
    {
        if (this == volu)
            System.out.println("这两个对象相等");
        else
            System.out.println("这两个对象不相等");
    }
}

public class Example
{
    public static void main(String[] args)
    {
        Cylinder volu1 = new Cylinder(1.0, 2);
        Cylinder volu2 = new Cylinder(1.0, 2);
        Cylinder volu3 = volu1; // volu3和volu1指向同一对象
        volu1.compare(volu2); //volu1和volu2尽管数值相同，在内存上是占据不同内存空间的互相独立的不同对象
        volu1.compare(volu3); //他们的值是同一对象在内存的首地址
    }
}
```

### 4.10.2 引用变量作为方法的返回值


```
class Person {
    private String name;
    private int age;
    public Person(String name, int age)
    {
        this.name  = name;
        this.age = age;
    }
    public Person compare(Person p)
    {
        if (this.age > p.age)
            return this;
        else
            return p;
    }
}

public class Example4_10_2
{
    public static void main(String[] args)
    {
        Person per1 = new Person("李三", 20);
        Person per2 = new Person("张四", 21);
        Person per3;
        per3 = per1.compare(per2);
        if (per3 == per1)
        {
            System.out.println("李三年纪大");
        }
        else
        {
            System.out.println("张四年纪大");
        }
    }
}
```

### 4.10.3 类类型的数组

```
class Person {
    private String name;
    private int age;
    public Person(String name, int age)
    {
        this.name  = name;
        this.age = age;
    }
    public void show()
    {
        System.out.println("姓名：" + name + "\t年龄：" + age);
    }
}

public class Example4_10_2
{
    public static void main(String[] args)
    {
        Person[] per;
        per = new Person[3];
        per[0] = new Person("大傻",21);
        per[1] = new Person("二傻",19);
        per[2] = new Person("三傻",20);
        per[1].show();
        per[0].show();
    }
}
```

### 4.10.4 以对象数组为参数进行方法调用

```
class Person {
    private String name;
    private int age;
    public Person(String name, int age)
    {
        this.name  = name;
        this.age = age;
    }
    public static int minAge(Person[] p)
    {
        int min = Integer.MAX_VALUE;
        for (int i=0; i<p.length; i++)
        {
            if (p[i].age < min)
                min = p[i].age;
        }
        return min;
    }
}

public class Example4_10_2
{
    public static void main(String[] args)
    {
        Person[] per;
        per = new Person[3];
        per[0] = new Person("大傻",21);
        per[1] = new Person("二傻",19);
        per[2] = new Person("三傻",20);
        System.out.println("最小的年纪是" + Person.minAge(per));
    }
}
```

注意这里的`minAge()`方法被声明为`static`，可以直接使用类名在调用该方法。