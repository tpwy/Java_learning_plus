# 第十章 JAVA语言的输入输出和文件处理

## 10.1 Java语言的输入输出类库

+ Java语言的输入输出功能必须借助输入输出包java.io来实现
+ Java开发环境提供了丰富的流类，完成从基本的输入输出到文件操作

### 10.1.1 流的概念

+ 流(Stream)是指计算机中各部件之间的数据流动
+ 根据数据的流动方向，可分为输入流和输出流
+ 根据流的内容划分，可分为字节流和字符流
+ Java中流的数据既可以是未经加工的原始二进制数据，也可以是经过一定编码处理后符合某种格式规定的特定数据
+ 流是由为(Bit)或字符(Character)所构成的序列
+ 用户可以通过流来读写数据、连接数据源，并可以将数据以字符或位组合的形式保存

#### 10.1.1.1 输入输出流

+ 在Java语言中，把不同类型的输入输出源抽象为流，其中输入输出的数据称为数据流(Data Stream)，用统一的方式来表示
+ 将数据从外设或外存传递到应用程序的流称为输入流(Input Stream)——只能读取数据而不能写入数据
+ 将数据从应用程序传递到外设或外存的流称为输出流(Output Stream)——只能写入数据而不能读取数据
+ 流式输入输出的最大特点是数据的获取和发送是沿着数据序列顺序进行，不能随意选择输入输出的位置

#### 10.1.1.2 缓冲流

+ 为了提高数据传输的效率，通常使用缓冲流(Buffered Stream)，即为一个流配有一个缓冲区(Buffer)，缓冲区是专门用于传送数据的一块内存
+ 向缓冲流写入数据时：系统将数据发送到缓冲区，缓冲区自动记录数据，当缓冲区满时，系统将数据全部发送到相应的外部设备
+ 从缓冲流读取数据时：系统冲缓冲区中读取数据，当缓冲区空时，系统从相关外部设备读取数据，尽可能将缓冲区填满

### 10.1.2 输入输出类库

+ 字节流每次读写8位二进制数，并且只能根据以二进制的原始方式读写，因此字节流又被称为二进制字节流(Binary Byte Stream)或位流(Bit Stream)
+ 字符流一次读写16位二进制数，并将其作为一个字符来处理
+ 字符流的源或目标通常是文本文件，可以实现Java程序中的内部格式与文本文件、显示输出、键盘输入等外部格式之间的转换
+ 对于含有非字符数据的必须用字节流来输入输出
+ InputStream、OutputStream、Reader和Writer是抽象类，不会直接使用。通常是根据这些类派生的子类来对文件处理

## 10.2 使用InputStream和OutputStream流类

InputStream和OutputStream类用于处理以位为单位的流

### 10.2.1 基本的输入输出流

#### InputStream流类

常用方法

| 方法 | 功能说明 |
|-|-|
| public int read() | 从输入流中的当前位置读取一个字节(8bit)的二进制数据，然后以此数据为低位字节，配上8个全0的高位字节合成一个16位的整形量(0~255)返回给调用此方法的语句，如输入流中的当前位置没有数据，则返回-1 |
| public int read(byte[] b) | 从输入流中的当前位置连续读入多个字节保存在数组b中，同时返回所读取到的字节数 |
| public int read(byte[] b, int off, int len) | 从输入流中的当前位置连续输入len个字节，从数组b的第off+1个元素位置开始存放，同时返回所读取到的字节数 |
| public int available() | 返回输入流中可以读取的字节数 |
| public long skip(long n) | 是位置指针从当前位置向后跳过n个字节 |
| public void mark(int readlimit) | 从当前位置处做一个标记，并且在输入流中读取readlimit个字节数，后该标记失效 |
| public void reset() | 将指针位置返回到标记的位置 |
| public void close() | 关闭输入流与外设的连接并释放所占用的系统资源 |

注意：流中的方法都声明抛出异常，所以程序中调用流方法时必须处理异常，否则编译无法通过

#### OutputStream 流类

