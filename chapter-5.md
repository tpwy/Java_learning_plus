# 第五章 继承

## 5.1 类、超类、子类

+ Java不支持多重继承，所以一个类只能有一个直接父类
+ Java语言中有一个名为`java.lang.Object`的特殊类，所有的类都是直接或间接地继承该类而得到的

### 5.1.1 定义子类

用关键字`extends`表示继承

```
public class subclass extends superclass
{
    ...
}
```

C++注释：

Java和定义继承类的方式十分相似，Java用关键字`extends`代替C++中的冒号`:`。在Java中，所有的继承都是公有继承，而没有C++中的私有继承和保护继承

关键字`extends`表明正在构造的新类派生于一个已存在的类
+ 以存在的类称为**超类(superclass)**、基类(base class)或父类(parent class)。
+ 新类称为**子类(subclass)**、派生类(derived class)或孩子类(child class)。

在通过扩展垂泪定义子类的时候，仅需要指出子类与超类的不同之处。因此在设计类的时候，应该将通用的方法放在超类中，而将具有特殊用途的方法放在子类中。

### 5.1.2 覆盖方法

当超类中的方法不适用于子类时，我们需要提供一个新方法来覆盖(override)超类中的这个方法

+ 问题：子类不能继承父类中的私有成员，因此也不能直接访问父类的私有成员
+ 解决1：通过`super`关键字来调用父类中的访问器或者私有成员本身(推荐)
+ 解决2：也可以将父类中的成员声明为`protected`，`protected`成员可以被三种类引用：类自身、与它在同一个包中的其他类，和其他包中该类的子类。（违反OOP提倡的数据封装原则，安全性比C++差，需谨慎使用）
+ 子类不能覆盖父类中声明为`final`或`static`的方法
+ 子类中不能删除继承的任何域和方法
+ `super`与`this`不同，不是一个对象的引用，不能将`super`赋给另一个对象变量。

C++ 注释：

在Java中使用关键字`super`调用超类中的方法，在C++中则采用超类名加上`::`操作符的形式

### 5.1.3 子类构造器

由于子类不能访问超类中的私有域，所有使用`super()`调用父类的构造器，对这部分私有域进行初始化

+ 如果子类的构造器没有显式地调用超类的构造器，则自动调用超类默认(没有参数)的构造器。
+ 如果超类中没有不带参数的构造器，子类构造器又不显式地调用超类的其他构造器，则Java编译器将报错
+ 为了避免编译器报错，必须在父类中定义一个没有参数的构造器
+ 与`this()`相同，`super()`语句必须放在子类构造器方法的第一行，因此`this()`和`super()`不能同时存在于一个构造器方法中

```
class Person
{
    private String name;
    private int age;
    public Person()
    {
        System.out.println("调用了Person类的无参数构造器");
    }
    public Person(String name, int age)
    {
        this.name = name;
        this.age = age;
        System.out.println("调用了Person类的有参数构造器");
    }
    protected void show()
    {
        System.out.println("姓名： " + name + "\t年纪：　" + age);
    }
}

class Student extends Person
{
    private String department;
    int age = 25;
    public Student(String name, int age, String department)
    {
        super(name, age);
        this.department = department;
        super.show();
        System.out.println("系别: " + department);
    }
    public Student()
    {
        System.out.println("调用子类中的无参数构造器");
    }
}

public class StudentTest {
    public static void main(String[] args)
    {
        Student stu = new Student("Nancy", 30, "information");
        System.out.println();
        Student ste = new Student();
    }
}

//输出结果

调用了Person类的有参数构造器
姓名： Nancy	年纪：　30
系别: information

调用了Person类的无参数构造器
调用子类中的无参数构造器
```

### 5.1.4 多态

有一种用来判断是否应该设计为继承关系的简单规则，就是"is-a"规则，它的另一种表述发是置换法则，表明程序中出现超类对象的任何地方都可以用子类对象置换

```
Person e;
e = new Person(...);

e = new Student(...);
// 我们可以通过这些方式用父类的对象访问子类的成员
// 但这种访问只限于"覆盖"这种情况的发生，如果是方法仅存于子类，就无法访问
```

