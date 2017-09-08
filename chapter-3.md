# 第三章 Java的基本程序设计结构

## 3.1 一个简单的Java应用程序

```
public class FirstSample
{
    public static void main(String[] args)
    {
        System.out.println("We will not use'Hello, World");
    }
}
```

程序解析:  
1. Java区分大小写。  
2. 关键字public称为访问修饰符\(access modifier\),这些修饰符用于控制程序的其他部分对这段代码的访问级别。  
3. 关键字class表明Java程序中的所有内容都包括在类中。  
4. class后面紧跟类名，标准的命名规范为:类名是以大写字母开头的名字，如果名字由多个单词组成，则每个单词的第一个字母都大写。\(骆驼命名法\)  
5. 源代码的文件名必须与公共类的类名相同，并用`.java`作为扩展名。  
6. 运行已编译的程序时，JVM从制定类中的main\(\)方法开始执行，因此类的源文件必须包含一个main方法。  
7. 在Java、C/C++中，我们使用大括号来划分程序的各个部分\(块\)，推荐匹配的大括号上下对齐。  
8. 每个句子都必须使用`；`结束。  
9. `System.out.println()`这里，我们使用了System.out对象并调用了它的println方法。  
10. Java与C/C++一样，都采用双引号分割字符串。  
11. print\(\)方法在输出之后不换行。

## 3.2 注释

```
/**
 * This is the first sample grogram in Core Java Chapter
 * @author tpwy
 * @version 1.01
  *  可以自动地生成文档
 */

public class FirstSample {
    /*
     * This a sample example about core java
     */
    public static void main(String[] args)
    {
        System.out.println("We will not use 'Hello, World'!"); //Just one line
    }
}
```

## 3.3 数据类型

* Java是一门强类型语言，必须为每一个变量声明一种类型。
* 数据存储在内存的一块空间上，程序设计语言用变量名来代表该数据存储空间的位置。
* 数据类型定义了数据的性质、取值方位、存储方式以及对数据所能进行的运算和操作。
* Java数据类型分为两大类:

  1. 基本数据类型\(primitive types\)
     * 由程序设计语言系统所定义，不可再划分的数据类型
     * 基本数据类型的数据所占内存大小是固定的
     * 基本数据类型在内存中存放的是数据本身
  2. 引用数据类型\(reference types\),简称引用类型
     * 引用类型在内存中存放的是指向该数据的地址，不是数据值本身。
     * 往往由多个基本数据组成。
     * 对引用数据类型的应用称为对象引用，引用数据类型也叫符合数据类型，在有的程序设计语言中称为指针。

* Java语言定义了4类共8种基本数据结构

  * 整型——byte、short、int、long
  * 浮点型——float、double
  * 布尔型——boolean
  * 字符型——char

### 3.3.1 整型

整型用来表示正整数、零和负整数。在Java中有三种进制的表示形式:

* 十进制: 用多个0～9之间的数字表示，其首位不能为0；
* 八进制: 以0开头，后跟多个0～7之间的数字；
* 十六进制: 以0x或0X开头，后跟多个0～9之间的数字或a～f之间的小写字母或A～F之间的大写字母。

补充:

Java7开始，加上前缀0b或0B就可以写二进制数，还可以为数字字面量加下划线以提高易读性。

四种整型的内容:

| 类型 | 存储需求 | 取值范围 |
| --- | --- | --- |
| byte | 1字节 | $$-2^7～2^7-1$$ |
| short | 2字节 | $$-2^15～2^15-1$$ |
| int | 4字节 | $$-2^31～2^31-1$$ |
| long | 8字节 | $$-2^63～2^63-1$$ |

一个整数隐含为整型\(int\),int类型最常用，long类型的数值需要有一个后缀L或l。byte和short类型主要用于特定的应用场合，如底层的文件处理或者需要控制占用存储空间量的大数组。

与C/C++的区别:  
1. 在Java中，整型的范围与运行Java的机器无关，所有的数值类型所占据的字节数量与平台无关。  
2. Java没有任何无符号\(unsigned\)形式的int、long、short或byte类型。

