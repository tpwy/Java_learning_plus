# 第八章 泛型程序设计

## 8.1 为什么使用泛型程序设计

+ 泛型程序设计(Generic programming)意味着编写的代码可以被不同类型的对象所重用
+ 泛型解决了类型强制转换异常的不安全隐患
+ 泛型实际上是在定义类、接口和方法是通过为其增加"类型参数(Type Parameters)"来实现的
+ 泛型所操作的数据类型的被指定为一个参数，即类型参数。(将数据类型参数化)
+ 类型参数的魅力在于：相比于强制类型转换，程序具有更好的可读性和安全性

## 8.1.1 泛型设计

+ 使用泛型类非常的容易简洁，但设计泛型类却没有那么容易。泛型程序员需要预测出所用类的未来可能有的所有用途
+ 未来解决泛型设计的困难，Java语言的设计者发明了通配符类型(wildcard type)。通配符类型非常抽象，却让库的构建着编写除尽可能灵活的方法
+ 泛型程序设计划分为三个能力级别，基本级别是仅仅使用泛型类而不考虑它们的工作方式和原因

##  8.2 定义简单泛型类

这里举一个泛型类(generic class)——`Pair`类

```
public class Pair <T> {
    private T first;
    private  T second;
    
    public Pair() {first = null; second = null;}
    public Pair(T first, T second) {this.first = first; this.second = second;}

    public T getFirst() {
        return first;
    }

    public T getSecond() {
        return second;
    }

    public void setFirst(T first) {
        this.first = first;
    }

    public void setSecond(T second) {
        this.second = second;
    }
}
```

注意： 类型变量使用大写形式，且比较短。在Java库中，使用变量E表示集合的元素类型，K和V分别表示表的关键字与值的类型。T(有时还用临近的U和S)表示任意类型

用具体的类型替换类型变量就可以实例化泛型类型了

```
Pair<String> // 声明
Pair<String>() // 空参数构造器
Pair<String>(String first, String secong) 

String getFirst() // 使用方法
void setFirst(String)
```

C++注释：

+ 从表面看，Java的泛型类类似于C++的模板类，唯一明显的不同是Java没有关键字`template`
+ 但是(忽然打脸),这两种机制还是有着本质的区别

## 8.3 泛型方法

+ 在普通类中也可以定义泛型方法，类型变量放在修饰符之后，返回类型之前
+ 调用泛型方法时，尖括号中放入具体类型，位于方法名前面。在大多数使用都可以不加上类型参数(编译器自动判断)

C++注释：C++中将类型参数放在方法名后面可能导致语法分析的歧义

## 8.4 类型变量的限定

在Java语言中可以在用泛型类创建对象时对数据类型做出限制

```
class ClassName <T extends ClassOne & ClassTwo>
```

+ 限定类型用“&”分割，而逗号用来分割类型变量
+ 对于实现了某接口的有限制泛型，同样适用关键字`extends`(理解为类和接口的子类)

C++注释： 在C++中不能对魔板参数的类型加以限制

## 8.5 通配符类型

### 8.5.1 通配符的概念

通配符类型中，允许类型参数变化

```
泛型类名<? extends T> o = null; //声明泛型类对象
```

这时`? extends T`表示T或T的子类型或是实现接口T的类

问题：使用通配符引用`Pair<? extends Employee>`会破坏`Pair<Manager>`对象吗？

分析：

```
Pair<Manager> managerBuddies = new Pair<>(ceo, cfo);
Pair<? extends Employee> wildcardBuddies = managerBuddies;
wildcardBuddies.setFirst(lowlyEmployee); // error

// 调用setFirst方法的过程
? extends Employee getFirst()
void setFirst(? extends Employee) // 无法确定?具体表示什么类型，?不能用来匹配
```

这个时候就需要使用有限定的通配符，区分安全的访问器方法和不安全的更改器方法了

### 8.5.2 通配符的超类型限定

超类型限定(supertype bound)

```
? super Manager //限制为Manager的所有超类型
```

+ 由于编译器无法知道`setFirst`方法的具体类型，因此只能接受`Manager`类型对象或某个子类型对象
+ 但如果调用`getFirst`，不能保证返回对象的类型，因此把它赋给了`Object`
+ 简单的说，超类型限定的通配符可以想泛型对象写入，带有子类型限定的通配符可以从泛型对象读取

### 8.5.3 无限定通配符

举个例子

```
Pair<?>

? getFirst()
void setFirst()
```

`Pair<?>`和`Pair`的本质不同在于：可以用任意`Object`对象调用原始`Pair`类的`setObject`方法(常用于简单操作)

举个例子

```
public static boolean hasNulls(Pair<?> p)
{
    return p.getFirst() == null || p.getSecond() == null;
}

public static <T> boolean hasNulls(Pair<T> p) // 可以避免使用通配符，不过可读性下降了
```

## 8.6 泛型代码与虚拟机

+ 虚拟机中没有泛型类型对象——所有对象都属于普通类。
+ 无论何时定义一个泛型类型，都自动提供了一个相应的原始类型(raw type)
+ 原始类型就是删去类型参数后的泛型类型名
+ 擦除(erased)类型变量，并替换为限定类型(无限定类型就用Object)

C++ 注释：Java泛型和C++模板的区别就在于：C++中每个模板的实例化产生不同的类型，这一现象称为"模板代码膨胀"。Java中不存在这种问题的困扰

+ 当程序调用泛型方法时，如果擦除返回类型，编译器将插入强制类型转换
+ 当存取泛型域时也要插入强制类型转换

有关Java泛型转换的事实：
+ 虚拟机中没有泛型，只有普通的类和方法
+ 所有的类型参数都用它们的限定类型替换
+ 桥方法被合成用来保持多态
+ 为保持类型安全性，必要时插入强制类型转换

## 8.7 约束和局限性

1. 不能用基本类型实例化类型参数
2. 运行时类型查询只适用于原始类型
3. 不能创建参数化类型数组(解决方法：使用ArrayList)
4. Varargs警告(向参数个数可变的方法传递一个泛型类型的实例将出现问题)
5. 不能实例化类型变量
6. 不能构造泛型数组
7. 泛型类的静态上下文中类型变量无效
8. 不能抛出或捕获泛型类的实例
9. 可以消除对受查异常的检查
10. 注意擦除后的冲突