在Java中，对象变量是多态的，一个`Person`变量既可以引用一个`Person`类对象，也可以引用`Person`类对象的任何一个子类的对象，如`Student`

```
Student nancy = new Student(...);
Person[] man = new person[3];
person[0] = nancy; //变量person[0]和nancy引用同一个对象，但是编译器将person[0]看成Person对象
Student m = Person[1]; //Error
```

### 5.1.5 理解方法调用

假设要调用`x.f(args)`，隐式参数ｘ声明为类Ｃ的一个对象。调用过程的详细描述：
1. 编译器查看对象的声明类和方法名。存在多个名为`f`的方法时，编译器将会一一列出所有C类和其超类中访问属性为`public`且名为`f`的方法
2. 编译器将查看调用方法时提供的参数类型。重载解析：如果在所有名为`f`的方法中存在一个与提供的参数类型完全匹配，就选择这个方法。如果编译器没有找到与参数类型匹配的方法，或者经过类型转换后有多个方法与之匹配，就会报告错误
3. 如果是`private`、`static`、`final`方法或者构造器，编译器就可以准确知道应该调用哪个方法，即静态绑定(static binding)。以此相对应的，调用的方法依赖于隐式参数的实际类型，并且在运行中实现动态绑定。
4. 当程序运行时，并且采用动态绑定调用方法时，虚拟机一定调用与x所引用对象的实际类型最适合的那个类的方法 

注意：
1. 返回类型不是签名中的一部分，所以在覆盖方法时一定要保证返回类型的兼容性
2. 每次调用方法都会进行搜索，时间开销相当大。因此虚拟机预定为每个类创建了一个方法表(method table)，列出所有方法的签名和实际调用的方法。如果使用`super`关键字调用方法，编译器将对隐式参数超类的方法进行搜索
3. 在覆盖一个方法的时候，子类方法不能低于超类方法的可见性

### 5.1.6 阻止继承：final类和方法

+ `final`类不允许扩展，无法利用该类来定义子类
+ `final`类所有的方法都自动称为`final`方法
+ `final`方法不能被子类覆盖
+ 声明为`final`的主要目的：确保它们不会再子类中改变语义
+ 在C++和C#中，如果没有特别地说明，所有的方法都不具有多态性
+ 如果一个方法被覆盖并且很短，编译器就能够对它进行优化处理，这个过程称为内联(inlining)

### 5.1.7 强制类型转换

+ 进行强制类型转换的唯一原因：在暂时忽视对象的实际类型之后，使用对象的全部功能
+ 如果试图在继承链上进行向下的类型转换，并"谎报"有关对象包含的内容，那么运行程序时系统将报告这个错误；如果没有捕获到这个异常，程序将中止
+ 在进行类型转换之前，应先用`instanceof`方法检查是否能够成功转换

```
if (person[1] instanceof Student)
{
    nancy = (Student) person[1];
    ....
}
```

+ 如果x为`null`，进行检查不会产生异常，只是返回false。因为`null`没有引用任何对象
+ 不推荐使用类型转换和`instanceof`运算符

C++注释：

当类型转换失败时，Java不会生成一个null对象 ，而是抛出一个异常。这种做法有点像C++中的引用(reference)转换。

### 5.1.8 抽象类

+ 为了提高程序的清晰度，包含一个或多个抽象方法的类本身必须被声明为抽象的

```
public abstract class Person
{
    private String name;
    ...
    public abstract String getDescription();
    
    public Person(String name)
    {
        this.name = name;
    }

    public String getName()
    {
        return this.name;
    }
}
```

+ 除了抽象方法，抽象类还可以包括具体数据和具体方法。建议尽量将通用的域和方法(不管是不是抽象的)放在超类(也不管是不是抽象的)
+ 抽象方法充当着站位的角色，它的具体实现在子类中。
+ 扩展抽象类有两个选择：
    1. 在抽象类中定义部分抽象类方法或不定义抽象类放在，这样就必须将子类也标记为抽象类
    2. 定义全部的抽象方法，这样子类就不是抽象的了
+ 类即使不包含抽象方法，也可以声明为抽象类，但要注意抽象类不能被实例化。
+ 可以定义一个抽象类的对象变量，但它只能引用非抽象子类的对象

C++注释：