### 3.3.2 浮点型

浮点数的表示方法:

* 标准计数法
* 科学计数法：eg 123.45可表示为`1.2345E+2`

两种浮点类型的具体内容:

| 类型 | 存储需求 | 取值范围 |
| --- | --- | --- |
| float | 4字节 | ±3.402 823 47E+38F\(有效位数为6~7位\) |
| double | 8字节 | ±1.797 693 134 862 315 70E+308\(有效位数为15位\) |

一个浮点数隐含为double型，float类型的数值需要有一个后缀F或f。在很多情况下float类型的精度很难满足需求，所以一般采用double类型。

所有的浮点数值计算都遵循IEEE 754规范，具体表示溢出和出错的情况的三个特殊浮点数值:

* 正无穷大——常量`Double.POSITIVE_INFINITY`
* 负无穷大——常量`Double.NEGATIVE_INFINITY`
* NaN\(不是一个数字\)——常量`Double.NaN` eg.计算0/0或者负数的平方根

注意:   
1. `Double.NaN`的使用方法

```
if (x == Double.NaN) // is never true

// 所有"非数值"的值都认为是不相同的，但可以用Double.NaN方法

if (Double.NaN(x)) // check whether x is "not a number"
```

1. 浮点数值不适用与无法接受舍入误差的金融计算中，浮点数值采用二进制系统表示，无法精确的表示分数1/10。\(就像十进制中无法精确的表示分数1/3一样\)

### 3.3.3 boolean类型

boolean类型又称逻辑型，有两个值: false和true用来判断逻辑条件。整型值和布尔值之间不能进行互相转换。

与C/C++的区别:

在C++中，数值甚至指针可以替代boolean值。值0相当于false，1相当于true。

### 3.3.4 char类型

* char类型原本用来表示单个字符，但现在有一些Unicode字符需要两个char值。\(详细参考《Java核心技术 卷I》的3.3.4节\)

* Java语言中的字符采用的是Unicode字符编码方法，在内存中占两个字节，是16位无符号的整数，共65536个。Unicode字符使用"\u0000"到"\uFFFF"之间的十六位进制数来表示的。Unicode的采用加强了Java语言处理多语种的能力。

* char类型的字面量要用单引号括起来。eg:

  * 'A'是编码值为65所对应的字符常量
  * "A"是包含一个字符A的字符串

特殊字符的转义序列:

| 转义序列 | 名称 | Unicode值 |
| --- | --- | --- |
| \b | 退格 | \u0008 |
| \t | 制表 | \u0009 |
| \n | 换行 | \u000a |
| \r | 回车 | \u000d |
| \" | 双引号 | \u0022 |
| \' | 单引号 | \u0027 |
| \ | 反斜杠 | \u005c |

## 3.4 关键字和标识符

### 3.4.1 关键字

* 关键字\(keyword\)也称保留字，是Java语言中被赋予特定含义的一些单词。Java语言不允许用户对关键字赋予其他的含义。
* 保留字\(reversed word\)包括包括关键字和未使用的保留字\(在现有Java版本中尚未使用，但以后版本可以会作为关键字使用\)

Java定义的关键字如下:

|  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- |
| abstract | assert | boolean | break | byte | case |
| catch | char | class | continue | default | do |
| double | else | enum | extends | false | final |
| finally | float | for | if | implements | import |
| instanceof | int | interface | long | native | new |
| null | package | private | protected | publlic | return |
| short | static | super | switch | synchronized | this |
| throw | throws | transient | true | try | void |
| volatile | while |  |  |  |  |

### 3.4.2 标识符

标识符\(indentifier\)是用来表示变量名、类名、方法名、数组名和文件名的有效字符序列。

标识符的规定:  
1. 标识符可以由字母、数字和下划线、美元符号等组合而成；  
2. 标识符必须以字符、下划线或美元符号开头，不能以数字开头。

实际应用中，标识符应具备一定程度的语义，关键字不能当做标识符来使用

编码习惯: 类名首字母大写；变量、方法和对象首字母小写。常量则大写所有字母，Java包\(Package\)属于特殊情况，所有字母小写。

