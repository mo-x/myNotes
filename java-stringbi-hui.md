\#\#\#\#\# String

1. String 不是基本类型。

1. String是一种数据结构即串。底层还是用char实现的。

1. String的字面量通常会放入缓冲池（Constant pool ），便于复用提高程序性能.

#####  静态初始化

字符串

代码示例如下：

```java
String s2 = "hello" + " world";
```

s2对象JVM编译的时候会优先进行字面的计算，等同于

```java
String s2 = "hello world";
```

####  final关键字

final 关键字修饰String类型时，jvm也会编译期间计算出字面量并放入常量缓冲池。（相当于静态初始化。在字节码中都是1dl指令放入缓冲池）

```java
 String str = "java";
 final String str1 = "ja";
 final String str2 = "va";
 String str3 = str1 + str2;
 System.out.println(str3== str);//true

```

##### 动态初始化

```java
  String s3 = " world";
  String s4 = "hello";
  String s5 = s4 + s3;
  String s6 = "hello"+new String(" world");
```

动态初始化 不是字面量的运算了 底层才用StringBuilder实现，相当于 new StringBuilder\(\).append\(s4\).append\(s3\);此时该对象不是存放于缓冲池而是在堆上。

