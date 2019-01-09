##### 注解分类

* 源码注解
* 编译时注解
* 运行时注解

\#\#\#\# 注解的生命周期

（1）注解被保留到源文件阶段

当javac把.Java源文件编译成.class时，就将相应的注解去掉。这种注解的生命周期就维持到源文件阶段。

（2）注解被保留到字节码文件阶段

在JVM通过ClassLoader向内存中加载字节码文件时候，JVM会去掉相应的注解。这种注解的生命周期就维持到字节码文件阶段。

注意：生命周期到源文件阶段和字节码文件阶段的注解，由于JVM执行内存中的字节码时候，相应的注解已经被Javac或者JVM去除，所以无法使用反射来访问相应的注解。

（3）注解被保留到内存中的字节码阶段

JVM运行内存的字节码时候，仍然可能会保留并且执行的某些注解。这种注解的生命周期就维持到内存字节码阶段。

注意：这个阶段，程序可以通过反射访问生命周期到内存字节码阶段的注解。

\#\#\#\#\#注解定义代码示例

```java
@Documented
@Target(ElementType.METHOD)
@Inherited
@Retention(RetentionPolicy.RUNTIME)
public @interface MethodInfo{
    String author() default "monster";
    String date();
    int revision() default 1;
    String comments();
}
```

@Documented – 表示使用该注解的元素应被javadoc或类似工具文档化，它应用于类型声明，类型声明的注解会影响客户端对注解元素的使用。如果一个类型声明添加了Documented注解，那么它的注解会成为被注解元素的公共API的一部分。

@Target – 作用域 表示支持注解的程序元素的种类，一些可能的值有TYPE, METHOD, CONSTRUCTOR, FIELD等等。如果Target元注解不存在，那么该注解就可以使用在任何程序元素之上。

@Target使用范围

*  CONSTRUCTOR:用于描述构造器
*  FIELD:用于描述域
*  LOCAL\_VARIABLE:用于描述局部变量
*  METHOD:用于描述方法
*  PACKAGE:用于描述包
*  PARAMETER:用于描述参数
*  TYPE:用于描述类、接口\(包括注解类型\) 或enum声明

@Inherited – 表示一个注解类型会被自动继承，如果用户在类声明的时候查询注解类型，同时类声明中也没有这个类型的注解，那么注解类型会自动查询该类的父类，这个过程将会不停地重复，直到该类型的注解被找到为止，或是到达类结构的顶层（Object）。

PS：

1.自动继承只适用于类上 在接口中不是会生效的。

2.子类只能继承父类类上的注解而不会继承方法上的。

@Retention –生命周期 表示注解类型保留时间的长短，它接收RetentionPolicy参数，可能的值有SOURCE, CLASS, 以及RUNTIME。

PS：

1.注解方法不能有参数也不能有异常生命

2.注解方法的返回类型局限于原始类型，字符串，枚举 注解 或以上类型构成的数组。

3.注解方法可以包含默认值

4.注解可以包含与其绑定的元注解，元注解为注解提供信息。（没有元注解的注解称为标识 注解）

\#\#\#\#\#解析注解代码示例