在C++中，有一种尾部用`=0`标记的抽象方法称为纯虚函数，如

```
class Person // C++
{
    public:
        virtual string getDescription()=0;
        ...
};
```

只要有一个纯虚函数，这个类就是抽象类。在C++中没有表示抽象类的特殊关键字

## 5.2 Object：所有类的超类

+ `Object`是Java中所有类的始祖，在Java中每个类都是由它扩展而来的。
+ 如果没有明确地指出超类，Object就被认为是这个类的超类
+ 使用`Object`类型的变量可以引用任何类型的变量
+ 在Java中，只有基本类型变量不是对象
+ 所有的数组类型，不论是对象数组还是基本类型的数组都扩展了`Object`类

```
Object obj;
Employee[] staff = new Employee[10];
obj = staff;
obj = new int[10];
```

C++注释：在C++中没有所有类的根类，但每个指针都可以转换成`void*`指针

### 5.2.1 equals 方法

1. `Object`类中的`equals`方法用于检测一个对象是否等于另一个对象
2. `equals`方法将判断两个对象是否具有相同的引用
3. `==`操作符判断两个变量的值，即它们的引用地址是否相同

```
import java.time.*;

class Employee
{
    ...
    public boolean  equals(Object otherObject)
    {
        if (this == otherObject)
            return true;
        if (otherObject == null)
            return false;
        if (getClass() != otherObject.getClass())
            return false;
        Employee other = (Employee) otherObject;
        return name.equals(other.name)
                && salary == other.salary
                && hireDay.equals(other.hireDay);
    }

}

class Manager extends Employee
{   
    ...
    public boolean eaquals(Object otherObject)
    {
        if (!super.equals(otherObject)) 
            return false;
        Manager other = (Manager) otherObject;
        return this.bonus == other.bonus;
    }
}
```

在子类中定义`equals`方法时，首先调用超类的`equals`。如果检测失败，对象就不可能相等，如果超类中的域都相等，则比较子类的实例域

### 5.2.2 相等测试与继承

如果隐式和显式的参数不属于同一个类，`equals`方法将如何处理?

+ 不建议使用`instanceof`进行检测。
+ Java语言要求`equals`方法具有以下特性：
    1. 自反性：任何非空引用`x`,`x.equals(x)`应该返回`true`
    2. 对称性：对于任何引用`x`和`y`，当且仅当`x.equals(y)`返回`true`，`y.equals(x)`也应该返回`true`
    3. 传递性：对于任何引用`x`、`y`和`z`，如果`x.equals(y)`返回`true`，`y.equals(z)`返回`true`，`x.equals(x)`也返回`true`
    4. 一致性：如果`x`和`y`引用的对象没有发生变化，反复调用`x.equals(y)`应该返回同样的结果
    5. 对于任何非空引用`x`，`x.equals(null)`应该返回`null`
+ 编写完美的`equals`方法的建议：
    1. 显示参数命名为`otherObject`，稍后需要将它转换成另一个叫做`other`的变量
    2. 检测`this`和`otherObject`是否引用同一个对象：`if (this == otherObject) return true;`
    3. 检测`otherObject`是否为`null`，如果为`null`返回`false`：`if (otherObject == null) return false;`
    4. 比较`this`和`otherObject`是否属于同一个类：
        + 如果`equals`的语义在每个子类中有所改变，就是用`getClass`方法来检测：`if (getClass() != otherObject.getClass()) return false;`
        + 如果所有的子类都拥有统一的语义，就是用`instanceof`检测，此时超类的`equals`方法应声明为`final`：`if(！(otherObject instanceOf ClassName)) return false;`
    5. 将`otherObject`转换为相应的类类型变量： `ClassName other = (ClassName) otherObject;`
    6. 现在开始对所有需要比较的域进行比较。使用`==`比较基本类型域，使用`equals`比较对象域。如果所有的域都匹配则返回`true`，否则返回`false`
