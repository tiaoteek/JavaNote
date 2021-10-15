# 注意：

1. 从实践的角度看，JVM的兼容性做得非常好，低版本的Java字节码完全可以正常运行在高版本的JVM上。
2. 严格区分大小写，还好，关键字都是小写



# 简介

```ascii
┌───────────────────────────┐
│Java EE                    │
│    ┌────────────────────┐ │
│    │Java SE             │ │
│    │    ┌─────────────┐ │ │
│    │    │   Java ME   │ │ │
│    │    └─────────────┘ │ │
│    └────────────────────┘ │
└───────────────────────────┘
```

因此我们推荐的Java学习路线图如下：

1. 首先要学习Java SE，掌握Java语言本身、Java核心开发技术以及Java标准库的使用；
2. 如果继续学习Java EE，那么Spring框架、数据库开发、分布式架构就是需要学习的；
3. 如果要学习大数据开发，那么Hadoop、Spark、Flink这些大数据平台就是需要学习的，他们都基于Java或Scala开发；
4. 如果想要学习移动开发，那么就深入Android平台，掌握Android App开发。

初学者学Java，经常听到JDK、JRE这些名词，它们到底是啥？

- JDK：Java Development Kit
- JRE：Java Runtime Environment

简单地说，JRE就是运行Java字节码的虚拟机。但是，如果只有Java源码，要编译成Java字节码，就需要JDK，因为JDK除了包含JRE，还提供了编译器、调试器等开发工具。

二者关系如下：

```ascii
  ┌─    ┌──────────────────────────────────┐
  │     │     Compiler, debugger, etc.     │
  │     └──────────────────────────────────┘
 JDK ┌─ ┌──────────────────────────────────┐
  │  │  │                                  │
  │ JRE │      JVM + Runtime Library       │
  │  │  │                                  │
  └─ └─ └──────────────────────────────────┘
        ┌───────┐┌───────┐┌───────┐┌───────┐
        │Windows││ Linux ││ macOS ││others │
        └───────┘└───────┘└───────┘└───────┘
```

- JSR规范：Java Specification Request	（JSR是一系列的规范，为了保证Java语言的规范性）
- JCP组织：Java Community Process       （负责审核JSR的组织就是JCP）

- RI：Reference Implementation             （通常来说只是一个“能跑”的正确的代码，不追求速度）
- TCK：Technology Compatibility Kit 

# java 入门

```java
public class Hello {
    public static void main(String[] args) {
        System.out.println("Hello, world!");
    }
}
```

`public`、`class`都是Java的关键字，必须小写，`Hello`是类的名字，按照习惯，首字母`H`要大写。而花括号`{}`中间则是类的定义。

Java规定，某个类定义的`public static void main(String[] args)`是Java程序的固定入口方法，因此，Java程序总是从`main`方法开始执行。