## 3.5 变量和常量

### 3.5.1 变量

变量具有四个基本要素: 1. 名字； 2. 类型； 3. 值； 4. 作用域\(变量适用的f范围\)

在Java中，每个变量都有一个类型\(type\), 声明变量时变量的类型位于变量名之前。eg:

```
double salary; //语句需要用分号结尾
int x, y, z; //可以同时用分号声明多个变量，但不推荐使用
long earthPopulation; //尽量避免乱七八糟的的符号，不允许使用空格
```

声明变量后可以对变量进行初始化，即赋初值，没有赋值的变量不可以直接使用。

```
int vacationDays;
vacationDays = 12;

int vacationDays = 12; //建议变量的声明放在代码第一次使用的地方
```

与C/C++的区别:

在C和C++区分变量的声明和定义。  
`int i = 10;`  
是一个定义，而  
`extern int i；`  
是一个声明。在Java中，不区分变量的声明和定义。

### 3.5.2 常量

在Java中，利用关键字`final`指示常量。eg

```
public class Constants
{
    public static void main(String[] args)
    {
        final double CM_PER_INCH = 2.54; 
        //final表示这个变量只能被赋值一次，赋值后就不能再更改，习惯上常量名适用全大写
        double paperWidth = 8.5;
        double paperHeight = 11;
        System.out.println("Paper size in centimeter:"
            + paperWidth * CM_PER_INCH + " by " + paperHeight * CM_PER_INCH);
    }
}
```

* 如果希望某个常量可以在一个类的多个方法中使用，通常将这个常量称为类常量，使用`static final`设置，需要注意类常量的定义位于main方法的外部。
* 如果一个变量被声明为`public`,那么其他类的方法也可以使用这个常量。

## 3.6 数字类型转换

### 3.6.1 数值型不同类型数据的转换

数值型数据的类型转换分为隐含类型转换\(或缺省类型转换\)和强制类型转换。

#### 3.6.1.1 隐含类型转换

把占用比特数小的数据转换成占用比特数多的数据，由编译系统自动完成，不需要额外的说明。

需要满足的条件:

* 转换前的数据类型与转换后的数据类型兼容
* 转换后的数据类型的表示范围比转换前的类型大

转换数据的优先类型:

> \(低\)byte——short——char——int——long——float——double\(高\)

* 类型的转换只限于该语句本身，并不影响原先变量的类型定义。

* 在表达式中，为了避免溢出，会将数据进行"扩大转换"成相同的类型再进行计算。

#### 3.6.1.2 强制类型转换

强制类型转换\(cast\),也称显性转换。同样不影响原先变量的类型定义，但在转换过程中可能丢失部分信息。eg

```
double x = 9.997;
int nx = (int) x; //x is 9
int nx = (int) Math.round(x); //x is 10
```

与C/C++的区别:

不要在boolean类型与任何数值类型之间进行强制类型转换。

### 3.6.2 字符串型数据与整型数据相互转换

字符串转换成数据型数值的方法:

* Byte.parseByte\(String s\)
* Short.parseShort\(String s\)
* Integer.parseInt\(String s\)
* Long.parseLong\(String s\)
* Float.parseFloat\(String s\)
* Double.parseDouble\(String s\)
* Boolean.parseDouble\(String s\)

```
String myNumber = "1234.567";
float myFloat = Float.parseFloat(myNumber);
```

数值型数据转换成字符串:可用加号来实现自动转换

```
int myInt = 1234;
String myString = "" + myInt;
```

## 3.7 运算符

### 3.7.1 算术运算符

#### 3.7.1.1 二元算术运算符

* 两个整数之间做除法时，只保留整数部分而舍弃小数部分。
* 整数被0除将会产生一个异常，而浮点数被0除则会得到一个无穷大或NaN。
* 对于去模运算符`%`，其操作数可以是浮点数，此时`a % b`与`a - ((int)( a / b) * b`的语义相同，结果为除完后的浮点数部分。
* 加运算符`+`可以连接字符串，如`"abc" + "de"`可以得到`"abcde"`