+ 如果在子类中重新定义`equals`，其中需要调用`super.equals(other)
+ 对于数组类型的域，可以使用静态的`Arrays.equals`方法检测
+ 为了避免发生参数类型错误，可以使用`@override`对覆盖超类的方法进行标记，如果发生错误，或正在定义一个新方法，编译器将给出错误报告。

### 5.2.3 hashCode　方法

+ 散列码(hash code)是由对象导出的一个整型值，散列码没有规律
+ 由于`hashCode`方法定义在`Object`类中，因此每个对象都有一个默认的散列码，其值为对象的存储地址
+ 如果重新定义`equals`方法，就必须重新定义`hashCode`方法，以便用户可以将对象导入到散列表中
+ `hashCode`方法应该返回一个整型数值，并合理地组合实例域的散列码，以便让各个不同的对象产生的散列码更加均匀
+ `equals`与`hashCode`的定义必须一致：如果`x.equals(y)`返回`true`，那么`x.hashCode()`和`y.hashCode()`应该具有相同的值。例子：如果定义的`Employee.equals`比较雇员的ID，那么`hashCode`方法就只需要散列ID
+ 可以使用`Arrays.hashCode`方法计算数组类型的散列码

```
// Spring类使用下列算法计算散列码

int hash = 0;
for (int i = 0; i < length(); i++)
    hash = 31 * hash + charAt(i);

// 推荐构建自定义类的hashCode方法

public int hashCode()
{
    return Object.hash(filed1, field2, field3);
}
```

### 5.2.4 toString 方法

+ `toString`方法用于返回表示对象值的字符串
+ 随处可见`toString`的原因是：只要对象与一个字符串通过操作符`+`连接起来，Java编译就会自动调用`toString`方法获得对象的字符串描述
+ `Object`类定义了`toString`方法用来打印输入对象所属的类名和散列码
+ 用`""+x`来替代`x.toString()`的好处是：如果x是基本类型也能照样执行
+ 如果x是任意一个对象，并调用`System.out.println(x);`，那么`println()`将会自动调用`x.toString()`并打印输出的字符串
+ 数组继承了`Object`类的`toString`方法，所以要想生成对象描述，需要使用静态方法`Arrays.toString()`，如果是多维数组的话则需要调用`Arrays.deepToString()`方法
+ `toString`方法是一种非常有用的调试工具(不过不是最好的)
+ 强烈建议自定义的每一个类增加`toString`方法

```
// toString方法的推荐定义
public String toString()
{
    return getClass().getName() + "[name= " + name + ", salary= " + salary + ", hireDay= " + hireDay + "]";
}

// 在子类中推荐
public String toString()
{
    return super.toString() + "[bonus= " + bonus + "]";
}
```

## 5.3 泛型数组列表

+ `ArrayList`类类似数组，但在添加和删除元素时可以自动调节数组容量和功能，而不需要为此编写任何代码
+ `ArrayList`是一个采用类型参数(type parameter)的泛型类(generic class)。为了制定数组列表保存的元素对象类型，需要用一堆尖括号将类名括起来加在后面。
+ 声明和构造一个数组列表

```
ArrayList<Employee> staff = new ArrayList<Employee>();
//简写
ArrayList<Employee> staff = new ArrayList<>();

//使用add方法
//如果调用add且内部数组已经满了，数组列表将会自动地创建一个更大的数组并将所有的对象拷贝过去
staff.add(new Employee("Harry",...)); 

//如果已经清楚或能够估计数组可能存储的元素数量，可以调用ensureCapacity方法
staff.ensureCapacity(100); //将分配一个包含100个对象的内部数组，调用100次add不用重新分配空间

//size方法返回数组列表中包含的实际元素数目
staff.size(); //等价于数组的length方法

/**一旦确认数组的大小不再发生变化，就可以调用trimToSize方法
 *该方法将存储区域的大小调整为当前元素数量所需要的存储空间数目
 *垃圾回收期将回收多余的存储空间
 */
