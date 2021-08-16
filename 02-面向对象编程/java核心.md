# Java核心类

## 字符串和编码

### String

在Java中，`String`是一个引用类型，它本身也是一个`class`。但是，Java编译器对`String`有特殊处理，即可以直接用`"..."`来表示一个字符串：

```java
String s1 = "Hello!";
```

实际上字符串在`String`内部是通过一个`char[]`数组表示的，因此，按下面的写法也是可以的：

```java
String s2 = new String(new char[] {'H', 'e', 'l', 'l', 'o', '!'});
```

Java字符串的一个重要特点就是字符串*不可变*。这种不可变性是通过内部的`private final char[]`字段，以及没有任何修改`char[]`的方法实现的。

### 字符串处理

当我们想要比较两个字符串是否相同时，要特别注意，我们实际上是想比较字符串的内容是否相同。必须使用`equals()`方法而不能用`==`。

```java
String s1 = "hello";
String s2 = "hello";
String s3 = "HELLO".toLowerCase();
System.out.println(s1 == s2);		//true
System.out.println(s1.equals(s2));	//true
System.out.println(s1 == s3);		//false
System.out.println(s1.equals(s3));	//true
```

实际上那只是Java编译器在编译期，会自动把所有相同的字符串当作一个对象放入常量池，自然`s1`和`s2`的引用就是相同的。所以，这种`==`比较返回`true`纯属巧合。一旦换成s3，`==`比较就会失败。

结论：两个字符串比较，必须总是使用`equals()`方法。要忽略大小写比较，使用`equalsIgnoreCase()`方法。

`String`类还提供了多种方法来搜索子串、提取子串。

```java
// 是否包含子串:
"Hello".contains("ll"); // true

//搜索子串的更多的例子：
"Hello".indexOf("l"); // 2
"Hello".lastIndexOf("l"); // 3
"Hello".startsWith("He"); // true
"Hello".endsWith("lo"); // true

//提取子串的例子：
"Hello".substring(2); // "llo"
"Hello".substring(2, 4); "ll"
    
//去除首尾空白字符。空白字符包括空格，\t，\r，\n：
//注意：trim()并没有改变字符串的内容，而是返回了一个新字符串。
"  \tHello\r\n ".trim(); // "Hello"

//strip()方法也可以移除字符串首尾空白字符。它和trim()不同的是，类似中文的空格字符\u3000也会被移除：
"\u3000Hello\u3000".strip(); // "Hello"
" Hello ".stripLeading(); // "Hello "
" Hello ".stripTrailing(); // " Hello"

//isEmpty()和isBlank()来判断字符串是否为空和空白字符串：
"".isEmpty(); // true，因为字符串长度为0
"  ".isEmpty(); // false，因为字符串长度不为0
"  \n".isBlank(); // true，因为只包含空白字符
" Hello ".isBlank(); // false，因为包含非空白字符

//替换子串
要在字符串中替换子串，有两种方法。
一种是根据字符或字符串替换：
String s = "hello";
s.replace('l', 'w'); // "hewwo"，所有字符'l'被替换为'w'
s.replace("ll", "~~"); // "he~~o"，所有子串"ll"被替换为"~~"
另一种是通过正则表达式替换：
String s = "A,,B;C ,D";
s.replaceAll("[\\,\\;\\s]+", ","); // "A,B,C,D"
上面的代码通过正则表达式，把匹配的子串统一替换为","。
    
//分割字符串
要分割字符串，使用split()方法，并且传入的也是正则表达式：
String s = "A,B,C,D";
String[] ss = s.split("\\,"); // {"A", "B", "C", "D"}

//拼接字符串
拼接字符串使用静态方法 join()，它用指定的字符串连接字符串数组：
String[] arr = {"A", "B", "C"};
String s = String.join("***", arr); // "A***B***C"

//格式化字符串
字符串提供了formatted()方法和format()静态方法，可以传入其他参数，替换占位符，然后生成新的字符串：
String s = "Hi %s, your score is %d!";
System.out.println(s.formatted("Alice", 80));
System.out.println(String.format("Hi %s, your score is %.2f!", "Bob", 59.5));
```

有几个占位符，后面就传入几个参数。参数类型要和占位符一致。

- `%s`：显示字符串；
- `%d`：显示整数；
- `%x`：显示十六进制整数；
- `%f`：显示浮点数。

占位符还可以带格式，例如`%.2f`表示显示两位小数。如果你不确定用啥占位符，那就始终用`%s`，因为`%s`可以显示任何数据类型