#### 3.7.1.2 一元算术运算符

| 算术符 | 功能 | 实例 |
| --- | --- | --- |
| + | 正值 | +a |
| - | 负值 | -a |
| ++ | 加一 | ++a或a++ |
| -- | 减一 | --a或a-- |

在表达式中使用自增与自减运算符时，前缀形式会先对操作数进行加一或减一运算后，再将结果用于表达式的操作；后缀形式则先进行表达式的操作后再对操作数进行加一或减一运算。

```
int m = 7;
int n = 7;
int a = x * ++m; // now a is 16, m is 8
int b = x * n++; // now b is 14, n is 8
```

**建议**: 不要在表达式中使用++和--来自找麻烦。

### 3.7.2 关系和boolean运算符

关系运算符有`>、<、<=、>=、==、!=`。注意不能在浮点数之间做`==`的比较，因为浮点数在表达上面难免有细小的误差存在，无法实现精确的相等。

逻辑运算符如下表所示:

| 运算符 | 功能 | 实例 | 运算规则 |
| --- | --- | --- | --- |
| & | 逻辑与 | a & b | 两个操作数均为true时，结果才为true |
| \| | 逻辑或 | a \| b | 两个操作数均为false时，结果才为false |
| ！ | 逻辑非 | !a | 将操作数取反 |
| ^ | 异或 | a ^ b | 两个操作数同真或同减时，结果才为false |
| && | 简洁与 | a && b | 两个操作数均为true时，结果才为true |
| \|\| | 简洁或 | a \|\| b | 两个操作数均为false时，结果才为false |

简洁运算与非简洁运算的区别在于：非简洁运算必须计算完左右两个表达式后才取结果，而简洁运算可能只取左边的表达式而不计算右边的表达式。

* 对于`&&`，只要左边表达式为`false`，就不计算右边表达式，结果为`false`
* 对于`||`，只要左边表达式为`true`，就不计算右边表达式，结果为`true`

推荐使用简洁运算

Java支持三元操作符`?:`。

```
condition ? expressionTrue : expressionFalse
```

### 3.7.3 位运算符

位置运算符是对操作数以二进制比特位为单位进行的操作和运算，Java语言提供了如下的位运算符：

| 运算符 | 功能 | 示例 | 运算规则 |
| --- | --- | --- | --- |
| ~ | 按位取反 | ~a | 将ａ按比特位取反 |
| & | 按位与 | a & b | 将a按比特位相与 |
| \| | 按位或 | a \| b | 将a按比特位相或 |
| ^ | 按位异或 | a ^ b | 将a按比特位相异或 |
| &gt;&gt; | 右移 | a &gt;&gt; b | 将a各比特位右移b位 |
| &lt;&lt; | 左移 | a &lt;&lt; b | 将a各比特位左移b位 |
| &gt;&gt;&gt; | 0填充右移 | a &gt;&gt;&gt; b | 将a各比特位右移b位,左边的空位一律为0 |

与C/C++的区别:

在C/C++中，不能保证&gt;&gt;是完成算术移位还是逻辑移位，Java则消除了这种不确型性。

### 3.7.4 赋值运算符

1. 赋值运算符可以形成连续赋值的情况，从右边开始执行。
2. 扩展赋值运算符
   * 注意: 如果运算符得到一个值，其类型与左侧操作数的类型不同，就会发生强制转换。

eg

```
int x
x += 3.5 //the same as (int)(x + 3.5)
```

### 3.7.5 运算符优先级

运算符优先级

| 运算符 | 结合性 |
| --- | --- |
| \[\] .\(\)\(方法调用\) | 左--右 |
| ! ~ ++ -- +\(一元运算\) -\(一元运算\) \(\)\(强制类型转换\) new | 右--左 |
| \* / % | 左--右 |
| + - | 左--右 |
| &lt;&lt; &gt;&gt; &gt;&gt;&gt; | 左--右 |
| &lt; &lt;= &gt; &gt;= instanceof | 左--右 |
| == != | 左--右 |
| & | 左--右 |
| ^ | 左--右 |
| \| | 左--右 |
| && | 左--右 |
| \|\| | 左--右 |
| ?: | 右--左 |
| = += -= \*= /= %= \|= ^= &lt;&lt;= &gt;&gt;= &gt;&gt;&gt;= | 右--左 |