![img](https://www.runoob.com/wp-content/uploads/2013/12/662E827A-FA32-4464-B0BD-40087F429E98.jpg)

## 如何运行Java程序

Java源码本质上是一个文本文件，我们需要先用`javac`把`Hello.java`编译成字节码文件`Hello.class`，然后，用`java`命令执行这个字节码文件：

```ascii
┌──────────────────┐
│    Hello.java    │<─── source code
└──────────────────┘
          │ compile
          ▼
┌──────────────────┐
│   Hello.class    │<─── byte code
└──────────────────┘
          │ execute
          ▼
┌──────────────────┐
│    Run on JVM    │
└──────────────────┘
```

因此，可执行文件`javac`是编译器，而可执行文件`java`就是虚拟机。

第一步，在保存`Hello.java`的目录下执行命令`javac Hello.java`：

```bash
$ javac Hello.java
```

如果源代码无误，上述命令不会有任何输出，而当前目录下会产生一个`Hello.class`文件：

```bash
$ ls
Hello.class	Hello.java
```

第二步，执行`Hello.class`，使用命令`java Hello`：

```bash
$ java Hello
Hello, world!
```

注意：给虚拟机传递的参数`Hello`是我们定义的类名，虚拟机自动查找对应的class文件并执行。

有一些童鞋可能知道，直接运行`java Hello.java`也是可以的：

```bash
$ java Hello.java 
Hello, world!
```

这是Java 11新增的一个功能，它可以直接运行一个单文件源码！

## Java 文档注释

Java 支持三种注释方式。

单行注释： **//** 

多行注释： **/\* \*/**，

说明注释：它以`/**`开始，以`*/`结束。

说明注释允许你在程序中嵌入关于程序的信息。你可以使用 javadoc 工具软件来生成信息，并输出到HTML文件中。说明注释，使你更加方便的记录你的程序信息。

## 基本数据类型

基本数据类型是CPU可以直接进行运算的类型。Java定义了以下几种基本数据类型：

- 整数类型：byte，short，int，long
- 浮点数类型：float，double
- 字符类型：char
- 布尔类型：boolean

计算机内存的最小存储单元是字节（byte），一个字节就是一个8位二进制数，即8个bit。它的二进制表示范围从`00000000`~`11111111`，换算成十进制是0~255，换算成十六进制是`00`~`ff`。

```ascii
       ┌───┐
  byte │   │
       └───┘
       ┌───┬───┐
 short │   │   │
       └───┴───┘
       ┌───┬───┬───┬───┐
   int │   │   │   │   │
       └───┴───┴───┴───┘
       ┌───┬───┬───┬───┬───┬───┬───┬───┐
  long │   │   │   │   │   │   │   │   │
       └───┴───┴───┴───┴───┴───┴───┴───┘
       ┌───┬───┬───┬───┐
 float │   │   │   │   │
       └───┴───┴───┴───┘
       ┌───┬───┬───┬───┬───┬───┬───┬───┐
double │   │   │   │   │   │   │   │   │
       └───┴───┴───┴───┴───┴───┴───┴───┘
       ┌───┬───┐
  char │   │   │
       └───┴───┘
```

### 整型

对于整型类型，Java只定义了带符号的整型，因此，最高位的bit表示符号位（0表示正数，1表示负数）。各种整型能表示的最大范围如下：

- byte：-128 ~ 127
- short: -32768 ~ 32767
- int: -2147483648 ~ 2147483647
- long: -9223372036854775808 ~ 9223372036854775807

### 浮点型

浮点类型的数就是小数，因为小数用科学计数法表示的时候，小数点是可以“浮动”的，如1234.5可以表示成12.345x10^2，也可以表示成1.2345x10^3，所以称为浮点数。

下面是定义浮点数的例子：

```java
float f1 = 3.14f;
float f2 = 3.14e38f; // 科学计数法表示的3.14x10^38
double d = 1.79e308;
double d2 = -1.79e308;
double d3 = 4.9e-324; // 科学计数法表示的4.9x10^-324
```

对于`float`类型，需要加上`f`后缀。

浮点数可表示的范围非常大，`float`类型可最大表示3.4x10^38，而`double`类型可最大表示1.79x10^308。

布尔类型`boolean`只有`true`和`false`两个值，布尔类型总是关系运算的计算结果：

```java
boolean b1 = true;
boolean b2 = false;
boolean isGreater = 5 > 3; // 计算结果为true
int age = 12;
boolean isAdult = age >= 18; // 计算结果为false
```

Java语言对布尔类型的存储并没有做规定，因为理论上存储布尔类型只需要1 bit，但是通常JVM内部会把`boolean`表示为4字节整数。

### 字符类型

字符类型`char`表示**一个**字符。Java的`char`类型除了可表示标准的ASCII外，还可以表示一个Unicode字符：

```java
char a = 'A';
char zh = '中';
```

### 引用类型

除了上述基本类型的变量，剩下的都是引用类型。例如，引用类型最常用的就是`String`字符串：

```java
String s = "hello";
```

### 常量

定义变量的时候，如果加上`final`修饰符，这个变量就变成了常量：

```java
final double PI = 3.14; // PI是一个常量
```

常量在定义时进行初始化后就不可再次赋值，再次赋值会导致编译错误。

常量的作用是用有意义的变量名来避免魔术数字（Magic number），例如，不要在代码中到处写`3.14`，而是定义一个常量。如果将来需要提高计算精度，我们只需要在常量的定义处修改，例如，改成`3.1416`，而不必在所有地方替换`3.14`。

根据习惯，常量名通常全部大写。

### var关键字

有些时候，类型的名字太长，写起来比较麻烦。例如：

```java
StringBuilder sb = new StringBuilder();
var sb = new StringBuilder();
```

这个时候，如果想省略变量类型，可以使用`var`关键字，编译器会根据赋值语句自动推断出变量`sb`的类型是`StringBuilder`。

### 变量的作用范围

在Java中，多行语句用{ }括起来。很多控制语句，例如条件判断和循环，都以{ }作为它们自身的范围。

定义变量时，要遵循作用域最小化原则，尽量将变量定义在尽可能小的作用域，并且，不要重复使用变量名。

### 小结

Java提供了两种变量类型：基本类型和引用类型

基本类型包括整型，浮点型，布尔型，字符型。

变量可重新赋值，等号是赋值语句，不是数学意义的等号。

常量在初始化后不可重新赋值，使用常量便于理解程序意图。

### 字符和字符串

#### 字符类型

字符类型`char`是基本数据类型，它是`character`的缩写。一个`char`保存一个Unicode字符：

```java
char c1 = 'A';
char c2 = '中';
```

因为Java在内存中总是使用Unicode表示字符，所以，一个英文字符和一个中文字符都用一个`char`类型表示，它们都占用两个字节。要显示一个字符的Unicode编码，只需将`char`类型直接赋值给`int`类型即可：

```java
int n1 = 'A'; // 字母“A”的Unicodde编码是65
```

还可以直接用转义字符`\u`+Unicode编码来表示一个字符：

```java
// 注意是十六进制:
char c3 = '\u0041'; // 'A'，因为十六进制0041 = 十进制65
```

字符串类型

和`char`类型不同，字符串类型`String`是引用类型，我们用**双引号`"..."`表示字符串**。一个字符串可以存储0个到任意个字符：

```java
String s = ""; // 空字符串，包含0个字符
String s1 = "A"; // 包含一个字符
String s2 = "ABC"; // 包含3个字符
String s3 = "中文 ABC"; // 包含6个字符，其中有一个空格
```

因为字符串使用双引号`"..."`表示开始和结束，那如果字符串本身恰好包含一个`"`字符怎么表示？例如，`"abc"xyz"`，编译器就无法判断中间的引号究竟是字符串的一部分还是表示字符串结束。这个时候，我们需要借助转义字符`\`：

```java
String s = "abc\"xyz"; // 包含7个字符: a, b, c, ", x, y, z
```

因为`\`是转义字符，所以，两个`\\`表示一个`\`字符：

```java
String s = "abc\\xyz"; // 包含7个字符: a, b, c, \, x, y, z
```

常见的转义字符包括：

- `\"` 表示字符`"`
- `\'` 表示字符`'`
- `\\` 表示字符`\`
- `\n` 表示换行符
- `\r` 表示回车符
- `\t` 表示Tab
- `\u####` 表示一个Unicode编码的字符

例如：

```java
String s = "ABC\n\u4e2d\u6587"; // 包含6个字符: A, B, C, 换行符, 中, 文
```

#### 字符串连接

Java的编译器对字符串做了特殊照顾：（字符不行，“ ”是字符串，’ ‘ 是字符）

可以使用`+`连接任意字符串和其他数据类型，这样极大地方便了字符串的处理；

如果用`+`连接字符串和其他数据类型，会将其他数据类型先自动转型为字符串；

```java
String s1 = "Hello";
String s2 = "world";
String s = s1 + " " + s2 + "!";

int age = 25;
String s = "age is " + age;
```

#### 多行字符串

如果我们要表示多行字符串，使用+号连接会非常不方便：

```java
String s = "first line \n"
         + "second line \n"
         + "end";
```

从  **Java 13**  开始，字符串可以用`"""..."""`表示多行字符串（Text Blocks）了。

#### 不可变特性

Java的字符串除了是一个引用类型外，还有个重要特点，就是字符串不可变。

执行`String s = "hello";`时，JVM虚拟机先创建字符串`"hello"`，然后，把字符串变量`s`指向它：

```ascii
      s
      │
      ▼
┌───┬───────────┬───┐
│   │  "hello"  │   │
└───┴───────────┴───┘
```

紧接着，执行`s = "world";`时，JVM虚拟机先创建字符串`"world"`，然后，把字符串变量`s`指向它：

```ascii
      s ──────────────┐
                      │
                      ▼
┌───┬───────────┬───┬───────────┬───┐
│   │  "hello"  │   │  "world"  │   │
└───┴───────────┴───┴───────────┴───┘
```

原来的字符串`"hello"`还在，只是我们无法通过变量`s`访问它而已。因此，字符串的不可变是指字符串内容不可变。

#### 空值null

引用类型的变量可以指向一个空值`null`，它表示不存在，即该变量不指向任何对象。例如：

```java
String s1 = null; // s1是null
String s2; // 没有赋初值值，s2也是null
String s3 = s1; // s3也是null
String s4 = ""; // s4指向空字符串，不是null
```

注意要区分空值`null`和空字符串`""`，空字符串是一个有效的字符串对象，它不等于`null`。

练习：

请将一组int值视为字符的Unicode编码，然后将它们拼成一个字符串：

```java
    int a = 72;
    int b = 105;
    int c = 65281;
    String s = “”+(char)a +(char)b +(char)c;
```

注：这里拼成字符串，需要用“ ”进行转换，char 只能转化成字符。string 只是指向。

#### 小结

Java的字符类型`char`是基本类型，字符串类型`String`是引用类型；

基本类型的变量是“持有”某个数值，引用类型的变量是“指向”某个对象；

引用类型的变量可以是空值`null`；

要区分空值`null`和空字符串`""`。

### 数组类型

定义一个数组类型的变量，使用数组类型“类型[]”，例如，`int[]`。和单个基本类型变量不同，数组变量初始化必须使用`new int[5]`表示创建一个可容纳5个`int`元素的数组。

Java的数组有几个特点：

- 数组所有元素初始化为默认值，整型都是`0`，浮点型是`0.0`，布尔型是`false`；
- 数组一旦创建后，大小就不可改变。
- 如果索引超出范围，运行时将报错。

要访问数组中的某一个元素，需要使用索引。数组索引从`0`开始，例如，5个元素的数组，索引范围是`0`~`4`。

可以修改数组中的某一个元素，使用赋值语句，例如，`ns[1] = 79;`。

可以用`数组变量.length`获取数组大小.

#### 基本类型数组

```java
int[] ns = new int[5];
ns[0] = 68;
// 直接声明时赋值
int[] ns = new int[] { 68, 79, 91, 85, 62 };
```

数组大小变了吗？看上去好像是变了，但其实根本没变。

对于数组`ns`来说，执行`ns = new int[] { 68, 79, 91, 85, 62 };`时，它指向一个5个元素的数组：

```ascii
     ns
      │
      ▼
┌───┬───┬───┬───┬───┬───┬───┐
│   │68 │79 │91 │85 │62 │   │
└───┴───┴───┴───┴───┴───┴───┘
```

执行`ns = new int[] { 1, 2, 3 };`时，它指向一个*新的*3个元素的数组：

```ascii
     ns ──────────────────────┐
                              │
                              ▼
┌───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┐
│   │68 │79 │91 │85 │62 │   │ 1 │ 2 │ 3 │   │
└───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┘
```

但是，原有的5个元素的数组并没有改变，只是无法通过变量`ns`引用到它们而已。

#### 引用类型数组

对于`String[]`类型的数组变量`names`，它实际上包含3个元素，但每个元素都指向某个字符串对象：

```ascii
          ┌─────────────────────────┐
    names │   ┌─────────────────────┼───────────┐
      │   │   │                     │           │
      ▼   │   │                     ▼           ▼
┌───┬───┬─┴─┬─┴─┬───┬───────┬───┬───────┬───┬───────┬───┐
│   │░░░│░░░│░░░│   │ "ABC" │   │ "XYZ" │   │ "zoo" │   │
└───┴─┬─┴───┴───┴───┴───────┴───┴───────┴───┴───────┴───┘
      │                 ▲
      └─────────────────┘
```

对`names[1]`进行赋值，例如`names[1] = "cat";`，效果如下：

```ascii
          ┌─────────────────────────────────────────────────┐
    names │   ┌─────────────────────────────────┐           │
      │   │   │                                 │           │
      ▼   │   │                                 ▼           ▼
┌───┬───┬─┴─┬─┴─┬───┬───────┬───┬───────┬───┬───────┬───┬───────┬───┐
│   │░░░│░░░│░░░│   │ "ABC" │   │ "XYZ" │   │ "zoo" │   │ "cat" │   │
└───┴─┬─┴───┴───┴───┴───────┴───┴───────┴───┴───────┴───┴───────┴───┘
      │                 ▲
      └─────────────────┘
```

这里注意到原来`names[1]`指向的字符串`"XYZ"`并没有改变，仅仅是将`names[1]`的引用从指向`"XYZ"`改成了指向`"cat"`，其结果是字符串`"XYZ"`再也无法通过`names[1]`访问到了。

关于引用类型的内容修改，都是只把引用的指向给换了，并没有在原始的位置修改。

```java
String[] names = {"ABC", "XYZ", "zoo"};
String s = names[1];
names[1] = "cat";
```

#### 小结

数组是同一数据类型的集合，数组一旦创建后，大小就不可变；

可以通过索引访问数组元素，但索引超出范围将报错；

数组元素可以是值类型（如int）或引用类型（如String），但数组本身是引用类型；

## 运算

### 整数运算

Java的整数运算遵循四则运算规则，可以使用任意嵌套的小括号。四则运算规则和初等数学一致。

整数的数值会直接精确到整数，即保留整数。

整数由于存在范围限制，如果**计算结果超出了范围，就会产生溢出**，而溢出*不会报错*，但结果不对。(正，反，补码)

简写的运算符，即`+=`，`-=`，`*=`，`/=`，`++`和`--`

#### 移位运算

在计算机中，整数总是以二进制的形式表示。可以对整数进行移位运算。（左移实际上就是不断地×2，右移实际上就是不断地÷2）

对整数`7`左移1位将得到整数`14`，左移两位将得到整数`28`：

```java
int n = 7;       // 00000000 00000000 00000000 00000111 = 7
int a = n << 1;  // 00000000 00000000 00000000 00001110 = 14
int b = n << 2;  // 00000000 00000000 00000000 00011100 = 28
```

左移到最高位变成`1`时，结果变成了负数。

类似的，对整数28进行右移，结果如下：

```java
int n = 7;       // 00000000 00000000 00000000 00000111 = 7
int a = n >> 1;  // 00000000 00000000 00000000 00000011 = 3
```

#### 位运算

位运算是按位进行与、或、非和异或的运算。

```java
//与：
n = 1 & 0; // 0
n = 1 & 1; // 1
//或：
n = 0 | 0; // 0
n = 0 | 1; // 1
//非：
n = ~0; // 1
n = ~1; // 0
//异或：相同为0，不同为1
n = 1 ^ 0; // 1
n = 1 ^ 1; // 0
```

#### 运算优先级

在Java的计算表达式中，运算优先级从高到低依次是：

- `()`
- `!` `~` `++` `--`
- `*` `/` `%`
- `+` `-`
- `<<` `>>` `>>>`
- `&`
- `|`
- `+=` `-=` `*=` `/=`

记不住也没关系，只需要加括号就可以保证运算的优先级正确。

#### 类型自动提升与强制转型

在运算过程中，如果参与运算的两个数类型不一致，那么计算结果为较大类型的整型。例如，`short`和`int`计算，结果总是`int`，原因是`short`首先自动被转型为`int`。

要注意，超出范围的强制转型会得到错误的结果，原因是转型时，`int`的两个高位字节直接被扔掉，仅保留了低位的两个字节：

```java
int i1 = 1234567;
short s1 = (short) i1; // -10617
```

#### 小结

整数运算的结果永远是精确的；

运算结果会自动提升；

可以强制转型，但**超出范围的强制转型会得到错误的结果**；

应该选择合适范围的整型（`int`或`long`），没有必要为了节省内存而使用`byte`和`short`进行整数运算。

### 浮点数运算

浮点数运算和整数运算相比，只能进行加减乘除这些数值计算，不能做位运算和移位运算。

浮点数虽然表示的范围大，但是浮点数常常无法精确表示。

浮点数`0.1`在计算机中就无法精确表示，因为十进制的`0.1`换算成二进制是一个无限循环小数，很显然，无论使用`float`还是`double`，都只能存储一个`0.1`的近似值。但是，`0.5`这个浮点数又可以精确地表示。

因为浮点数常常无法精确表示，因此，浮点数运算会产生误差：

```java
double x = 1.0 / 10;
double y = 1 - 9.0 / 10;
//0.1
//0.09999999999999998
```
由于浮点数存在运算误差，所以比较两个浮点数是否相等常常会出现错误的结果。正确的**比较方法**是**判断两个浮点数之差的绝对值是否小于一个很小的数**：

#### 类型提升

如果参与运算的两个数其中一个是整型，那么整型可以自动提升到浮点型：

需要特别注意，在一个复杂的四则运算中，**两个整数的运算不会出现自动提升的情况**。例如：

```java
double d = 1.2 + 24 / 5; // 5.2
```

计算结果为`5.2`，原因是编译器计算`24 / 5`这个子表达式时，按两个整数进行运算，结果仍为整数`4`。

#### 溢出

整数运算在除数为`0`时会报错，而浮点数运算在除数为`0`时，不会报错，但会返回几个特殊值：

- `NaN`表示Not a Number
- `Infinity`表示无穷大
- `-Infinity`表示负无穷大

例如：

```java
double d1 = 0.0 / 0; // NaN
double d2 = 1.0 / 0; // Infinity
double d3 = -1.0 / 0; // -Infinity
```

实际运算中很少碰到，我们只需要了解即可。

#### 强制转型

可以将浮点数强制转型为整数。在转型时，浮点数的小数部分会被丢掉。如果转型后超过了整型能表示的最大范围，将返回整型的最大值。例如：

```java
int n1 = (int) 12.3; // 12
int n2 = (int) 12.7; // 12
int n2 = (int) -12.7; // -12
int n3 = (int) (12.7 + 0.5); // 13
int n4 = (int) 1.2e20; // 2147483647
```

##### 四舍五入

如果要进行四舍五入，可以对浮点数加上0.5再强制转型：

```java
double d = 2.6;
int n = (int) (d + 0.5);
```

#### 小结

浮点数常常无法精确表示，并且浮点数的运算结果可能有误差；

比较两个浮点数通常比较它们的差的绝对值是否小于一个特定值；

整型和浮点型运算时，整型会自动提升为浮点型；

可以将浮点型强制转为整型，但超出范围后将始终返回整型的最大值。

### 布尔运算

对于布尔类型`boolean`，永远只有`true`和`false`两个值。

布尔运算是一种关系运算，包括以下几类：

- 比较运算符：`>`，`>=`，`<`，`<=`，`==`，`!=`
- 与运算 `&&`
- 或运算 `||`
- 非运算 `!`

关系运算符的优先级从高到低依次是：

- `!`
- `>`，`>=`，`<`，`<=`
- `==`，`!=`
- `&&`
- `||`

```java
boolean isGreater = 5 > 3; // true
int age = 12;
boolean isZero = age == 0; // false
boolean isNonZero = !isZero; // true
boolean isAdult = age >= 18; // false
boolean isTeenager = age >6 && age <18; // true
```

#### 短路运算

布尔运算的一个重要特点是短路运算。如果一个布尔运算的表达式能提前确定结果，则后续的计算不再执行，直接返回结果。“**与**” 的时候有了“0”，“**或**” 的时候有了“1”。

#### 三元运算符

Java还提供一个三元运算符`b ? x : y`，它根据第一个布尔表达式的结果，分别返回后续两个表达式之一的计算结果。示例：

```java
public class Main {
    public static void main(String[] args) {
        int n = -100;
        int x = n >= 0 ? n : -n;
        System.out.println(x);
    }
}
```

## 流程控制

### 输入和输出

#### 输出

在前面的代码中，我们总是使用`System.out.println()`来向屏幕输出一些内容。

`println`是print line的缩写，表示输出并换行。因此，如果输出后不想换行，可以用`print()`：

##### 格式化输出

```java
double d = 12900000;
System.out.println(d); // 1.29E7
```

如果要把数据显示成我们期望的格式，就需要使用格式化输出的功能。格式化输出使用`System.out.printf()`，通过使用占位符`%?`，`printf()`可以把后面的参数格式化成指定格式：

```java
double d = 3.1415926;
System.out.printf("%.2f\n", d); // 显示两位小数3.14
System.out.printf("%.4f\n", d); // 显示4位小数3.1416
```

Java的格式化功能提供了多种占位符，可以把各种数据类型“格式化”成指定的字符串：

| 占位符 | 说明                             |
| :----- | :------------------------------- |
| %d     | 格式化输出整数                   |
| %x     | 格式化输出十六进制整数           |
| %f     | 格式化输出浮点数                 |
| %e     | 格式化输出科学计数法表示的浮点数 |
| %s     | 格式化字符串                     |

注意，由于%表示占位符，因此，连续两个%%表示一个%字符本身。

占位符本身还可以有更详细的格式化参数。下面的例子把一个整数格式化成十六进制，并用0补足8位：

```java
 System.out.printf("n=%d, hex=%08x", n, n); // 注意，两个%占位符必须传入两个数
```

详细的格式化参数请参考JDK文档[java.util.Formatter](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Formatter.html#syntax)

#### 输入

```java
Scanner scanner = new Scanner(System.in); // 创建Scanner对象
System.out.print("Input your name: "); // 打印提示
String name = scanner.nextLine(); // 读取一行输入并获取字符串
System.out.print("Input your age: "); // 打印提示
int age = scanner.nextInt(); // 读取一行输入并获取整数
System.out.printf("Hi, %s, you are %d\n", name, age); // 格式化输出
```

首先，我们通过`import`语句导入`java.util.Scanner`，`import`是导入某个类的语句，必须放到Java源代码的开头，后面我们在Java的`package`中会详细讲解如何使用`import`。

然后，创建`Scanner`对象并传入`System.in`。`System.out`代表标准输出流，而`System.in`代表标准输入流。直接使用`System.in`读取用户输入虽然是可以的，但需要更复杂的代码，而通过`Scanner`就可以简化后续的代码。

有了`Scanner`对象后，要读取用户输入的字符串，使用`scanner.nextLine()`，要读取用户输入的整数，使用`scanner.nextInt()`。`Scanner`会自动转换数据类型，因此不必手动转换。

```java
import java.util.Scanner;
public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in); // 创建Scanner对象
        System.out.print("Input your name: "); // 打印提示
        String name = scanner.nextLine(); // 读取一行输入并获取字符串
        System.out.print("Input your age: "); // 打印提示
        int age = scanner.nextInt(); // 读取一行输入并获取整数
        System.out.printf("Hi, %s, you are %d\n", name, age); // 格式化输出
    }
}
```

首先，我们通过`import`语句导入`java.util.Scanner`，`import`是导入某个类的语句，必须放到Java源代码的开头，后面我们在Java的`package`中会详细讲解如何使用`import`。

然后，创建`Scanner`对象并传入`System.in`。`System.out`代表标准输出流，而`System.in`代表标准输入流。直接使用`System.in`读取用户输入虽然是可以的，但需要更复杂的代码，而通过`Scanner`就可以简化后续的代码。

有了`Scanner`对象后，要读取用户输入的字符串，使用`scanner.nextLine()`，要读取用户输入的整数，使用`scanner.nextInt()`。`Scanner`会自动转换数据类型，因此不必手动转换。

作业：计算这次比上次成绩提高的比例。

```java
import java.util.Scanner;
public class App {
    public static void main(String[] args) throws Exception {
        // System.out.println("Hello, World!");
        Scanner scanner = new Scanner(System.in);
        System.out.print("last score:");
        int lscore = scanner.nextInt();
        System.out.print("this score:");
        int tscore = scanner.nextInt();
        float pre = (float)(tscore-lscore)/lscore *100;
        System.out.printf("成绩提高了%.2f%%", pre);

    }
}
```

#### 小结

Java提供的输出包括：`System.out.println()` / `print()` / `printf()`，其中`printf()`可以格式化输出；

Java提供Scanner对象来方便输入，读取对应的类型可以使用：`scanner.nextLine()` / `nextInt()` / `nextDouble()` / ...

### if判断

`if`语句的基本语法是：

```java
if (条件) {
    // 条件满足时执行
}else{
    //条件不满足时执行
}
```

跟C语言里的差不多，跟python不一样.

由于使用缩进格式，很容易把两行语句都看成`if`语句的执行块，但实际上只有第一行语句是`if`的执行块。在使用git这些版本控制系统自动合并时更容易出问题，所以不推荐忽略花括号的写法。

#### 判断引用类型相等

在Java中，判断值类型的变量是否相等，可以使用`==`运算符。但是，判断引用类型的变量是否相等，`==`表示“引用是否相等”，或者说，是否指向同一个对象。例如，下面的两个String类型，它们的内容是相同的，但是，分别指向不同的对象，用`==`判断，结果为`false`：

```java
String s1 = "hello";
String s2 = "HELLO".toLowerCase();
if ( s1== s2) {
    System.out.println("s1 == s2");
} else {
    System.out.println("s1 != s2");
}
```

要判断引用类型的变量内容是否相等，必须使用`equals()`方法：

```java
if (s1.equals(s2)) {
    System.out.println("s1 equals s2");
} else {
    System.out.println("s1 not equals s2");
}
```

注意：执行语句`s1.equals(s2)`时，如果变量`s1`为`null`，会报`NullPointerException`：

要避免`NullPointerException`错误，可以利用短路运算符`&&`：`if (s1 != null && s1.equals("hello"))`

还可以把一定不是`null`的对象`"hello"`放到前面：例如：`if ("hello".equals(s)) { ... }`。

#### 练习

请用`if ... else`编写一个程序，用于计算体质指数BMI，并打印结果。

BMI = 体重(kg)除以身高(m)的平方

BMI结果：

- 过轻：低于18.5
- 正常：18.5-25
- 过重：25-28
- 肥胖：28-32
- 非常肥胖：高于32

```java
    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        System.out.print("输入身高(m)：");
        float hell = scan.nextFloat();
        System.out.print("输入体重(kg)：");
        float weight = scan.nextFloat();
        float bmi = weight/(hell*hell);
        if (bmi<18.5){
            System.out.println("过轻：低于18.5");
        }
        else if(bmi>=18.5 && bmi<25){
            System.out.println("正常：18.5-25");
        }
        else if(bmi>=25 && bmi<28){
            System.out.println("过重：25-28");
        }
        else if(bmi>=28 && bmi<32){
            System.out.println("肥胖：28-32");
        }
        else{
            System.out.println("非常肥胖：高于32");
        }
    
    }
```

### switch多重选择

`switch`语句根据`switch (表达式)`计算的结果，跳转到匹配的`case`结果，然后继续执行后续语句，直到遇到`break`结束执行。

switch case 语句语法格式如下：

```java
switch(expression){
    case value :
       //语句
       break; //可选
    case value :
       //语句
       break; //可选
    //你可以有任意数量的case语句
    default : //可选
       //语句
}
```

- switch 语句中的变量类型可以是： byte、short、int ，char，或者 String 类型了，同时 case 标签必须为字符串常量或字面量。`switch`语句还可以使用枚举类型，枚举类型我们在后面讲解。
- case 语句中的值的数据类型必须与变量的数据类型相同，而且**只能是常量或者字面常量**。
- case 语句后值匹配到了，才执行之后的语句，如果碰到break,终止switch语句，执行后续代码，但case 后的break非必须，但最好有，否则会连续执行后边的case语句。
- switch 语句可以包含一个 default 分支，该分支一般是 switch 语句的最后一个分支（可以在任何位置，但建议在最后一个）。default 在没有 case 语句的值和变量值相等的时候执行。default 分支不需要 break 语句。

**switch case 执行时，一定会先进行匹配，匹配成功返回当前 case 的值，再根据是否有 break，判断是否继续输出，或是跳出判断。**

使用`switch`时，如果遗漏了`break`，就会造成严重的逻辑错误，而且不易在源代码中发现错误。从Java 12开始，`switch`语句升级为更简洁的表达式语法，使用类似模式匹配（Pattern Matching）的方法，保证只有一种路径会被执行，注意新语法使用`->`，如果有多条语句，需要用`{}`括起来。不要写`break`语句，因为新语法只会执行匹配的语句，没有穿透效应。

```java
switch (fruit) {
case "apple" -> System.out.println("Selected apple");
case "pear" -> System.out.println("Selected pear");
case "mango" -> {
      System.out.println("Selected mango");
      System.out.println("Good choice!");
}
default -> System.out.println("No fruit selected");
}
```

还可以返回值

```java
int opt = switch (fruit) {
       case "apple" -> 1;
       case "pear", "mango" -> 2;
       default -> 0;
}; // 注意赋值语句要以;结束
```

#### yield

大多数时候，在`switch`表达式内部，我们会返回简单的值。

但是，如果需要复杂的语句，我们也可以写很多语句，放到`{...}`里，然后，用`yield`返回一个值作为`switch`语句的返回值：

```java
int opt = switch (fruit) {
    case "apple" -> 1;
    default -> {
        int code = fruit.hashCode();
         yield code; // switch语句返回值
     }
};
```

#### 小结

`switch`语句可以做多重选择，然后执行匹配的`case`语句后续代码；

`switch`的计算结果必须是整型、字符串或枚举类型；

注意千万不要漏写`break`，建议打开`fall-through`警告；

总是写上`default`，建议打开`missing default`警告；

从Java 14开始，`switch`语句正式升级为表达式，不再需要`break`，并且允许使用`yield`返回值。

练习：

使用`switch`实现一个简单的石头、剪子、布游戏。

```java
    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        boolean flag = true;
        while(flag){
            System.out.print("(剪刀-0；石头-1；布-2)输入：");
            int a = scan.nextInt();
            switch(a){
                case 0: System.out.println("你出的剪刀");break;
                case 1: System.out.println("你出的石头");break;
                case 2: System.out.println("你出的布");break;
                default:System.out.println("不允许！重新出拳！");
            }
            if(a==0||a==1||a==2){
                int pc = (int)(Math.random()*10)%3;
                switch(pc){
                    case 0: System.out.println("PC出的剪刀");break;
                    case 1: System.out.println("PC出的石头");break;
                    case 2: System.out.println("PC出的布");break;
                    default:System.out.println("PC出错！");
                }
                switch(a-pc){
                    case -2:System.out.println("you win");break;
                    case -1:System.out.println("you lose");break;
                    case 0:System.out.println("peace");break;
                    case 1:System.out.println("you win");break;
                    case 2:System.out.println("you lose");break;
                    default:System.out.println("error!!!");
                }
            }
            System.out.print("是否继续yes(1),no(0)：");
            int f = scan.nextInt();
            if(f==0){
                flag = false;
            }
        }
        scan.close();
    }
```

### while循环

```java
while (条件表达式) {
    循环语句
}
// 继续执行后续代码
```

注意到`while`循环是先判断循环条件，再循环，因此，有可能一次循环都不做。

如果循环条件永远满足，那这个循环就变成了死循环。死循环将导致100%的CPU占用，用户会感觉电脑运行缓慢，所以要避免编写死循环代码。

### do-while循环

在Java中，`while`循环是先判断循环条件，再执行循环。而另一种`do while`循环则是先执行循环，再判断条件，条件满足时继续循环，条件不满足时退出。它的用法是：

```java
do {
    执行循环语句
} while (条件表达式);
```

可见，`do while`循环会至少循环一次。

### for循环

`for`循环的功能非常强大，它使用计数器实现循环。`for`循环会先初始化计数器，然后，在每次循环前检测循环条件，在每次循环后更新计数器。计数器变量通常命名为`i`。（使用方式同C语言）

```java
for (初始条件; 循环检测条件; 循环后更新计数器) {
    // 执行语句
}
```

1到100求和用`for`循环：

```java
for (int i=1; i<=100; i++) {
   sum = sum + i;
}
```

使用`for`循环时，千万不要在循环体内修改计数器！在循环体中修改计数器常常导致莫名其妙的逻辑错误。对于下面的代码：

```java
for (int i=0; i<ns.length; i++) {
    System.out.println(ns[i]);
    i = i + 1;
}
```

`for`循环还可以缺少初始化语句、循环条件和每次循环更新语句，例如：

```java
// 不设置结束条件:
for (int i=0; ; i++)
// 不设置结束条件和更新语句:
for (int i=0; ;)
// 什么都不设置:
for (;;)
```

`for`循环经常用来遍历数组，因为通过计数器可以根据索引来访问数组的每个元素，但是，很多时候，我们实际上真正想要访问的是数组每个元素的值。Java还提供了另一种`for each`循环，它可以更简单地遍历数组：

```java
public static void main(String[] args) {
     int[] ns = { 1, 4, 9, 16, 25 };
     for (int n : ns) {
         System.out.println(n);
     }
}
```

和`for`循环相比，`for each`循环的变量n不再是计数器，而是直接对应到数组的每个元素。`for each`循环的写法也更简洁。但是，`for each`循环无法指定遍历顺序，也无法获取数组的索引。

除了数组外，`for each`循环能够遍历所有“可迭代”的数据类型，包括后面会介绍的`List`、`Map`等。

```java
for (int i=0;i<ns.length; i++) {
    System.out.println(ns[i]);}

for(int n:ns){
    System.out.println(n);}
```

#### 小结

`for`循环通过计数器可以实现复杂循环；

`for each`循环可以直接遍历数组的每个元素；

最佳实践：计数器变量定义在`for`循环内部，循环体内部不修改计数器；

### break和continue

在循环过程中，可以使用`break`语句跳出当前循环。而`continue`则是提前结束本次循环，直接继续执行下次循环。在多层嵌套的循环中，`continue`语句同样是结束本次自己所在的循环。

#### 小结

`break`语句可以跳出当前循环；

`break`语句通常配合`if`，在满足条件时提前结束整个循环；

`break`语句总是跳出最近的一层循环；

`continue`语句可以提前结束本次循环；

`continue`语句通常配合`if`，在满足条件时提前结束本次循环。

### 数组操作

#### 数组遍历

遍历数组可以使用`for`循环，`for`循环可以访问数组索引，通过索引找数组元素；

`for each`循环直接迭代每个数组元素，但无法获取索引；

使用`Arrays.toString()`可以快速获取数组内容。

```java
System.out.println(Arrays.toString(ns));
```

#### 数组排序

```java
for(int i=0;i<ns.length-1;i++){
    for(int j=0;j<ns.length-1-i;j++){
        if(ns[j] > ns[j+1]){
            int tmp = ns[j];
            ns[j]=ns[j+1];
            ns[j+1] = tmp;
         }
    }
}
//效果同上
Arrays.sort(ns);
```

数组也是引用类型，对于引用类型，数据在内存中的位置不变，但数组中的索引变了，导致的结果发生变化。

排序前，这个数组在内存中表示如下：

```ascii
                   ┌──────────────────────────────────┐
               ┌───┼──────────────────────┐           │
               │   │                      ▼           ▼
         ┌───┬─┴─┬─┴─┬───┬────────┬───┬───────┬───┬──────┬───┐
ns ─────>│░░░│░░░│░░░│   │"banana"│   │"apple"│   │"pear"│   │
         └─┬─┴───┴───┴───┴────────┴───┴───────┴───┴──────┴───┘
           │                 ▲
           └─────────────────┘
```

调用`Arrays.sort(ns);`排序后，这个数组在内存中表示如下：

```ascii
                   ┌──────────────────────────────────┐
               ┌───┼──────────┐                       │
               │   │          ▼                       ▼
         ┌───┬─┴─┬─┴─┬───┬────────┬───┬───────┬───┬──────┬───┐
ns ─────>│░░░│░░░│░░░│   │"banana"│   │"apple"│   │"pear"│   │
         └─┬─┴───┴───┴───┴────────┴───┴───────┴───┴──────┴───┘
           │                              ▲
           └──────────────────────────────┘
```

原来的3个字符串在内存中均没有任何变化，但是`ns`数组的每个元素指向变化了。

##### 小结

常用的排序算法有冒泡排序、插入排序和快速排序等；

冒泡排序使用两层`for`循环实现排序；

交换两个变量的值需要借助一个临时变量。

可以直接使用Java标准库提供的`Arrays.sort()`进行排序；

对数组排序会直接修改数组本身

#### 二维数组

跟C语言一样。

二维数组就是数组的数组，

多维数组的每个数组元素长度都不要求相同；

```java
int[][] ns = {
    { 1, 2, 3, 4 },
    { 5, 6 },
    { 7, 8, 9 }
};
```

这个二维数组在内存中的结构如下：

```ascii
                    ┌───┬───┬───┬───┐
         ┌───┐  ┌──>│ 1 │ 2 │ 3 │ 4 │
ns ─────>│░░░│──┘   └───┴───┴───┴───┘
         ├───┤      ┌───┬───┐
         │░░░│─────>│ 5 │ 6 │
         ├───┤      └───┴───┘
         │░░░│──┐   ┌───┬───┬───┐
         └───┘  └──>│ 7 │ 8 │ 9 │
                    └───┴───┴───┘
```

三维数组就是二维数组的数组:

```ascii
                              ┌───┬───┬───┐
                   ┌───┐  ┌──>│ 1 │ 2 │ 3 │
               ┌──>│░░░│──┘   └───┴───┴───┘
               │   ├───┤      ┌───┬───┬───┐
               │   │░░░│─────>│ 4 │ 5 │ 6 │
               │   ├───┤      └───┴───┴───┘
               │   │░░░│──┐   ┌───┬───┬───┐
        ┌───┐  │   └───┘  └──>│ 7 │ 8 │ 9 │
ns ────>│░░░│──┘              └───┴───┴───┘
        ├───┤      ┌───┐      ┌───┬───┐
        │░░░│─────>│░░░│─────>│10 │11 │
        ├───┤      ├───┤      └───┴───┘
        │░░░│──┐   │░░░│──┐   ┌───┬───┐
        └───┘  │   └───┘  └──>│12 │13 │
               │              └───┴───┘
               │   ┌───┐      ┌───┬───┬───┐
               └──>│░░░│─────>│14 │15 │16 │
                   ├───┤      └───┴───┴───┘
                   │░░░│──┐   ┌───┬───┐
                   └───┘  └──>│17 │18 │
                              └───┴───┘
```

打印多维数组可以使用`Arrays.deepToString()`；

最常见的多维数组是二维数组，访问二维数组的一个元素使用`array[row][col]`。

#### 命令行参数

Java程序的入口是`main`方法，而`main`方法可以接受一个命令行参数，它是一个`String[]`数组。

命令行参数类型是`String[]`数组；

命令行参数由JVM接收用户输入并传给`main`方法；

如何解析命令行参数需要由程序自己实现。