**类型转换**

要把任意基本类型或引用类型转换为字符串，可以使用静态方法`valueOf()`。这是一个重载方法，编译器会根据参数自动选择合适的方法：

```
String.valueOf(123); // "123"
String.valueOf(45.67); // "45.67"
String.valueOf(true); // "true"
String.valueOf(new Object()); // 类似java.lang.Object@636be97c
```

要把字符串转换为其他类型，就需要根据情况。例如，把字符串转换为`int`类型：

```java
int n1 = Integer.parseInt("123"); // 123
int n2 = Integer.parseInt("ff", 16); // 按十六进制转换，255
```

把字符串转换为`boolean`类型：

```java
boolean b1 = Boolean.parseBoolean("true"); // true
boolean b2 = Boolean.parseBoolean("FALSE"); // false
```

要特别注意，`Integer`有个`getInteger(String)`方法，它不是将字符串转换为`int`，而是把该字符串对应的系统变量转换为`Integer`：

```java
Integer.getInteger("java.version"); // 版本号，11
```

**转换为char[]**

`String`和`char[]`类型可以互相转换，方法是：

```java
char[] cs = "Hello".toCharArray(); // String -> char[]
String s = new String(cs); // char[] -> String
```

如果修改了`char[]`数组，`String`并不会改变：这是因为通过`new String(char[])`创建新的`String`实例时，它并不会直接引用传入的`char[]`数组，而是会复制一份，所以，修改外部的`char[]`数组不会影响`String`实例内部的`char[]`数组，因为这是两个不同的数组。从`String`的不变性设计可以看出，如果传入的对象有可能改变，我们需要复制而不是直接引用。

### 字符编码

在早期的计算机系统中，为了给字符编码，美国国家标准学会（ANSI）制定了一套英文字母、数字和常用符号的编码，它占用一个字节，编码范围从`0`到`127`，最高位始终为`0`，称为`ASCII`编码。为了统一全球所有语言的编码，全球统一码联盟发布了`Unicode`编码，它把世界上主要语言都纳入同一个编码,`Unicode`编码需要两个或者更多字节表示。

因为英文字符的`Unicode`编码高字节总是`00`，包含大量英文的文本会浪费空间，所以，出现了`UTF-8`编码，它是一种变长编码，用来把固定长度的`Unicode`编码变成1～4字节的变长编码。`UTF-8`编码的另一个好处是容错能力强。如果传输过程中某些字符出错，不会影响后续字符，因为`UTF-8`编码依靠高字节位来确定一个字符究竟是几个字节，它经常用来作为传输编码。

在Java中，`char`类型实际上就是两个字节的`Unicode`编码。果我们要手动把字符串转换成其他编码，可以这样做：

```JAVA
byte[] b1 = "Hello".getBytes(); // 按系统默认编码转换，不推荐
byte[] b2 = "Hello".getBytes("UTF-8"); // 按UTF-8编码转换
byte[] b2 = "Hello".getBytes("GBK"); // 按GBK编码转换
byte[] b3 = "Hello".getBytes(StandardCharsets.UTF_8); // 按UTF-8编码转换
```

注意：转换编码后，就不再是`char`类型，而是`byte`类型表示的数组。

如果要把已知编码的`byte[]`转换为`String`，可以这样做：

```JAVA
byte[] b = ...
String s1 = new String(b, "GBK"); // 按GBK转换
String s2 = new String(b, StandardCharsets.UTF_8); // 按UTF-8转换
```

始终牢记：Java的`String`和`char`在内存中总是以Unicode编码表示。

### 延伸阅读

对于不同版本的JDK，`String`类在内存中有不同的优化方式。具体来说，早期JDK版本的`String`总是以`char[]`存储，而较新的JDK版本的`String`则以`byte[]`存储：如果`String`仅包含ASCII字符，则每个`byte`存储一个字符，否则，每两个`byte`存储一个字符，这样做的目的是为了节省内存，因为大量的长度较短的`String`通常仅包含ASCII字符，对于使用者来说，`String`内部的优化不影响任何已有代码，因为它的`public`方法签名是不变的。

**小结**

- Java字符串`String`是不可变对象；
- 字符串操作不改变原字符串内容，而是返回新字符串；
- 常用的字符串操作：提取子串、查找、替换、大小写转换等；
- Java使用Unicode编码表示`String`和`char`；
- 转换编码就是将`String`和`byte[]`转换，需要指定编码；
- 转换为`byte[]`时，始终优先考虑`UTF-8`编码。