| 方法 | 功能说明 |
|-|-|
| public void write(int b) | 将参数b的低位字节写入到输出流中 |
| public void write(byte[] b) | 将字节数组b中的全部字节按顺序写入到输出结果 |
| public void write(byte[] b, int offf, int len) | 
| public void flush() | 强制清空缓冲区并执行向外设写操作 |
| public void close() | 

### 10.2.2 输入输出流的应用

#### 10.2.2.1 文件输入输出流

+ FileInputStream和FileOutputStream主要负责完成对本地磁盘文件的顺序输入与输出操作的流
+ 在生成FileInputStream类的对象时，若找不到指定的文件，则抛出FileNotFoundException异常，该异常必须捕获或声明抛出
+ 在生成FileOutputStream类的对象时，若指定的文件不存在，则创建一个新的文件；若文件已存在，则清楚原文件的内容进行覆盖
+ 在进行文件的读写操作时会产生IOException异常，该异常必须捕获或声明抛出

FileInputStream类的构造方法

| 构造方法 | 功能说明 |
|-|-|
| public FileInputStream(String name) | 以名为name的文件为数据源建立文件输入流 |
| public FileInputStream(File file) | 以指定名字的文件对象file为数据源建立文件输入流 |
| public FileInputStream(FileDescriptor fdObj) | 根据文件描述对象fdObj为输入端建立一个文件夹输入流 |

FileOutputStream类的构造方法

| 构造方法 | 功能说明 |
|-|-|
| public FileOutputStream(String name) |
| public FileOutputStream(String name, boolean append) | 以指定名字的文件为接收端建立文件输出流，并必定写入方法，append为true是输出字节被写入文件的末尾 |
| public FileOutputStream(FileDescriptor fdObj) |

+ File是在Java.io包中定义的一个类，每个File类对象表示一个磁盘文件或文件夹，其对象属性包含了文件或文件夹的相关信息，可以通过调用方法来获得
+ FileDescriptor类无法实例化，拥有三个静态成员:in、out和err，分别对应标准输入流、标准输出流和标准错误流，利用它们可以在标准输入流和标准输出流上建立文件输入输出流，实现键盘输入或屏幕输出操作

#### 10.2.2.2 顺序输入流

+ 顺序输入流类SequenceInputStream是InputStream的直接子类，可以将多个输入流顺序连接在一起，形成单一的输入数据流，没有对应的输出数据流。
+ 在进行输入时，顺序输入流一次打开每个输入流并读取数据，在读完完毕后将流关闭然后自动切换到下一个输入流

SequenceInputStream类的构造方法

| 构造方法 | 功能说明 |
|-|-|
| public SequenceInputStream(Enumeration e) | 创建一个串行输入流，连接枚举对象e中的所有输入流 |
| public SequenceInputStream(InputStream s1, InputStream s2) | 创建一个串行输入流，连接输入流s1和s2 |

SequenceInputStream类的常用方法

| 方法 | 功能说明 |
|-|-|
| public int available() |
| public void close() |
| public int read() |
| public int read(byte[] b, int off, int len) |

#### 10.2.2.3 管道输入输出流

+ 管道字节输入流PipedInputStream和管道字节输出流PipedOutputStream类提供了利用管道方式进行数据输入输出管理的类
+ 管道流用于将一个应用程序或线程的输出连接到另一个程序或线程作为输入，使得相连线程能够通过PipedInputStream和PipedOutputStream类进行数据交换
+ 这两个类必须结合使用，管道输入流作为管道的接收端，管道输出流作为管道的发送端。
+ 管道输入输出流的两种连接方法
    1. 在构造方法中给出对应的管道流，在创建对象时进行连接
    2. 利用管道字节输入输出流提供的connect()方法进行连接

PipedInputStream类的常用方法

| 方法 | 功能说明 |
|-|-|
| public int available() |
| public void close() |
| public int read() |
| public int read(bytep[] b, int off, int len) |
| protected void receive(int b) |
| public void connect(PipedOutputStream src) |

PipedOutputStream类的常用方法

| 方法 | 功能说明 |
|-|-|
| public void close() |
| public void connect(PipedInputStream snk) |
| public void write(int b) |
| public void write(byte[] b, int off, int len) |
| public void flush() |

#### 10.2.2.4 过滤输入输出流