staff.trimToSize();
```

+ 数组列表的容量和数组大小有一个非常重要的区别：如果为数组分配100个元素的存储空间，数组就有100个空位置可供使用。而容量为100的数组列表只是拥有保存100个元素的潜力，但是最初甚至完成初始化构造前并不含有任何元素

C++注释：

1. `ArrayList`类似于C++的`vector`模板，两者都是泛型类型。但`vector`模板为了便于访问元素重载了`[]`运算符，而Java没有运算符重载所有必须调用显式方法
2. C++向量是值拷贝，而在Java中赋值语句的操作是让两个变量都引用同一个数组列表

### 5.3.1 访问数组列表元素

使用set方法设置第i个元素。注意只有i小于或等于数组列表的大小时才能调用，set方法只能替换数组已有元素，添加新元素还是使用add方法

```
staff.set(i, harry);//等价于数组中的赋值 a[i] = harry;
```

获得数组列表的元素

```
Employee m = staff.get(i); //等价于数组中的 Employee m = staff[i];
```

在没有泛型类时，原始的`ArrayList`类提供的get方法只能返回`Object`，因此get方法的调用者必须对返回值进行类型转换

```
Employee e = (Employee) staff.get(i);
```

原始的ArrayList存在一定的危险性，因为add和set方法都允许接受任何类型的对象

```
staff.set(i, "Harry"); //编译器不会给出警告
staff<Employee>.set(i, "Harry"); //Error
```

为了灵活扩展数组，又能方便访问数组元素，推荐使用以下技巧：

```
ArrayList<X> list = new ArrayList<>();
while(...)
{
    x = ...;
    list.add(x);
}

X[] a = new X[list.size()];
list.toArray(a);
```

使用带索引参数的add方法

```
int n = staff.size() / 2;
staff.add(n, e);
```

删除元素

```
X e = staff.remove(n);
```

使用`for each`遍历数组列表

```
for (Employee e : staff)
    do something with e

// 等价于

for (int i; i<staff.size(); i++)
{
    Emplyee e = staff.get(i);
    do something with e
}
```

### 5.3.2 类型化与原始数组列表的兼容性

```
public class EmployeeeDB
{
    public void update(ArrayList list) {...}
    public ArrayList find(String query) {...}
}
```

+ 可以将一个类型化的数组列表传递给`update`方法，而并不需要进行类型转换
+ 尽管这样做不会报错或者警告，但这样调用并不安全
+ 如果数组列表中的元素不是`Employee`类型，对这些元素进行检索时将发生异常

```
ArrayList<Employee> staff = ...;
employeeDB.update(staff);
```

相反的，将一个原始的`ArrayList`付给一个类型化的`ArrayList`会得到一个警告

```
ArrayList<Employee> result = employeeDB.find(query); //yield warning

// 使用类型转换并不能避免问题
ArrayList<Employee> result = (ArrayLIst<Employee>) employeeDB.find(query);
```

鉴于兼容性的考虑，编译器在对类型转换进行检查后，如果没有发现违反规则的现象，就将所有的类型化数组列表转换为原始`ArrayList`对象。

一旦能确保不会发生严重后果，可以用`@SuppressWarnings("unchecked")`标注来标记这个变量能够接受类型转换

## 5.4 对象包装器和自动装箱

+ 有时需要将基本类型转换为对象。所有的基本类型都有一个与之相对应的类，这些类称为包装器(wrapper)。
+ 例子：`Integer、Long、Double、Short、Byte、Character、Void和Boolean`，前六个类派生于公共的超类`Number`
+ 对象包装器类是不可变的，即一旦构造了包装器就不允许更改包装在其中的值。
+ 对象包装器类还是`final`，因此不能定义它们的子类

假设定义一个整型数组列表，而尖括号中的类型参数不允许是基本类型的时候，就需要使用对象包装器类

```
ArrayList<Integer> list = new ArrayList<>(); //由于效率非常低，只适合构造小型集合

list.add(3); //将自动转换为list.add(Integer.valueOf(3));
//这种变换被称为自动装箱(autoboxing)，也称自动打包(autowrapping)

int n = list.get(i); // 将Integer对象赋给int值的时候将会自动拆箱

Integer n = 3;
n++; // 在算术表达式中也能自动的装箱拆箱
```

大多数情况下，容易有一种假象，即基本类型和它们的对象包装器类是一样的，只是它们的相等性不同

自动装箱规范要求`boolean、byte、char <= 127`,介于-128和127之间的`short`和`int`被包装在固定的对象中，所以会出现：

```
public static void main(String[] args) 
{
    Integer a = 100;
    Integer b = 100;
    System.out.println("a==b: " + (a==b));
    System.out.println("a.equals(b)" + a.equals(b));
    System.out.println();

    Integer x = 1000;
    Integer y = 1000;
    System.out.println("x==y: " + (x==y));
    System.out.println("x.equals(y）" + x.equals(y));
}