## 3.8 枚举类型

自定义枚举类型:控制变量的取值在一个有限的集合内。  
eg

```
enum Size { SMALL, MEDIUM, LARGE, EXTRA_LARGE};
Size s = Size.MEDIUM; //Size变量这能存储这个类型声明中给定的某个枚举值或者null
// null表示这个变量没有设置任何值
```

## 3.9 字符串

Java没有内置的字符串类型，而是在标准Java类库中提供了一个预定义类叫做String。用括号括起来的字符串都是String类的一个实例。

### 3.9.1 创建字符串的三种格式

```
//格式一：
String s; //声明字符串引用变量s，此时s的值为null
s = new String("hello"); //在堆内存中分配空间，并将s指向该字符串首地址

//格式二：
String s = new String("hello"); //格式一的合并

//格式三：
String s = "hello"; //直接为新建的String对象赋值，在声明变量时直接初始化

//推荐使用格式三
```

### 3.9.2 字符串的几种常用方法

```
// 子串
String greeting = "Hello";
String s = greeting.substring(0, 3);

// 拼接
String expletive = "Expletive";
String PG13 = "deleted";
String message = expletetive + PG13;

//多个字符串拼接
String all = String.join("/", "S", "M", "L", "XL");//all is the string"S/M/L/ML"

//检测字符串是否相等
"Hello".equals(greeting);
"Hello".equalsIgnoreCase("hello"); //不区分大小写
//不要使用 == 来检测两个字符串是否相等，这个运算只能确定它们是否放置在同一位置上
```

与C/C++的区别:

Java不能直接修改字符串，String类对象称为不可变字符串，大致类似于C中的char\*指针，而非字符型数组。

### 3.9.3 空串和null串

* 空串`""`是长度为0的字符串，是一个Java对象，有自己的串长度\(0\)和内容\(空\)
* String变量还可以存放一个特殊的值，名为null，这表示目前没有任何对象与该变量关联。

```
//检验字符串是否为空
if (str.length() == 0)
if (str.equals(""))

//检查一个字符串是否为null
if (str == null)
```

## 3.10 输入输出

### 3.10.1 读取输入

读取"标准输入流"System.in需要构造一个Scanner对象，并与"标准输入流"System.in"相关联。eg

```
import java.util.*;
public class LinputTest {
    public static void main(String[] args)
    {
        String s1, s2;
        Scanner reader = new Scanner(System.in);
        System.out.print("the frist number： ");
        s1 =  reader.nextLine(); 
        System.out.print("the second number： ");
        s2 = reader.nextLine();
        System.out.println("what you input is " + s1 + " and " + s2);
    }
}
```

注意:

* `next()`方法会自动去掉输入有效符号之前的空格键、Tab键和回车键等，只有输入有效符号之后才会对将它们视为分隔符或结束符。
* `nextLine()`方法将读取一整行的所有字符，直到遇到作为结束符的回车键。
* `HasNextXXX()`方法将会判断用户输入是否是相应类型的数据，`nextXXX()`方法则对输入进行读取。
* `Scanner`类定义在java.util包中，需要使用`import`加载。

要想读取一个密码，可以采用以下的示例

```
Console cons = System.console();
String username = cons.readLine("User name: ");
char[] passed = cons.readPassword("Password: ");
```

### 3.10.2 格式化输出

使用`printf`方法。eg:

```
double x = 1000.0 / 3.0;
// 8个字符的宽度和小数点后两个字符的精度打印x
System.out.printf("%8.2f", a);
```

用于`printf`的转换符

| 转换符 | 类型 | 举例 |
| --- | --- | --- |
| d | 十进制整位 | 159 |
| x | 十六进制整位 | 9f |
| o | 八进制整位 | 237 |
| f | 定点浮点数 | 15.9 |
| e | 指数浮点数 | 1.59e+01 |
| s | 字符串 | Hello |
| c | 字符 | H |
| b | 布尔 | True |
| tx或Tx | 日期时间 | 已过时，应使用java.time类 |
| % | 百分号 | % |