+ 过滤字节输入流类FilterInputStream和过滤字节输出流类FilterOutputStream分别实现了在数据读、写操作的同时进行数据处理
+ 在输入输出数据的同时能够对所传输的数据做指定类型或格式的转换，即可实现对二进制字节数据的理解和编码转换
+ FilterInputStream和FilterOutputStream也是两个抽象类，它们分别派生出数据输入流类DataInputStream和数据输出流类DataOutputStream
+ Java语言中安装基本数据类型的进行读写的就是这两个类，每次可以直接读取一个int(4字节)型数据
+ 数据输入流类和数据输出流类的构造方法
    + DataInputStream(InputStream in)——建立一个新的数据输入流，从指定的输入流in读数据
    + DataOutputStream(OutputStram out)——建立一个新的数据输出流，向指定的输出流out写数据

DataInputStream类的常用方法

| 方法 | 功能说明 |
|-|-|
| public boolean readBoolean() |
| public byte readByte() |
| public char readChar() |
| public short readShort() |
| public int readInt() |
| public float readFloat() |
| public long readLong() |
| public double readDouble() |

DataOutputStream类的常用方法

| 方法 | 功能说明 |
|-|-|
| public boolean writeBoolean() |
| public byte writeByte() |
| public char writeChar() |
| public short writeShort() |
| public int writeInt() |
| public float writeFloat() |
| public long writeLong() |
| public double writeDouble() | 

#### 10.2.2.5 标准输入输出

+ 对一般的计算机系统，标准输入设备通常指键盘，标准输出设备通常指屏幕显示器
+ Java系统事先在System类中定义了静态的流对象System.in、System.out和System.err
+ System.in是BufferedInputStream类的对象
+ System.out是PrintStream类的对象，是过滤字节输出流类FilterOutputStream类的子类
+ System.err也是派生于PrintStream,用于为用户显示错误信息

### 10.3 使用Reader和Writer流类

Reader类的方法

| 方法 | 功能说明 |
|-|-|
| public int read() | 从输入流中读一个字符 |
| public int read(char[] cbuf) | 从输入流中读最多cbuf.length个字符并存入字符数组cbuf中 |
| public int read(char[] cbuffer, int off, int len) | 从输入流中读最多len个字符，存入字符数组cbuffer中从off开始的位置 |
| public long skip(long n) | 从输入流中最多向后跳n个字符 |
| public boolean ready() | 判断流是否做好读的准备 |
| public void mark(int readAheadLimit) | 标记流当前的位置 |
| public boolean markSupported | 测试输入流是否直接mark |
| public void reset() | 重定位输入流 |
| public void close() | 关闭输入流 |

Writer类的方法

| 方法 | 功能说明 |
|-|-|
| public void write(int c) | 将单一字符c输出到流中 |
| public void write(String str) | 将字符串str输出到流中 |
| public void write(char[] cbuf) | 将字符数组cbuf输出到流 |
| public void write(char[] cbuf, int off, int len) | 将字符数组按制定的格式输出(off表示索引，len表示写入的字符数)到流中 |
| public void flush() | 将缓冲区的数据写到文件中 |
| public void close() | 关闭输出流 |

### 10.3.1 使用FileReader类和FileWriter类

FileReader类的构造方法

| 构造方法 | 功能说明 |
|-|-|
| public FileReader(String name) |

FileReader类的构造方法

| 构造方法 | 功能说明 |
|-|-|
| public FileWriter(String fileanme) |根据所给文件名创建一个可供写入字符数据的输出流对象，原先的文件将被覆盖 |
| public FileWriter(String filename, boolean a) | 如果a设置为true，则会将数据追加在原先文件的后面 |

### 10.3.2 使用BufferedReader类和BufferedWriter类

BufferedReader类的构造方法

| 构造方法 | 功能说明 |
|-|-|
| public BufferedReader(Reader in) |
| public BufferedReader(Reader in, int size) | 创建缓冲区字符输入流，并设置缓冲区大小 |

BufferedReader类的常用方法

| 方法 | 功能说明 |
|-|-|
| public int read() |
| public int read(char[] cbuf) |
| public int read(char[] cbuf, int off, int len) |
| public long skip(long n) |
| public String readLine() | 读取一行字符串 |
| public void close() |

