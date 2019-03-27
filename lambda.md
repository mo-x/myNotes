1. 函数接口是只有一个抽象方法的接口，用作 Lambda 表达式的类型

2. Lambda 表达式中引用的局部变量必须是 final 或既成事实上的 final 变量。\(引用值，而不是变量

3. 函数式接口

| 接口 | 参数 | 返回类型 | 示例 |
| :--- | :--- | :--- | :--- |
| Predicate&lt;T&gt; | T | boolean |  |
| UnaryOperator&lt;T&gt; | T | T | 逻辑非\(!\) |
| BinaryOperator&lt;T&gt; | \(T,T\) | T | 求两个数的乘积 |
| Consumer&lt;T&gt; | T | void |  |
| Function&lt;T,R&gt; | T | R |  |
| Supplier&lt;T&gt; | None | T | 工厂方法 |
|  |  |  |  |

4. map

如果有一个函数可以将一种类型的值转换成另外一种类型，map 操作就可以 使用该函数，将一个流中的值转换成一个新的流。

List&lt;String&gt; collected = Stream.of\("a", "b", "hello"\).map\(string -&gt; string.toUpperCase\(\)\).collect\(toList\(\)\);

5. flatMap

方法可用 Stream 替换值，然后将多个 Stream 连接成一个 Stream

List&lt;Integer&gt; together = Stream.of\(Arrays.asList\(1, 2\), Arrays.asList\(3, 4\)\).flatMap\(Collection::stream\).collect\(Collectors.toList\(\)\);

filter 遍历数据并检查其中的元素时，可尝试使用 Stream 中提供的新方法 filter

6. educe 操作可以实现从一组值中生成一个值。在上述例子中用到的 count、min 和 max 方

法，因为常用而被纳入标准库中。事实上，这些方法都是 reduce 操作。

7. forEachOrdered-保证有序

forEach-无序

转换成集合 stream.collect\(toCollection\(TreeSet::new\)\);

8. 转换成值

example:

```
public Optional<Artist> biggestGroup(Stream<Artist> artists) {
    Function<Artist,Long> getCount = artist -> artist.getMembers().count();
    return artists.collect(maxBy(comparing(getCount)));
}
```

9. 数据分块

partitioningBy，它接受一个流，并将其分成两部分

![](/assets/partitioningBy.png)

example:

* 将艺术家组成的流分成乐队和独唱歌手两部分

```
public Map<Boolean, List<Artist>> bandsAndSolo(Stream<Artist> artists) {
    return artists.collect(partitioningBy(artist -> artist.isSolo()));
}
```

* 使用方法引用将艺术家组成的 Stream 分成乐队和独唱歌手两部分

```
public Map<Boolean, List<Artist>> bandsAndSoloRef(Stream<Artist> artists) {
return artists.collect(partitioningBy(Artist::isSolo));
}
```

10. 数据分组

数据分组是一种更自然的分割数据操作，与将数据分成 ture 和 false 两部分不同，可以使用任意值对数据分组。

![](/assets/groupingBy.png)

example:

* 使用主唱对专辑分组

```
public Map<Artist, List<Album>> albumsByArtist(Stream<Album> albums) {
return albums.collect(groupingBy(album -> album.getMainMusician()));
}
```

11. 字符串

example:

* 使用流和收集器格式化艺术家姓名

```
String result =artists.stream().map(Artist::getName).collect(Collectors.joining(", ", "[", "]"));
```

12. mapping 允许在收集器的容器上执行类似 map 的操作

13. 并行化流操作

并 行 化 操 作 流 只 需 改 变 一 个 方 法 调 用。 如 果 已 经 有 一 个 Stream 对 象， 调 用 它 的

parallel 方法就能让其拥有并行操作的能力。如果想从一个集合类创建一个流，调用

parallelStream 就能立即获得一个拥有并行能力的流。

影响性能的五要素是：数据大小、源数据结构、值是否装箱、可用的 CPU 核数量，以

及处理每个元素所花的时间。

14. CompletableFuture



