# 第二章 Java语言开发环境

## 2.1 Java开发工具

+ Java开发工具JDK(Java Development Kit)由Java API、Java运行环境和一组建立、测试工具的Java实用程序组成，核心是Java API。
+ Java API(Application Programming Interface)是Java提供的标准类库供编程人员使用，开发人员需要用这些类来实现Java语言的功能。
+ Java API包括一些重要的语言结构以及基本图形、网络和文件I/O等。
+ 作为JDK的使用程序，工具库中的主要程序都放在JDK安装文件夹，子文件夹的内容如下：
    + bin————存放几个常用的命令程序
    + db————包含Apache Derby 数据库等开放资源，支持JDBC4.0规范
    + include————存放与C程序相关的头文件
    + jre————存放Java运行环境(Java Runtime Environment,  JRE)相关的文件
    + lib————存放Java类库
+ bin子文件夹中包含的几个常用命令:
    + javac
    
        Java编译器，将Java源代码文件转换成字节码文件
    + java
    
        Java解释器，执行Java程序的字节码文件
    + apppletviewer
    
        小程序浏览器，执行嵌入在HTML文件中的Java小程序的Java浏览器
    + javadoc
    
        根据Java源代码及说明语句生成Java程序的HTML格式的帮助文档
    + jdb
    
        Java调试器，可以逐行执行程序、设置断点和检查变量

    + jar
    
        创建扩展名为.jar(Java Archieve,Java归档)的压缩文件，与zip压缩文件格式相同

### 2.1.1 JDK的下载与安装

### Windows

1. 下载JDK
    32位选择-i586.exe，64位选择-x64.exe

2. 安装JDK
    一路Next即可

3. 设置JDK的环境变量
    + 系统变量Path:`C:\\Program File\xxx\bin;`
    + 类路径ClassPath:`.;C:\\Program File\xxx\lib\tools.jar`

### Linux

+ tar.gz包解压，export环境变量
+ `pacman -S jdk`

### 2.1.2 安装库源文件和文档

+ JDK安装目录下有`src.zip`压缩包，其中包含了所有公共类库的源代码。
+ JDK帮助文档需要在Oracle官网上独立下载，解压即可使用。
+ 《Java核心技术 卷I》的程序示例在`http://horstmann.com/corejava`下，自行下载解压使用。

## 2.2 JDK的使用

### 2.2.1 使用命令行工具编译和运行代码

以下内容均在Linux系统下进行

```
mkdir ~/java_exercise
cd ~/java_exercise
vim Welcome.java //这里我直接用vscode来写

/**
 * This program displays a greeting for the reader.
 * @version 1.0 2017-7-4
 * @author qingqi
 */
public class Welcome
{
    public static void main(String[] args)
    {
        String greeting = "Welcome to Core Java!";
	System.out.println(greeting);
	for (int i = 0; i < greeting.length(); i++)
	    System.out.print("=");
	System.out.println();
    }
}

javac Welcome.java
java Welcome

[Output]

Welcome to Core Java!
=====================
```

### 2.2.2 使用集成开发环境

推荐使用全平台支持的Eclipse，下面是用Eclipse编写程序的一般步骤:

1. 启动Eclipse，从menu中选择File>New>Project。
2. 从向导对话框中选择Java Project。
3. 点击Next（用不用default location看你自己设置了，这里我们选择上文创建的Welcome.java所在的位置）。
4. 点击Finish按钮，工程创建完成。
5. 在工程窗口找到Welcome.java双击获得代码窗口。
6. 鼠标右键点击工程名（Welcome），选择Run>Run as application。程序输出会显示在控制台窗格。

### 2.2.3 运行图形化应用程序

这里我们选择教材示例

```
cd ./corejava/v1ch02/ImageViewer
javac ImageViewer.java
java ImageViewer

//ImageViewer将会在corejava的第10~12章介绍，这里就不po代码了(太长影响阅读)
```
### 2.2.4 构建并运行 applet

吐槽:现在的浏览器基本都不支持java applet，基本上就没啥学习的必要了，这里我就跳过了，等正式上课再好好看算了。