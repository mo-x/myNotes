函数接口是只有一个抽象方法的接口，用作 Lambda 表达式的类型  
1. 函数式接口编写示例

1. ```java
   /** 函数式接口编写示例 添加 @FunctionalInterface 注解标明该接口为函数式接口 */
   @FunctionalInterface
   public interface GreetingService {

     void sayMessage(String message);

     /** 允许有默认方法 */
     default void doSomeMoreWork1() {
       // Method body
     }

     default void doSomeMoreWork2() {
       // Method body
     }

     /** 允许静态方法 */
     static void printHello() {
       System.out.println("Hello");
     }

     /**
      * 允许重写Object public 方法
      *
      * @param obj
      * @return
      */
     @Override
     boolean equals(Object obj);
   }
   ```

   1. 使用示例

```java
 GreetingService greetService = message -> System.out.println("Hello " + message);
 greetService.sayMessage("name");
```

* Lambda 表达式中引用的局部变量必须是 final 或既成事实上的 final 变量。\(引用值，而不是变量）

  ![](/assets/final.png)

  * Why?

    * > 1. 这是由Java对lambda表达式的实现决定的，在Java中lambda表达式是匿名类语法上的进一步简化，其本质还是调用对象的方法。  
      >    在Java中方法调用是值传递的，所以在lambda表达式中对变量的操作都是基于原变量的副本，不会影响到原变量的值。  
      >    综上，假定没有要求lambda表达式外部变量为final修饰，那么开发者会误以为外部变量的值能够在lambda表达式中被改变，而这实际是不可能的，所以要求外部变量为final是在编译期以强制手段确保用户不会在lambda表达式中做修改原变量值的操作。
      >
      > 2. lambda表达式的支持是拥抱函数式编程，而函数式编程本身不应为函数引入状态，从这个角度看，外部变量为final也一定程度迎合了这一特点

* 常用流的操作

  > 判断一个操作是惰性求值还是及早求值很简单:只需看它的返回值。如果返回值是Stream， 那么是惰性求值;如果返回值是另一个值或为空，那么就是及早求值

* * collect
  * * collect\(toList\(\)\)方法由Stream里的值生成一个列表，是一个及早求值操
    * collect\(toSet\(\)\)方法由Stream里的值生成一个set列表，是一个及早求值操作。
  * map

    * 如果有一个函数可以将一种类型的值转换成另外一种类型，map 操作就可以 使用该函数，将一个流中的值转换成一个新的流。对流中的每个元素应用函数。

    * ```java
       List<String> list = Stream.of("a", "b", "hello").map(string -> string.toUpperCase()).collect(Collectors.toList());
      ```

  * flatmap

    * 方法可用 Stream 替换值，然后将多个 Stream 连接成一个 Stream

    * ```java
      List<Integer> together = Stream.of(Arrays.asList(1, 2), Arrays.asList(3, 4)).flatMap(Collection::stream).collect(Collectors.toList());
      ```

  * filter

    * 遍历数据并检查其中的元素时，可尝试使用 Stream 中提供的新方法 filter

  * reduce

    * 操作可以实现从一组值中生成一个值。在上述例子中用到的 count、min 和 max 方

      法，因为常用而被纳入标准库中。事实上，这些方法都是 reduce 操作。

  * forEach

    * forEachOrdered-保证有序
    * forEach 无序
* 函数式接口

| 接口 | 参数 | 返回类型 | 示例 |
| :--- | :--- | :--- | :--- |
| Predicate&lt;T&gt; | T | boolean | 这张唱片发行了吗 |
| UnaryOperator&lt;T&gt; | T | T | 逻辑非\(!\) |
| BinaryOperator&lt;T&gt; | \(T,T\) | T | 求两个数的乘积 |
| Consumer&lt;T&gt; | T | void | 输出一个值 |
| Function&lt;T,R&gt; | T | R | 获得Artist对象的名字 |
| Supplier&lt;T&gt; | None | T | 工厂方法 |

1. 收集器

   1. 转换成集合

      ```java
      stream.collect(toCollection(TreeSet::new));
      ```

   2. 转换成值

      ```java
      public Optional<Artist> biggestGroup(Stream<Artist> artists) {
          Function<Artist,Long> getCount = artist -> artist.getMembers().count();
          return artists.collect(maxBy(comparing(getCount)));
      }
      ```

   3. 数据分块 -&gt; partitioningBy，它接受一个流，并将其分成两部分![](/assets/partitioningBy.png)example: 将艺术家组成的流分成乐队和独唱歌手两部分

      ```java
      public Map<Boolean, List<Artist>> bandsAndSolo(Stream<Artist> artists) {
          return artists.collect(partitioningBy(artist -> artist.isSolo()));
      }
      ```

   4. 数据分组 -&gt; 一种更自然的分割数据操作，与将数据分成 ture 和 false 两部分不同，可以使用任意值对数据分组。![](/assets/groupingBy.png)example:

      ```java
      public Map<Artist, List<Album>> albumsByArtist(Stream<Album> albums) {
            return albums.collect(groupingBy(album -> album.getMainMusician()));
          }
      ```

   5. 字符串

      example:

   6. 使用方法引用将艺术家组成的 Stream 分成乐队和独唱歌手两部分

      1. ```java
         String result =artists.stream().map(Artist::getName).collect(Collectors.joining(", ", "[", "]"));
         ```

   7. 查找与匹配

   8. mapping 允许在收集器的容器上执行类似 map 的操作

   9. 并行化流操作

      1. 并 行 化 操 作 流 只 需 改 变 一 个 方 法 调 用。 如 果 已 经 有 一 个 Stream 对 象， 调 用 它 的

         parallel 方法就能让其拥有并行操作的能力。如果想从一个集合类创建一个流，调用

         parallelStream 就能立即获得一个拥有并行能力的流。

         影响性能的五要素是：数据大小、源数据结构、值是否装箱、可用的 CPU 核数量，以

         及处理每个元素所花的时间。

   10. 方法引用:

      1. Classname::methodName