使用BufferedWriter类，缓冲区中的数据在最后必须用flush()方法清空

BufferedWriter类的构造方法

| 方法 | 功能说明 |
|-|-|
| public BufferedWriter(Writer out) |
| public BufferedWriter(Writer out, int size) |

BufferedWriter类的常用方法

| 方法 | 功能说明 |
|-|-|
| public void write(int  c) |
| public void write(char[] cbuf, int off, int len) |
| public void write(String str, int off, int len) |
| public void newLine() |
| public void flush() |
| public void closr() |

## 10.4 文件的处理和随机方法

### 10.4.1 Java语言对文件和文件夹的管理

#### 10.4.1.1 创建File类对象

三种构造方法

| 构造方法 | 功能说明 |
|-|-|
| public File(String path) | 用path参数创建File对象所对应的磁盘文件名或文件夹名及其路径 |
| public File(String path, String name) | 以path为路径，name为文件或文件夹名创建File对象 |
| public File(File dir, String name) | 用一个已经存在代表某磁盘文件夹的File对象dir作为文件夹，以name作为文件或文件夹名来创建对象 |

+ path可以是绝对路径、也可以是相对路径
+ 由于不同系统使用的文件夹分隔符不同，为了兼容不同平台，可以利用静态变量File.separator来做文件夹分隔符

```
"d:" + File.separator + "java" + File.separator  + "myfile"
```

#### 10.4.1.2 获取文件或文件夹属性

| 方法 | 功能说明 |
|-|-|
| public boolean exists() |
| public boolean isFile() |
| public boolean isDirectory() |
| public String getName() |
| public String getPath() |
| public long length() | 返回文件的字节数 |
| public boolean canRead() |
| public boolean canWrite() |
| public String[] list() | 将文件夹中所有文件名保存在字符创数组中返回 |
| public boolean equals(File f) |

#### 10.4.1.3 文件或文件夹操作

| 方法 | 功能说明 |
|-|-|
| public boolean renameTo(File newFile) |
| public boolean delete() |
| public boolean mkdir() |

### 10.4.2 对文件的随机访问

+ 前面介绍的流类实现的是对磁盘文件的顺序读写，并且读和写都要分别创建不同的对象
+ 随机访问文件类RandomAccessFile也是java.io包中定义的，用于任意位置、任意类型的文件访问，并且在文件读取方法中支持文件的任意读取而不只是顺序读取

RandomAccessFile类的构造方法

| 构造方法 | 功能说明 |
|-|-|
| public RandomAccessFile(String name, String mode) | 以name来指定随机文件流对象所对应的文件名，以mode来表示对文件的访问模式 |
| public RandomAccessFile(File file, String mode) |

说明：

访问模式mode表示所创建的随机读写文件的操作状态，mode的取值如下：
1. r——表示以只读方式打开文件
2. rw——表示以读写方式打开文件，可以同时实现读和写两种操作

RandomAccessFile类中用于读取操作的常用方法

| 方法 | 构造方法 |
|-|-|
| public void close() |
| public final FileDescripter getFD() | 获取文件描述符 |
| public long getFilePointer() | 返回文件指针的当前位置 |
| public long length() |
| public int skipBytes(int n) |
| public int read() |
| public int read(byte[] b, int off, int len) |
| public final void readFully(byte[] b) | 遇到文件结束符，则抛出EOFException异常 |
| public final void readFilly(byte[] b, int off, int len) |
| public final boolean readBoolean() |
| public final byte readByte() |
| public final char readChar() | 
| public final String readLine() |
| public void seek(long pos) | 设置文件指针位置｜

RandomAccessFile类中用于写入操作的常用方法

| 方法 | 构造方法 |
|-|-|
| public void write(int b) |
| public void writeBoolean(boolean v) |
| public void writeByte(int v) |
| public void writeBytes(String s) |
| public void writeChar(String s) |
| public void writeDouble(double v) |
| public void writeFloat(float v) |
| public void writeInt(int v) |
| public void writeLone(long v) |
| public void writeShort(int v) |
| public void writeUTF(String str) |