## StringBuilder

Java编译器对`String`做了特殊处理，使得我们可以直接用`+`拼接字符串。虽然可以直接拼接字符串，但是，在循环中，每次循环都会创建新的字符串对象，然后扔掉旧的字符串。这样，绝大部分字符串都是临时对象，不但浪费内存，还会影响GC效率。为了能高效拼接字符串，Java标准库提供了`StringBuilder`，它是一个可变对象，可以预分配缓冲区，这样，往`StringBuilder`中新增字符时，不会创建新的临时对象。

```java
String s = "";
for (int i = 0; i < 1000; i++) {
    s = s + "," + i;
}
=======>
StringBuilder sb = new StringBuilder(1024);
for (int i = 0; i < 1000; i++) {
    sb.append(',');
    sb.append(i);
}
String s = sb.toString();

//StringBuilder还可以进行链式操作：
   sb.append("Mr ")
          .append("Bob")
          .append("!")
          .insert(0, "Hello, "); 
```

注意：对于普通的字符串`+`操作，并不需要我们将其改写为`StringBuilder`，因为Java编译器在编译时就自动把多个连续的`+`操作编码为`StringConcatFactory`的操作。在运行期，`StringConcatFactory`会自动把字符串连接操作优化为数组复制或者`StringBuilder`操作。

`StringBuffer`是Java早期的一个`StringBuilder`的线程安全版本，它通过同步来保证多个线程操作`StringBuffer`也是安全的，但是同步会带来执行速度的下降。`StringBuilder`和`StringBuffer`接口完全相同，现在完全没有必要使用`StringBuffer`。

**小结**

- `StringBuilder`是可变对象，用来高效拼接字符串；
- `StringBuilder`可以支持链式操作，实现链式操作的关键是返回实例本身；
- `StringBuffer`是`StringBuilder`的线程安全版本，现在很少使用。

## StringJoiner

类似用分隔符拼接数组的需求很常见，所以Java标准库提供了一个`StringJoiner`来干这个事：

```java
import java.util.StringJoiner;
public class Main {
    public static void main(String[] args) {
        String[] names = {"Bob", "Alice", "Grace"};
        //var sj = new StringJoiner(", ");				//使用','隔开
        var sj = new StringJoiner(", ", "Hello ", "!");		//使用','隔开，指定开头结尾
        for (String name : names) {
            sj.add(name);
        }
        System.out.println(sj.toString());
    }
}
```

**String.join()**

`String`还提供了一个静态方法`join()`，这个方法在内部使用了`StringJoiner`来拼接字符串，在不需要指定“开头”和“结尾”的时候，用`String.join()`更方便：

```java
String[] names = {"Bob", "Alice", "Grace"};
var s = String.join(", ", names);
```

**小结**

- 用指定分隔符拼接字符串数组时，使用`StringJoiner`或者`String.join()`更方便；
- 用`StringJoiner`拼接字符串时，还可以额外附加一个“开头”和“结尾”。

## 包装类型

Java的数据类型分两种：

- 基本类型：`byte`，`short`，`int`，`long`，`boolean`，`float`，`double`，`char`
- 引用类型：所有`class`和`interface`类型

引用类型可以赋值为`null`，表示空，但基本类型不能赋值为`null`：

```java
String s = null;
int n = null; // compile error!
```

想要把`int`基本类型变成一个引用类型，我们可以定义一个`Integer`类，它只包含一个实例字段`int`，这样，`Integer`类就可以视为`int`的包装类（Wrapper Class）：

因为包装类型非常有用，Java核心库为每种基本类型都提供了对应的包装类型：

| 基本类型 | 对应的引用类型      |
| :------- | :------------------ |
| boolean  | java.lang.Boolean   |
| byte     | java.lang.Byte      |
| short    | java.lang.Short     |
| int      | java.lang.Integer   |
| long     | java.lang.Long      |
| float    | java.lang.Float     |
| double   | java.lang.Double    |
| char     | java.lang.Character |

我们可以直接使用，并不需要自己去定义

```java
int i = 100;
// 通过new操作符创建Integer实例(不推荐使用,会有编译警告):
Integer n1 = new Integer(i);
// 通过静态方法valueOf(int)创建Integer实例:
Integer n2 = Integer.valueOf(i);
// 通过静态方法valueOf(String)创建Integer实例:
Integer n3 = Integer.valueOf("100");
```

**Auto Boxing**

因为`int`和`Integer`可以互相转换，所以，Java编译器可以帮助我们自动在`int`和`Integer`之间转型：