//output
a==b: true
a.equals(b)true

x==y: false
x.equals(y）true
```

+ 为了避免`==`运算符的不确定的结果，我们调用`equals`方法来比较包装器对象
+ 由于包装器类引用可以是`null`，所有自动装箱可能抛出`NullPointerException`异常
+ 如果在一个条件表达式中混合使用`Integer`和`Double`类型，`Integer`值就会拆箱提升为`double`，再装箱为`Double`
+ 装箱和拆箱是编译器认可的，而不是虚拟机。编译器在生成类的字节码时插入必要的方法调用，而虚拟机只负责执行

```
Integer n = 1;
Double x = 2.0;
System.out.println(true? n: x); //print 1.0
```

由于Java方法都是按值传递的，因此不可能编写下面这样一个能够增加参数值的Java方法：
```
public static void triple(int x) //doesn't work
{
    x = 3 * x;
}

// 替换成Integer也不起作用，因为Integer对象是不可变的

// 想要编写一个修改数值参数值的方法，就需要使用在org.omg.CORBA包中定义的持有者(holder)类型
// 每个持有者类型都包括一个公有(!)域值可以访问存储在里面的值

import org.omg.CORBA.IntHolder;
public class Example {
    public static void main(String[] args)
    {
        IntHolder t = new IntHolder(5);
        triple(t);
        System.out.println(t.value);
    }
    public static void triple(IntHolder x)
    {
        x.value = 3 * x.value;
    }
}
```

## 5.5 参数数量可变的方法

现在的Java版本提供了可以用可变的参数数量调用的方法(有时称为"变参"方法)

举个变参方法的例子：`printf`方法

```
public class PrintStream
{
    public PrintStream printf(String fmt, Object... args) 
    {
        return format(fmt, args);
    }
}
```

这里的省略号表明这个方法可以接收任意数量的对象(除fmt参数之外)。实际上`printf`方法只接收了两个参数：格式化字符串和`Object[]`数组(如果调用者提供的是基本类型的值，则自动装箱功能将它们转换为对象)。然后扫描`fmt`字符串，将第i个格式说明符和`arg[i]`的值匹配起来

```
// 用户也可以自己定义可变参数的方法
public static double max(double...values)
{
    double largest = Double.MIN_VALUE;
    for (double v: values)
    {
        if (v > largest)
            largest = v;
    }
    return largest;
}
```

## 5.6 枚举类

+ 比较枚举类的值时，永远不需要调用`equals`，而直接使用`==`就可以
+ 如果需要的话，可以在枚举类型中添加一些构造器、方法和域。当然构造器只是在构造枚举常量的时候被调用

```
// 典型例子
public enum Size {SMALL, MEDIUM, LARGE, EXTRA_LARGE};

public enum Size
{
    SMALL("S"), MEDIUM("M"), LARGE("L"), EXTRA_LARGE("XL");

    private String abbreviation;

    private Size(String abbreviation)
        this.abbreviation = abbreviation;
    public String getAbbreviation()
        return abbreviation;
}
```

所有的枚举类型都是`Enum`类的子类

## 5.7 反射

+ 反射库(reflection library)提供了一个非常丰富且精心设计的工具集以便编写能够动态操纵Java代码的程序。
+ 能够分析类能力的程序称为反射(reflective)。反射机制可以用来：
    1. 在运行时分析类的能力
    2. 在运行是查看对象
    3. 实现通过的数组操作代码
    4. 利用`Method`对象，这个对象很像C++中的函数指针
+ 反射常用与工具构造而不是应用程序设计
（暂时跳过）

## 5.8 继承的设计技巧

1. 将公共操作和与放在超类中
2. 不要使用没有受保护的域。(protected适用于哪些不提供一般用途而在子类中重新定义的方法)
3. 使用继承实现"is-a"关系
4. 除非所有继承的方法都有意义，否则不要使用继承
5. 在覆盖方法时不要改变预期的行为
6. 使用多态而非类型信息
7. 不要过多的使用反射