### 3.10.3 文件输入与输出

```
// 读取文件
Scanner in = new Scanner(path.get("myfile.txt"), "UTF-8"); //指定使用UTF-8字符编码

// 写入文件
PrintWriter out = new PrintWriter("myfile.txt", "UTF-8"); //如果文件不存在，则创建它

//注意：相对路径的从Java虚拟机启动的当前路径出发的，可以通过一下调用找到路径位置
String dir = System.getProperty("user.dir");

//如果觉得定位路径麻烦的话可以采用绝对路径

// 用一个不存在的文件构造一个Scanner或用一个不能被创建的文件名构造PrintWriter会发生异常。

// 我们需要告知编译器：已经知道有可能出现"输入/输出"异常。这需要在main方法中用throws字句标记
public static void main(String[] args) throws IOException
{
    Scanner in = new Scanner(path.get("myfile.txt"), "UTF-8")
    ...
}
```

## 3.11 控制流程

与C/C++的区别:

* Java没有`goto`语句，但支持带标签的`break`语句。
* 不能在嵌套的两个块\(符合语句\)声明同名变量。
* Java中的`if()`和`else if()`语句中条件表达式的奇偶过必须是逻辑型量。
* `do-while`语句先执行循环体代码块再检测循环条件。
* 如果希望在for语句之外使用循环计数器的最终值，就要确保这个变量在循环语句前面并且在外部声明!

```
for (int i = 1; i < 10; i++)
{
    ...
}
// i no longer defined here

int i;
for (i = 1; i < 10; i++)
{
    ...
}
// i is stil here
```

* `switch`语句与C/C++完全一样,如果没有使用`break`语句，将实现不同判断语句流入相同分支的效果。
* `switch`的case标签可以是：1. char、byte、short或int的常量表达式；2. 枚举常量\(不需要指明枚举名\)； 2. 字符串字面量
* 不推荐使用`switch`语句

```
import java.util.*;
public class LinputTest
{
    public static void main(String[] args)
    {
        int month, day;
        Scanner reader = new Scanner(System.in);
        System.out.println("请输入月份");
        month = reader.nextInt();
        switch (month)
        {
            case 2:
                day = 28;
                break;
            case 4:
            case 6:
            case 9:
            case 11:
                day = 30;
                break;
            default:
                day = 31;
                break;
        }
        System.out.println(month + "月份有" + day + "天");
        reader.close();
    }
}
```

* 与C++不同，Java提供了一种带标签的`break`语句来跳出多重嵌套的循环语句，注意标签必须紧跟一个冒号
* 注意区分`break`和`continue`语句，如果容易混淆的话则尽量不使用这两个语句。

## 3.12 大数值

`java.math`包中的两个常用类：`BigInteger`和`BigDecimal`可以处理任意精度的整数运算和浮点数运算

* 使用静态的`valueOf`方法将普通数值转换成大数值：`BigInterger a = BigInterger.valueOf(100);`
* 不能使用人们熟悉的算术运算符来处理大数值。
* 与C++不同，Java没有提供运算符重载功能。

```
BigInteger c = a.add(b); // c = a + b
BigInteger d = c.multiply(b.add(BigInteger.valueOf(2))); // d = c * (b + 2)
```

## 3.13 数组

### 3.13.1 基本概念

数组是用来存储相同数据类型值的集合，通过整型下标可以访问数值中的每一个元素。

### 3.12.2 内存分配

Java语言的内存分为栈内存和堆内存

* 栈内存: 在方法中定义的一些基本类型的变量和对象的引用变量都在方法的栈内存中分配。
  * 当在一段代码块定义一个变量时，Java就在栈内存中为这个变量分配内存空间
  * 超出变量的作用域后，Java会自动释放掉为该变量所分配的内存空间