```java
Integer n = 100; // 编译器自动使用Integer.valueOf(int)
int x = n; // 编译器自动使用Integer.intValue()
```

这种直接把`int`变为`Integer`的赋值写法，称为自动装箱（Auto Boxing），反过来，把`Integer`变为`int`的赋值写法，称为自动拆箱（Auto Unboxing）。

注意：自动装箱和自动拆箱只发生在编译阶段，目的是为了少写代码。装箱和拆箱会影响代码的执行效率，因为编译后的`class`代码是严格区分基本类型和引用类型的。并且自动拆箱执行时可能会报`NullPointerException`。

**不变类**

所有的包装类型都是不变类。我们查看`Integer`的源码可知，它的核心代码如下：

```
public final class Integer {
    private final int value;
}
```

因此，一旦创建了`Integer`对象，该对象就是不变的。

对两个`Integer`实例进行比较要特别注意：绝对不能用`==`比较，因为`Integer`是引用类型，必须使用`equals()`比较

```java
Integer x = 127;
Integer y = 127;
Integer m = 99999;
Integer n = 99999;
System.out.println("x == y: " + (x==y)); // true
System.out.println("m == n: " + (m==n)); // false
System.out.println("x.equals(y): " + x.equals(y)); // true
System.out.println("m.equals(n): " + m.equals(n)); // true
```

仔细观察结果的童鞋可以发现，`==`比较，较小的两个相同的`Integer`返回`true`，较大的两个相同的`Integer`返回`false`，这是因为`Integer`是不变类，编译器把`Integer x = 127;`自动变为`Integer x = Integer.valueOf(127);`，为了节省内存，`Integer.valueOf()`对于较小的数，始终返回相同的实例，因此，`==`比较“恰好”为`true`，但我们*绝不能*因为Java标准库的`Integer`内部有缓存优化就用`==`比较，必须用`equals()`方法比较两个`Integer`。

 **按照语义编程，而不是针对特定的底层实现去“优化”。**

因为`Integer.valueOf()`可能始终返回同一个`Integer`实例，因此，在我们自己创建`Integer`的时候，以下两种方法：

- 方法1：`Integer n = new Integer(100);`
- 方法2：`Integer n = Integer.valueOf(100);`

方法2更好，因为方法1总是创建新的`Integer`实例，方法2把内部优化留给`Integer`的实现者去做，即使在当前版本没有优化，也有可能在下一个版本进行优化。

我们把能创建“新”对象的静态方法称为静态工厂方法。`Integer.valueOf()`就是静态工厂方法，它尽可能地返回缓存的实例以节省内存。

 **创建新对象时，优先选用静态工厂方法而不是new操作符。**

如果我们考察`Byte.valueOf()`方法的源码，可以看到，标准库返回的`Byte`实例全部是缓存实例，但调用者并不关心静态工厂方法以何种方式创建新实例还是直接返回缓存的实例。

**小结**

Java核心库提供的包装类型可以把基本类型包装为`class`；

自动装箱和自动拆箱都是在编译期完成的（JDK>=1.5）；

装箱和拆箱会影响执行效率，且拆箱时可能发生`NullPointerException`；

包装类型的比较必须使用`equals()`；

整数和浮点数的包装类型都继承自`Number`；

包装类型提供了大量实用方法。

## JavaBean

JavaBean主要用来传递数据，即把一组数据组合成一个JavaBean便于传输。此外，JavaBean可以方便地被IDE工具分析，生成读写属性的代码，主要用在图形界面的可视化设计中。

我们通常把一组对应的读方法（`getter`）和写方法（`setter`）称为属性（`property`）。例如，`name`属性：

- 对应的读方法是`String getName()`
- 对应的写方法是`setName(String)`

只有`getter`的属性称为只读属性（read-only），例如，定义一个age只读属性：

- 对应的读方法是`int getAge()`
- 无对应的写方法`setAge(int)`

类似的，只有`setter`的属性称为只写属性（write-only）。

很明显，只读属性很常见，只写属性不常见。

通过IDE，可以快速生成`getter`和`setter`。

**小结**

JavaBean是一种符合命名规范的`class`，它通过`getter`和`setter`来定义属性；

属性是一种通用的叫法，并非Java语法规定；

可以利用IDE快速生成`getter`和`setter`；

使用`Introspector.getBeanInfo()`可以获取属性列表。

## 枚举类

## 记录类

## BigInteger

## BigDecimal

## 常用工具类