\`\`\`java

@MethodInfo\(author = "monster", comment = "class", Date = "2012-2-2", revision = 2\)

public class MethodTest {

@MethodInfo\(author = "monster", comment = "type", Date = "2012-2-2", revision = 2\)

public void test\(\) {}

public static void main\(String\[\] args\) {

// 获取类上的注解

try {

Class&lt;?&gt; c = Class.forName\("annotation.MethodTest"\);

//MethodInfo类上是否有注解

if \(c.isAnnotationPresent\(MethodInfo.class\)\) {

MethodInfo d = \(MethodInfo\) c.getAnnotation\(MethodInfo.class\);

System.out.println\(d.author\(\)\);

System.out.println\(d.comment\(\)\);

System.out.println\(d.Date\(\)\);

System.out.println\(d.revision\(\)\);

}

// 获取该类上面的所有声明类型

Method\[\] methods = c.getDeclaredMethods\(\);

Annotation\[\] annotations;

//解析方法上的注解

for \(Method method : methods\) {

// 获取每个方法上面所声明的所有注解信息

annotations = method.getDeclaredAnnotations\(\);

System.out.println\("name " + method.getName\(\)\);

// 再遍历所有的注解，打印其基本信息

for \(Annotation an : annotations\) {

System.out.println\("方法名为：" + method.getName\(\) + "其上面的注解为：" + an.annotationType\(\).getSimpleName\(\)\);

Method\[\] meths = an.annotationType\(\).getDeclaredMethods\(\);

// 遍历每个注解的所有变量

for \(Method meth : meths\) {

System.out.println\("注解的变量名为：" + meth.getName\(\)\);

}

}

}



//获取方法上的注解另外一种方式

for \(Method m : methods \) {

Annotation\[\] as = m.getAnnotations\(\);

for \(Annotation a : as\) {

if \( a instanceof MethodInfo \) {

MethodInfo d = \(MethodInfo\) a;

System.out.println\(" age " + d.author\(\)\);

}

}

}



} catch \(ClassNotFoundException e\) {

e.printStackTrace\(\);

}

}

\`\`\`

\#\#\#\#\# 利用注解实现Hirbernate 面向对象编程简单实例

首先建立@Table注解 来完成表的映射

\`\`\`java

@Retention\(RetentionPolicy.RUNTIME\)

@Target\({ElementType.TYPE}\)

public @interface Table {

String value\(\);

}

\`\`\`

建立@Column注解

\`\`\`java

@Target\({ElementType.FIELD}\)

@Retention\(RetentionPolicy.RUNTIME\)

public @interface Column {

String value\(\);

}

\`\`\`

创建Bean映射数据库表

\`\`\`java

@Table\("tb\_bean"\)

public class Bean {

@Column\("name"\)

private String name;

@Column\("age"\)

private int age;

@Column\("city"\)

private String city;

//getter and setter ...

}

\`\`\`

创建query方法

\`\`\`java

private static String query\(Bean bean\) {

if \(bean == null\) {

return null;

}

StringBuilder sb = new StringBuilder\(\);

Class&lt;?&gt; c = bean.getClass\(\);

// 檢查是否有注解

if \(!c.isAnnotationPresent\(Table.class\)\) {

return null;

}

Table t = \(Table\) c.getAnnotation\(Table.class\);

// 获取注释里填写的表名

String name = t.value\(\);

// 拼装SQL

sb.append\(" SELECT \* FROM "\).append\(name\).append\(" WHERE 1 = 1"\);

// 获取所有字段

Field\[\] arr = c.getDeclaredFields\(\);

for \(Field f : arr\) {

// 获取字段上的注解

if \(!f.isAnnotationPresent\(Column.class\)\) {

continue;

}

// 拼装get方法名

String getMethodName = "get" + f.getName\(\).substring\(0, 1\).toUpperCase\(\) + f.getName\(\).substring\(1\);

try {

// 反射获取get方法

Method getMethod = c.getMethod\(getMethodName\);

// 執行get方法

Object filedValue = getMethod.invoke\(bean\);

// 拼裝sql

if \(filedValue != null\) {

/\*\*

\*判断字段类型 本代码中只做了 字符串和数字类型的判断

\*日期等类型未做处理

\*/

if \(filedValue instanceof Integer && Integer.valueOf\(filedValue.toString\(\)\) == 0\) {

continue;

}

sb.append\(" and "\).append\(f.getName\(\)\);

if \(filedValue instanceof String\){

sb.append\("="\).append\("'"\).append\(filedValue.toString\(\)\).append\("'"\);

}else if \( filedValue instanceof Integer \){

sb.append\("="\).append\(filedValue.toString\(\)\);

}

}

} catch \(Exception e\) {

e.printStackTrace\(\);

}

}

return sb.toString\(\);

}

\`\`\`

main方法测试

\`\`\`java

public static void main\(String\[\] args\) {

Bean bean = new Bean\(\);

bean.setAge\(11\);

System.out.println\(query\(bean\)\);

}

\`\`\`

console 打印输出SQL语句

SELECT \* FROM bean WHERE 1 = 1 and age=11