* 堆内存： 用来存放由`new`运算符创建的对象和数组，在堆中分配的内存由Java虚拟机的自动垃圾回收器来管理
  * 在堆中创建一个数组或对象后，同时还在栈中定义一个特殊的变量，该变量的取值等于数组或对象在堆内存的首地址，即该变量就成了数组或对象的引用变量
  * 引用变量在运行到其作用域外后被释放，而数组或对象本身还在堆内存中没有被释放；
  * 数组或对象没有引用变量指向它时，会变成垃圾，无法再被使用，但仍占用内存空间，在随后一个不确定的时间被垃圾回收器释放掉，这也是Java比较占内存的原因

### 3.12.3 一维数组

声明数组

```
int[] a = new int[100]; //创建一个可以存储100个整数的数组
int a[]; //也可以这样声明数组，但推荐上面的风格

int* a = new int[100]; //C++,与上述语句等同
```

1. 创建一个数字数组时，所有元素都初始化为0；
2. `boolean`数组的元素都初始化为`false`；
3. 对象数组的元素都初始化为`null`。如果希望这个数组包含空串，可以使用`for (int i = 0; i < a.length; i++) 数组名[i] = "";`
4. 一旦创建了数组，就不能再改变数组的大小了\(但可以改变每一个数组元素\)，如果需要在运行过程中扩展数组的大小，则应该使用“数组列表”\(array list\)。
5. 利用`new`运算符为数组元素分配内存空间的方式称为动态内存分配方式

一维数组的初始化

```
int[] smallPrice = {2, 3, 4, 5, 6}; // 不需要调用new运算符
new int[] {2, 3, 4, 5, 6}; // 创建匿名数组
smallPrice = new int[] {7, 8, 9, 10, 11, 12}; // 在不创建新变量的情况下重新初始化smallPrice

new elementType[0]; //创建长度为0的数组
```

数组拷贝

```
int[] luckyNumbers = smallPrimes; // 两个变量引用同一个数组
LuckyNumbers[5] = 12; // now smallPrimes[5] is also 12
int[] copiedLuckyNumbers = Arrays.copyOf(luckyNumbers, 2  * luckyNumbers.length); // 第二个参数是新数组的长度，常用于扩展新数组
```

命令行参数

每一个Java应用参数都有一个带`String arg[]`参数的`main`方法。这个参数表示`main`方法将接收一个字符串数组，也就是命令行参数

与C/C++的区别: 在Java应用程序的`main`方法中，程序名并没有存储在`args`数组中。

```
public class Example3_12_3
{
    public static void main(String[] args)
    {
        if (args.length == 0 || args[0].equals("-h"))
            System.out.print("Hello, ");
        else if(args[0].equals("-g"))
            System.out.print("Googbye, ");
        for (int i = 1; i < args.length; i++)
            System.out.print("" + args[i] + " ");
        System.out.println("!");
    }
}
```

### 3.12.4 `foreach` 语句

```
// 格式
for (type element: collection) statement

// 例子
int[] a = {1, 2, 3, 4, 5, 6};
for (int element: a)
    System.out.println(element);

// 等同于for循环
for (int i = 0; i < a.lenth; i++)
    System.out.println(a[i]);

// 简单打印数组的所有值
System.out.println(Arrays.toSprint(a));
```

### 3.12.5 多维数组

我们常用二维数组\(也称矩阵\)来存储表格信息

```
//创建二维数组
int[][] a; //声明矩阵
a = new int[3][4] //分配一块内存空间，供3行4列的整型数组a使用

//不调用new来初始化
int[][] magicSquare = 
    {
        {16, 3, 2, 13}, 
        {5, 10, 11, 8}, 
        {9, 6, 7, 12},
        {4, 15, 14, 1}
    }

//创建不规则矩阵
int[][] x;
x = new int[3][]; //矩阵行数为3
x[0] = new int[3]; //第一行列数为3，此时x[0]是数组的引用变量
x[1] = new int[2]; //第二行列数为2

//多维数组
int[][][] b; //声明三维数组，其他数组依此类推

//快速打印多维数组的元素列表
System.out.println(Arrays.deepTostring(magicSquare));
```



